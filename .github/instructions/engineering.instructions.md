---
applyTo: "{**/*.{md,ts,tsx,js,jsx,json,yml,yaml,cs,sql},src/**,docs/**}"
---

# BCChina Engineering Instructions

## 目标
本文件用于约束工程设计、技术方案、实现评审、API 设计、数据设计和研发交付内容，确保输出满足 BCChina 的工程标准，并适配英语测评产品、IELTS 相关系统、Mini program、3Ups website、AI mock test 等业务场景。

所有技术设计、Engineering Review、API Document、系统交互图、数据模型、研发任务拆分、实现建议，必须遵守本规范。

---

## 角色与职责边界（v4.x · 强制）

> 本规范是**工程质量标准库**，被多个 agent 共同加载。**同一条标准对不同角色的含义不同**：产出方按它**撰写**，评审方按它**核对**。下表是唯一权威的"谁产出 / 谁评审"边界，任何 agent 不得越界产出。

| 工程产物 | 产出方（Author） | 治理 SKILL | Eng Reviewer 的动作 |
|---|---|---|---|
| High-level Architecture / Component / Application Diagram | **IT Architect** | `it-architecture-spec` | 评审（Architecture Challenge），**不产出** |
| Sequence Diagram / System Interaction Flow | **IT Architect** | `it-architecture-spec` | 评审，**不产出** |
| Database ERD | **IT Architect** | `it-architecture-spec` | 评审，**不产出** |
| API Document / Service Boundary Table | **IT Architect** | `it-architecture-spec` | 评审，**不产出** |
| ADR（架构决策记录） | **IT Architect** | `it-architecture-spec` | 评审，**不产出** |
| NFR Targets（性能/可靠/安全档位） | **NFR Architect** | `nfr-spec` | 核对 QAS 是否覆盖 NFR（NFR Verification），**不产出档位** |
| Epic / Feature / User Story / AC | **Product Planner** | `ac-writing-spec` | AC 合规核对，**不产出** |
| Task 拆分与细估 | **Task Planner** | — | Task Readiness 评审 |

**Eng Reviewer v4.1 = 纯评审**：只产出 8 类评审动作（Scope Challenge / Architecture Challenge / Blast Radius / NFR Verification / Capacity / AC 合规 / Task Readiness / Coverage Verification）+ 反向 Refinement Request（仅 Architecture / NFR）。**显式禁止**自行产出 C2/C3 架构图、ERD、API、Sequence、NFR 档位。下文所有标注「强制」的设计产出，均指**对 Author 角色强制**；Eng Reviewer 将其作为**评审检查项（核对存在性与质量）**，而非自身的撰写义务。

---

## 总体工程原则

- 先定义系统边界，再讨论实现细节
- 先定义服务职责，再设计 API、消息流、数据库
- 先设计失败路径和恢复机制，再优化 happy path
- 先保证可读、可维护、可追踪，再优化技巧性实现
- 默认考虑可扩展、可测试、可监控
- 所有方案应可支撑产品、研发、测试、运维共同评审
- 所有设计必须贴合 BCChina 现有系统语境，不得脱离真实系统环境空想架构

---

## 适用范围

本文件是上述工程产物的**共享质量标准**，按"角色与职责边界"表分配产出 / 评审责任：

- Technical Design / High-level Architecture / Component / Application Diagram → **IT Architect 产出**
- Sequence Diagram / System Interaction Flow / Database ERD → **IT Architect 产出**
- API Document / Service Boundary Table → **IT Architect 产出**
- Engineering Review（对以上产物的评审）→ **Eng Reviewer 产出**（纯评审，不重新撰写设计）
- Task 拆分中的研发实现部分 → **Task Planner 产出**
- Copilot 输出的技术实现建议 → 适用本规范，但须先确认当前角色边界

---

## 工程输出优先级

如无特殊说明，工程输出优先级如下：

1. 正确性
2. 可维护性
3. 可观测性
4. 可扩展性
5. 性能
6. 实现复杂度控制

---

## 架构规范

