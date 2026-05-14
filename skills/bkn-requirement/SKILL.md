---
name: bkn-requirement
description: 当需要从业务访谈、会议纪要、录音转写、PRD、BRD、流程说明、系统/数据材料或粗略想法中，整理业务场景中心 PRD、追问清单、业务验收用例和 BKN_Creator 交接摘要时使用。
---

# BKN Requirement

## 目标

在正式 BKN 建模之前使用本 Skill。它负责把业务专家的自然语言需求、会议纪要、PRD 或业务材料，转化为可被业务专家评审、产品/交付团队使用、AI 工程师追问、并可交给 `bkn-creator` 继续建模的标准 PRD。

当前版本：`V0.4`。V0.4 的主线是业务场景，而不是对象、关系、逻辑属性和 Action。BKN 元素只用于后台检查和 `BKN_Creator` 交接摘要。

本 Skill 不生成 `.bkn` 文件，不绑定数据视图，不推送知识网络，也不执行平台操作。这些是 `bkn-creator` 和 `kweaver-core` 的下游职责。

## 适用场景

当用户需要完成以下任务时使用本 Skill：

- 从访谈纪要、录音转写、PRD、BRD、流程说明、系统截图、数据字典或粗略想法整理需求；
- 帮助 AI 工程师与业务专家对话，澄清业务场景、流程、规则、系统、数据、输入输出和验收样例；
- 将偏技术、偏建模或偏 POC 的材料重构为业务用户可评审的标准 PRD；
- 评估某个业务场景是否已经具备后续 BKN 建模条件；
- 生成业务验收用例、待确认问题和 `BKN_Creator` 交接摘要。

如果用户已经有确认后的建模需求，并明确要求创建、更新、校验、绑定、测试或推送 BKN，应转交 `bkn-creator`。

## 核心原则

默认使用业务语言。业务用户通常表达的是流程、规则、标准、表单、系统、数据、输入、输出、目标、审批和异常；不要要求业务用户先理解对象、关系、逻辑属性、Action 或 Skill。

主线应是：

```text
业务目标 → 业务场景 → 当前流程 → 目标流程 → 输入输出
→ 业务规则 / 判断标准 → 系统与数据来源 → 决策与人工确认
→ 异常边界 → 界面交互 → 业务验收用例 → BKN_Creator 交接摘要
```

BKN 元素承担三类作用：

1. 作为 AI 工程师检查 PRD 完整性的后台维度；
2. 作为追问业务专家时的线索；
3. 作为交给 `bkn-creator` 的摘要，不作为 PRD 主体叙事。

## 核心流程

按以下顺序执行。不要跳过缺口识别：未确认问题是必需输出，不是失败。

1. **整理输入材料**
   - 识别输入来源：粗略想法、访谈记录、录音转写、PRD、流程图、系统说明、数据字典、截图、历史案例。
   - 区分已确认事实、假设、冲突信息和缺失信息。
   - 如果输入是录音转写或会议纪要，读取 `references/meeting-transcript-processing.md`。

2. **识别业务场景**
   - 先识别业务目标、用户角色、触发条件、当前流程、目标流程、业务输入、业务输出、成功指标。
   - 将材料拆成一个个可验收场景；综合性需求也要拆出主场景和跨场景规则。
   - 使用 `references/fde-method.md` 中的 FDE 式业务访谈方法；不要把 BKN 映射内容写进 PRD 主体。

3. **还原业务规则与决策**
   - 记录规则口径、判断标准、异常边界、人机协同点、审批点、拒绝执行边界和可配置项。
   - 用业务语言表达规则，不把规则提前写成 BKN Action 或执行工厂算子。

4. **发现系统、表单与数据现状**
   - 识别业务系统、表单、数据源、事实来源、关键字段、对象 ID、刷新频率、质量风险、访问限制和外部写回系统。

5. **生成业务验收用例**
   - 每个核心场景至少包含典型用例、边界用例、权限/拒绝用例、数据不足用例和证据解释用例。
   - 验收用例应使用业务输入和业务期望，不以对象/关系/逻辑属性作为主字段。

6. **评估需求质量**
   - 标注需求成熟度：`R0 Idea`、`R1 Use Case Brief`、`R2 PRD Candidate`、`R3 PRD Ready`、`R4 Implementation Ready`。
   - 输出 `quality_assessment`，重点评估业务场景完整度、用户工作流、业务规则、系统数据、权限治理、验收用例和 BKN_Creator 交接质量。
   - 使用 `references/quality-scoring.md`。

