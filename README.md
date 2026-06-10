# skills-learning

English | [中文](#中文)

## English

This repository contains learning-oriented skills and supporting material.

### Rules

- Keep study content, note-taking, and teaching artifacts here.
- Keep executable learning skills here, but do not treat this repo as the default agent skill catalog.
- If a learning topic later becomes a real agent workflow, move the executable part into `skills-develop`.
- Put reusable shared skills in `skills-common`.

### Install

Use project scope for repo-local installs:

```bash
npx skills add /Users/yangzhao/Code/skills-learning -y
```

Use global scope when you want the learning assets available across all projects:

```bash
npx skills add /Users/yangzhao/Code/skills-learning -g -y
```

Project scope is the default. `-g` switches to user-level installation.

### Layout

- `skills/learning/`
- `learning/`
- `reference/`
- `lessons/`
- `records/`

Sibling repos:

- [skills-develop](../skills-develop/README.md)
- [skills-common](../skills-common/README.md)

## 中文

这个仓库只放学习型 skill 和配套材料。

### 规则

- 学习内容、笔记、教学产物都放这里。
- 可以放可执行的学习型 skill，但不要把这个仓库暴露成默认的 agent skill 目录。
- 如果某个学习主题后来变成了真正的 agent 工作流，把可执行部分迁到 `skills-develop`。
- 可复用的通用 skill 放到 `skills-common`。

### 安装

项目级安装，适合只在当前仓库生效：

```bash
npx skills add /Users/yangzhao/Code/skills-learning -y
```

全局安装，适合所有项目都可用：

```bash
npx skills add /Users/yangzhao/Code/skills-learning -g -y
```

`npx skills add` 默认是项目级，`-g` 会切换成用户级安装。

### 目录

- `skills/learning/`
- `learning/`
- `reference/`
- `lessons/`
- `records/`

兄弟仓库：

- [skills-develop](../skills-develop/README.md)
- [skills-common](../skills-common/README.md)
