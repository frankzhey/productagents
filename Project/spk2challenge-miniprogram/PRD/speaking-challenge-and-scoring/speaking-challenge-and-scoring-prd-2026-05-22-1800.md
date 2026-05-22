---
project: spk2challenge-miniprogram
epic_id: EPIC-speaking-challenge-and-scoring
epic_name: Speaking Challenge and Scoring
epic_source: B
created: 2026-05-22-1800
maintainer: "@frankzhey"
upstream_snapshot:
  value: Project/spk2challenge-miniprogram/Value/value-architect-2026-05-08-0000.md
  solution: Project/spk2challenge-miniprogram/Solution/speaking-challenge-and-scoring/speaking-challenge-and-scoring-solution-brief-2026-05-22-1028.md
  nfr: Project/spk2challenge-miniprogram/NFR/project-wide/spk2challenge-miniprogram-project-wide-nfr-2026-05-22-1100.md
  architecture: Project/spk2challenge-miniprogram/Architecture/speaking-challenge-and-scoring/speaking-challenge-and-scoring-architecture-2026-05-22-1500.md
upstream_sources:
  value: { type: local, path: Project/spk2challenge-miniprogram/Value/value-architect-2026-05-08-0000.md }
  solution: { type: local, path: Project/spk2challenge-miniprogram/Solution/speaking-challenge-and-scoring/speaking-challenge-and-scoring-solution-brief-2026-05-22-1028.md }
  nfr: { type: local, path: Project/spk2challenge-miniprogram/NFR/project-wide/spk2challenge-miniprogram-project-wide-nfr-2026-05-22-1100.md, scope: project-wide }
  architecture: { type: local, path: Project/spk2challenge-miniprogram/Architecture/speaking-challenge-and-scoring/speaking-challenge-and-scoring-architecture-2026-05-22-1500.md }
design_source:
  mode: pm-input
  ref: upstream-only; no Magic Patterns or Figma provided
status: draft
pm_confirmation:
  status: pending
  confirmed_by: PM
  confirmed_at: null
  confirmation_note: null
skills_loaded:
  - skills/project-context-loader/SKILL.md
  - skills/ac-writing-spec/SKILL.md
project_loader:
  pm_confirmed_project: spk2challenge-miniprogram
  pm_confirmed_epic: speaking-challenge-and-scoring
  pp_mode: single
  loader_at: 2026-05-22-1800
---

> **上游引用**（Wiki Publisher 合并发布时会自动拉取展开）
> - Value Frame: Project/spk2challenge-miniprogram/Value/LATEST.md
> - Solution Brief: Project/spk2challenge-miniprogram/Solution/speaking-challenge-and-scoring/LATEST.md
> - NFR Reference: Project/spk2challenge-miniprogram/NFR/project-wide/LATEST.md
> - Architecture Reference: Project/spk2challenge-miniprogram/Architecture/speaking-challenge-and-scoring/LATEST.md
> - 本 PRD 仅产出 Epic-Feature-Story 三级骨架与 AC，不重复战略层 / Journey / Process / Roadmap / 架构设计细节。

## §1 Epic Definition

| 字段 | 内容 |
|---|---|
| **Epic ID** | `EPIC-speaking-challenge-and-scoring` |
| **Epic Name** | Speaking Challenge and Scoring |
| **Source** | B 来自 Solution Brief |
| **KPI 对齐** | K1, K2, K3, K6, K7, K8, K9 |
| **关联 Phase** | MVP |

**Context**（≤300 字）：
本 Epic 是小程序口语挑战 MVP 的端到端价值闭环，覆盖用户进入挑战、查看题目、录音提交、AI 异步评分、IELTS band/CEFR 解释、基础提分建议、评分可信度说明、结果页 website 导流与 guardrail 监控。本期目标不是建设完整学习系统或 website 付费承接详情，而是验证移动端轻量练习到 AI 评分再到深度承接的最短闭环。

**Scope In**：
- 小程序挑战入口、挑战列表、题目详情、开始挑战与中断恢复。
- 麦克风授权、录音控件、无效录音拦截、音频上传、幂等评分任务创建、短轮询查询、超时失败恢复。
- 结果页总分、维度分、IELTS band、CEFR 参考区间、基础提分建议、映射版本与练习参考说明。
- Website CTA、深链低敏参数、score bucket、归因埋点、默认承接降级。
- 隐私授权提示、投诉反馈入口、K6/K7/K8/K9 guardrail 监控事件。

**Scope Out**：
- 连续挑战、任务、提醒、留存机制（E2）。
- Website 内题库、AI 工具、锦囊内容与付费承接细节（E3）。
- 多科目测评入口、个性化学习路径、跨端统一学习身份（Future E4-E6）。
- 完整运营数据平台、完整 AI 模型训练闭环、官方 IELTS 成绩解释。

## §2 Feature List

| Feature ID | Feature Name | Description | Value | Source |
|---|---|---|---|---|
| F1 | Challenge Participation Flow | 在小程序首页、挑战列表和题目详情中提供轻量挑战入口、题目说明、作答前准备和中断恢复，让用户能在移动端低成本开始一次口语挑战。 | 降低首次参与门槛，提升挑战启动率与首次作答完成率，直接支撑 K1、K2。 | Solution §2 |
| F2 | Voice Scoring Pipeline | 提供麦克风授权、录音控件、无效录音拦截、音频上传、对象存储、幂等评分任务创建、短轮询查询和评分失败兜底，确保用户提交后能稳定拿到结果或恢复路径。 | 直接支撑 K1、K2、K6、K7，是用户感知 AI 测评价值和系统可靠性的核心链路。 | Solution §2 |
| F3 | Band-and-CEFR Result Card | 将评分服务返回的结构化结果转译成 IELTS band、CEFR 参考区间、分维度解读和基础提分建议，并在缺模板或边界分数时提供可理解的兜底解释。 | 提升结果可理解性与信任度，让用户知道自己处于什么水平以及下一步如何练习。 | Solution §2 |
| F4 | Website Handoff CTA | 在结果页按来源场景和 score bucket 展示 website 承接入口，深链只透传低敏分层参数，并记录点击、到达和后续转化归因。 | 提升 K3 导流点击率，为 K5 付费转化提供高意向流量，同时降低敏感分数跨端扩散风险。 | Solution §2 |
| F5 | Scoring Trust and Guardrails | 为评分结果增加练习参考说明、可信度/限制表达、投诉反馈入口、隐私授权提示和关键埋点监控，避免用户误解为官方成绩并支持问题追踪。 | 降低 K8 投诉率和 K9 隐私事故风险，提升 AI 评分结果的可解释性和可运营性。 | Solution §2 |

## §3 User Stories and AC

### Feature F1 — Challenge Participation Flow（来自 §2）

