# 《<场景名> PRD》

> 使用 `$bkn-requirement` 按 V0.4 业务场景中心标准整理。  
> 输入来源：`<输入材料>`。  
> 文档定位：面向业务专家、产品 / 交付团队和 AI 工程师评审的标准 PRD。  
> 说明：本文不直接生成 BKN 模型；BKN 对象、关系、逻辑属性和 Action 仅在末尾作为 `BKN_Creator` 交接摘要。

---

## 1. 文档信息

| 字段 | 内容 |
|---|---|
| 文档名称 |  |
| 文档版本 | V0.4 标准 PRD 草稿 |
| 输入来源 |  |
| 业务领域 |  |
| 主要读者 |  |
| 文档状态 | 草稿 / 已评审 / 已确认 |
| 需求成熟度 | R0 / R1 / R2 / R3 / R4 |

## 2. 业务背景与目标

### 2.1 业务背景

### 2.2 当前痛点

### 2.3 业务目标

| 目标 | 说明 |
|---|---|

### 2.4 成功指标

| 指标 | 业务验收口径 |
|---|---|

## 3. 业务用户与职责

| 用户角色 | 主要职责 | 高频问题 / 任务 | 权限边界 |
|---|---|---|---|

## 4. 场景总览

| 场景 ID | 场景名称 | 主要用户 | 业务目标 | 优先级 | 当前成熟度 |
|---|---|---|---|---|---|

## 5. 场景需求详述

### 5.1 S1 <场景名称>

| 项目 | 内容 |
|---|---|
| 触发方式 |  |
| 当前流程 |  |
| 目标流程 |  |
| 输入 |  |
| 输出 |  |
| 业务规则 |  |
| 异常边界 |  |
| 人工确认 |  |
| 交互期望 |  |
| 验收标准 |  |

## 6. 跨场景业务规则

| 规则 ID | 规则名称 | 业务描述 | 影响场景 | 待确认 |
|---|---|---|---|---|

## 7. 业务系统、表单与数据来源

| 系统 / 表单 / 数据 | 业务用途 | 事实来源 | 关键字段 / 信息 | 主要风险 |
|---|---|---|---|---|

## 8. 权限、审批与合规要求

| 能力 / 操作 | 允许角色 | 控制要求 |
|---|---|---|

## 9. 界面 / 交互期望

| 页面 / 入口 | 关键交互 | 输出 |
|---|---|---|

## 10. 非功能需求

| 类别 | 要求 | 验收方式 |
|---|---|---|
| 性能 |  |  |
| 可解释性 |  |  |
| 可追踪性 |  |  |
| 可配置性 |  |  |
| 安全合规 |  |  |

## 11. 业务验收用例

| ID | 场景 | 用户输入 | 期望业务输出 | 通过标准 |
|---|---|---|---|---|

## 12. 待确认问题与访谈追问清单

| 问题 | 问谁 | 为什么要问 | 不确认的风险 | 建议问法 | 优先级 |
|---|---|---|---|---|---|

## 13. 质量评估摘要

> 默认输出摘要即可；当用户要求完整结构化评分时，按 `references/quality-scoring.md` 输出完整 `quality_assessment`。

```yaml
quality_assessment:
  schema_version: bkn-requirement.v0.4
  requirement_maturity: R0 | R1 | R2 | R3 | R4
  total_score:
  readiness_decision:
    quality_route: hold | interview | prd_refinement | business_review | bkn_creator_handoff
    mapped_handoff_route: hold | interview | extract_after_prd_refinement | extract_after_scene_split | create_after_business_confirmation | update
    decision_reason:
  critical_gaps:
```

## 14. 结构化需求索引（可选）

> 仅当用户要求机器可读 schema、系统集成或自动检查时输出完整 `requirement_schema`。字段结构见 `references/output-schema.md`。

```yaml
requirement_schema:
  schema_version: bkn-requirement.v0.4
  document:
    title:
    version:
    status:
  business_scenarios:
  systems_forms_and_data:
  business_acceptance_cases:
  unresolved_questions:
```

## 15. BKN_Creator 交接摘要

```yaml
bkn_creator_handoff:
  schema_version: bkn-requirement.v0.4
  source_prd:
  handoff_route: hold | interview | extract_after_prd_refinement | extract_after_scene_split | create_after_business_confirmation | update
  requirement_maturity: R0 | R1 | R2 | R3 | R4
  business_confirmed:
    business_scenarios:
    business_objects:
    business_rules:
    business_systems:
    business_acceptance_cases:
  candidate_only:
    candidate_objects:
    candidate_relations:
    candidate_logic_properties:
    candidate_actions:
    candidate_risk_types:
    candidate_skills:
  needs_bkn_creator_decision:
  critical_gaps:
  suggested_next_step:
```
