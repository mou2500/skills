# Skill RPG 升级循环 · v1.1.0

> **把每个 skill 当成 RPG 角色来练级。**
> 用得多 → 经验积累 → 教训提炼 → 触发 darwin-skill 自动优化 → 等级提升。
> 你从"手动优化 skill" 变成"设计系统让 skill 从使用中学习并自己进化"。

---

## 概述

**skill-rpg-loop** 是 [Loop Engineering](https://github.com/addyosmani/loop-engineering) 理念的一个具体实现。它将 darwin-skill 的自动优化能力与角色扮演游戏的升级机制结合, 让技能在使用中自然进化。

| 要素 | 含义 |
|------|------|
| **XP (经验值)** | 每次使用 skill, 根据结果获取 XP |
| **Level (等级)** | XP 积累到阈值后, 触发 darwin-skill 优化, 升级 |
| **Lessons (教训)** | 使用中暴露的问题被记录, 升级时作为优化方向 |
| **进化向量** | darwin-skill 对 SKILL.md 做 9 维度评分 + 定向改进 |

---

## 核心设计

### XP 权重体系

XP 不是"用一次 +1" 的简单堆叠, 而是按使用结果加权:

| 结果 | XP | 含义 | 处理 |
|------|----|------|------|
| `ok` | +1 | 一次成功, 无修正 | 归档为成功模式 |
| `modified` | +3 | 需修正后才成功, 有教训 | 记入 lessons, 升级时优化 |
| `failed` | +5 | 多轮失败或放弃 | 最高优先级, 升级时优先修 |

经验的价值由**内容**决定 — 失败案例的教训 > 成功案例的常规操作。

### 宝可梦式积累

XP 不设上限, 升级不归零:

```
升级阈值: XP ≥ 5
升级消耗: XP -= 5 (不归零)
连升: 若剩余 XP ≥ 5, 继续升级直至 < 5
```

### 教训驱动 + 目的检查

升级时带着 lessons 中的真实问题去改, 但升级本身不是目的:

```
Disovery → 读 lessons → 提取 TOP 3 高频问题
   ↓
Phase 2 (darwin) → 带着 lessons 定向优化
   ↓
Verify → 检查 lessons 问题是否修复
   ↓
你确认 → 标记已修复 / 保留到下次
```

---

## 系统架构

### 五步映射 (Loop Engineering)

| 五步 | 实现 |
|------|------|
| **Discovery** | 读 `~/.claude/memory/skill-usage-log.md` 发现使用记录; 扫描 `~/.claude/skills/` 自动注册新 skill |
| **Handoff** | 用 sub-agent 触发 darwin-skill 优化, 同时带入 lessons |
| **Verification** | darwin-skill 独立 judge 评分 + lessons 修复检查 + 你确认 |
| **Persistence** | 更新 `~/.claude/memory/skill-levels.md` + `~/.claude/memory/lessons/<skill>.md` |
| **Scheduling** | `/loop` 会话内 / `codepilot_schedule_task` 云调度 |

### 文件体系

```
~/.claude/skills/skill-rpg-loop/
├── SKILL.md          ← 本 skill 定义 (含完整工作流)
└── README.md         ← 本文件

~/.claude/memory/
├── skill-levels.md         ← 等级 state (所有 skill 的 XP/等级/履历)
├── skill-usage-log.md      ← 使用日志 (agent 自动写入, 含结果和教训)
├── patches/                ← 第三方 skill 的优化建议 (不直接改源码)
└── lessons/
    ├── _template.md        ← 教训文件模板
    ├── my-skill.md         ← (随使用积累)
    └── ...                 ← 每个 skill 在首次产生教训时自动创建
```

---

## 使用方式

### 查看等级

```
/skill-rpg-loop 查看等级
→ 显示所有 skill 的等级、XP、下次升级所需次数
```

### 查看教训

```
/skill-rpg-loop 查看教训: my-skill
→ 展示 my-skill 的 lesson 文件 (成功模式 + 需修正模式)
```

### 手动记 XP

```
/skill-rpg-loop 记录使用: my-skill
→ 追问: 结果如何? (ok / modified / failed)
→ 若有教训, 追问: 发生了什么?
→ XP 加权, 教训记入 lessons
```

### 手动触发升级

```
/skill-rpg-loop 升级所有待升级技能
→ 找出所有 XP ≥ 5 的 skill
→ 逐个执行 darwin-skill 优化 (带 lessons)
→ 每次升级后让你确认
```

### 定时自动巡检 (推荐)

```
# 会话内 (关闭终端即停):
/loop 6h /skill-rpg-loop 自动巡检

# 云调度 (关电脑也在跑):
/skill-rpg-loop 设置 6 小时自动巡检升级
```

---

## 前置依赖

| 依赖 | 用途 | 安装方式 |
|------|------|---------|
| **darwin-skill** (v2.0+) | skill 自动优化引擎 (9 维度评分 / hill-climbing / 独立 judge) | `npx skills add alchaincyf/darwin-skill` |
| **Agent 工具** | sub-agent 隔离 (Generator / Evaluator 分离) | 当前 CLI 内置 |
| **MCP** | 通知升级结果 | 当前 CLI 内置 |

---

## 设计哲学

> **"Build the loop, but build it like someone who intends to stay the engineer."**
> — HuaShu, Loop Engineering

- **生成交给系统**: darwin-skill 自动评估、改代码、测试、回滚
- **判断留给你**: 每次升级后展示改动, 你决定保留或回滚
- **经验来自内容, 不是次数**: 一次成功是"用", 多次修正才是"学"
- **教训驱动升级**: 带着真实案例去改, 不是 9 维度空洞提分
- **目的检查**: 升级前先问"设计有没有偏离目的", 不必要的优化不强行升级

---

## 路线图

- [ ] **自动发现新 skill**: ✅ v1.0.0 — 巡检时扫描磁盘自动注册
- [ ] **跨 session 日志**: 全局 CLAUDE.md 指令, 所有 session 自动写 usage-log
- [ ] **lessons 聚合**: 同类型教训聚合, 自动识别高频模式
- [x] **目的检查**: ✅ v1.1.0 — 升级前 🔴 CHECKPOINT 问"设计有没有偏离目的"

- [ ] **升级结果可视化**: darwin 优化后生成评分变化卡片
- [ ] **等级称号系统**: 不同等级获得不同称号和优化优先级

---

## Credits

- **Loop Engineering** — Addy Osmani / HuaShu
- **darwin-skill** — alchaincyf
- **SkillLens / SkillOpt** — Microsoft Research

> "The agent forgets, the repo doesn't." — 但 level 和经验值不会忘。