7. **生成 BKN_Creator 交接摘要**
   - 仅在 PRD 末尾输出 `bkn_creator_handoff`。
   - 必须分为 `business_confirmed`、`candidate_only`、`needs_bkn_creator_decision`。
   - 不能把候选对象、关系、逻辑属性或 Action 伪装成业务已确认事实。

## 输出模式

用户未指定时，默认使用 `prd_mode`，输出业务场景中心标准 PRD。

- `prd_mode`：输出标准 PRD，适合业务、产品、客户、交付和项目评审。默认推荐。
- `review_mode`：评估已有 PRD 或访谈纪要，输出评分、问题和改写建议。
- `interview_mode`：输出访谈模板、追问清单和会议纪要处理建议。
- `handoff_mode`：仅输出交给 `bkn-creator` 的交接摘要，适合 PRD 已确认后的下游交接。

如用户明确要求“只做建模输入”，提醒这属于 `bkn-creator` 范围，并可先输出 handoff。

## 标准 PRD 结构

完整 PRD 默认使用以下结构。需要完整模板时读取 `assets/requirements-template.md`。

```markdown
# 《<场景名> PRD》

## 1. 文档信息
## 2. 业务背景与目标
## 3. 业务用户与职责
## 4. 场景总览
## 5. 场景需求详述
## 6. 跨场景业务规则
## 7. 业务系统、表单与数据来源
## 8. 权限、审批与合规要求
## 9. 界面 / 交互期望
## 10. 非功能需求
## 11. 业务验收用例
## 12. 待确认问题与访谈追问清单
## 13. BKN_Creator 交接摘要
```

## 质量门禁

最终输出前检查：

- 是否能让业务专家不懂 BKN 也能评审；
- 是否有明确业务目标、用户角色、触发条件、输入、输出和成功指标；
- 是否按场景组织，而不是按对象、关系、逻辑属性、Action 组织；
- 每个场景是否有当前流程、目标流程、业务规则、异常边界、人工确认点和界面期望；
- 是否记录业务系统、表单、数据源、事实来源、字段口径、刷新频率、访问限制和数据质量风险；
- 业务验收用例是否覆盖典型问题、分析请求、决策建议、权限拒绝、数据不足和证据解释；
- 是否包含 AI 工程师追问清单，并说明“问谁、为什么问、不确认的风险、建议问法”；
- `BKN_Creator` 交接摘要是否仅放在末尾，并区分 `business_confirmed`、`candidate_only`、`needs_bkn_creator_decision`；
- 没有把 `operator` 当作业务逻辑直接写入 PRD 主体；如需表达，应写为业务规则或对象逻辑属性候选，交由 `BKN_Creator` 判定。

## 参考资料

- FDE 业务访谈方法：`references/fde-method.md`，需要 FDE 式追问方法时读取。
- BKN 需求发现方法：`references/bkn-requirement-method.md`，需要把业务材料整理成 PRD 时读取。
- 质量评分规则：`references/quality-scoring.md`，需要评分、成熟度或推荐路线时读取。
- 访谈问题库：`references/interview-question-bank.md`，需要生成访谈提纲或追问清单时读取。
- 会议纪要处理：`references/meeting-transcript-processing.md`，输入是会议纪要、录音转写或访谈摘要时读取。
- 输出 Schema：`references/output-schema.md`，用户要求机器可读 schema 或完整结构化输出时读取。
- FDE 到 BKN 后置映射：`references/fde-bkn-mapping.md`，仅在 `handoff_mode` 或 PRD 已形成后生成 `BKN_Creator` 交接摘要时读取。

## 模板与案例

- 访谈模板：`assets/interview-template.md`，准备业务访谈时使用。
- 需求文档模板：`assets/requirements-template.md`，输出完整 PRD 时使用。
- 业务验收用例模板：`assets/scenario-test-case-template.md`，补充验收用例时使用。
- BKN_Creator 交接模板：`assets/bkn-creator-handoff-template.md`，输出 handoff 时使用。
- 下游 Agent / Skill Card 模板：`assets/downstream-agent-card-template.md`，仅在 PRD 完成后且用户要求设计下游 Agent / Skill 时使用。
- 案例：按领域读取 `references/examples/operations-alert-triage.md`、`references/examples/supply-chain-risk.md`、`references/examples/customer-service.md`，不要批量加载全部示例。
