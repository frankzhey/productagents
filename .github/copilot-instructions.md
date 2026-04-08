# BCChina Copilot Instructions (Enterprise Version)

## 1. Product Rules (PM)

### PRD Structure

PRD must follow a structured format phased by scope:

* S1: Product brief, value hypothesis, KPI tree (north star, leading indicators, guardrails)
* S2: Business process flow (swimlane, happy + exceptions), GWT scenarios (3-5 key: happy, failure, edge cases), roadmap with phases
* S3: User journey map (persona, stages, emotions, pain points), system interaction flow (swimlane: user→channel→gateway→services→data), service boundary table (owns/does not own), key technical decisions (sync/async, queue strategy, external providers)
* S4: Non-functional requirements (NFRs)

### User Story & Acceptance Criteria

* User stories in “As a … I want … so that …” format
* Acceptance Criteria (AC) must be in “Given / When / Then” format, with clear pass/fail conditions
* Each user story ties to a feature and epic hierarchy:

  * Epic → Feature → Story (Jira: Epic, Task, Sub-task)

## 2. Engineering Rules (研发)

### 架构原则

* 所有设计先有高层架构图 (High-level Architecture Design)
* 关键系统必须提供组件架构图 (High-level Component Diagram)
* 使用同步/异步明确，定义队列策略，外部依赖要清晰

### API 规范

* API 文档必备（OpenAPI 格式）
* 错误处理必须有标准错误码与描述
* retry 机制：定义可重试接口与最大重试次数
* 日志规范：日志需包含 trace ID、调用链、错误级别

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

## 5. Coding Rules (代码)

### React 规范

* 使用 Functional Components
* Hooks 优先，避免 class

## UI work
Separate:
- Layout
- Components
- State
- Data dependencies
- Responsive behavior