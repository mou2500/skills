---
name: skill-rpg-loop
description: "Skill RPG 升级循环（经验驱动, 含教训积累）。每个 skill 的 XP 由使用结果决定: ok=+1, modified=+3, failed=+5。XP ≥ 5 触发 darwin-skill 升级, XP -= 5 (不归零)。升级时自动带入 lessons 优化方向。支持查看等级/经验/教训/履历, 定时自动巡检。使用场景: '练级/升级技能/查看等级/检查需要升级的技能/跑一次升级循环/升级所有待升级技能/查看履历/查看教训/练练这些技能'"
author: 何牟
version: 1.0.0
tags: [optimization, meta, learning, loop]
capabilities: Skill经验值管理(ok+1/modified+3/failed+5)、XP≥5触发darwin-skill升级、等级/经验/教训/履历查看、定时自动巡检
scenarios: 技能练级、技能升级、查看技能等级、技能经验审查
paired_with: [darwin-skill, sebastian]
---

# Skill RPG 升级循环

> **Loop Engineering 实践**: 把每个 skill 当成 RPG 角色来练级。
> 每次使用根据结果积累 XP (ok=+1, modified=+3, failed=+5), 教训记入 lessons。
> XP ≥ 5 → 触发 darwin-skill, 带着 lessons 优化 → 等级提升。XP -= 5, 剩余带入下级。
> 你从"手动优化 skill" 变成 "设计系统让 skill 从使用中学习并自己进化"。

---

## 设计哲学

### 为什么要做这个

Loop Engineering 的核心观点之一: **稀缺的从来不是生成 (generation), 是判断 (judgment)**。本 skill 把这句话落地:

- **生成交给系统**: darwin-skill 自动评估、改代码、测试、回滚
- **判断留给你**: 每次升级后展示改变, 你决定保留 or 回滚
- **经验来自内容, 不是次数**: XP 按"一次成功/需修正/多轮失败"分别加权, 失败案例最有价值
- **教训驱动升级**: 升级时不是盲目优化, 而是带着 lessons 中的真实问题去改
- **目的检查**: 升级不是为了"攒经验"本身, 而是为了让 skill 更贴合使用场景。升级前先问"设计有没有偏离目的"

### Loop Engineering 五步映射

| 五步 | 本 skill 的实现 |
|------|----------------|
| **Discovery** | 读 `~/.claude/memory/skill-usage-log.md` 发现新使用记录, 同步到 skill-levels.md 后找出 XP ≥ 5 的 skill |
| **Handoff** | 用 sub-agent 触发 darwin-skill 优化该 skill |
| **Verification** | darwin-skill 自带的独立 judge 评分 + 你确认保留/回滚 |
| **Persistence** | 更新 `~/.claude/memory/skill-levels.md` (等级、XP、履历) |
| **Scheduling** | `/loop` 定时跑 / codepilot_schedule_task 后台跑 |

### 六部件映射

| 部件 | 本 skill 的对应 |
|------|----------------|
| **Automations** | 6 小时定时循环 (或你自定义) |
| **Worktrees** | 每次 darwin-skill 优化在自己的分支上 (git) |
| **Skills** | 本文件 + darwin-skill — skill 练 skill |
| **Connectors** | MCP: codepilot_notify 通知升级结果 |
| **Sub-agents** | darwin-skill 自带独立 judge agent |
| **Memory** | `~/.claude/memory/skill-levels.md` — agent 会忘, repo 不会 |

---

## 等级系统

### 规则

| 规则 | 说明 |
|------|------|
| XP 获取 | 每次使用 skill 完成任务 → XP 由结果决定: ok=+1, modified=+3, failed=+5 |
| 什么是 ok | 一次成功, 无用户修正 → 流程正确, 固化即可 |
| 什么是 modified | 需修正后才成功 → 有教训值得学, 升级时优化 |
| 什么是 failed | 多轮失败或放弃 → 最值得优化, 升级时优先修 |
| 升级阈值 | XP ≥ 5 可触发升级 (dawin-skill 优化) — **宝可梦式积累** |
| 等级公式 | 等级 = floor(总升级次数) |
| XP 扣减 | 升级后 **XP -= 5** (不归零), 剩余 XP 带入下一级; 若剩余 ≥ 5 则连升 |
| 同 skill 不重复升级 | 连续 2 次升级间隔 < 1 小时 → 跳过 (防抖) |

