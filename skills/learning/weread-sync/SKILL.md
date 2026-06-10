---
name: weread-sync
description: "将微信读书划线增量同步到 Obsidian vault。当用户说'拉取划线'、'导出划线'、'同步划线'、'拉取《书名》的划线'、'导出笔记到vault'、'weread sync'、'同步微信读书'时触发。也适用于用户提到某本书并想把划线保存到笔记库的场景。"
version: 1.0.0
---

# WeRead Sync — 微信读书划线同步到 Vault

将微信读书中的划线和想法增量同步到 `sources/book/{书名}.md`。

核心原则：**只追加，不覆盖，不删除**。本 skill 是搬运工——把远端新增的不可变事实投影到本地文件，不做解读和重组。

## 前置条件

- 环境变量 `WEREAD_API_KEY` 已设置（格式 `wrk-xxxxxxxx`）
- 若未设置，提示用户：`export WEREAD_API_KEY=<你的apikey>`

## 触发与输入

用户会说：
- "拉取《穷查理宝典》的划线"
- "同步一下经济学原理"
- "导出划线"（无书名 → 问一句"哪本书？"）

从用户输入提取书名。没有书名则追问。

## 执行流程

### Step 1: 搜索书籍 → 拿 bookId

```bash
curl -s -X POST "https://i.weread.qq.com/api/agent/gateway" \
  -H "Authorization: Bearer $WEREAD_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"api_name": "/store/search", "keyword": "{书名}", "scope": 10, "skill_version": "1.0.3"}'
```

- 命中 1 本 → 直接用
- 命中多本 → 列出候选（编号+书名+作者），让用户选
- 0 结果 → 报错终止

记录：bookId, title, author, cover, category, isbn, publisher, publishTime。

### Step 2: 拉取划线

```bash
curl -s -X POST "https://i.weread.qq.com/api/agent/gateway" \
  -H "Authorization: Bearer $WEREAD_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"api_name": "/book/bookmarklist", "bookId": "{bookId}", "skill_version": "1.0.3"}'
```

回包结构：`updated[]`（划线数组）+ `chapters[]`（章节映射）。

**如果 `updated` 为空或不存在 → 报告"该书无划线"，终止，不创建文件。**

### Step 3: 拉取想法（可选）

```bash
curl -s -X POST "https://i.weread.qq.com/api/agent/gateway" \
  -H "Authorization: Bearer $WEREAD_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"api_name": "/review/list/mine", "bookid": "{bookId}", "count": 100, "skill_version": "1.0.3"}'
```

注意：此接口 bookid 是小写。如果 `hasMore=1`，用 `synckey` 翻页拉完。

将想法按 `chapterUid` + `range` 关联到对应划线（想法的 range 包含在划线的 range 区间内时关联）。

### Step 4: 拉取阅读进度

```bash
curl -s -X POST "https://i.weread.qq.com/api/agent/gateway" \
  -H "Authorization: Bearer $WEREAD_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"api_name": "/book/getprogress", "bookId": "{bookId}", "skill_version": "1.0.3"}'
```

提取：progress, recordReadingTime, updateTime。

### Step 5: 差异计算

检查文件是否存在：`sources/book/{title}.md`

#### 首次（文件不存在）

全量写入，跳到 Step 6。

#### 增量（文件已存在）

1. 验证文件有 `doc_type: weread-highlights-reviews` frontmatter。没有 → 报错"同名文件存在但非 weread-sync 生成，拒绝写入"，终止。

2. **ID 标准化**：API 返回的 bookmarkId 用下划线分隔（如 `837932_101_466-533`），但本地 block reference 统一用短横线（如 `^837932-101-466-533`）。比对时将 API 的 `_` 替换为 `-`，写入时也用 `-` 格式。

3. 提取本地已有 bookmarkId：
```bash
grep -oP '\^[\w-]+' "sources/book/{title}.md" | sed 's/^\^//' | sort > /tmp/wr_local_ids.txt
```

4. 提取远端所有 bookmarkId 并标准化（`_` → `-`）：
```bash
echo '{step2_json}' | jq -r '.updated[].bookmarkId' | tr '_' '-' | sort > /tmp/wr_remote_ids.txt
```

5. 计算差集：
```bash
comm -23 /tmp/wr_remote_ids.txt /tmp/wr_local_ids.txt > /tmp/wr_new_ids.txt
```

6. 如果差集为空 → 报告"无新增划线，已是最新"，终止。

6. 从 Step 2 JSON 中筛选新增条目（bookmarkId 在差集中的）。

### Step 6: 格式化与写入

#### Frontmatter（仅首次创建时生成）

```yaml
---
doc_type: weread-highlights-reviews
bookId: "{bookId}"
title: {title}
tags:
  - {tag1}
  - {tag2}
reviewCount: 0
noteCount: {划线实际条数}
author: {author}
cover: {cover_url}
readingStatus: "{status}"
progress: {progress}%
readingTime: {X小时Y分钟}
readingDate: {首条划线日期}
isbn: {isbn}
lastReadDate: {最后一条划线的日期}
---
```

