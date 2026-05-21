---
applyTo: "{**/*.md,docs/**,requirements/**,prd/**}"
---

# Product Instructions（PRD 文件级 Contract）

> 本文件定义 BCChina 产品文档的**文件级最小约束**。Copilot 在编辑任何 PRD / 产品文档时自动注入本文件。
>
> **本文件只定义"什么是合格的 PRD 文档"，不定义"agent 如何工作"。**
> - Agent 工作流见各 `*.agent.md`
> - AC 详细写法见 `skills/ac-writing-spec/SKILL.md`

---

## 1. 适用范围

本文件适用于以下产品文档：

- Product / Opportunity Brief
- PRD
- Epic / Feature / Story 拆解
- User Story
- Acceptance Criteria
- KPI Tree
- Business Process Flow / GWT Scenarios
- Roadmap / User Journey Map
- System Interaction Flow / Service Boundary Table
- Non-functional Requirements

---

## 2. 输出语言规则（强制）

- **User Story** → 英文（标准格式：`As a [user], I want to [action], so that [benefit]`）
- **Acceptance Criteria** → 中文（GIVEN / WHEN / THEN）
- **其他内容** → 中文

---

## 3. Epic / Feature / Story 层级（强制）

| 层级 | 定义 |
|------|------|
| **Epic** | 业务目标 / 核心产品模块 / 主题，跨多个功能块或多个迭代 |
| **Feature** | Epic 下的功能模块，对用户或业务有相对独立的价值表达 |
| **Story** | 可拆分、可开发、可测试的需求单元 |

**强制规则：**
- 不允许混淆 Epic / Feature / Story
- 每条 Story 必须明确归属 Feature 和 Epic
- 每个 Feature 下应有清晰 Story 列表

---

## 4. Acceptance Criteria 规范

AC 必须用中文，采用 GIVEN / WHEN / THEN 多行格式，至少覆盖：
- 1 个主流程（Happy Path）
- 1 个失败流程
- 必要边界情况

**详细规则**（格式 / 覆盖类别 / 状态机 / 按钮置灰 / 表单校验 / 写法模板 / 自主补全分级）见：

> 📘 **`skills/ac-writing-spec/SKILL.md`**
>
> 写 AC 或评审 AC 之前必须先 Read 该 SKILL，禁止凭记忆写 AC。

---

## 5. PRD 源文件必含章节（骨架 contract）

PRD 源文件由 Product Planner 产出，必须保持 **Epic → Feature → Story → AC** 的可执行需求骨架。Value / Solution 的战略、Journey、Process、GWT Top、Roadmap 不在 PRD 源文件中重复展开，由 Wiki Publisher 在发布态合并。

### S1 — Epic Definition
- Epic ID / Epic Name / Source
- KPI 对齐（来自 Value，如适用）
- Context / Scope In / Scope Out

### S2 — Feature List
- Feature ID / Feature Name / Description / Value / Source
- Feature ID 必须跨阶段稳定，不得重排或复用退役编号

### S3 — User Stories and AC
- 按 Feature 分组
- 每个 Story 必须含 Stable Story ID、User Story、upstream_refs、中文 AC、变更记录
- AC 必须遵守 `skills/ac-writing-spec/SKILL.md`

### S4 — Estimation / Engineering Notes / NFR Reference
- Story-level Estimation
- Engineering Notes
- **NFR Reference**（v4.6 改为引用模式 · 不再原创 NFR 详细字段）
  - 必含：NFR LATEST 路径 + 状态 + 关键摘要 / 未产出提示
  - 禁止：在 PRD §6 重写 NFR 详细字段（性能 / 可用性 / 容量等具体值都从 NFR LATEST 引用）
  - 来源：`Project/{project}/NFR/{epic-slug}/LATEST.md`（Epic 级优先）→ 回退 `Project/{project}/NFR/project-wide/LATEST.md`
- Capacity Summary（如有 Solution Brief，必须与 Solution §6 Phase-level Workload 做偏差对比）

