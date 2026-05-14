# FDE 到 KWeaver BKN 的后置映射

仅在以下情况读取本文件：

- PRD 已基本形成，需要生成或检查 `BKN_Creator` 交接摘要；
- 用户明确要求解释 FDE-style 需求拆解与 KWeaver BKN 的对应关系；
- `handoff_mode` 需要把业务场景转成候选建模线索。

不要在业务 PRD 主体中直接输出本文件的建模字段。

## 需求组件映射

| 需求组件 | 含义 | KWeaver BKN 交接线索 |
|---|---|---|
| Object model | 核心对象、派生对象、用例对象和 Link | `candidate_objects`、`candidate_relations` |
| Lifecycle diagram | 用户通过 Actions 修改对象状态的状态机 | `candidate_actions`、状态流转、审批 |
| Enrichments | 支撑决策输入的数据增强 | `candidate_logic_properties`，可能是指标型或算子型对象逻辑属性 |
| Interface expectations | 不同界面的意图和交互期望 | DIP / Agent 工作台 / Skill 交互需求 |
| Governance | 权限、安全、审批和审计 | `candidate_risk_types`、权限和审批策略 |

## 从业务 PRD 提取 handoff 线索

| PRD 内容 | handoff 候选 |
|---|---|
| 业务名词、工作项、事件、任务、提案、证据 | 候选对象 |
| 属于、包含、依赖、影响、供应、负责、拥有 | 候选关系 |
| 评分、排序、优先级、可用量、影响范围、推荐、模拟 | 候选逻辑属性 |
| 创建、提交、审批、关闭、发布、提醒、写回 | 候选 Action |
| 必须审批、不能自动、只读、脱敏、回滚 | 候选治理要求 |
| 问答、分析、建议、解释、生成草案 | 候选 Skill / Agent 能力 |

## 交接分层

`bkn_creator_handoff` 必须分三层：

| 层级 | 内容 |
|---|---|
| `business_confirmed` | 业务专家可直接确认的场景、业务对象、规则、系统和验收用例。 |
| `candidate_only` | AI 工程师根据 PRD 推导出的候选对象、关系、逻辑属性、Action、Skill、数据视图。 |
| `needs_bkn_creator_decision` | 需要下游决定的建模问题，如对象边界、关系对象、逻辑属性挂载、Action 执行模式。 |

## 注意事项

- `operator` 不是业务逻辑本身，而是对象逻辑属性的一种实现候选。
- PRD 主体应说“优先级如何判断”“可用量如何计算”“影响范围如何解释”，不要提前说“调用某个 operator”。
- 高风险业务动作优先建成草案、建议或审批流程，不应默认自动执行。