**Tag 推断规则**（仅首次）：
- 根据书的 category、title、内容领域推断 2-4 个 tag
- Tag 是领域/主题维度，用于跨目录聚合（如 `经济学`、`投资`、`思维模型`）
- 不加 `weread-highlights-reviews`（那是 doc_type，不是 tag）
- 不重复文件夹信息（不加 `book`）

#### 文件正文结构

```markdown
# 元数据
> [!abstract] {title}
> - ![ {title}|200]({cover_url})
> - 书名： {title}
> - 作者： {author}
> - 简介： {intro}
> - 出版时间： {publishTime}
> - ISBN： {isbn}
> - 分类： {category}
> - 出版社： {publisher}
> - PC地址：https://weread.qq.com/web/reader/{bookId的hex编码}

# 高亮划线
## {章节标题}
> 📌 [{markText}](<weread://bestbookmark?bookId={bookId}&chapterUid={chapterUid}&rangeStart={rangeStart}&rangeEnd={rangeEnd}>)
> ⏱ {createTime转YYYY-MM-DD HH:mm:ss} ^{bookmarkId}

# 读书笔记

# 本书评论
```

#### 每条划线的 Literal Template

**严格按此格式，逐字填空，不做任何变体：**

```
> 📌 [{markText}](<weread://bestbookmark?bookId={bookId}&chapterUid={chapterUid}&rangeStart={rangeStart}&rangeEnd={rangeEnd}>)
> ⏱ {YYYY-MM-DD HH:mm:ss} ^{bookmarkId}
```

其中 range 解析：API 返回 `range: "1165-1203"` → rangeStart=1165, rangeEnd=1203。

**bookmarkId 标准化**：API 返回 `837932_101_466-533`，写入时转为 `837932-101-466-533`（`_` → `-`），与已有文件格式一致。

如有关联想法，紧跟其后（无空行）：
```
> 💭 {想法内容}
```

每条划线之间空一行。

#### 章节分组与排序

- 用 `chapters[]` 数组将划线按 `chapterUid` 分组
- 章节标题从 `chapters[].title` 获取（通过 chapterUid 匹配）
- **首次写入**：章节按 `chapterIdx` 排序，章节内划线按 `rangeStart` 升序
- **增量写入**：新划线追加到对应 `## 章节标题` 的末尾（下一个 `##` 或 `# 读书笔记` 之前）

#### 增量写入的锚点定位

找到章节末尾的方法：
1. 在文件中定位 `## {章节标题}`
2. 找到这个 heading 之后、下一个 `##` 或 `# 读书笔记` 之前的最后一行内容
3. 在该位置之后插入新划线

如果新划线的章节在文件中不存在：
- 在 `# 读书笔记` 之前插入新的 `## {章节标题}` + 划线内容

### Step 7: 更新 Frontmatter 元数据（增量时）

- `noteCount`：重新计数本地文件中 `^` block reference 的数量（`grep -c '\^' file`）
- `lastReadDate`：取所有划线中最新的 createTime，转为 YYYY-MM-DD

**不碰 tags**（用户可能已手动修改）。

### Step 8: 报告

```
完成：《{title}》新增 {N} 条划线 → sources/book/{title}.md
```

增量时额外说明：`（累计 {total} 条）`

## 铁律

这些是硬约束，不可违反：

1. **只追加不删除**：已有划线永不删除，即使远端删了
2. **不改已有文本**：已有划线的 markText 不修改，即使远端有更新
3. **不碰用户内容**：`<mark>` 标签、手动添加的段落、任何非 `> 📌` 开头的行——一律不碰
4. **去重靠 ID**：以 `^{bookmarkId}` 的存在性判断，不比对文本内容
5. **Tags 写一次**：首次推断 tags 后，增量永不修改 tags
6. **noteCount = 本地实数**：用 grep 统计本地 `^` 数量，不用 API 返回值
7. **0 划线不建文件**：API 返回空 → 报错终止，不创建空文件
8. **同名保护**：文件存在但无 `doc_type` → 报错，不覆盖
9. **原子性**：所有新增条目一次性写入；如果中途出错，不留半成品

## 大批量优化（>50 条新增）

当新增划线超过 50 条时：
1. 将 API 返回的完整 JSON 写入临时文件（`/tmp/wr_highlights_{bookId}.json`）
2. 用 `jq` 提取 ID 和字段，避免大量 JSON 占用对话上下文
3. 分章节批次写入，每个章节一次 Edit 操作
4. 完成后清理临时文件

## 与 weread-skills 的关系

本 skill 依赖 weread-skills 定义的 API 调用规范（gateway 地址、鉴权方式、参数格式）。两者分工：
- `weread-skills`：交互式查询（搜书、看书架、看统计）
- `weread-sync`：持久化同步（划线 → vault 文件）

调用 API 时遵循 weread-skills 的通用规则（skill_version、参数平铺、时间戳转换等）。
