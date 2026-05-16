# 案例：供应链风险响应 PRD 片段

## 业务需求

当供应商出现停产、处罚、舆情或交付异常时，AI 应帮助供应链团队识别受影响产品、客户和订单，推荐处置方案，并在适当情况下创建审查任务或发送预警。

## 场景总览

| 场景 | 用户 | 目标 | 优先级 |
|---|---|---|---|
| 识别供应商风险事件 | 供应链运营 | 判断事件类型和影响级别 | P0 |
| 分析影响路径 | 供应链运营、客户经理 | 找到受影响产品、客户和订单 | P0 |
| 推荐处置方案 | 供应链负责人 | 排序处置优先级和替代方案 | P0 |
| 创建审查任务 | 供应链运营 | 生成风险审查任务草案 | P1 |

## 场景需求详述

| 项目 | 内容 |
|---|---|
| 触发方式 | 外部风险事件、供应商交付异常或人工录入事件。 |
| 当前流程 | 人工查询供应关系、订单和客户影响，靠经验判断优先级。 |
| 目标流程 | 系统展示事件类型、影响路径、受影响订单、客户和处置建议。 |
| 输入 | 供应商、事件描述、发生时间、证据来源、产品线。 |
| 输出 | 风险分类、影响路径、客户和订单清单、处置建议、任务草案。 |
| 业务规则 | 高风险供应商状态变更必须审批；外部通知必须审计；客户影响判断必须包含证据路径。 |
| 异常边界 | 缺少供应关系或产能数据时，不得声称存在替代供应商。 |
| 交互期望 | 支持影响路径钻取、客户筛选、任务草案、预警通知和证据查看。 |

## 业务验收用例

| ID | 场景 | 用户输入 | 期望业务输出 | 通过标准 |
|---|---|---|---|---|
| AC-01 | 分析影响路径 | “A 供应商停产会影响哪些产品和客户？” | 返回产品、客户、订单和关系路径 | 路径证据完整 |
| AC-02 | 推荐处置方案 | “今天应优先处理哪些供应商风险？” | 按影响、紧急度和证据排序 | 排序规则可解释 |
| AC-03 | 创建审查任务 | “为 A 供应商创建风险审查任务。” | 生成任务草案和审批状态 | 不直接改风险等级 |
| AC-04 | 数据不足 | “B 公司是不是 A 的替代供应商？” | 说明缺少关系或产能数据并要求确认 | 不幻觉补全 |

## BKN_Creator 交接线索

```yaml
bkn_creator_handoff:
  schema_version: bkn-requirement.v0.5
  scenario_handoff_matrix:
    - scenario_id: S2
      scenario_name: 分析影响路径
      business_goal: 找到受影响产品、客户和订单。
      confirmed_business_rules:
        - 客户影响判断必须包含证据路径。
      acceptance_cases:
        - AC-01
      conceptual_model_layer:
        - name: 供应商
          scenario_id: S2
          type_hint: business_object
          confirmation_status: confirmed
          evidence_ref: S2/AC-01
        - name: 风险事件
          scenario_id: S2
          type_hint: event
          confirmation_status: confirmed
          evidence_ref: S2/AC-01
      relationship_layer:
        - name: 风险事件影响供应商
          scenario_id: S2
          source_business_term: 风险事件
          target_business_term: 供应商
          business_meaning: 风险事件可能影响供应商履约。
          confirmation_status: candidate
          evidence_ref: S2/AC-01
      dynamic_layer:
        - name: 影响路径分析
          scenario_id: S2
          kind: decision
          trigger: 用户询问某供应商事件影响范围。
          human_confirmation: 高风险处置需人工确认。
          confirmation_status: candidate
          evidence_ref: S2/AC-01
      governance_layer:
        - name: 高风险状态变更审批
          scenario_id: S2
          permission_subject: 供应链负责人
          controlled_action: 更新供应商风险等级
          approval_or_audit: 必须审批并留痕
          confirmation_status: confirmed
          evidence_ref: 业务规则/AC-03
      skill_agent_layer:
        - user_task: 查询供应商风险影响
          scenario_id: S2
          agent_capability: 影响路径解释
          expected_answer_or_action: 返回产品、客户、订单和证据路径。
          acceptance_case_ref: AC-01
          confirmation_status: candidate
  business_confirmed:
    business_scenarios:
      - 识别供应商风险事件
      - 分析影响路径
      - 推荐处置方案
      - 创建审查任务
    business_objects:
      - 供应商
      - 风险事件
      - 产品
      - 客户
      - 订单
      - 证据文档
    business_rules:
      - 高风险供应商状态变更必须审批。
      - 客户影响判断必须包含证据路径。
  candidate_only:
    candidate_objects:
      - Supplier
      - RiskEvent
      - SupplyRelation
      - Product
      - Customer
      - SalesOrder
      - MitigationTask
      - EvidenceDocument
    candidate_relations:
      - Supplier supplies Product
      - Product used_in SalesOrder
      - SalesOrder belongs_to Customer
      - RiskEvent affects Supplier
    candidate_logic_properties:
      - RiskEvent.risk_event_classification
      - Supplier.supply_chain_impact_path
    candidate_actions:
      - CreateMitigationTask
      - SendSupplierAlert
  needs_bkn_creator_decision:
    - RiskEvent 与 EvidenceDocument 是否需要独立证据对象。
    - 供应商风险等级更新是否只能生成审批草案。
    - 替代供应商推荐是否作为逻辑属性候选或独立 Skill。
  critical_gaps:
    - 供应关系和客户订单事实来源未确认。
    - 风险分级规则和审批人未确认。
  requires_business_confirmation:
    - 高风险供应商定义。
    - 外部通知是否允许真实发送。
```
