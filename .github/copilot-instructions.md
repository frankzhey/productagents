# BCChina Copilot Instructions (Enterprise Version)

## 目的
本文件定义 BCChina 仓库级的全局 AI 协作规则。  
所有 Agent、Copilot 输出、文档生成、设计说明、研发评审、代码建议，默认必须先遵守本文件，再叠加相关的局部 instructions 文件。

---

## 指令优先级（强制）

默认优先级如下：

1. 用户当前明确要求
2. `.github/copilot-instructions.md`（全局规则）
3. `.github/instructions/*.instructions.md`（局部规则）
4. `.github/agents/*.agent.md`（角色规则）
5. skills 内部说明

如果规则冲突，按以上顺序处理。

---

## 适用范围

本文件适用于：

- Product Planner
- UX Prototyper
- Eng Reviewer
- Wiki Publisher
- Knowledge Retriever
- 其他自定义 Agent
- Copilot 生成的 PRD、UX 文档、Engineering Review、HTML Prototype、React 代码、技术方案

---

## 工作方式（强制）

在执行任何任务前，必须先判断：

1. 当前任务属于哪一类：
   - 产品规划
   - UX / UI 设计
   - 工程评审
   - 代码实现
   - Task 拆分
   - Wiki 发布
   - 历史知识检索

2. 当前任务是否命中局部 instructions：
   - 前端/UI相关 → 读取 `frontend.instructions.md`
   - 产品文档相关 → 读取 `product.instructions.md`
   - 工程设计相关 → 读取 `engineering.instructions.md`

3. 当前任务是否涉及 Azure DevOps：
  - 如请求涉及 Azure DevOps Wiki、Work Item、Boards、Repos、Pipelines、Test Plans，优先检查 Azure DevOps MCP server 是否已有可用工具
  - 如 MCP 工具可满足需求，优先通过 MCP 完成查询或写入，而不是仅输出静态建议

<!--3. 当前任务是否需要参考历史知识：
   - 在生成 PRD、UX 文档、Engineering Review 前，优先搜索 Azure DevOps Wiki 中最相关的历史页面
   - 优先参考最近、结构完整、与当前系统或业务最相似的 3–5 个页面
   - 历史内容仅作为参考，当前需求优先-->

---
## 1. Product Rules (PM)

### PRD Structure

PRD must follow a structured format phased by scope:

* S1: Product brief, value hypothesis, KPI tree (north star, leading indicators, guardrails)
* S2: Business process flow (swimlane, happy + exceptions), GWT scenarios (3-5 key: happy, failure, edge cases), roadmap with phases
* S3: User journey map (persona, stages, emotions, pain points), system interaction flow (swimlane: user→channel→gateway→services→data), service boundary table (owns/does not own), key technical decisions (sync/async, queue strategy, external providers)
* S4: Non-functional requirements (NFRs)

### User Story & Acceptance Criteria

* User stories in “As a … I want … so that …” format
* User story 必须使用英文，清晰表达用户角色、需求和价值
* Acceptance Criteria (AC) 必须使用中文，采用 “Given / When / Then” 格式，明确通过/失败条件
* 每个 user story 必须关联到 feature 和 epic 层级：


  * Epic → Feature → Story (Jira: Epic, Task, Sub-task)

### PRD output format
默认输出应尽量包含：

* Feature Summary
* Epic
* Feature List
* User Stories
* Acceptance Criteria
* User Flow
* UI & UX Requirements
* Non-functional Requirements
* Tracking & Metrics（如适用）
* Future Extension Ideas（如适用）


## 2. Engineering Rules (研发)

### 架构原则

* 所有设计先有高层架构图 (High-level Architecture Design)
* 关键系统必须提供组件架构图 (High-level Component Diagram)
* 使用同步/异步明确，定义队列策略，外部依赖要清晰

### 工程设计输出要求

如任务涉及研发实现评审或技术方案，默认应考虑：

* Sequence diagrams（2–5 个，从系统流程衍生）
* Database ERD（实体来自 service boundary ownership）
* API Document
* 错误处理
* retry 机制
* logging / tracing 规范

### API 规范

* API 文档必备（OpenAPI 格式）
* 错误处理必须有标准错误码与描述
* retry 机制：定义可重试接口与最大重试次数
* 日志规范：日志需包含 trace ID、调用链、错误级别
* API 方案必须说明：
- endpoint
- method
- request
- response
- error model
- auth / permission（如适用）
- retry / timeout（如适用）
### 错误处理规范

* 所有设计必须考虑：
- 外部依赖失败
- 超时
- 幂等性
- 重试机制
- 用户可感知反馈
- 系统日志记录
### Logging 规范

