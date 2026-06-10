# skills-learning

English | [中文](README.zh-CN.md)

This repository contains learning-oriented skills and supporting material.

## Rules

- Keep study content, note-taking, and teaching artifacts here.
- Keep executable learning skills here, but do not treat this repo as the default agent skill catalog.
- If a learning topic later becomes a real agent workflow, move the executable part into `skills-develop`.
- Put reusable shared skills in `skills-common`.

## Install

Use project scope for repo-local installs:

```bash
npx skills add /Users/yangzhao/Code/skills-learning -y
```

Use global scope when you want the learning assets available across all projects:

```bash
npx skills add /Users/yangzhao/Code/skills-learning -g -y
```

Project scope is the default. `-g` switches to user-level installation.

## Layout

- `skills/learning/`
- `learning/`
- `reference/`
- `lessons/`
- `records/`

Sibling repos:

- [skills-develop](../skills-develop/README.md)
- [skills-common](../skills-common/README.md)
