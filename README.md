# skills-learning

<p align="center"><em>Learning skills and supporting material. Keep the teaching flow, not the noise.</em></p>

<p align="center">
  <img alt="License MIT" src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge">
  <img alt="Agent Skills Standard" src="https://img.shields.io/badge/Agent%20Skills-Standard-6DA544?style=for-the-badge">
  <img alt="skills.sh Compatible" src="https://img.shields.io/badge/skills.sh-Compatible-1E6FFF?style=for-the-badge">
  <img alt="Runtime" src="https://img.shields.io/badge/Runtime-Claude%20Code%20·%20Codex%20·%20Cursor-8A2BE2?style=for-the-badge">
</p>

<p align="center"><strong>This repository contains learning-oriented skills and the materials that support them.</strong></p>

English | [中文](README.zh-CN.md)

* * *

## What belongs here

- study content, note-taking, and teaching artifacts
- executable learning skills that are meant to stay in a learning lane
- artifacts that support understanding, retention, or review

## What stays out

- the default agent skill catalog for development work
- shared reusable skills that should live in `skills-common`
- workflow-only engineering skills that belong in `skills-develop`

## Install

### Project scope

Install into the current repo.

```bash
npx skills@latest add YangZhaoWeblog/skills-learning -y
```

### Global scope

Install into your user space.

```bash
npx skills@latest add YangZhaoWeblog/skills-learning -g -y
```

Project scope is the default. `-g` switches to user-level installation.

### Single skill

Install only one skill from this repository.

```bash
npx skills@latest add YangZhaoWeblog/skills-learning --skill td-learn-deep -y
```

Install one local skill directory.

```bash
npx skills@latest add /Users/yangzhao/Code/skills-learning/skills/learning/td-learn-deep -y
```

Use one skill without installing it.

```bash
npx skills@latest use YangZhaoWeblog/skills-learning@td-learn-deep
```

Replace `td-learn-deep` with the target skill name. Add `-g` to install that single skill globally.

* * *

## Skills

| Skill | What it does |
| --- | --- |
| `canvas-make-cards` | Turn deep-learn notes into canvas cards. |
| `canvas-refine` | Refine scattered canvas excerpts into a tree. |
| `coach` | Give mentor-style feedback on learning output. |
| `td-card-coach` | Tighten question/answer quality for cards. |
| `td-decompose` | Decide whether a topic needs a DAG or a deep dive. |
| `td-extract-atoms` | Extract atomic notes from text into Zettelkasten files. |
| `td-learn-deep` | Deep-dive one concrete concept step by step. |
| `td-light-learn` | Attach a small knowledge patch to an existing note. |
| `td-make-cards` | Turn understood material into review cards. |
| `td-synthesize` | Build a MOC by connecting learned concepts. |
| `weread-skills` | Search books, shelves, notes, reviews, and stats. |
| `weread-sync` | Sync WeRead highlights into Obsidian. |
| `xray-self` | Inspect long-term knowledge and work patterns. |

## Layout

- `skills/learning/`
- `learning/`
- `reference/`
- `lessons/`
- `records/`

Sibling repos:

- [skills-develop](../skills-develop/README.md)
- [skills-common](../skills-common/README.md)
