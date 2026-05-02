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

## 5. PRD 必含章节（骨架 contract）

PRD 必须包含以下章节（详细要求见各 agent 的输出结构定义）：

### S1 — 价值与目标
- Product / Opportunity Brief
- Value Hypothesis
- KPI Tree（North Star + Leading Indicators + Guardrails）

### S2 — 范围与流程
- Epic / Feature / Story 拆解
- Business Process Flow（至少 1 happy path + 2 exception paths，建议泳道）
- GWT Scenarios（top 3-5，覆盖 happy / failure / edge cases）
- Roadmap with Phases（MVP / Phase 2 / Future Extension）

### S3 — 用户与系统
- User Journey Map（persona / stages / emotions / pain points）
- System Interaction Flow（user → channel → gateway → services → data）
- Service Boundary Table（Owns / Does NOT Own / 外部依赖）
- Key Technical Decisions（产品视角：sync/async / queue / external providers）

### S4 — 非功能与扩展
- Non-functional Requirements（performance / compatibility / availability / retry / security / observability / data limits）
- Tracking & Metrics（如适用）
- Future Extension Ideas（如适用）

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
- ❌ 禁止只写 happy path，不写异常路径
- ❌ 禁止只写页面功能，不写业务流程
- ❌ 禁止忽略系统边界
- ❌ 禁止把实现细节写成产品逻辑，或把产品逻辑丢给研发自行推断
- ❌ 禁止 AC 中混入 UI 视觉描述（颜色、布局、字号），UI 视觉由 UX Prototyper 决定

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