### 等级与频率建议

| 等级 | 含义 | 优化频率建议 |
|------|------|-------------|
| L0 | 刚注册 | 尽快凑 5 XP 首升 |
| L1-L3 | 初学者 | 正常每 5 次一升 |
| L4-L6 | 熟练 | 每 10 次一升 (达芬奇计划: 累积更多经验再做大优化) |
| L7+ | 大师 | 按需手动触发 |

---

## 工作流

### 模式 1: 自动从使用日志积累 XP (Discovery)

不依赖你手动汇报。skill-rpg-loop 从日志文件自动发现使用记录:

0. **自动发现新 skill**: 扫描 `~/.claude/skills/*/SKILL.md`, 比对 state 文件:
   - 磁盘上有但 state 没有 → 自动追加: 来源=upstream (可手动更正), Lv0, XP=0, 显示"新发现"
   - 磁盘上已删除但 state 还有 → 标记为"已卸载" (保留履历不丢)
1. 读 `~/.claude/memory/skill-usage-log.md` (全局 CLAUDE.md 要求所有 skill 写此文件)
2. 比对 skill-levels.md 已记录的"上次同步位置", 找出新增记录
3. 每条新增记录, 读取"结果"列确定 XP 权重:
   - `ok` → XP +1 (流程正确, 固化即可)
   - `modified` → XP +3 (有教训, 升级时优化)
   - `failed` → XP +5 (最值得优化)
   - 若有教训内容, 同步写入 `~/.claude/memory/lessons/<skill>.md`
4. 更新"上次同步位置"
5. 检查是否有 skill XP ≥ 5
   - 否 → 更新 state 文件, 报告当前等级和 XP
   - 是 → 进入升级模式 (模式 2)

### 模式 2: 升级循环 (自动触发)

当某 skill XP ≥ 5 时:

```
1. 读取 state 文件, 确认待升级 skill 和 its 来源

   ├─ own (自有技能): 直接优化 SKILL.md (全量修改)
   └─ upstream (第三方技能): 不碰 SKILL.md, 产出"补丁建议"
      ~/.claude/memory/patches/<skill>-<date>.md

🔴 CHECKPOINT · 🛑 STOP: 目的检查
   - 升级不是为了"攒经验升级"本身, 而是为了让 skill 更贴合使用场景
   - 自问: 这个 skill 的 XP 高是因为它出了问题要修, 还是只是用得频繁?
   - 如果 lessons 无任何需修正模式 → 仅 ok 记录构成的 XP → 考虑跳过升级
   - 如果 lessons 有需修正模式 → 继续, 带着 lessons 优化

2. 读 ~/.claude/memory/lessons/<skill>.md, 提取教训方向
3. 根据来源类型分支:
   a) **own** → spawn sub-agent (agent type: general):
      任务: "运行 darwin-skill 优化 [skill名] 的 SKILL.md"
      额外指令: "优先修复 lessons 中的 TOP 问题"
   b) **upstream** → spawn sub-agent (agent type: general):
      任务: "运行 darwin-skill 评估 [skill名] 的 SKILL.md, 但不要修改文件"
      额外指令: "评估 lessons 中的 TOP 问题是否在上游新版本中已解决, 输出补丁建议到 ~/.claude/memory/patches/<skill>-<date>.md"
      补丁建议格式: "建议在哪步之后插入什么内容 / 需要向上游作者提什么 PR"
4. 接收 sub-agent 返回的优化结果 (分数变化, 改动摘要, 补丁文件路径)

🔴 CHECKPOINT · 你审
   - 展示: 改了什么? 修了 lessons 中的哪条?
   - 你确认"好" → 保留改动, 在 lessons 标记已修复
   - 你确认"不好" 🛑 → darwin 自动回滚, lessons 保留到下次

5. 更新 state 文件:
   - 等级 +1
   - XP -= 5 (不归零, 按宝可梦规则)
   - 如果更新后 XP ≥ 5: 回到步骤 1 继续 (连升), 重复此流程
   - 直到 XP < 5 为止
   - 记录履历 (日期, 评分变化, 连升级数, 修复了哪些教训)
6. 通过 codepilot_notify 通知你升级完成
```