#### Story EPIC-speaking-challenge-and-scoring-F1-S01 — 首页挑战入口

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S01`

**upstream_refs**:
- persona: P1
- journey_stage: J1
- scenarios: [BP-H1]
- kpi_alignment: [K1, K2]

**User Story（英文）**:
> As an IELTS learner, I want to see a clear speaking challenge entry on the miniapp home page, so that I can quickly start a speaking attempt.

**Acceptance Criteria**（中文）：

AC1：首页展示可参与挑战入口
GIVEN 用户进入小程序首页
AND 当前存在至少一条已发布且可参与的口语挑战
WHEN 首页完成数据加载
THEN 系统展示口语挑战入口
AND 入口展示 `challengeTitle`、`entryStatus` 和开始操作
AND 用户点击入口后进入对应题目详情页

AC2：首页无可参与挑战时不展示误导入口
GIVEN 用户进入小程序首页
AND 当前无已发布且可参与的口语挑战
WHEN 首页完成数据加载
THEN 系统不展示可点击的开始挑战入口
AND 展示无可参与挑战的提示文案 [假设]
AND 页面保留进入挑战列表或返回首页其他内容的路径

AC3：首页入口加载失败
GIVEN 用户进入小程序首页
WHEN 挑战入口接口返回系统错误或超时
THEN 系统展示通用失败提示："系统繁忙，请稍后重试" [假设]
AND 首页其他模块不受影响
AND 用户刷新首页后可重新加载挑战入口

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F1-S02 — 挑战列表浏览

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S02`

**upstream_refs**:
- persona: P1
- journey_stage: J1
- scenarios: [BP-H1]
- kpi_alignment: [K1]

**User Story（英文）**:
> As an IELTS learner, I want to browse available speaking challenges, so that I can choose a prompt to attempt.

> AC 降级覆盖（≥3 条）
> 降级理由：本 Story 仅查询展示，无状态变更 / 无三方调用 / 无敏感字段

**Acceptance Criteria**（中文）：

AC1：默认加载挑战列表
GIVEN 用户进入挑战列表页
WHEN 页面首次加载完成
THEN 系统展示当前可参与挑战列表
AND 默认按 `publishTime` 倒序排列 [假设]
AND 每条记录展示 `challengeTitle`、`difficultyLabel`、`publishTime` 和 `entryStatus`

AC2：字段展示规范
GIVEN 用户查看挑战列表
WHEN 系统返回挑战数据
THEN 页面按字段规范展示 `challengeTitle`、`difficultyLabel`、`publishTime` 和 `entryStatus`
AND 字段为空时展示 `—`
AND 不展示内部配置字段或技术状态值

AC3：列表空状态
GIVEN 用户进入挑战列表页
WHEN 当前无可参与挑战数据
THEN 页面展示无挑战提示 [假设]
AND 保留返回首页的路径
AND 页面不展示空白列表容器

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F1-S03 — 题目详情与开始挑战

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S03`

**upstream_refs**:
- persona: P1
- journey_stage: J2
- scenarios: [BP-H1]
- kpi_alignment: [K1, K2]

**User Story（英文）**:
> As an IELTS learner, I want to read the prompt details before recording, so that I know what I should answer.

**Acceptance Criteria**（中文）：

AC1：题目详情展示
GIVEN 用户从首页或挑战列表进入题目详情页
WHEN 题目详情加载成功
THEN 系统展示 `promptText`、`instruction`、`suggestedDuration` 和开始挑战操作
AND 页面展示练习参考性质说明
AND 题目内容与入口指向的 `challengeId` 保持一致

AC2：开始挑战条件
GIVEN 用户位于题目详情页
AND 当前题目状态为 `published`
WHEN 用户点击开始挑战
THEN 系统进入录音作答页
AND 录音页携带 `challengeId` 和 `promptId`
AND 系统不在此步骤创建评分任务

AC3：题目不可参与
GIVEN 用户位于题目详情页
AND 当前题目状态为 `offline`、`expired` 或 `unsupported`
WHEN 用户点击开始挑战
THEN 系统阻止进入录音页
AND 展示题目不可参与提示 [假设]
AND 引导用户返回挑战列表

AC4：重复点击开始挑战
GIVEN 用户位于题目详情页
WHEN 用户连续多次点击开始挑战
THEN 系统只执行一次页面跳转
AND 不创建重复挑战会话记录

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F1-S04 — 中断恢复

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S04`

**upstream_refs**:
- persona: P1
- journey_stage: J2
- scenarios: [BP-H1]
- kpi_alignment: [K2]

**User Story（英文）**:
> As an IELTS learner, I want to resume a challenge after interruption, so that I do not need to select the prompt again.

**Acceptance Criteria**（中文）：

AC1：保留待恢复挑战
GIVEN 用户已进入录音作答页
AND 当前录音尚未成功提交
WHEN 用户切后台、误返回或页面重载
THEN 系统保留最近一次待恢复的 `challengeId` 和 `promptId`
AND 待恢复记录不包含原始录音文件

AC2：恢复入口展示
GIVEN 用户再次进入首页或挑战入口
AND 系统存在有效待恢复记录
WHEN 页面加载完成
THEN 系统展示继续挑战入口
AND 用户点击后回到对应题目详情页或录音页 [假设]

AC3：恢复记录失效
GIVEN 用户点击继续挑战入口
WHEN 对应题目已下线、过期或恢复记录损坏
THEN 系统提示当前挑战无法继续 [假设]
AND 清除无效恢复记录
AND 引导用户返回挑战列表

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F1-S05 — 题目缺失降级

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S05`

**upstream_refs**:
- persona: P1
- journey_stage: J1
- scenarios: [BP-H1]
- kpi_alignment: [K1]

**User Story（英文）**:
> As an IELTS learner, I want a safe fallback when no challenge is available, so that I do not hit a dead end.

**Acceptance Criteria**（中文）：

AC1：无新题降级
GIVEN 用户进入首页或挑战列表
WHEN 当前无可参与挑战题
THEN 系统展示无新题提示 [假设]
AND 提供返回首页或查看历史题的路径 [假设]
AND 不展示未发布或无评分支持的题目

AC2：降级入口范围
GIVEN 系统展示历史题或备用入口
WHEN 用户点击备用入口
THEN 系统只展示允许参与且支持评分的题目
AND 不展示 `offline` 或 `unsupported` 状态题目

AC3：备用数据加载失败
GIVEN 系统需要加载历史题或备用题
WHEN 备用数据接口失败
THEN 系统展示通用失败提示
AND 保留返回首页路径
AND 不进入空白页面

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

### Feature F2 — Voice Scoring Pipeline（来自 §2）

#### Story EPIC-speaking-challenge-and-scoring-F2-S01 — 麦克风权限申请

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S01`

**upstream_refs**:
- persona: P1
- journey_stage: J3
- scenarios: [BP-U1]
- kpi_alignment: [K2, K9]

**User Story（英文）**:
> As an IELTS learner, I want microphone permission guidance, so that I understand why access is needed before recording.

**Acceptance Criteria**（中文）：

AC1：首次进入录音页申请权限
GIVEN 用户首次进入录音作答页
AND 当前设备未授予麦克风权限
WHEN 页面初始化完成
THEN 系统发起麦克风权限申请
AND 展示麦克风用途说明
AND 在授权完成前提交操作不可用

AC2：授权成功
GIVEN 用户位于录音作答页
AND 系统已发起麦克风权限申请
WHEN 用户同意授权
THEN 系统进入可录音状态
AND 开始录音操作可用

AC3：拒绝授权
GIVEN 用户位于录音作答页
AND 系统已发起麦克风权限申请
WHEN 用户拒绝授权
THEN 系统展示重新授权引导
AND 禁止用户继续录音提交
AND 页面保留返回题目详情的路径

