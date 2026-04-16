---
applyTo: "{**/*.md,docs/**,requirements/**,prd/**}"
---

# Product Instructions

- 输出 PRD 时必须遵守 Epic → Feature → Story 结构
- User Story 用英文
- Acceptance Criteria 用中文 Given / When / Then
- 优先输出 Jira-ready 格式

---

# BCChina Product Instructions

## 目标
本文件用于约束产品输出，包括 PRD、Opportunity Brief、Feature Definition、User Story、Acceptance Criteria、Roadmap、User Journey、业务流程、系统交互说明等，确保 BCChina 产品文档结构统一、可评审、可交付、可被研发直接使用。

所有产品文档、需求结构化输出、PRD、Epic / Feature / Story 拆解、用户流程说明，必须遵守本规范。

---

## 总体产品原则

- 先定义问题，再定义方案
- 先定义用户价值，再定义功能
- 先定义流程，再定义页面
- 先定义边界，再定义需求细节
- 先保证需求可理解、可验证、可交付，再考虑扩展性描述
- 产品输出应服务于 PM、设计、研发、测试和交付共同协作
- 所有产品文档应尽量结构化，避免大段抽象描述

---

## 适用范围

本文件适用于：

- Product / opportunity brief
- PRD
- Epic / Feature / Story 拆解
- User Story
- Acceptance Criteria
- KPI tree
- Business Process Flow
- GWT Scenarios
- Roadmap
- User Journey Map
- System Interaction Flow
- Service Boundary Table（产品视角）
- Non-functional Requirements（产品视角）
- Copilot 生成的产品文档

---

## PRD 结构规范（强制）

所有 PRD 必须遵循以下分层结构：

### S1
- Product / opportunity brief
- Value hypothesis
- KPI tree（north star + leading indicators + guardrails）

### S2
- Business process flow（swimlane，至少 1 条 happy path + 2 条 exception paths）
- GWT scenarios（top 3–5）
- Roadmap with phases

### S3
- User journey map（persona, stages, emotions, pain points）
- System interaction flow（user → channel → gateway → services → data）
- Service boundary table（Owns / Does NOT Own）
- Key technical decisions（产品视角记录：sync/async, queue strategy, external providers）

### S4
- Non-functional requirements

---

## Product / Opportunity Brief 规范

必须说明：

- 业务背景
- 问题是什么
- 当前为什么要做
- 目标用户是谁
- 对业务和用户的价值是什么

禁止只写“要做什么”，不解释“为什么做”。

---

## Value Hypothesis 规范

每个重要产品需求都应说明：

- 假设是什么
- 为什么认为它成立
- 如果成立，会带来什么价值
- 如何验证

建议格式：

- Hypothesis
- Expected user value
- Expected business value
- Validation signal

---

## KPI Tree 规范

必须区分：

- North Star
- Leading Indicators
- Guardrails

### 要求
- 不要只写单个 KPI
- KPI 必须能映射到当前需求价值
- Guardrails 必须说明副作用控制

---

## Epic / Feature / Story 层级规范

### Epic
- 业务目标 / 核心产品模块 / 主题
- 跨多个功能块或多个迭代

### Feature
- Epic 下的功能模块
- 对用户或业务有相对独立的价值表达

### Story
- 可拆分、可开发、可测试的需求单元

### 规则
- 不允许混淆 Epic / Feature / Story
- 每条 Story 必须明确归属 Feature 和 Epic
- 每个 Feature 下应有清晰 Story 列表

---

## User Story 规范（强制）

User Story 必须用英文，并采用标准格式：
As a [user], I want to [action], so that [benefit]

- 要求
  - 面向用户价值
  - 不混入实现方案
  - 一条 Story 表达一个清晰意图
  - Story 尽量可独立理解和执行

### Acceptance Criteria 规范（强制）

Acceptance Criteria 必须用中文，并采用：
Given ...
When ...
Then ...
- 要求
  - 每个场景分开写
  - 至少覆盖：
    - 1 个主流程
    - 1 个失败流程
    - 必要边界情况
  - 必须可测试、可验证
  - 禁止模糊表达，如“系统应该更好”“页面应流畅”