### 1. High-level Architecture Design（强制 · IT Architect 产出 / Eng Reviewer 评审）
当任务涉及系统级功能、跨系统集成、异步流程、AI评分、上传/回调/结果回传等场景时，**IT Architect** 必须给出高层架构设计；Eng Reviewer 核对其是否齐全、合理，而非自行补画。

至少说明：

- 用户入口
- 渠道层
- 网关 / BFF / API gateway
- 应用服务
- 数据存储
- 外部依赖
- 异步链路
- 结果回传路径

---

### 2. High-level Components Architecture / Application Diagram（强制 · IT Architect 产出 / Eng Reviewer 评审）
关键系统必须由 **IT Architect** 说明组件关系，尤其在以下场景：

- Mini program + backend
- 3Ups website + backend
- AI scoring service
- callback / async result processing
- channel → service → data 的复杂链路

至少说明：

- 组件名称
- 组件职责
- 调用关系
- 数据流向
- 同步 / 异步边界

---

## 系统边界规则

所有技术方案必须明确：

- 当前功能属于哪个系统
- 哪些能力由本系统 Owns
- 哪些能力由其他系统提供
- 哪些能力不应在本系统内实现

必须优先输出 Service Boundary Table，格式建议：

| Service / Component | Owns | Does NOT Own | Notes |
|---|---|---|---|

---

## 关键技术决策（强制）

每个重要功能都应明确以下决策：

- sync or async
- queue strategy
- callback / polling / event-driven
- external providers
- storage strategy
- retry strategy
- timeout strategy
- idempotency strategy

如果是 AI mock / score / upload / result 返回相关场景，必须优先考虑异步设计和失败恢复。

---

## Sequence Diagram 规范

### 适用场景
以下情况建议输出 2–5 个 sequence diagrams：

- 上传 / 回调 / 出分
- 登录 / 鉴权 / 绑定 unionId
- async scoring
- result retrieval
- retry / failure recovery

### 要求
- 来源于 S3 system interaction flow
- 不超过 5 个重点 sequence
- 至少包含：
  - 1 个 happy path
  - 1 个 failure path
  - 1 个 edge case（如适用）

### 必须说明
- actor
- service
- request
- response
- async event / queue / callback
- failure point

---

## Database ERD 规范

### 适用场景
涉及实体、记录、结果、上传、绑定关系、状态流转时必须考虑 ERD。

### 要求
实体必须从 Service Boundary Ownership 推导，不允许随意堆表。

至少说明：

- entity name
- primary key
- foreign key
- ownership
- status fields
- timestamps
- relation

### 特别要求
对以下场景必须明确建模：

- 用户唯一性（如 unionId）
- 一次提交限制
- 上传记录
- AI评分记录
- 回调状态
- 最终结果状态

---

## API Document 规范

所有 API 设计必须包含：

- API name
- endpoint
- method
- purpose
- auth / permission
- request fields
- response fields
- error codes
- retry / timeout strategy（如适用）

### API 设计原则
- 命名清晰
- 输入输出字段语义明确
- 避免暴露无意义技术细节
- 同一领域保持一致命名风格
- 优先考虑幂等性和向后兼容性

### 错误返回
必须定义：

- business error
- validation error
- auth error
- dependency failure
- system error

---

## 错误处理规范

所有工程方案必须考虑：

- 用户输入错误
- 外部服务失败
- 超时
- 网络异常
- 重复提交
- 回调重复
- 状态不一致
- 部分成功 / 部分失败

### 原则
- 错误要可定位
- 错误要可恢复
- 错误要有统一处理策略
- 对用户的错误提示和对系统的技术日志应分离

---

## Retry 机制规范

如涉及外部服务、网络传输、AI评分、消息队列、异步回调，必须考虑 retry。

### 默认要求
- 仅对可重试错误重试
- 明确最大重试次数
- 使用退避策略（如 exponential backoff）
- 避免无限重试
- 设计幂等性，避免重复写入

### 必须说明
- 哪些接口可重试
- 哪些事件不可重试
- 重试后如何识别最终失败
- 最终失败如何补偿或告警

---