### 模式 3: 定时自动巡检 (Loop)

用 `/loop` 或 `codepilot_schedule_task` 定时触发:

```
/loop 6h /skill-rpg-loop 自动巡检
```

Discovery 阶段:
1. 读取 state 文件, 找出所有 XP ≥ 5 的 skill
2. 按 XP 从高到低排序, 优先升最接近升级的
3. 一次循环只升 1-2 个 (避免 token 爆表)
4. 若无待升级 skill → 报告当前状态, 跳过

### 模式 4: 查看状态

```
/skill-rpg-loop 查看等级
/skill-rpg-loop 查看履历
/skill-rpg-loop 哪些 skill 快升级了
```

---

## State 文件格式

文件位置: `~/.claude/memory/skill-levels.md`

### 技能等级表

| Skill | 来源 | 等级 | XP | 下次升级 | 上次优化 | 上次使用 |
|-------|------|------|----|---------|---------|---------|
| my-skill | own | 2 | 3 | 2 | 2026-06-20 | 2026-06-22 |
| third-party-skill | upstream | 1 | 7 | 3 | 2026-06-21 | 2026-06-22 |

- **下次升级** = `5 - (XP % 5)`; "就绪" 表示 XP % 5 == 0
- **来源** = `own` (自有, 可直接改 SKILL.md) / `upstream` (第三方, 不碰源码)
- **连升** = 若升级后剩余 XP ≥ 5, 连续升级直至 < 5 (一次巡检可升多级)

### 升级履历

| 日期 | Skill | 事件 | 等级变化 | darwin 评分变化 | 修复教训 |
|------|-------|------|---------|---------------|---------|
| 2026-06-21 | my-skill | 升级 | L0 → L1 | 72 → 84 (+12) | 缺少使用示例 |

### 系统状态

```
- 上次循环时间: 2026-06-22 08:00
- 循环间隔: 6 小时
- 总触发次数: 15
- 总升级次数: 3
- 日志同步位置: 第 42 行 (skill-usage-log.md 已处理到的行号)
```

### 使用日志文件

`~/.claude/memory/skill-usage-log.md` — 由全局 CLAUDE.md 指令驱动, 所有 session 的 agent 自动写入。每行包含结果列和教训列。

```
# Skill 使用日志

| 时间 | Skill | 任务摘要 | 结果 | 教训 |
|------|-------|---------|------|------|
| 2026-06-22 11:30 | my-skill | 生成图片 | ok | - |
| 2026-06-22 11:35 | my-skill | 人物头像 | modified | 未提前确认用户偏好 |
```

---

## Sub-agent 协议

### Generator (升级执行 agent)

```
你是一个 skill 升级执行 agent。
任务: 对 [skill名] 执行 darwin-skill 优化。
先检查 skill-levels.md 中该 skill 的"来源"列:

```
own → 直接优化: read → evaluate → modify SKILL.md → verify
```
```
upstream → 只评估不修改: read → evaluate → 写补丁建议到 ~/.claude/memory/patches/
          ❌ 不修改 SKILL.md 本身
          ❌ 不创建 test-prompts (上游技能, 测试由作者负责)