* 关键业务流程必须明确：

- trace id
- request id（如适用）
- service name
- error level
- 关键上下文参数

## 3. Terminology (统一术语)

### 工作项定义

* Epic: 业务大目标或核心产品模块
* Feature: Epic 下的具体功能
* Story: 可开发的用户需求单元
* UX: 用户体验设计
* UI: 用户界面设计
* Mock: 模拟测试（如口语、写作）
* Score: 分数或评估结果
* CEFR: 欧洲语言共同框架（等级）

### 公司背景
* 公司专注于英语测评产品
* 核心产品：IELTS
* 公司当前重点从 TOB 向 TOC / mobile operations 转移
* Mini program 是重要渠道
* 3Ups website 是面向客户的重要前台与产品聚合平台

### 内部系统列表

* IOC admin: 管理 IETLS 核心后台运营
* ICS: 口语考试平台
* IVAP: 场地管理
* Speakup: 口语 AI 模拟
* Write up: 写作 AI 模拟
* Score up: 综合模拟测试（听说读写）
* products list
* Back end systems
	- IOC admin : Manage the core back end operations of IETLS test
	- MIS2 : older MIS system support IELTS operations
	- ITAP: For examiners calendar management
	- ISTAR: Recording managment for IOP, paper test 

* Test Delivery systems
  - ICS: Speaking test platform
  - IDV: check in tool 
  - IMP: Incidents management, support IETLS test incidents
  - IVAP: venue management
  - EM: examiners management 
  - CA/CD: Central allocation, and central delivery to support the allocations of examinersrs
  - Post test: manage user complaints to revise mark by listening the files via Post test
  - OLM: online marking to make the second marks
  
* Single Systems
  - CPMIS: the platform for procurement teams to manage activities of procurements
  - DOORS2: manage the other tests registrations
  
* TOC systems
  - IELTS website: one CMS framework company website
  - Mini program: company will focus on mobile operations, shift the focus from TOB to TOC
  - 3Ups website: one frontend website and one backend system. will combine 3 main mock test modules : Write up for writing, speakup for speaking, Scoreup for mock platform. 3UJps website will manage the products and face to customers for ordering, and use the products online
  - touch points: Big data platform, manage all the data of channels, transaction data to provide the data analysis of operation teams
 
* Test assessment
	- Speakup : for speaking mock test with AI capabilites
	- Write up : for write mock test with AI capabilities
	- Score up: For end to end mock test from listening, writing, reading, speaking

* Partner system
  - TOB webchat miniprogram
  - backend system 
  
* Global systems landing
 - global education test localizations
 - China solutions in global side 

## 4. Delivery Process (流程)

### Agent Handoff

* PRD → UX Prototyper: 基于 PRD 生成 UI/UX 设计
* UX → Eng Reviewer: 评估技术可行性
* Eng → Wiki Publisher: 发布 PRD 和 UX 文档到 Wiki

### Wiki 发布规范

* PRD 发布至 `/wiki/{epic-name}`
* UX 发布至 `/wiki/{epic-name}/ui-prototype`
* Engineering Review 发布至 `/wiki/{epic-name}/engineering-review`

## 5. Coding Rules (代码)

### React 规范

* 优先使用 Functional Components
* 优先使用 Hooks
* 组件职责单一
* 页面结构清晰
* 明确 loading / empty / error / success 状态
* 不要在一个组件中塞入过多业务逻辑
### 文件结构规范
src/
  components/
  pages/
  services/
  hooks/
  types/
  utils/
* 要求：

- 组件与页面分离
- services 负责 API 调用
- hooks 负责状态逻辑复用

### 代码输出要求

* Copilot 生成代码时必须：

- 保持结构清晰
- 使用统一命名
- 不生成随机样式
- 不重复造轮子
- 优先复用现有组件 / 结构
- 对外部调用考虑错误处理、重试和日志

## 6. Role Boundaries（角色边界）
* Product Planner只负责：

- PRD
- Epic / Feature / Story / AC
- 产品逻辑结构

* UX Prototyper只负责：

- UX 文档
- 页面结构
- 用户流程
- HTML Prototype

* Eng Reviewer只负责：

- 技术方案
- 工程风险
- 架构边界
- API / 数据 / sequence / service decisions
* Wiki Publisher只负责：

- 文档类型识别
- 路径生成
- Azure DevOps Wiki 发布

* Task Planner只负责：
- Task 拆分
- 估算
- 依赖识别

* 禁止跨角色越权执行

## UI work
Separate:
- Layout
- Components
- State
- Data dependencies
- Responsive behavior