## Logging / Monitoring 规范

所有关键业务链路必须明确 logging 和 monitoring 方案。

### 日志至少包含
- timestamp
- service name
- request id / trace id
- user / session key（如适用，注意脱敏）
- business key（如 submission id / scoring id）
- error level
- key context

### 必须监控
- 上传成功率
- AI评分回调成功率
- 出分延迟
- API失败率
- 重试次数
- 异常状态积压
- 关键用户路径转化率（如适用）

---

## 性能与非功能要求（NFR）

所有 Engineering Review 默认需要考虑：

- performance
- reliability
- scalability
- compatibility
- retry
- logging
- security
- observability
- data integrity

如果是用户上传、录音、AI评分、结果页等场景，必须特别考虑：

- 上传耗时
- 文件大小限制
- 并发请求控制
- 结果一致性
- 状态可追踪性

---

## 安全与数据处理

必须明确：

- auth / permission
- 用户标识绑定（如 unionId）
- 敏感字段脱敏
- 文件访问权限
- callback 校验
- 防重复提交
- 防越权读取结果

---

## 与公司系统对齐（强制）

输出技术方案时，必须优先使用公司现有术语和系统边界。

常见系统包括：

### Back-end Systems
- IOC admin
- MIS2
- ITAP
- ISTAR

### Test Delivery Systems
- ICS
- IDV
- IMP
- IVAP
- EM
- CA/CD
- Post test
- OLM

### TOC Systems
- IELTS website
- Mini program
- 3Ups website
- Touch points

### Test Assessment
- Speakup
- Write up
- Score up

如果当前功能依赖这些系统，必须说明：
- 是否集成
- 集成点是什么
- Own / Not Own 边界是什么

---

## 研发任务拆分规则

当输出 Task / Engineering work items 时，建议按以下维度拆分：

- FE
- BE
- Integration
- Data
- QA
- DevOps / Monitoring（如适用）

任务必须可执行，不要输出过大、不可估的任务块。

---

## Copilot 输出要求

先按"角色与职责边界"确认当前角色：

**当作为设计 Author（IT Architect / NFR Architect / Product Planner）输出技术方案、API 设计、数据设计、任务拆分时，必须：**

1. 先定义系统边界
2. 再定义交互流程
3. 再定义技术决策
4. 再定义 API / data / retry / logging
5. 明确失败路径
6. 使用公司术语和系统名
7. 输出结构必须适合评审与研发 handover
8. 优先考虑 BCChina 现有系统复用，而不是默认新建系统

**当作为 Eng Reviewer（纯评审）输出 Engineering Review 时，必须：**

1. 以上述 1–8 为**评审检查项**，逐项核对 Author 产物是否齐全、合理、可追踪
2. 缺项 / 不合规 → 通过反向 Refinement Request（仅 Architecture / NFR）回提 Author，**不自行补写**
3. 不产出 C2/C3 架构图、ERD、API、Sequence、NFR 档位

---

## 禁止事项

> 以下针对**设计 Author 角色**；Eng Reviewer 另有约束：禁止自行产出架构图 / ERD / API / Sequence / NFR 档位，缺项一律回提 Author。

- 禁止脱离 service boundary 直接谈实现
- 禁止只写 happy path，不写异常路径
- 禁止只写 API，不写数据和状态模型
- 禁止没有 retry / logging / error handling
- 禁止忽略异步链路和回调一致性
- 禁止为了解释方便而虚构不存在的系统
- 禁止直接复制历史方案而不结合当前上下文判断

---

## 默认输出建议

> 下列清单是 **IT Architect 设计产出**的默认结构；**Eng Reviewer 不按此清单撰写**，而是逐项核对其存在性与质量（详见 `eng-review-spec`）。

如无特殊要求，IT Architect 技术输出建议包含：

- Scope / Context
- High-level Architecture
- Component Diagram
- System Interaction Flow
- Service Boundary Table
- Key Technical Decisions
- Sequence Diagrams
- Database ERD
- API Document
- Error Handling
- Retry Strategy
- Logging & Monitoring
- Risks / Open Questions