# Sebastian — 工程管家 v2

> 版本 2.4.0 | 作者：何牟 | 来源：内部自建

## 概述

Sebastian 是一个元技能（meta-skill），负责编排多步骤工作流。它是调度员，不是执行者。

核心能力：
- Phase 0 意图理解（预过滤 → 6 维分解 → 澄清门）
- Skills 加权匹配与三级定级（EXACT / INDIRECT / NOMATCH）
- 阈值熔断与反问机制
- Scope Guard 内容 vs 格式对齐检查
- 冲突消解规则（上游优先 / 垂直分离 / 早退出）
- 7 种工作流模板编排
- 双轨记录：Lessons 复盘 + rpg-loop XP 经验值
- 索引健康诊断与学习日志审查

## 自建技能列表

工作流中用到的以下自建技能需要经 rpg-loop 记录 XP：

- `sebastian` — 工程管家（自身）
- `bifeng` — 多文体中文写作
- `novel-learner` — 小说创作学习
- `panel-of-experts` — 多专家协作
- `shuixian` — 美术总监
- `text2img` — 文生图
- `img2img` — 图生图
- `image-recognition` — 识图
- `skill-rpg-loop` — 技能 RPG 升级循环

## 快速使用

```bash
/sebastian <任务描述>        # 核心编排
/sebastian update index      # 更新索引
/sebastian list              # 列出索引
/sebastian find <关键词>      # 搜索技能
/sebastian diagnose index    # 索引健康诊断
/sebastian review            # 审查学习日志
```

## 相关技能

- `find-skill` — 搜索安装新技能
- `darwin-skill` — 技能自动优化
- `skill-creator` — 创建新技能
- `skill-rpg-loop` — XP 经验值与升级循环