AC4：权限查询异常
GIVEN 用户进入录音作答页
WHEN 客户端权限查询失败
THEN 系统展示权限状态获取失败提示 [假设]
AND 录音操作保持不可用
AND 用户可重新进入页面再次尝试

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S02 — 录音控件

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S02`

**upstream_refs**:
- persona: P1
- journey_stage: J3
- scenarios: [BP-H1]
- kpi_alignment: [K2, K6]

**User Story（英文）**:
> As an IELTS learner, I want to start, stop, preview, and rerecord my answer, so that I can control the recording before submission.

**Acceptance Criteria**（中文）：

AC1：录音状态机
GIVEN 用户已授予麦克风权限
WHEN 用户进入录音作答页
THEN 系统支持 `idle`、`recording`、`recorded`、`submitting` 和 `error` 状态
AND 每次状态变化都以当前录音会话为准

AC2：开始与停止录音
GIVEN 用户位于录音作答页
AND 当前状态为 `idle`
WHEN 用户点击开始录音
THEN 系统进入 `recording` 状态
AND 停止录音操作可用
AND 提交操作不可用

AC3：停止后生成待提交录音
GIVEN 用户正在录音
WHEN 用户点击停止录音
THEN 系统结束录音并进入 `recorded` 状态
AND 生成待提交音频
AND 用户可试听或重录 [假设]

AC4：重录
GIVEN 用户已生成待提交音频
WHEN 用户点击重录
THEN 系统清除当前待提交音频
AND 页面回到 `idle` 状态
AND 上一次音频不得继续用于提交

AC5：录音组件异常
GIVEN 用户位于录音作答页
WHEN 录音组件初始化失败或录音中断
THEN 系统进入 `error` 状态
AND 当前录音作废
AND 用户可重新开始录音

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S03 — 无效录音校验

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S03`

**upstream_refs**:
- persona: P1
- journey_stage: J3
- scenarios: [BP-U2]
- kpi_alignment: [K2, K6]

**User Story（英文）**:
> As an IELTS learner, I want invalid recordings to be blocked, so that unusable audio does not enter scoring.

**Acceptance Criteria**（中文）：

AC1：无有效录音时提交不可用
GIVEN 用户位于录音作答页
AND 当前不存在有效待提交音频
WHEN 用户查看提交操作
THEN 提交操作处于不可点击状态
AND 页面提示需先完成有效录音 [假设]

AC2：客户端基础校验
GIVEN 用户已停止录音
WHEN 录音为空或时长低于最小门槛
THEN 系统判定为无效录音
AND 展示重新录制提示 [假设]
AND 不进入上传流程

AC3：服务端有效性校验
GIVEN 用户提交了通过客户端校验的音频
WHEN Upload Gateway 判定音频损坏、格式不支持或超过文件上限
THEN 系统拒绝该音频进入评分链路
AND 不创建评分任务
AND 用户可返回录音页重新录制

AC4：重复提交限制
GIVEN 用户已点击提交并进入上传流程
WHEN 用户再次点击同一录音的提交操作
THEN 系统不发起重复上传请求
AND 当前提交操作保持不可用

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S04 — 音频上传与对象存储

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S04`

**upstream_refs**:
- persona: P1
- journey_stage: J3
- scenarios: [BP-H1, BP-U2]
- kpi_alignment: [K2, K6, K9]

**User Story（英文）**:
> As an IELTS learner, I want my validated recording to upload safely, so that the scoring service can process it.

**Acceptance Criteria**（中文）：

AC1：上传触发范围
GIVEN 用户已有一条通过本地校验的音频
AND 当前 `promptId` 有效
WHEN 用户点击提交
THEN 系统上传当前音频文件
AND 上传请求关联 `promptId`、用户会话和 `clientRequestId`
AND 上传成功前不创建评分任务

AC2：上传成功
GIVEN 用户已触发音频上传
WHEN 上传成功并完成服务端有效性校验
THEN 系统返回 `audioRef`
AND `audioRef` 可用于创建评分任务
AND 原始录音地址不在前端长期暴露

AC3：上传失败
GIVEN 用户已触发音频上传
WHEN 上传接口超时、网络失败或对象存储失败
THEN 系统展示上传失败提示 [假设]
AND 不创建评分任务
AND 用户可基于同一待提交录音重试上传

AC4：离页或网络中断
GIVEN 用户处于上传中状态
WHEN 用户离开页面、切后台或网络中断
THEN 系统以服务端最终上传状态为准
AND 用户重新进入后可查看是否需要重试 [假设]
AND 不展示未确认成功的结果页

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S05 — 幂等评分任务创建

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S05`

**upstream_refs**:
- persona: P1
- journey_stage: J4
- scenarios: [BP-H1, BP-E1]
- kpi_alignment: [K1, K6, K7]

**User Story（英文）**:
> As an IELTS learner, I want the system to create or reuse a scoring task safely, so that duplicate submissions do not create duplicate scoring jobs.

**Acceptance Criteria**（中文）：

AC1：上传成功后创建任务
GIVEN 系统已获得有效 `audioRef`
AND 当前 `promptId` 和用户会话有效
WHEN 系统调用评分任务创建接口
THEN 系统创建评分任务或复用已有评分任务
AND 返回唯一 `taskId`
AND 页面进入评分处理中状态

AC2：幂等键规则
GIVEN 系统准备创建评分任务
WHEN 后端生成幂等键
THEN 幂等键至少基于用户、题目、`audioRef` 和 `clientRequestId`
AND 相同幂等键的重复请求复用既有 `taskId`
AND 不重复消耗 AI 评分资源

AC3：任务创建失败
GIVEN 系统已获得有效 `audioRef`
WHEN 评分任务创建接口返回系统错误或超时
THEN 页面展示任务创建失败提示 [假设]
AND 不进入结果展示态
AND 用户可重试创建任务

AC4：业务规则不满足
GIVEN 系统已获得有效 `audioRef`
WHEN 后端判定录音不满足评分业务规则
THEN 系统阻止创建评分任务
AND 展示明确业务提示 [假设]
AND 用户可返回录音页重新作答

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S06 — 短轮询评分状态查询

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S06`

**upstream_refs**:
- persona: P1
- journey_stage: J4
- scenarios: [BP-H1, BP-U3]
- kpi_alignment: [K1, K6, K7]

**User Story（英文）**:
> As an IELTS learner, I want the miniapp to check scoring progress automatically, so that I can receive the result without manual searching.

**Acceptance Criteria**（中文）：

AC1：短轮询启动
GIVEN 页面已获得有效 `taskId`
AND 当前评分任务未结束
WHEN 页面进入评分处理中态
THEN 系统每 2 秒查询一次评分任务状态 [来自 Architecture §3.4]
AND 最长查询窗口为 30 秒 [来自 NFR §1]
AND 查询同一 `taskId` 时不创建新评分任务

AC2：状态机覆盖
GIVEN 页面正在查询评分状态
WHEN 后端返回任务状态
THEN 页面至少识别 `created`、`queued`、`processing`、`succeeded`、`failed`、`timeout` 和 `cancelled`
AND 未知状态按异常处理
AND 不提前展示未完成分数

AC3：成功状态
GIVEN 页面正在查询评分状态
WHEN 状态返回 `succeeded`
THEN 系统加载结果展示数据
AND 页面进入结果展示态
AND 停止继续轮询该 `taskId`

AC4：查询接口异常
GIVEN 页面正在查询评分状态
WHEN 状态查询接口短时失败或网络异常
THEN 系统在超时窗口内继续查询同一 `taskId`
AND 不直接丢弃当前评分任务
AND 达到阈值后进入超时或失败兜底路径

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S07 — 超时与失败恢复

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S07`