### S5 — Open Questions / Future / Coverage Matrix / Changelog
- Open Questions 三层聚合（V- / S- / P-）
- Future Extension（仅记录 PRD 拆解中新增的边界外扩展点）
- **Coverage Matrix**（v4.6 新增 · 强制）
  - **必填两块**：§X.1 Solution Flow Risk 追溯 + §X.2 8 类场景维度覆盖率
  - **追溯**：Solution §5 每条 BP-X Path ID（type=solution_risk）必须 100% 追溯到本 PRD 的 Story + AC
  - **8 类维度自检**：happy / unhappy / failure / edge / permission / state / retry / empty-expired-duplicate（按 ac-writing-spec v1.1 §3.5）
  - **覆盖率目标**：每个 Feature ≥ 6 / 8（推荐 ≥ 7）
  - **校验**：Eng Reviewer v4.0 §X Coverage Verification 警示性校验（不阻塞 PRD 发布，但要求 PM accept risk）
- 已沉淀规则索引
- PRD-level Changelog

> ⚠️ Value Hypothesis、KPI Tree、Roadmap、User Journey、Business Process Flow、流程难点（BP-X Path ID）来自 Value / Solution 源文件；PRD 发布到 Wiki 时由 Wiki Publisher 合并。  
> ⚠️ NFR Targets 详细字段由 NFR Architect 产出（v4.6 拆出独立 agent），PRD §6 仅引用。  
> ⚠️ 三层 IT 架构（C2 Container / ADR / Sequence / ERD / API / Deployment）由 IT Architect 产出（v4.6 拆出独立 agent），PRD 不重写。  
> ⚠️ Architecture Challenge / Service Boundary / Blast Radius / Coverage Verification 由 Eng Reviewer 在工程评审阶段产出。

---

## 6. 总体产品原则

- 先定义问题，再定义方案
- 先定义用户价值，再定义功能
- 先定义流程，再定义页面
- 先定义边界，再定义需求细节
- 先保证需求可理解、可验证、可交付，再考虑扩展性描述

---

## 7. 禁止事项（红线）

- ❌ 禁止混淆 Epic / Feature / Story
- ❌ 禁止只输出高层概念，不落地
- ❌ 禁止跳过 AC
- ❌ 禁止在 PRD 源文件中重复展开上游 Value / Solution 章节
- ❌ 禁止 Story 只写 happy path，不写异常 / 权限 / 空状态 / 错误处理等必要 AC
- ❌ 禁止只写页面功能，不写 Story 级业务规则与验收标准
- ❌ 禁止忽略上游 Solution 中的系统边界和工程约束引用
- ❌ 禁止把实现细节写成产品逻辑，或把产品逻辑丢给研发自行推断
- ❌ 禁止 AC 中混入 UI 视觉描述（颜色、布局、字号），UI 视觉由 UX Prototyper 决定
- ❌ **v4.6 禁止**：在 PRD §6 重写 NFR 详细字段（NFR Architect 职责，仅引用 NFR LATEST）
- ❌ **v4.6 禁止**：在 PRD 中原创架构图 / Container / ADR / ERD / API 等技术设计（IT Architect 职责）
- ❌ **v4.6 禁止**：跳过 §X Coverage Matrix 或仅写 solution_risk 不补 8 类维度自检

---

## 8. 不在本文件范围

以下内容**不在本 instructions 范围**，请到对应文件查看：

| 内容 | 位置 |
|------|------|
| AC 详细格式 / 覆盖规范 / 写法模板 / 自主补全分级 | `skills/ac-writing-spec/SKILL.md` |
| Product Planner 工作流（Mode A/B/C 输入、Step 0-11、Quality Gate、Rule Sedimentation） | `.github/agents/product-planner.agent.md` |
| Story 拆分规则 / FCS 评分 | `.github/agents/story-splitter.agent.md` |
| 工程评审规则（Scope Challenge、Blast Radius、API/ERD） | `.github/agents/eng-reviewer.agent.md` |
| 估算映射规则（Story Size / Points / Units） | `.github/agents/product-planner.agent.md` §估算章节 |
| 项目永久规则库 | `.github/Rules/{project}-rules.md` |
