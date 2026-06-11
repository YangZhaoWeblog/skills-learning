# skills-learning

<p align="center"><em>学习型 skill 和配套材料。保留学习流程，不保留噪音。</em></p>

<p align="center">
  <img alt="License MIT" src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge">
  <img alt="Agent Skills Standard" src="https://img.shields.io/badge/Agent%20Skills-Standard-6DA544?style=for-the-badge">
  <img alt="skills.sh Compatible" src="https://img.shields.io/badge/skills.sh-Compatible-1E6FFF?style=for-the-badge">
  <img alt="Runtime" src="https://img.shields.io/badge/Runtime-Claude%20Code%20·%20Codex%20·%20Cursor-8A2BE2?style=for-the-badge">
</p>

<p align="center"><strong>这个仓库只放学习型 skill 以及支撑它们的材料。</strong></p>

中文 | [English](README.md)

* * *

## 放这里

- 学习内容、笔记、教学产物
- 放在学习路径里的可执行 skill
- 帮助理解、记忆、复盘的材料

## 不放这里

- 默认的开发类 agent skill 目录
- 应该放到 `skills-common` 的通用 skill
- 应该放到 `skills-develop` 的工程工作流 skill

## 安装

### 项目级

装到当前仓库。

```bash
npx skills add /Users/yangzhao/Code/skills-learning -y
```

### 全局级

装到你的用户空间。

```bash
npx skills add /Users/yangzhao/Code/skills-learning -g -y
```

`npx skills add` 默认是项目级，`-g` 会切换成用户级安装。

* * *

## Skill

| Skill | 作用 |
| --- | --- |
| `canvas-make-cards` | 把深度学习笔记变成 canvas 卡片。 |
| `canvas-refine` | 把零散摘录整理成树。 |
| `coach` | 对学习产出做教练式反馈。 |
| `td-card-coach` | 打磨卡片的提问和答案。 |
| `td-decompose` | 判断主题要不要拆成知识 DAG。 |
| `td-extract-atoms` | 从文本里提取原子笔记。 |
| `td-learn-deep` | 对一个具体概念做深挖。 |
| `td-light-learn` | 给已有笔记补一个小知识点。 |
| `td-make-cards` | 把已理解的内容转成复习卡片。 |
| `td-synthesize` | 把多个概念缝成一张 MOC。 |
| `weread-skills` | 搜书、书架、笔记、书评和统计。 |
| `weread-sync` | 把微信读书划线同步到 Obsidian。 |
| `xray-self` | 透视长期知识和工作模式。 |

## 目录

- `skills/learning/`
- `learning/`
- `reference/`
- `lessons/`
- `records/`

兄弟仓库：

- [skills-develop](../skills-develop/README.zh-CN.md)
- [skills-common](../skills-common/README.zh-CN.md)
