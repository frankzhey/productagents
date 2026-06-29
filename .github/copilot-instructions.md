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

4. 如当前 Epic 文件夹存在 `context-memo.md`，各 Agent 直接读取该文件作为历史参照，**不得重新调用 Knowledge Retriever 或触发 ADO Wiki 搜索**。context-memo.md 为 Epic 级缓存，全 Epic 生命周期内共享复用。

5. **多 project 并行场景（v3.0 强制）**：除 Value Architect（项目入口）外，所有 agent（Solution / Product Planner / Eng Reviewer / Wiki Publisher）启动时必须先 Read `skills/project-context-loader/SKILL.md` 执行上下文加载协议：
   - 询问 PM project name
   - 校验 `Project/{project}/Value/LATEST.md` 存在性（不存在进入不一致循环 ≤3 次）
   - 加载 Value / Rules / context-memo
   - 按 agent 类型列出可选范围或进入例外流程（Solution: Value Epic List；Product Planner: Value Epic List + Solution 状态；Eng Reviewer: 本地 PRD List 或 Wiki fallback；Wiki Publisher: 由输入文件 frontmatter 决定）
   - PM 确认（单选 / 全选 / Wiki fallback / 手工输入）

   **禁止跳过此协议直接处理 handoff 传入的 project + epic**。

<!--3. 当前任务是否需要参考历史知识：
   - 在生成 PRD、UX 文档、Engineering Review 前，优先搜索 Azure DevOps Wiki 中最相关的历史页面
   - 优先参考最近、结构完整、与当前系统或业务最相似的 3–5 个页面
   - 历史内容仅作为参考，当前需求优先-->

---
## 1. Product Rules (PM)

> 本节仅保留全局最小约束，**PRD 文件级 contract 详见 `instructions/product.instructions.md`**（必含章节 / 输出语言 / Epic-Feature-Story 层级 / AC 规范 / 禁止事项）。
> Feature Gate、User Story 拆分优先级、FCS、Story 数量与 Story Points / Man-day / Units 映射规则见 `skills/story-splitting-spec/SKILL.md`。
> AC 详细写法、覆盖规范、写法模板见 `skills/ac-writing-spec/SKILL.md`。
> Product Planner 工作流（Project & Epic 选择 / 上游自动检测 / 设计稿输入 / Quality Gate / Rule Sedimentation）见 `agents/product-planner.agent.md`。

### 全局红线（不可被局部规则覆盖）

* User Story 必须英文，AC 必须中文（GIVEN / WHEN / THEN 多行格式）
* Epic → Feature → Story 三级结构不可混淆
* 每条 Story 必须有归属 Feature / Epic 和完整 AC
* PRD 必须落盘到 `Project/{project}/PRD/{epic-slug}/{epic-slug}-prd-{YYYY-MM-DD-HHmm}.md` 并更新 `Project/{project}/PRD/{epic-slug}/LATEST.md`，禁止只输出对话窗口
* PRD 本地源文件只产出 Epic → Feature → Story → AC 及必要估算 / NFR / OQ；Value / Solution 内容由 Wiki Publisher 在发布态合并，不在 PRD 源文件中重复展开


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

* Value Architect → Solution Architect：基于 Value Roadmap 中的 1 个或多个 Epic 展开 Solution Brief（每 Epic 独立文件）
* Solution Architect → Product Planner：基于一个或多个 Solution Brief 拆 Feature / Story / AC（Product Planner 需再次确认单 Epic / 多 Epic / ALL）
* Product Planner → UX Prototyper：基于 PRD 生成 UI/UX 设计
* Product Planner / UX → Eng Reviewer：评估技术可行性（`local` / `wiki-fallback` / `manual-input`）
* Eng → Task Planner：基于评审拆研发任务
* All → Wiki Publisher：按 v3.2 路径表发布到 ADO Wiki

### Wiki 发布规范（v3.2 · 唯一权威）

> v3.2 起，发布路径以 **project name** 为根目录，命名后缀 `-solution` / `-PRD` 严格强制。Wiki Publisher 自动注入协作元数据 + frontmatter YAML 保真 + SVG Attachment 同步上传。

| 文档类型 | 发布路径 | 模式 |
|---|---|---|
| Value Frame | `/{project}` | standard（项目主页） |
| Solution Brief（v1.4 业务方案） | `/{project}/{epic-slug}-solution` | standard |
| PRD（含 upstream）（v4.6 含 NFR Ref + Coverage Matrix） | `/{project}/{epic-slug}-PRD` | **merged** |
| PRD（独立） | `/{project}/{epic-slug}-PRD` | standard |
| UX | `/{project}/{epic-slug}-PRD/ui-prototype` | standard |
| Engineering Review（v4.0 纯评审 + SVG Attachment） | `/{project}/{epic-slug}-PRD/engineering-review` | standard |
| Task Planning | `/{project}/{epic-slug}-PRD/task-planning` | standard |
| **Architecture** ⭐ v3.2（IT Architect 产出 + SVG Attachment） | `/{project}/{epic-slug}-PRD/architecture` | standard |
| **ADR** ⭐ v3.2（每条 ADR 一页） | `/{project}/{epic-slug}-PRD/architecture/adr-{slug}` | standard |
| **NFR（Epic 级）** ⭐ v3.2 | `/{project}/{epic-slug}-PRD/nfr` | standard |
| **NFR（Project-wide）** ⭐ v3.2 | `/{project}/project-wide-nfr` | standard |
| **Refinement Request** ⭐ v3.2（反向 RR · 仅有时） | `/{project}/{epic-slug}-PRD/{architecture 或 engineering-review}/{type}-refinement-{stamp}` | standard |

> 详细发布逻辑由 `agents/wiki-publisher.agent.md` v3.2 统一执行。Agent 输出 frontmatter 必须含 `project` + `maintainer` 字段（v3.2 必填 · 唯一所有权标识）。Epic 级文档必须含 Epic 标识字段（Solution / Eng / Architecture / NFR Epic 级使用 `epic`，PRD 使用 `epic_id`）。
>
> 历史示例产出如果缺少 `project_loader` 或新版 `skills_loaded` 字段，视为 legacy artifact，不要求批量迁移；新产出与重大版本 refinement 必须补齐当前 frontmatter。
>
> **v2.x 旧路径 `/wiki/{epic-name}` 已废弃**。

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
