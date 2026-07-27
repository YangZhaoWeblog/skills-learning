---
name: xray-self
description: Scan an Obsidian vault and write a concise, evidence-led self portrait centered on the highest-leverage next decision, including the relevant ability circle and bottleneck. Use when the user says xray-self, 自我透视, vault 自画像, 隐藏模式, 给我做画像, 自我诊断, 体检, or asks to inspect long-term knowledge/work patterns across an Obsidian vault.
user_invocable: true
---

# xray-self

## Mission

扫描 Obsidian vault，把人当作一个可观察、可修正的对象：从完整证据里挑出**最影响下一步的一个判断**，同时指出可调用的能力圈和会限制它的缺口。目标不是写全自我档案，也不是心理分析，而是提高外部可验证产出的质量和频率。

## Operating rules

- 默认扫描当前 vault：`/Users/yangzhao/Documents/MyDigitalGarden`。
- 优先使用 Obsidian CLI；CLI 不足时用 Glob/Grep/Read/Bash 读只读信息。
- 输出写入：`2.工作稿/个人规划/xray-self/自画像-{YYYY-MM-DD}.md`。
- 必须创建/更新一份画像文件；不要只在终端口头总结。
- 不做心理/医学诊断，不把人格标签当结论。
- 默认是**决策画像**：1 个主判断、最多 3 条决定性证据、1 个能力圈、1 个关键缺口、1-2 个行动。用户明确要“完整画像 / 全景画像”时，才可写最多 3 个相互独立的判断。
- 完成五层扫描不等于把五层都写进正文。只输出会改变主判断、能力圈或行动的证据；原始统计留在扫描过程。
- 不把单一信号写成刺痛；刺痛至少需要两个独立信号，并且只能说明“若不处理的代价”，不得重述主判断。
- 不把推测写成事实；所有深层判断都标 `[推测]`。
- 每个核心结论必须写：证据、置信度、可推翻条件。

## Scan method

按五层扫描，不可跳层：

1. **统计分布**：目录、标签、文件数量、近期活跃、主题频率。
2. **加工深度**：raw/source → inbox → 工作稿 → deep/light-learn → 原子笔记 → canvas/card → MOC → 外部输出。
3. **链接拓扑**：backlinks、outlinks、hub、bridge、orphan、deadend、unresolved。
4. **无意识痕迹**：重复词、空文件、搁置、过度系统化、反复 should、强烈措辞。
5. **时间轨迹**：最近增强、衰减、反复返回的问题、输出变化。

扫描后先写内部判断账本，不要立刻成文：

| 候选判断 | 决定性证据 | 反证 / 例外 | 能改变的下一步 |
|---|---|---|---|

选择证据最短、行动影响最大的一个判断。若没有判断同时满足“至少两条独立信号”和“能改变行动”，如实写“本次无高置信度断点”，不要用数据量补篇幅。

## Evidence protocol

| 层级 | 含义 | 可写什么 |
|---|---|---|
| `[确定]` | 可直接观察 | 数量、路径、标签、链接、文件存在、修改记录 |
| `[较确定]` | 多信号模式 | 执行力分布、加工深度差异、主题聚集/搁置 |
| `[推测]` | 深层解释 | 欲望层级、核心焦虑、操作系统、被忽视资产 |

- 每条决定性证据必须能回指到路径、链接查询或扫描事实；精确数字没有改变判断时不写。
- 判断“当前策略 / 当前状态 / 已暂停 / 已取代”前，核对文档 `status`、更新时间、明确措辞和来源优先级。`discussion`、候选池或研究稿不会自动取代现行计划。
- unresolved 链接只能证明入口未解析，不能单独证明欲望、拖延或执行失败。
- 能力圈也必须有证据，并写明它**在本次行动中能做什么**和**不能替代什么**；不要列与当前判断无关的优点。

## Output discipline

使用 `references/portrait-template.md`。一条判断只保留一个主载体：标题/开场给默认动作，结论表给证据与可推翻条件，刺痛只写不处理的代价，行动只写下一步。

- 不重复同一判断于摘要、扫描、结论、刺痛和资产。
- 表格只用于稳定的横向比较；扫描层不需要逐层展开时，用短证据表。
- 行动只修复主判断揭示的断点；期限是建议，不得伪装成扫描事实。

## Completion checklist

- [ ] 完成五层扫描，并从判断账本选出最高杠杆结论。
- [ ] 默认不超过 1 个主判断、3 条决定性证据、1 个能力圈、1 个关键缺口、2 个行动。
- [ ] 明确区分 `[确定]` / `[较确定]` / `[推测]`，且每个判断有可推翻条件。
- [ ] 刺痛由两条独立信号支撑，且新增的是代价而非重复。
- [ ] 能力圈说明本轮可调用的能力与边界。
- [ ] 画像已写入 vault 指定路径。