**upstream_refs**:
- persona: P1
- journey_stage: J4
- scenarios: [BP-U3]
- kpi_alignment: [K6, K7, K8]

**User Story（英文）**:
> As an IELTS learner, I want clear recovery options when scoring fails or times out, so that I know what to do next.

**Acceptance Criteria**（中文）：

AC1：评分超时
GIVEN 页面已对 `taskId` 执行短轮询
WHEN 30 秒内未获得 `succeeded` 终态 [来自 NFR §1]
THEN 系统展示结果生成较慢提示
AND 提供继续等待、刷新结果、重新提交或稍后查看路径 [假设]
AND 保留 `taskId` 便于排查

AC2：评分失败
GIVEN 页面正在查询评分状态
WHEN 任务状态返回 `failed`
THEN 系统展示评分失败提示 [假设]
AND 不展示不完整分数或解释
AND 用户可返回录音页重新作答

AC3：继续等待或刷新
GIVEN 用户处于超时恢复页面
WHEN 用户选择继续等待或刷新结果
THEN 系统继续查询同一 `taskId`
AND 不创建新的评分任务

AC4：重新提交
GIVEN 用户处于超时或失败恢复页面
WHEN 用户选择重新提交
THEN 系统引导用户重新录音或重新上传
AND 新尝试生成新的提交上下文
AND 旧 `taskId` 不继续驱动当前结果页

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

### Feature F3 — Band-and-CEFR Result Card（来自 §2）

#### Story EPIC-speaking-challenge-and-scoring-F3-S01 — 总分与分维度展示

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S01`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [BP-H1]
- kpi_alignment: [K1, K8]

**User Story（英文）**:
> As an IELTS learner, I want to see my overall and dimension scores, so that I can understand the outcome of my attempt.

> AC 降级覆盖（≥3 条）
> 降级理由：本 Story 仅查询展示，无状态变更 / 无三方调用 / 无敏感字段

**Acceptance Criteria**（中文）：

AC1：结果页默认展示
GIVEN 用户已获得 `succeeded` 状态的评分结果
WHEN 结果页加载完成
THEN 系统展示 `overallScore` 和分维度结果
AND 展示 `taskId` 对应的结果数据
AND 不展示其他用户或其他任务结果

AC2：字段展示规范
GIVEN 用户查看结果页
WHEN 系统返回评分结果字段
THEN 页面展示 `overallScore`、`fluencyScore`、`lexicalScore`、`grammarScore` 和 `pronunciationScore` [假设]
AND 字段缺失时展示 `—`
AND 不展示内部原始评分字段

AC3：结果数据异常
GIVEN 用户进入结果页
WHEN 结果数据缺失或结构不完整
THEN 系统不展示错误分数
AND 展示结果加载异常提示 [假设]
AND 用户可刷新结果

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F3-S02 — IELTS band 与 CEFR 对照

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S02`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [BP-H1, BP-U4]
- kpi_alignment: [K1, K8]

**User Story（英文）**:
> As an IELTS learner, I want to see IELTS band and CEFR reference together, so that I can map my result to a familiar level.

**Acceptance Criteria**（中文）：

AC1：band 与 CEFR 展示
GIVEN 用户已获得成功评分结果
AND 当前结果存在可映射等级区间
WHEN 结果页加载完成
THEN 系统展示 IELTS band
AND 同时展示 CEFR 参考等级
AND 标注该结果为练习参考

AC2：映射版本一致
GIVEN 系统需要展示 band 与 CEFR 对照
WHEN 系统读取映射规则
THEN 使用当前生效的 `mappingVersion`
AND 同一分数在同一 `mappingVersion` 下返回一致结果

AC3：映射缺失
GIVEN 用户查看结果页
WHEN 当前结果缺少映射规则或 CEFR 模板
THEN 系统不展示错误等级
AND 使用通用参考说明 [假设]
AND 结果页其他分数信息仍可展示

AC4：映射口径待确认
GIVEN PRD 当前引用 Value V-OQ3
WHEN PM + Eng 尚未确认固定映射或内部解释版映射
THEN 本 Story 标记为待口径确认
AND 进入 §9 P-OQ1 跟踪

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F3-S03 — 基础提分建议

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S03`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [BP-H1]
- kpi_alignment: [K1, K3, K8]

**User Story（英文）**:
> As an IELTS learner, I want basic improvement suggestions, so that I know what to practice next.

**Acceptance Criteria**（中文）：

AC1：建议生成范围
GIVEN 用户已获得成功评分结果
AND 系统存在匹配的建议模板
WHEN 结果页加载完成
THEN 系统展示基础提分建议
AND 建议基于 score bucket、维度短板或 band 区间生成 [假设]
AND 建议不承诺官方提分结果

AC2：建议模板缺失
GIVEN 用户已获得成功评分结果
WHEN 当前 score bucket 或维度组合缺少建议模板
THEN 系统展示通用练习建议 [假设]
AND 不展示空白建议模块

AC3：建议内容治理
GIVEN 系统展示基础提分建议
WHEN 建议模板被更新
THEN 新结果使用新模板版本
AND 历史结果保留生成时的 `mappingVersion` 或 `templateVersion`

AC4：建议加载异常
GIVEN 用户进入结果页
WHEN 建议模板服务或配置读取失败
THEN 系统隐藏局部建议模块或展示通用建议 [假设]
AND 不影响分数和 band/CEFR 展示

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F3-S04 — 边界分数解释回退

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S04`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [BP-U4]
- kpi_alignment: [K8]

**User Story（英文）**:
> As an IELTS learner, I want understandable fallback explanations for boundary scores, so that unclear mappings do not confuse me.

**Acceptance Criteria**（中文）：

AC1：边界分数识别
GIVEN 用户评分结果落在 band 或 CEFR 边界区间
WHEN 结果页生成解释
THEN 系统使用边界分数解释规则 [假设]
AND 不夸大该结果的准确性

AC2：通用解释回退
GIVEN 系统无法匹配精确解释模板
WHEN 结果页加载解释模块
THEN 系统使用通用等级解释
AND 展示练习参考说明
AND 不展示错误或空白解释