## Business Process Flow 规范

- 必须以业务视角输出流程，建议使用泳道思维表达。

- 至少包含
  - 1 条 happy path
  - 2 条 exception paths
- 必须说明
  - 用户动作
  - 系统反馈
  - 关键判断点
  - 异常分支
- 禁止只写主流程，忽略异常情况
- 禁止只写抽象流程，不说明用户动作和系统反馈
- 禁止只写文字描述，建议配合流程图表达
## GWT Scenarios 规范

- 必须输出 top 3–5 个场景，覆盖主要用户行为和关键边界情况
- 每个场景必须清晰表达 Given / When / Then
- 场景必须可测试、可验证
- 禁止只写主流程场景，必须包含失败场景和边界场景
- 禁止模糊表达，如“系统应该更好”“页面应流畅
- 至少覆盖：
  - 1 个 happy path
  - 1 个 failure path
  - edge cases（按需）
- 每个场景尽量简洁，便于产品、设计、研发、测试共同评审

## Roadmap with Phases 规范

- Roadmap 需说明：
  - phase 1 做什么
  - phase 2 做什么
  - 哪些能力可以延后
  - 哪些是 MVP
  - 哪些是 future extension
- 禁止把所有内容一次性塞进首期范围。

## User Journey Map 规范

- 必须从用户视角说明：
  - persona
  - stages
  - emotions
  - pain points
  - key needs / moments
- 原则
  - 不只是列页面
  - 必须体现用户感受与阻碍点
  - 可为 UX 与 AC 提供依据

## System Interaction Flow 规范

- 产品输出必须具备系统交互理解，而不只是页面需求。至少说明：
  - 用户
  - 渠道（如 Mini program / IELTS website / 3Ups）
  - gateway
  - service
  - data
- 原则
  - 用于帮助产品、设计、研发理解系统链路
  - 不要求实现细节，但必须清楚上下游系统关系
## Service Boundary Table（产品视角）
- 产品文档中也必须说明系统边界，至少回答：
  - 这个功能由哪个系统负责
  - 哪个系统不负责
  - 外部依赖有哪些
  - 是否需要集成 IOC admin / ICS / Mini program / 3Ups / Touch points 等
- 禁止默认“一切都由当前系统负责”
## Non-functional Requirements（产品视角）
- PRD 中必须包含 NFR，至少考虑：
  - performance
  - compatibility
  - availability
  - retry / timeout（如用户流程受影响）
  - security / permission（如适用）
  - observability（如业务关键链路）
  - data limits（如文件大小、数量、频次）

## 产品输出建议结构

- 如无特殊说明，产品输出建议包含：
- Feature Summary
- Product / Opportunity Brief
- Value Hypothesis
- KPI Tree
- Epic
- Feature List
- User Stories
- Acceptance Criteria
- Business Process Flow
- GWT Scenarios
- Roadmap with Phases
- User Journey Map
- System Interaction Flow
- Service Boundary Table
- Key Technical Decisions
- UI & UX Requirements
- Non-functional Requirements
- Tracking & Metrics（如适用）
- Future Extension Ideas（如适用）

## Copilot 输出要求

- 当生成产品文档时，必须：
- 先定义问题和价值
- 再定义 Epic / Feature / Story
- 再定义流程与场景
- 再定义系统交互与边界
- 最后补充 UI / UX / NFR / metrics / extension
- 保持结构化
- User Story 用英文
- AC 用中文
- 优先参考历史 Wiki 中相似产品、系统、流程案例

## 禁止事项
- 禁止混淆 Epic / Feature / Story
- 禁止只输出高层概念，不落地
- 禁止跳过 AC
- 禁止只写 happy path，不写异常路径
- 禁止只写页面功能，不写业务流程
- 禁止忽略系统边界
- 禁止不结合公司系统语境随意命名
- 禁止把实现细节写成产品逻辑，或把产品逻辑丢给研发自行推断
## 默认输出风格
- 结构化
- 清晰
- 便于评审
- 便于 handover
- 面向产品、设计、研发共同协作