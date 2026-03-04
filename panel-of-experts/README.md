# Panel of Experts

[English](#english) | [中文](#中文)

---n

## English

### Overview
A multi-expert collaboration framework for solving complex, multi-dimensional problems through role-based analysis and synthesis.

**Version**: V2.1 (Enhanced with streamlined experts, differentiated outputs, and user checkpoints)

### Core Concept
Simulate collaboration among multiple domain experts, each analyzing the problem from their specialized perspective. The Synthesizer then integrates all analyses into a comprehensive solution.

### The 9 Expert Roles

1. **Orchestrator** - Analyzes complexity, designs workflow, provides time baseline
2. **Info Collector** - Gathers information from multiple sources (web, memory, media)
3. **Info Verifier** - Validates sources, checks facts, rates credibility
4. **Technical Architect** - Assesses technical feasibility and architecture options
5. **System Analyst** - Analyzes systemic relationships and interactions
6. **Quantitative Analyst** - Provides data-driven analysis and cost-benefit evaluation
7. **Growth Strategist** - Designs long-term plans with milestones and evolution paths
8. **Synthesizer** - Integrates all expert outputs into unified, actionable solutions
9. **Iteration Expert** - Observes process imperfections and provides optimization suggestions

### Workflow

```
User Problem → Orchestrator → [Info Collection] → Expert Analysis → Synthesis → Solution
                    ↓                ↓                  ↓              ↓
            Complexity      Verification          Multiple      Differentiated
            Assessment      (if needed)            Experts        Output
```

### Complexity Levels

| Level | Dimensions | Experts | Output Length | Use Case |
|-------|-----------|---------|---------------|----------|
| **Simple** | 1-2 | 1-2 | 1 page | Quick decisions, clear boundaries |
| **Medium** | 3-4 | 2-4 | Standard | Multi-faceted analysis, balanced depth |
| **Complex** | 5+ | Multiple rounds | Comprehensive | Deep analysis, strategic planning |

### When to Use

✅ **Best For:**
- Strategic planning and decision-making
- Technology selection and architecture design
- Complex problem analysis with multiple dimensions
- Career development and long-term planning
- Situations requiring diverse expertise perspectives

⚠️ **Consider Alternatives For:**
- Simple questions with single answers
- Time-critical decisions requiring immediate response
- Problems with well-established solutions

### Key Features

- **Differentiated Output**: Adjusts detail level based on problem complexity
- **Core Insights First**: Each expert outputs 3-5 key insights before detailed analysis
- **User Checkpoints**: Allows user confirmation and workflow adjustment at key nodes
- **Feedback Loop**: Systematic collaboration effectiveness tracking
- **Relay Prompting**: Each expert writes prompts for the next expert in chain

### File Structure

```
panel-of-experts/
├── SKILL.md                           # Main skill definition
└── references/
    ├── prompt-orchestrator.md         # Orchestrator prompts
    ├── prompt-info-collector.md       # Info Collector prompts
    ├── prompt-info-verifier.md        # Info Verifier prompts
    ├── prompt-technical-architect.md  # Technical Architect prompts
    ├── prompt-system-analyst.md       # System Analyst prompts
    ├── prompt-quantitative-analyst.md # Quantitative Analyst prompts
    ├── prompt-growth-strategist.md    # Growth Strategist prompts
    ├── prompt-synthesizer.md          # Synthesizer prompts
    ├── prompt-iteration-expert.md     # Iteration Expert prompts
    ├── expert-frameworks.md           # Analysis frameworks
    └── examples.md                    # Usage examples
```

### Version History

- **V2.1** (Current): Streamlined from 11 to 9 experts, added differentiated outputs and user checkpoints
- **V2.0**: Added Time Manager, Info Collector, and Info Verifier
- **V1.0**: Base version with 6 core experts

---

## 中文

### 概述
通过基于角色的分析和综合，解决复杂、多维度问题的多专家协作框架。

**版本**：V2.1（精简专家、差异化输出、增加用户检查点的增强版本）

### 核心概念
模拟多个领域专家的协作，每位专家从其专业角度分析问题。汇总专家随后将所有分析整合成完整的解决方案。

### 9位专家角色

1. **调度专家（Orchestrator）** - 分析复杂度、设计工作流、提供时间基准
2. **信息收集专家（Info Collector）** - 从多源渠道收集信息（网络、记忆、媒体）
3. **信息核验专家（Info Verifier）** - 验证来源、核查事实、评级可信度
4. **技术架构专家（Technical Architect）** - 评估技术可行性和架构选项
5. **系统分析专家（System Analyst）** - 分析系统性关系和相互作用
6. **量化思维专家（Quantitative Analyst）** - 提供数据驱动分析和成本效益评估
7. **成长规划专家（Growth Strategist）** - 设计带里程碑和演进路径的长期规划
8. **汇总专家（Synthesizer）** - 整合所有专家输出为统一、可执行的解决方案
9. **迭代专家（Iteration Expert）** - 观察过程中的不足并提供优化建议

### 工作流程

```
用户问题 → 调度专家 → [信息收集] → 专家分析 → 汇总整合 → 解决方案
                ↓            ↓            ↓            ↓
           复杂度评估    信息核验      多位专家     差异化输出
                       （如需要）
```

### 复杂度等级

| 等级 | 维度数 | 专家数 | 输出长度 | 适用场景 |
|------|--------|--------|----------|----------|
| **简单** | 1-2 | 1-2 | 1页纸 | 快速决策、边界清晰 |
| **中等** | 3-4 | 2-4 | 标准 | 多面分析、平衡深度 |
| **复杂** | 5+ | 多轮 | 全面 | 深度分析、战略规划 |

### 何时使用

✅ **最适合：**
- 战略规划和决策制定
- 技术选型和架构设计
- 多维度复杂问题分析
- 职业发展和长期规划
- 需要多元专业视角的情况

⚠️ **考虑替代方案：**
- 单一答案的简单问题
- 需要立即响应的时间关键决策
- 已有成熟解决方案的问题

### 关键特性

- **差异化输出**：根据问题复杂度调整详细程度
- **核心观点前置**：每位专家先输出3-5条核心洞察，再进行详细分析
- **用户检查点**：允许用户在关键节点确认和调整工作流
- **反馈闭环**：系统化的协作效果追踪
- **接力提示词**：每位专家为链中的下一位专家撰写提示词

### 文件结构

```
panel-of-experts/
├── SKILL.md                           # 主技能定义
└── references/
    ├── prompt-orchestrator.md         # 调度专家提示词
    ├── prompt-info-collector.md       # 信息收集专家提示词
    ├── prompt-info-verifier.md        # 信息核验专家提示词
    ├── prompt-technical-architect.md  # 技术架构专家提示词
    ├── prompt-system-analyst.md       # 系统分析专家提示词
    ├── prompt-quantitative-analyst.md # 量化思维专家提示词
    ├── prompt-growth-strategist.md    # 成长规划专家提示词
    ├── prompt-synthesizer.md          # 汇总专家提示词
    ├── prompt-iteration-expert.md     # 迭代专家提示词
    ├── expert-frameworks.md           # 分析框架
    └── examples.md                    # 使用示例
```

### 版本历史

- **V2.1**（当前）：从11位精简至9位专家，增加差异化输出和用户检查点
- **V2.0**：增加时间管理员、信息收集专家和信息核验专家
- **V1.0**：基础版本，含6位核心专家