AC3：局部模块隐藏
GIVEN 某一解释模块缺少必要字段
WHEN 结果页渲染
THEN 系统可隐藏该局部模块 [假设]
AND 保留总分、band 和可用解释

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F3-S05 — 解释版本管理

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S05`

**upstream_refs**:
- persona: P3
- journey_stage: J8
- scenarios: [BP-U4]
- kpi_alignment: [K8]

**User Story（英文）**:
> As a content operator, I want result explanations to carry mapping and template versions, so that changes can be audited and interpreted consistently.

**Acceptance Criteria**（中文）：

AC1：版本字段记录
GIVEN 系统生成评分结果解释
WHEN 结果写入或展示
THEN 系统记录 `mappingVersion` 和 `templateVersion`
AND 版本字段与当前解释内容一致

AC2：历史结果版本保持
GIVEN 用户查看历史评分结果
WHEN 当前解释模板已经更新
THEN 系统按历史结果保存的版本展示或标记原版本 [假设]
AND 不用新模板静默改写历史解释

AC3：版本缺失处理
GIVEN 系统生成结果解释
WHEN `mappingVersion` 或 `templateVersion` 缺失
THEN 系统记录异常事件
AND 使用通用解释回退
AND 进入 §9 P-OQ2 跟踪治理口径

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

### Feature F4 — Website Handoff CTA（来自 §2）

#### Story EPIC-speaking-challenge-and-scoring-F4-S01 — 结果页 CTA 渲染

**Story ID**：`EPIC-speaking-challenge-and-scoring-F4-S01`

**upstream_refs**:
- persona: P1
- journey_stage: J6
- scenarios: [BP-H1]
- kpi_alignment: [K3, K5]

**User Story（英文）**:
> As an IELTS learner, I want a relevant website entry after seeing my result, so that I can continue with deeper practice tools.

**Acceptance Criteria**（中文）：

AC1：CTA 展示条件
GIVEN 用户已查看成功评分结果
AND 系统已生成 `scoreBucket`
WHEN 结果页加载完成
THEN 系统展示 website CTA
AND CTA 与当前来源场景和 `scoreBucket` 匹配 [假设]

AC2：CTA 不可用条件
GIVEN 用户查看结果页
WHEN website 承接配置关闭或当前结果不可导流
THEN 系统不展示可点击 CTA
AND 不影响结果页主体信息展示

AC3：CTA 重复点击
GIVEN 用户已点击 website CTA
WHEN 用户在短时间内重复点击同一 CTA
THEN 系统不重复创建归因记录 [假设]
AND 保留第一次有效点击的 `attributionId`

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F4-S02 — 深链参数透传

**Story ID**：`EPIC-speaking-challenge-and-scoring-F4-S02`

**upstream_refs**:
- persona: P1
- journey_stage: J6
- scenarios: [BP-H1]
- kpi_alignment: [K3, K5, K9]

**User Story（英文）**:
> As a growth operator, I want the miniapp to pass only low-sensitivity handoff parameters, so that website attribution works without exposing raw scores.

**Acceptance Criteria**（中文）：

AC1：允许透传参数
GIVEN 用户点击 website CTA
WHEN 系统生成深链
THEN 深链只包含 `sourceScene`、`scoreBucket`、`attributionId` 和必要签名参数
AND 不包含原始录音地址、明文手机号或原始敏感分数

AC2：score bucket 取值
GIVEN 系统生成深链参数
WHEN 计算 `scoreBucket`
THEN `scoreBucket` 只能取 `low`、`mid`、`high` 或 `unknown` [假设]
AND 分层规则不在前端暴露完整评分公式

AC3：深链签名与时效
GIVEN 系统生成 website 深链
WHEN 用户跳转到 website
THEN website 可校验参数签名和有效期 [来自 Architecture §3.7]
AND 校验失败时进入默认承接页 [假设]

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F4-S03 — Website 默认承接降级

**Story ID**：`EPIC-speaking-challenge-and-scoring-F4-S03`

**upstream_refs**:
- persona: P1
- journey_stage: J6
- scenarios: [BP-U5]
- kpi_alignment: [K3, K5]

**User Story（英文）**:
> As an IELTS learner, I want the handoff to land safely even when parameters are missing, so that I do not see a dead end.

**Acceptance Criteria**（中文）：

AC1：缺参降级
GIVEN 用户点击 website CTA
WHEN 深链缺少 `sourceScene` 或 `scoreBucket`
THEN 系统进入 website 默认承接页 [假设]
AND 记录归因缺参事件
AND 不出现 404 或空白页

AC2：无匹配承接模块
GIVEN website 收到有效深链参数
WHEN website 无匹配承接模块
THEN website 展示默认 AI 工具集合承接页 [假设]
AND 保留 `attributionId` 追踪

AC3：导流失败恢复
GIVEN 用户点击 website CTA
WHEN 跳转失败或目标页不可访问
THEN 小程序展示跳转失败提示 [假设]
AND 用户可再次尝试或留在结果页

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

#### Story EPIC-speaking-challenge-and-scoring-F4-S04 — 导流归因埋点

**Story ID**：`EPIC-speaking-challenge-and-scoring-F4-S04`

**upstream_refs**:
- persona: P3
- journey_stage: J8
- scenarios: [BP-H1, BP-U5]
- kpi_alignment: [K3, K5]

**User Story（英文）**:
> As a growth operator, I want handoff attribution events, so that I can measure website click and conversion performance.

**Acceptance Criteria**（中文）：

AC1：结果页曝光埋点
GIVEN 用户进入评分结果页
WHEN 结果页成功展示
THEN 系统记录结果页曝光事件
AND 事件包含 `taskId`、`sourceScene` 和 `scoreBucket`
AND 不包含原始敏感分数

AC2：CTA 点击埋点
GIVEN 用户查看结果页 CTA
WHEN 用户点击 CTA
THEN 系统记录 CTA 点击事件
AND 生成或复用 `attributionId`
AND 事件可用于计算 K3

AC3：Website 到达与转化归因
GIVEN 用户通过 CTA 进入 website
WHEN website 成功接收 `attributionId`
THEN website 记录到达事件
AND 后续转化可按 `attributionId` 归因 [假设]

AC4：埋点失败
GIVEN 用户点击 CTA
WHEN 埋点接口短时失败
THEN 不阻塞用户跳转
AND 系统记录可重试或补偿的归因失败日志 [假设]

**变更记录**：
- v1.0 (2026-05-22)：基于新版 Solution §9 重建。

---

### Feature F5 — Scoring Trust and Guardrails（来自 §2）

#### Story EPIC-speaking-challenge-and-scoring-F5-S01 — 练习参考性质说明

**Story ID**：`EPIC-speaking-challenge-and-scoring-F5-S01`

**upstream_refs**:
- persona: P1
- journey_stage: J2, J5
- scenarios: [BP-H1, BP-U4]
- kpi_alignment: [K8]

**User Story（英文）**:
> As an IELTS learner, I want to know that the score is for practice reference, so that I do not misunderstand it as an official IELTS result.

**Acceptance Criteria**（中文）：

AC1：题目详情说明
GIVEN 用户进入题目详情页
WHEN 页面加载完成
THEN 系统展示“练习参考，非官方成绩”性质说明
AND 说明不阻止用户继续挑战

AC2：结果页说明
GIVEN 用户进入评分结果页
WHEN 结果页加载完成
THEN 系统展示“练习参考，非官方成绩”性质说明
AND 说明与分数、band/CEFR 解释同时可见 [假设]

AC3：说明缺失保护
GIVEN 系统准备展示题目详情或结果页
WHEN 合规说明配置缺失
THEN 系统使用默认说明文案 [假设]
AND 记录配置缺失事件

**变更记录**：
- v1.0 (2026-05-22)：新增 F5，承接新版 Solution。

---

#### Story EPIC-speaking-challenge-and-scoring-F5-S02 — 隐私授权提示

**Story ID**：`EPIC-speaking-challenge-and-scoring-F5-S02`

**upstream_refs**:
- persona: P1
- journey_stage: J3
- scenarios: [BP-H1]
- kpi_alignment: [K9]

**User Story（英文）**:
> As an IELTS learner, I want to understand how my voice recording will be used, so that I can decide whether to submit it.

**Acceptance Criteria**（中文）：

AC1：提交前授权说明
GIVEN 用户准备提交录音
WHEN 用户查看提交区域
THEN 系统展示录音用途、保存、评分和授权边界说明
AND 若训练复用范围未确认，则不得默认声明可用于训练

AC2：未确认授权时阻止提交
GIVEN 用户准备提交录音
AND 必要授权确认未完成 [假设]
WHEN 用户点击提交
THEN 系统阻止提交
AND 提示用户先确认授权说明

AC3：授权记录
GIVEN 用户确认授权并提交录音
WHEN 系统接收提交请求
THEN 系统记录授权确认时间、版本和用户标识引用
AND 授权记录可用于后续审计

AC4：授权文案待确认
GIVEN NFR-OQ3 仍为 open
WHEN PRD 进入评审
THEN 本 Story 标记为 Compliance 待确认
AND 进入 §9 P-OQ3 跟踪

**变更记录**：
- v1.0 (2026-05-22)：新增 F5，承接新版 Solution 与 NFR。

---

#### Story EPIC-speaking-challenge-and-scoring-F5-S03 — 结果可信度表达

**Story ID**：`EPIC-speaking-challenge-and-scoring-F5-S03`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [BP-H1, BP-U4]
- kpi_alignment: [K8]

**User Story（英文）**:
> As an IELTS learner, I want to understand the limitations of AI scoring, so that I can interpret the result appropriately.

**Acceptance Criteria**（中文）：

AC1：可信度说明展示
GIVEN 用户进入评分结果页
WHEN 结果页加载完成
THEN 系统展示 AI 评分限制和适用范围说明 [假设]
AND 说明不宣称结果等同官方成绩

AC2：置信度字段处理
GIVEN AI Scoring Engine 返回置信度或质量标记
WHEN 结果页展示可信度说明
THEN 系统将技术字段转译为用户可理解说明 [假设]
AND 不展示原始内部错误码

AC3：低置信度结果
GIVEN 评分结果被标记为低置信度 [假设]
WHEN 结果页展示
THEN 系统提示用户可重新作答或查看练习建议
AND 不隐藏基本结果，除非结果不可用

**变更记录**：
- v1.0 (2026-05-22)：新增 F5，承接新版 Solution。

---

#### Story EPIC-speaking-challenge-and-scoring-F5-S04 — 投诉与反馈入口

**Story ID**：`EPIC-speaking-challenge-and-scoring-F5-S04`

**upstream_refs**:
- persona: P1, P2
- journey_stage: J7
- scenarios: [BP-U3, BP-U4]
- kpi_alignment: [K8]

**User Story（英文）**:
> As an IELTS learner, I want to submit feedback when the score feels wrong, so that support can investigate issues.

**Acceptance Criteria**（中文）：

AC1：反馈入口展示
GIVEN 用户进入评分结果页
WHEN 结果页加载完成
THEN 系统展示反馈入口 [假设]
AND 入口关联当前 `taskId`

AC2：反馈提交字段
GIVEN 用户打开反馈入口
WHEN 用户填写反馈
THEN 系统至少收集 `feedbackReason`、`description` 和当前 `taskId`
AND 不要求用户重复填写系统已知任务信息

AC3：反馈提交成功
GIVEN 用户已填写有效反馈内容
WHEN 用户提交反馈
THEN 系统创建反馈记录
AND 展示提交成功提示 [假设]
AND 反馈记录可供 Ops / Support 查询

AC4：重复反馈
GIVEN 用户已针对同一 `taskId` 提交过反馈
WHEN 用户再次提交反馈
THEN 系统允许补充或限制重复提交 [待 PM 确认]
AND 进入 §9 P-OQ4 跟踪最终规则

**变更记录**：
- v1.0 (2026-05-22)：新增 F5，承接新版 Solution。

---

#### Story EPIC-speaking-challenge-and-scoring-F5-S05 — Guardrail 监控埋点

**Story ID**：`EPIC-speaking-challenge-and-scoring-F5-S05`

**upstream_refs**:
- persona: P2
- journey_stage: J7
- scenarios: [BP-H1, BP-U2, BP-U3, BP-U5]
- kpi_alignment: [K6, K7, K8, K9]

**User Story（英文）**:
> As a scoring operator, I want guardrail events to be recorded, so that I can monitor scoring success, latency, complaints, and privacy risks.

**Acceptance Criteria**（中文）：

AC1：评分成功率事件
GIVEN 用户完成录音提交并创建评分任务
WHEN 任务进入 `succeeded`、`failed` 或 `timeout` 终态
THEN 系统记录任务终态事件
AND 事件包含 `taskId`、`traceId`、`status` 和耗时
AND 事件可用于计算 K6

AC2：评分耗时事件
GIVEN 用户提交有效录音
WHEN 系统返回完整评分结果
THEN 系统记录从提交到结果展示的耗时
AND 事件可用于计算 K7 平均返回时长

AC3：投诉反馈事件
GIVEN 用户提交反馈或投诉
WHEN 反馈记录创建成功
THEN 系统记录反馈事件
AND 事件可用于计算 K8 投诉率

AC4：隐私与访问审计事件
GIVEN 内部用户访问录音、评分结果或反馈详情
WHEN 访问动作发生
THEN 系统记录审计事件
AND 事件包含 `actor`、`action`、`resourceType` 和 `traceId`
AND 事件可用于 K9 风险排查

**变更记录**：
- v1.0 (2026-05-22)：新增 F5，承接新版 Solution、NFR 与 Architecture。

## §4 Story-level Estimation

| Story ID | Size | Points | Units | Effort | Complexity | Confidence | Notes |
|---|:---:|---:|---:|---:|:---:|:---:|---|
| EPIC-speaking-challenge-and-scoring-F1-S01 | S | 2 | 1-2 | 0.5-1 day | Low | High | 首页入口读取与跳转。 |
| EPIC-speaking-challenge-and-scoring-F1-S02 | S | 2 | 1-2 | 0.5-1 day | Low | High | 列表展示。 |
| EPIC-speaking-challenge-and-scoring-F1-S03 | M | 3 | 2-4 | 1-2 days | Medium | High | 题目状态与开始挑战。 |
| EPIC-speaking-challenge-and-scoring-F1-S04 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 中断恢复规则待 UX 确认。 |
| EPIC-speaking-challenge-and-scoring-F1-S05 | S | 2 | 1-2 | 0.5-1 day | Low | Medium | 无题降级。 |
| EPIC-speaking-challenge-and-scoring-F2-S01 | M | 3 | 2-4 | 1-2 days | Medium | High | 麦克风权限。 |
| EPIC-speaking-challenge-and-scoring-F2-S02 | M | 3 | 2-4 | 1-2 days | Medium | High | 录音状态机。 |
| EPIC-speaking-challenge-and-scoring-F2-S03 | M | 3 | 2-4 | 1-2 days | Medium | High | 客户端 + 服务端校验。 |
| EPIC-speaking-challenge-and-scoring-F2-S04 | L | 5 | 4-8 | 2-4 days | High | Medium | 上传、对象存储、失败恢复。 |
| EPIC-speaking-challenge-and-scoring-F2-S05 | L | 5 | 4-8 | 2-4 days | High | Medium | 幂等任务创建。 |
| EPIC-speaking-challenge-and-scoring-F2-S06 | L | 5 | 4-8 | 2-4 days | High | Medium | 短轮询、状态机、结果聚合。 |
| EPIC-speaking-challenge-and-scoring-F2-S07 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 超时失败恢复。 |
| EPIC-speaking-challenge-and-scoring-F3-S01 | S | 2 | 1-2 | 0.5-1 day | Low | High | 分数展示。 |
| EPIC-speaking-challenge-and-scoring-F3-S02 | M | 3 | 2-4 | 1-2 days | Medium | Medium | band/CEFR 口径待确认。 |
| EPIC-speaking-challenge-and-scoring-F3-S03 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 建议模板治理。 |
| EPIC-speaking-challenge-and-scoring-F3-S04 | S | 2 | 1-2 | 0.5-1 day | Low | Medium | 模板缺失回退。 |
| EPIC-speaking-challenge-and-scoring-F3-S05 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 版本追踪。 |
| EPIC-speaking-challenge-and-scoring-F4-S01 | S | 2 | 1-2 | 0.5-1 day | Low | High | CTA 配置展示。 |
| EPIC-speaking-challenge-and-scoring-F4-S02 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 深链签名和低敏参数。 |
| EPIC-speaking-challenge-and-scoring-F4-S03 | S | 2 | 1-2 | 0.5-1 day | Low | Medium | 默认承接降级。 |
| EPIC-speaking-challenge-and-scoring-F4-S04 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 归因埋点。 |
| EPIC-speaking-challenge-and-scoring-F5-S01 | S | 2 | 1-2 | 0.5-1 day | Low | High | 参考性质说明。 |
| EPIC-speaking-challenge-and-scoring-F5-S02 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 授权文案与审计。 |
| EPIC-speaking-challenge-and-scoring-F5-S03 | S | 2 | 1-2 | 0.5-1 day | Low | Medium | 可信度表达。 |
| EPIC-speaking-challenge-and-scoring-F5-S04 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 反馈提交。 |
| EPIC-speaking-challenge-and-scoring-F5-S05 | M | 3 | 2-4 | 1-2 days | Medium | Medium | Guardrail 事件。 |

## §5 Engineering Notes

### §5.1 Architecture LATEST 引用

| 项 | 值 |
|---|---|
| 路径 | `Project/spk2challenge-miniprogram/Architecture/speaking-challenge-and-scoring/LATEST.md` |
| Wiki | `/spk2challenge-miniprogram/speaking-challenge-and-scoring-PRD/architecture` |
| ADR | `/spk2challenge-miniprogram/speaking-challenge-and-scoring-PRD/architecture/adr-{slug}` |
| 状态 | ✅ 已产出 |
| Last synced | 2026-05-22-1500 |
| Architect | @frankzhey |

### §5.2 Story 级 Architecture Trace

| Story Range | 涉及 Container | 涉及 ADR | 涉及 API / Data Flow | 涉及 EXP |
|---|---|---|---|---|
| F1-S01 ~ F1-S05 | Challenge BFF / Mini Program Client | — | `GET /api/challenges`, `GET /api/challenges/{challengeId}` | Solution §7 Q7 |
| F2-S01 ~ F2-S03 | Mini Program Client / Upload Gateway | ADR-002 | `POST /api/voice-submissions/upload-token`, `POST /api/voice-submissions/{submissionId}/complete` | Solution §7 Q2, Q6 |
| F2-S04 ~ F2-S07 | Upload Gateway / Scoring Job Service / AI Scoring Engine | ADR-001, ADR-002 | `POST /api/scoring-tasks`, `GET /api/scoring-tasks/{taskId}` | Solution §7 Q1, Q2, Q3, Q7 |
| F3-S01 ~ F3-S05 | Result Composition Service / Content Mapping Service | — | `GET /api/scoring-tasks/{taskId}/result` | Solution §7 Q5 |
| F4-S01 ~ F4-S04 | Handoff Attribution Service / Website | ADR-003 | `POST /api/handoffs`, Data Flow: result -> score bucket -> attribution | Solution §7 Q4 |
| F5-S01 ~ F5-S05 | Ops & Observability / Support / Result Composition | ADR-001, ADR-002, ADR-003 | feedback + audit_log + guardrail metrics | Solution §7 Q6, Q7 |

### §5.4 业务侧补充

- **涉及计算逻辑**：IELTS band 与 CEFR 对照、score bucket 分层、低置信度解释均为强业务口径，当前存在 V-OQ3 / IT-OQ4 / P-OQ1，需 PM + Eng 确认。
- **涉及数据同步**：小程序到 website 通过 `attributionId` 做导流归因；埋点失败不阻塞用户跳转，补偿策略待 Eng 细化。
- **涉及第三方 vendor**：AI Scoring Engine vendor / ownership 未确认，错误码到用户行为映射需在 Eng Review 前补齐。

## §6 NFR Reference

### §6.1 NFR LATEST 引用

| 项 | 值 |
|---|---|
| 路径（Epic 级优先） | `Project/spk2challenge-miniprogram/NFR/speaking-challenge-and-scoring/LATEST.md` |
| 回退（Project-wide） | `Project/spk2challenge-miniprogram/NFR/project-wide/LATEST.md` |
| Wiki | `/spk2challenge-miniprogram/project-wide-nfr` |
| 状态 | ✅ Project-wide 已产出；Epic 级未产出 |
| Last synced | 2026-05-22-1100 |

### §6.2 关键 NFR 摘要

| NFR | 档位 | 关键值 |
|---|:---:|---|
| Performance SLA | custom | API p95 <= 500ms / 异步评分平均 <=20s / 最大 <=30s / 上传成功到任务创建 <=2s p95 |
| Availability | 中 | 99.9%；评分结果成功返回率 >=97% |
| Capacity | 中 | DAU 1k-10k / MAU 10k-100k / 峰值 100-1k QPS / 单文件 <=10MB / 年增 <=500GB |
| Data Sensitivity | 中 | 录音、学习记录、用户标识、手机号等 PII/教育数据 |
| Compliance | 中 | 等保二级 + 教育部备案；数据不出境 |
| Retention | 中 | 3 年，1 年热 + 2 年冷；临时上传缓存 <=7 天 |
| Geo | 低 | 仅大陆，数据驻留大陆境内 |

## §7 Capacity Summary

Total Features: 5  
Total Stories: 26  
Total Estimated Units: 58-108 units  
Total Estimated Effort: 29-54 days  
Suggested Sprint Count: 4-6  
Suggested Team Count: 小程序 FE 1-2 + BE 2 + AI/Scoring 1 + QA 1 + Growth/Content part-time  
Main Complexity Drivers: 异步评分、音频上传、幂等任务、短轮询超时、band/CEFR 映射、低敏深链、隐私授权与 guardrail 监控。  
Main Assumptions: 复用现有小程序登录态、对象存储、基础监控和 website 承接框架。

Capacity 对比校验:

| 项 | 值 |
|---|---:|
| Solution Brief Phase-level | 60-120 units |
| Product Planner Story-level | 58-108 units |
| 偏差 | -3.3% 至 -10.0% |
| 结论 | 偏差 <= 30%，合理范围 |

## §8 Estimation Disclaimer

以上估算属于 PRD 阶段 planning-level estimation；仅用于范围判断、资源预估和优先级决策；不代表研发最终承诺；最终单位估算和任务拆分以 Eng Reviewer / Task Planner refinement 为准。

## §9 Open Questions

来自 Value Frame（status=open 条目）:

| OQ ID | Question | Status | Owner |
|---|---|---|---|
| V-OQ1 | 小程序首期挑战机制最小版本是什么：单题挑战、每日挑战、连续打卡，还是榜单竞赛 | open | PM |
| V-OQ2 | 评分结果是否直接展示完整 IELTS band descriptor 解释，还是先展示简化版结论再展开详情 | open | PM |
| V-OQ3 | IELTS band 与 CEFR 对照表采用固定映射还是内部解释版映射 | open | PM + Eng |
| V-OQ4 | website 承接页的首期目标是题库浏览、AI 工具试用，还是直接会员/产品购买转化 | open | PM |
| V-OQ5 | 语音数据的保存周期、授权提示和可复用范围如何定义 | open | PM + Eng |
| V-OQ6 | 小程序评分返回的目标时延能否稳定控制在 20 秒内 | open | Eng |
| V-OQ7 | 首期是否只覆盖指定简化题库，还是同时支持自由题目扩展 | open | PM |

来自 Solution Brief（status=open 条目）:

| OQ ID | Question | Status | Owner |
|---|---|---|---|
| S-OQ1 | 评分返回前是否允许用户离开页面并稍后查看结果 | open | PM + Eng |
| S-OQ2 | 音频存储是否需要做脱敏、分级保留或训练用途隔离 | open | Eng + Compliance |
| S-OQ3 | 小程序到 website 是否需要账号打通后再跳转，还是允许匿名承接 | open | PM + Eng |
| S-OQ4 | 承接页首版是否允许直接出现价格和购买 CTA，还是先用工具试用承接 | open | PM + Growth |
| S-OQ5 | 本 Epic 合计 60-120 units，是否在第一次研发评审拆分为里程碑迭代 | open | Eng + PM |
| S-OQ6 | 评分可信度说明是否需要 Compliance 预审固定模板 | open | Compliance + PM |
| S-OQ7 | 评分通道与小程序的技术栈 / 团队归属 / 时间窗约束 | open | PM + Eng |

本 PRD 新增:

| OQ ID | Question | Status | Owner |
|---|---|---|---|
| P-OQ1 | IELTS band 与 CEFR 映射口径、边界分数解释、`mappingVersion` 治理是否采用固定映射或内部解释版映射 | open | PM + Eng |
| P-OQ2 | 结果解释模板版本管理是否需要内容审核流，以及历史结果是否按旧模板永久展示 | open | PM + Content |
| P-OQ3 | 录音授权文案是否允许训练复用；撤回授权和删除请求如何处理 | open | Compliance + PM + AI |
| P-OQ4 | 同一 `taskId` 是否允许用户重复提交反馈，还是只能补充一次 | open | PM + Support |
| P-OQ5 | AI Scoring Engine 错误码到用户提示和重试行为的映射表由谁维护 | open | AI + Eng + PM |

## §10 Future Extension

- E2 可在 F1 / F2 之上增加每日挑战、连续打卡、提醒和复练任务，但不进入本 PRD。
- E3 可基于 F4 的 `scoreBucket` 和 `attributionId` 建设更细 website 承接页、工具试用和付费转化路径。
- Future E6 可把当前匿名或小程序登录态作答升级为跨端统一学习账号。

## §X Coverage Matrix

### §X.1 三向 Trace（v4.8）

| Source Path | NFR Tier | Architecture Ref | Type | Feature | Story | AC | Notes |
|---|---|---|---|---|---|---|---|
| BP-H1 端到端主流程 | PERF-custom / AVAIL-T2 | Challenge BFF / Upload Gateway / Scoring Job Service / Result Composition / ADR-001 / ADR-002 / ADR-003 | solution_risk | F1-F4 | F1-S01, F1-S03, F2-S04, F2-S05, F2-S06, F3-S01, F4-S01 | 多处 AC | 100% 主流程覆盖 |
| BP-U1 麦克风权限被拒绝 | DATA-T2 / COMPL-T2 | Mini Program Client | solution_risk | F2 | F2-S01 | AC1-AC4 | 权限与恢复 |
| BP-U2 无效录音被拦截 | PERF-custom / CAP-T2 | Upload Gateway / ADR-002 | solution_risk | F2 | F2-S03, F2-S04 | AC1-AC4 | 不进入评分链路 |
| BP-U3 评分超时或失败 | PERF-custom / AVAIL-T2 | Scoring Job Service / AI Scoring Engine / ADR-001 | solution_risk | F2 | F2-S06, F2-S07 | AC1-AC4 | 2s poll / 30s timeout |
| BP-U4 映射口径或解释模板缺失 | COMPL-T2 | Result Composition / Content Mapping Service | solution_risk | F3 | F3-S02, F3-S04, F3-S05 | AC1-AC4 | 映射回退与版本治理 |
| BP-U5 导流参数缺失或 website 无承接页 | DATA-T2 / COMPL-T2 | Handoff Attribution Service / ADR-003 | solution_risk | F4 | F4-S02, F4-S03, F4-S04 | AC1-AC4 | 不传原始分数 |
| BP-E1 同一题目重复提交 | PERF-custom / AVAIL-T2 | Scoring Job Service / ADR-002 | solution_risk | F2 | F2-S05, F2-S06, F2-S07 | AC1-AC4 | 幂等复用 taskId |
| privacy authorization | DATA-T2 / COMPL-T2 / RETN-T2 | Security Architecture / audit_log | prd_extension | F5 | F5-S02, F5-S05 | AC1-AC4 | 录音授权与审计 |
| complaint feedback | AVAIL-T2 | feedback_ticket / Ops & Observability | prd_extension | F5 | F5-S04, F5-S05 | AC1-AC4 | K8 投诉率 |

### §X.2 8 类场景维度覆盖率（按 Feature）

| Feature | happy | unhappy | failure | edge | permission | state | retry | empty-expired-duplicate | 覆盖率 |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| F1 | ✅ | ✅ | ✅ | ✅ | ⭕ | ✅ | ⭕ | ✅ | 6/8 |
| F2 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 8/8 |
| F3 | ✅ | ✅ | ✅ | ✅ | ⭕ | ✅ | ⭕ | ✅ | 6/8 |
| F4 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 8/8 |
| F5 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕ | ✅ | 7/8 |

### §X.3 Quality Gate

- [x] Solution §5 每条 BP-X 都在 §X.1 中找到，type=solution_risk。
- [x] NFR Tier 列引用 project-wide NFR：PERF-custom / AVAIL-T2 / CAP-T2 / DATA-T2 / COMPL-T2 / RETN-T2。
- [x] Architecture Ref 列引用 Architecture LATEST 的 Container / ADR。
- [x] 每个 Feature 8 类场景维度覆盖率 >= 6。
- [x] 未覆盖维度已作为风险接受或后续 Story 入口记录。

## §11 已沉淀规则索引

本次推动 Project Rules 新增 / 更新：

- Layer 2：短轮询默认每 2 秒查询一次，最长查询窗口 30 秒；超过后进入超时恢复路径。
- Layer 2：评分任务状态至少覆盖 `created` / `queued` / `processing` / `succeeded` / `failed` / `timeout` / `cancelled`。
- Layer 2：website handoff 只允许透传 `sourceScene` / `scoreBucket` / `attributionId` / 签名参数。
- Layer 3：score bucket 仅允许 `low` / `mid` / `high` / `unknown`，不得透传原始敏感分数。
- Layer 3：评分结果解释必须记录 `mappingVersion` 和 `templateVersion`。

## §12 PRD-level Changelog

- v1.0 (2026-05-22-1800)：基于 Solution v2026-05-22-1028、project-wide NFR v2026-05-22-1100、Architecture v2026-05-22-1500 生成新版 PRD；接管 Solution §9 Story List，新增 F5 Scoring Trust and Guardrails，升级 §5 Architecture Reference、§6 NFR Reference、§X 三向 Coverage Matrix。