```

步骤:
1. 确认 darwin-skill 已安装 (在 ~/.claude/skills/darwin-skill/SKILL.md)
2. 读取该 skill 的 SKILL.md
3. 读取 ~/.claude/memory/lessons/[skill名].md, 了解已知问题和成功模式
4. 执行 darwin-skill 的完整 Phase 0.5 → Phase 1 → Phase 2 → Phase 3
5. Phase 2 优化时, 优先修复 lessons 中的高频问题
6. 返回优化结果: 新旧分数、改动摘要、评估模式 (full_test/dry_run)、解决了 lessons 中的哪些条目
约束:
- 不跳过 Phase 0.5 (test-prompts 设计)
- 不跳过独立 judge 评估
- 如果 darwin-skill 报告的优化后分数 < 优化前, 标记为 revert
- 如果 lessons 有高频问题, 必须至少修复 TOP 1
```

### Evaluator (验证 agent)

```
你是一个升级验证 agent。
任务: 验证 darwin-skill 对 [skill名] 的优化结果。
检查:
1. darwin 是否使用了独立 judge (不是自己改自己评)
2. 分数变化是否真实 (分数是否有对比 baseline)
3. 是否有 test-prompts 实测 (不是纯 dry_run)
4. 是否满足 darwin 的 8 条反例黑名单
5. lessons 中的高频问题是否被修复 (至少 TOP 1)
结论: pass / fail (附理由)
```

---

## 异常处理

| 场景 | 处理 |
|------|------|
| state 文件不存在 | 重新初始化默认状态表 |
| darwin-skill 优化失败 | XP 不重置, 标记为"待重试", 下次再试 |
| 分数下降 (优化后退步) | darwin 自动 revert, XP 不重置 |
| multiple skill 同时待升级 | 一次只升 1 个, token 预算控制 |
| 连续 3 次升级失败 | 通知你人工介入 |
| 同 skill 1 小时内重复触发 | 跳过 (防抖), XP 保留 |
| **lessons 仅 ok 记录** (无需修正模式) | **跳过升级** — XP 仅来自成功使用, non-skill 没问题就不要修 |

---

## 约束规则

1. **一次只升级一个 skill** — 避免 token blowout
2. **升级后必须给你确认机会** — 不能静默接受 darwin 的结果
3. **不修改 state 文件格式** — 读和写必须兼容当前 schema
4. **XP 只增不减** — 即使升级失败也不扣 XP
5. **记录一切** — 每次触发、升级、失败都写入履历
6. **不自动删除旧履历** — agent 会忘, 文件不会

---

## 使用方式

### 日常使用 (记 XP)
```
用户: "我刚用 my-skill 完成了任务, 记一次经验"
→ skill-rpg-loop 记录使用: my-skill
→ XP +1, 检查是否需要升级
```

### 手动触发升级
```
用户: "升级所有待升级的技能"
→ 找出所有 XP ≥ 5 的 skill, 逐个升级
```

### 查看状态
```
用户: "看看哪些技能快升级了"
→ 读取 state, 显示 XP 最接近满的 top 5
```

### 设置定时巡检
```
用户: "设置 6 小时自动巡检升级"
→ codepilot_schedule_task(
    name="skill-rpg 自动巡检",
    kind="ai_task",
    schedule_type="cron",
    schedule_value="0 */6 * * *",
    prompt="执行 skill-rpg-loop 自动巡检, 检查所有注册 skill 的 XP"
  )
```

### 启动会话内循环
```
用户: "/loop 2h /skill-rpg-loop 自动巡检"
→ 每 2 小时在当前会话中跑一次巡检
```

---

## 设计灵感

> "Loop engineering sits one floor above the harness."
> — Addy Osmani

> "Build the loop, but build it like someone who intends to stay the engineer."
> — HuaShu, Loop Engineering

本 skill 把 Loop Engineering 的五步六部件落地到实际工具中:
- 自动化调度 → `codepilot_schedule_task`
- 子 agent 隔离 → Agent 工具 + darwin-skill
- 独立验证 → darwin-skill 的独立 judge
- 持久化状态 → ~/.claude/memory/skill-levels.md
- 经验值驱动 → 游戏化反馈

> **"The agent forgets, the repo doesn't."** — 但 level 和经验值不会忘。
