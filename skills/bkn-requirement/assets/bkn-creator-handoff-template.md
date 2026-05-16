# BKN_Creator 交接模板

字段名保留英文，便于后续 `bkn-creator` 消费；字段内容和说明使用中文。PRD 正文应先给出中文业务可读摘要，再给出下面的机器可读 schema。

V0.5 要求明确区分业务已确认内容、建模候选内容和需要 `BKN_Creator` 判定的内容。不要把候选对象、关系、逻辑属性或 Action 写成业务已确认事实。

交接摘要必须先按场景输出 `scenario_handoff_matrix`，再做全局归并。每个候选项都必须能追溯到 `scenario_id`、`evidence_ref` 和 `confirmation_status`。

## 中文业务可读摘要

```markdown
### 按场景收敛摘要

| 场景 | 概念模型层 | 关系层 | 动力层 | 治理层 | Skill / Agent 应用层 | 证据 |
|---|---|---|---|---|---|---|

### 全局归并摘要

#### 业务已确认内容
#### 仍属建模候选
#### 需下游建模阶段判定的问题
```

## 机器可读 schema

```yaml
bkn_creator_handoff:
  schema_version: bkn-requirement.v0.5
  source_prd:
  handoff_route: hold | interview | extract_after_prd_refinement | extract_after_scene_split | create_after_business_confirmation | update
  requirement_maturity: R0 | R1 | R2 | R3 | R4
  quality_score:
  business_domain:
  use_case_name:
  candidate_network_name:
  scenario_handoff_matrix:
    - scenario_id:
      scenario_name:
      business_goal:
      capability_goal:
      decision_points:
        - decision_point_id:
          decision_name:
          decision_owner:
          required_evidence:
          automation_boundary:
          error_cost:
      expected_operational_effect:
      confirmed_business_rules:
      acceptance_cases:
      conceptual_model_layer:
        - name:
          scenario_id:
          type_hint:
          confirmation_status: confirmed | candidate | unresolved | rejected
          evidence_ref:
      relationship_layer:
        - name:
          scenario_id:
          source_business_term:
          target_business_term:
          business_meaning:
          confirmation_status: confirmed | candidate | unresolved | rejected
          evidence_ref:
      dynamic_layer:
        - name:
          scenario_id:
          kind: metric | calculation | decision | action_draft | action_execute | state_change
          trigger:
          human_confirmation:
          confirmation_status: confirmed | candidate | unresolved | rejected
          evidence_ref:
      governance_layer:
        - name:
          scenario_id:
          permission_subject:
          controlled_action:
          approval_or_audit:
          confirmation_status: confirmed | candidate | unresolved | rejected
          evidence_ref:
      skill_agent_layer:
        - user_task:
          scenario_id:
          agent_capability:
          expected_answer_or_action:
          acceptance_case_ref:
          confirmation_status: confirmed | candidate | unresolved | rejected
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
    data_view_candidates:
    external_writeback_systems:
  needs_bkn_creator_decision:
    internal_modeling_questions:
  critical_gaps:
  requires_business_confirmation:
  schema_validation:
    missing_required_fields:
    inconsistent_fields:
    validation_notes:
  suggested_next_step:
```

## 字段说明

| 字段 | 说明 |
|---|---|
| `schema_version` | 输出结构版本，V0.5 固定为 `bkn-requirement.v0.5`。 |
| `handoff_route` | 建议 `BKN_Creator` 下游路线。PRD 未完成时应优先 `interview`、`extract_after_prd_refinement` 或 `extract_after_scene_split`。 |
| `business_confirmed` | 已能被业务专家直接确认的场景、对象、规则、系统和验收用例，使用业务语言。 |
| `candidate_only` | AI 工程师根据 PRD 推导出的 BKN 候选项，仍需 `BKN_Creator` 判断。 |
| `needs_bkn_creator_decision` | 明确交给 `BKN_Creator` 决定的建模问题，如对象边界、关系对象、逻辑属性挂载、Action 执行模式。 |
| `scenario_handoff_matrix` | 按场景映射概念模型层、关系层、动力层、治理层、Skill / Agent 应用层；每项必须有 `evidence_ref` 和 `confirmation_status`。 |
| `critical_gaps` | 阻碍进入下一成熟度的关键缺口。 |
| `requires_business_confirmation` | 仍需业务专家确认的事项。 |
| `schema_validation` | 对 handoff 必填字段、正文一致性和缺失项的校验结果。 |
| `suggested_next_step` | 建议下一步。 |

## 进入 business_confirmed 的门禁

只有同时满足以下条件，才能进入 `business_confirmed`：

- 来自至少一个明确场景；
- 有业务证据或验收用例；
- 不是工程推断；
- 业务含义清楚；
- 未标注“范围待确认”。
