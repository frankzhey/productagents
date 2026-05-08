---
project: spk2challenge-miniprogram
epic_id: EPIC-speaking-challenge-and-scoring
epic_name: Speaking Challenge and Scoring
epic_source: B
created: 2026-05-08-0400
maintainer: "@frankzhey"
upstream_snapshot:
  value: Project/spk2challenge-miniprogram/Value/value-architect-2026-05-08-0000.md
  solution: Project/spk2challenge-miniprogram/Solution/speaking-challenge-and-scoring/speaking-challenge-and-scoring-solution-brief-2026-05-08-0200.md
  magic_patterns_editor: N/A
status: draft
skills_loaded:
  - skills/ac-writing-spec/SKILL.md
---

> **上游引用**
> - Value Frame: Project/spk2challenge-miniprogram/Value/LATEST.md
> - Solution Brief: Project/spk2challenge-miniprogram/Solution/speaking-challenge-and-scoring/LATEST.md
> - 本 PRD 仅产出 Epic-Feature-Story 三级骨架与 AC，不重复战略层 / Journey / Process / GWT Top 等上游内容

## §1 Epic Definition

| 字段 | 内容 |
|---|---|
| **Epic ID** | `EPIC-speaking-challenge-and-scoring` |
| **Epic Name** | Speaking Challenge and Scoring |
| **Source** | B 来自 Solution Brief |
| **KPI 对齐** | K1, K2, K3, K6, K7, K8, K9 |
| **关联 Phase** | MVP |

**Context**：
本 Epic 是小程序口语挑战 MVP 的首个端到端价值闭环，覆盖“进入挑战、录音提交、AI 评分、IELTS band/CEFR 解释、结果页导流到 website”的完整路径。对用户而言，只有当评分结果可返回、可理解且能给出下一步入口时，价值才算闭环；对业务而言，该闭环同时验证小程序参与、评分稳定性、结果理解度和导流点击四类核心指标。

**Scope In**：
- 首页挑战入口、挑战列表与题目详情
- 麦克风权限、录音交互、无效录音拦截
- 音频上传、评分任务创建、短轮询查询、超时失败兜底
- 结果页总分、IELTS band、CEFR 对照、分维度解读与解释性质说明
- website 导流 CTA、参数透传与归因事件记录

**Scope Out**：
- 连续挑战、任务留存和回访提醒
- website 内部题库编排、AI 工具深度体验与付费承接页面细节
- 多科测评入口、跨端统一账号与个性化学习路径
- 长期运营平台和完整数据中台能力

## §2 Feature List

| Feature ID | Feature Name | Description | Value | Source |
|---|---|---|---|---|
| F1 | Challenge Participation Flow | 在小程序首页和挑战列表提供轻量挑战入口、题目说明、作答前准备和中断恢复，让用户能在数秒内进入录音作答前一步。 | 降低首次参与门槛，提升挑战启动率与首次作答完成率。 | Solution §2 |
| F2 | Voice Scoring Pipeline | 提供录音权限、录音控件、提交校验、评分任务编排、结果聚合和失败兜底，确保评分能在目标时限内返回结构化结果。 | 直接支撑 K1 / K2 / K6 / K7，是用户感知 AI 测评的核心环节。 | Solution §2 |
| F3 | Band-and-CEFR Result Card | 将评分结果转译成 IELTS band、CEFR 对照、分维度解读和基础下一步建议，让用户能理解结果并知道接下来该做什么。 | 提升结果可理解性与信任度，支撑后续复练和导流。 | Solution §2 |
| F4 | Website Handoff CTA | 在结果页按用户分数和场景展示合适的 website 入口，透传来源与分层参数，让用户从轻量评分自然过渡到深度工具。 | 提升 K3 导流点击率，为 K5 付费转化提供高意向流量。 | Solution §2 |

## §3 User Stories and AC

### Feature F1 — Challenge Participation Flow（来自 §2）

#### Story EPIC-speaking-challenge-and-scoring-F1-S01 — 首页挑战卡片曝光

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S01`

**upstream_refs**:
- persona: P1
- journey_stage: J1
- scenarios: [S1]
- kpi_alignment: [K1, K2]

**User Story（英文）**:
> As an IELTS learner, I want to see a clear speaking challenge entry on the miniapp home page, so that I can quickly know where to start a speaking attempt.

**Acceptance Criteria**：

AC1：首页展示挑战入口
GIVEN 用户进入小程序首页
AND 当前存在至少一条已发布且可参与的口语挑战
WHEN 首页完成数据加载
THEN 系统在首页展示挑战卡片
AND 挑战卡片展示题目标题、挑战状态和主操作入口
AND 点击主操作入口后进入该题目的详情页

AC2：无权限或无可参与挑战时不展示误导入口
GIVEN 用户进入小程序首页
AND 当前无可参与的口语挑战或当前渠道不允许展示挑战能力
WHEN 首页完成数据加载
THEN 系统不展示可点击的挑战开始入口
AND 展示替代提示文案或备用入口 [假设]
AND 不出现点击后空白或报错页面

AC3：挑战入口幂等曝光
GIVEN 用户已在同一会话中多次返回首页
WHEN 首页再次加载挑战卡片区域
THEN 系统按最新可参与挑战数据刷新展示内容
AND 不因重复进入而生成重复参与记录

AC4：首页挑战入口加载失败时的兜底
GIVEN 用户进入小程序首页
WHEN 挑战入口数据接口返回系统错误或超时
THEN 系统展示通用失败提示“系统繁忙，请稍后重试” [假设]
AND 首页其他区域仍可继续访问
AND 用户可通过刷新首页再次尝试加载挑战入口

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F1-S02 — 挑战列表浏览

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S02`

**upstream_refs**:
- persona: P1
- journey_stage: J1
- scenarios: [S1]
- kpi_alignment: [K1]

**User Story（英文）**:
> As an IELTS learner, I want to browse available speaking challenges, so that I can choose a prompt that I want to attempt.

> AC 降级覆盖（≥3 条）
> 降级理由：本 Story 仅查询展示，无状态变更 / 无三方调用 / 无敏感字段

**Acceptance Criteria**：

AC1：默认加载挑战列表
GIVEN 用户进入挑战列表页
WHEN 页面首次加载完成
THEN 系统默认展示当前可参与的挑战列表
AND 默认排序为发布时间倒序 [假设]
AND 每条列表项至少展示题目标题、难度标签和参与入口

AC2：字段展示规范
GIVEN 用户查看挑战列表
WHEN 系统返回挑战数据
THEN 列表按字段规范展示 `title`、`difficultyLabel`、`publishTime` 和 `entryStatus`
AND 当某字段缺失时展示 `—`
AND 不展示内部配置字段或技术状态值

AC3：空状态展示
GIVEN 用户进入挑战列表页
WHEN 当前无可参与挑战数据
THEN 页面展示“No challenge available now”提示 [假设]
AND 保留返回首页或查看历史题的路径
AND 页面不显示空白列表容器

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F1-S03 — 题目详情与开始挑战

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S03`

**upstream_refs**:
- persona: P1
- journey_stage: J2
- scenarios: [S1]
- kpi_alignment: [K1, K2]

**User Story（英文）**:
> As an IELTS learner, I want to read the prompt details and explicitly start the challenge, so that I know what I should say before I record.

**Acceptance Criteria**：

AC1：题目详情展示
GIVEN 用户从首页或挑战列表进入题目详情页
WHEN 题目详情加载成功
THEN 系统展示题目正文、作答说明、建议时长和开始挑战按钮
AND 题目内容与首页入口指向的题目保持一致

AC2：开始挑战的前置条件
GIVEN 用户位于题目详情页
AND 当前题目状态为可参与
WHEN 用户查看开始挑战按钮
THEN 按钮处于可点击状态
AND 点击后进入录音作答页
AND 进入录音页时携带当前题目唯一标识 `promptId`

AC3：题目不可参与时阻止开始
GIVEN 用户位于题目详情页
AND 当前题目状态为下线、过期或不可参与
WHEN 用户点击开始挑战按钮
THEN 系统阻止进入录音页
AND 展示当前题目不可参与的提示文案 [假设]
AND 引导用户返回挑战列表或选择其他题目

AC4：重复点击开始按钮的幂等处理
GIVEN 用户在题目详情页连续多次点击开始挑战按钮
WHEN 第一次点击已触发页面跳转
THEN 系统仅执行一次跳转
AND 不创建重复的挑战会话记录

AC5：详情加载失败时的恢复
GIVEN 用户从挑战入口进入题目详情页
WHEN 题目详情接口返回系统错误或超时
THEN 页面展示失败提示与重试按钮
AND 用户点击重试后重新请求当前题目详情
AND 页面保留返回上一页的路径

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F1-S04 — 中断恢复

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S04`

**upstream_refs**:
- persona: P1
- journey_stage: J2
- scenarios: [S1]
- kpi_alignment: [K2]

**User Story（英文）**:
> As an IELTS learner, I want the miniapp to remember the challenge I was about to do, so that I can continue without reselecting it after an interruption.

**Acceptance Criteria**：

AC1：保留最近一次待开始题目
GIVEN 用户已从题目详情点击开始挑战
AND 录音页尚未成功提交有效录音
WHEN 用户因切后台、误返回或页面重载中断当前流程
THEN 系统保留最近一次待完成的 `promptId`
AND 用户再次进入挑战入口时可恢复到该题目的详情或录音页 [假设]

AC2：恢复入口的展示条件
GIVEN 用户再次进入首页或挑战列表
AND 系统存在未完成的待恢复挑战
WHEN 页面加载完成
THEN 系统展示继续挑战入口
AND 继续挑战入口指向上次中断的题目
AND 当待恢复记录已失效时不展示该入口

AC3：无待恢复记录时不展示恢复入口
GIVEN 用户再次进入挑战入口
WHEN 系统不存在有效的待恢复挑战记录
THEN 系统不展示继续挑战入口
AND 用户按正常流程重新选择挑战题

AC4：恢复记录异常时的失败处理
GIVEN 用户点击继续挑战入口
WHEN 恢复记录对应题目已下线或恢复数据损坏
THEN 系统提示当前挑战无法继续
AND 清除无效恢复记录
AND 引导用户返回挑战列表重新选择题目

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F1-S05 — 题目缺失降级

**Story ID**：`EPIC-speaking-challenge-and-scoring-F1-S05`

**upstream_refs**:
- persona: P1
- journey_stage: J1
- scenarios: [S1]
- kpi_alignment: [K1]

**User Story（英文）**:
> As an IELTS learner, I want the miniapp to show a safe fallback when no fresh challenge is available, so that I do not hit a dead end.

**Acceptance Criteria**：

AC1：无新题时的降级策略
GIVEN 用户进入首页或挑战列表
WHEN 当前无新发布且可参与的挑战题
THEN 系统进入降级模式
AND 展示历史题入口或备用练习入口 [假设]
AND 页面说明当前无新题但仍可继续体验

AC2：降级入口的操作范围
GIVEN 系统进入降级模式
WHEN 用户点击备用入口
THEN 系统仅跳转到允许展示的备用题或历史题
AND 不展示未发布、已下线或无评分支持的题目

AC3：降级模式的幂等与恢复
GIVEN 系统已处于降级模式
WHEN 后台补充了新的可参与挑战题
THEN 系统在下次页面刷新后恢复正常挑战入口展示
AND 不保留过期的降级提示状态

AC4：降级数据加载失败
GIVEN 系统需加载备用题或历史题数据
WHEN 备用数据接口失败
THEN 系统展示通用失败提示
AND 保留返回首页的路径
AND 不出现点击后空白页

**变更记录**：
- v1.0 (2026-05-08)：初版

---

### Feature F2 — Voice Scoring Pipeline（来自 §2）

#### Story EPIC-speaking-challenge-and-scoring-F2-S01 — 麦克风权限申请

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S01`

**upstream_refs**:
- persona: P1
- journey_stage: J3
- scenarios: [S2]
- kpi_alignment: [K2, K9]

**User Story（英文）**:
> As an IELTS learner, I want the miniapp to request microphone access with clear guidance, so that I understand why permission is needed before I record.

**Acceptance Criteria**：

AC1：首次进入录音页发起权限申请
GIVEN 用户首次进入录音作答页
AND 当前设备尚未授予麦克风权限
WHEN 页面初始化完成
THEN 系统发起麦克风权限申请
AND 同时展示权限用途说明

AC2：授权成功后允许进入录音态
GIVEN 用户位于录音作答页
AND 系统已发起麦克风权限申请
WHEN 用户同意授权
THEN 系统进入可录音状态
AND 录音按钮处于可点击状态

AC3：拒绝授权时的失败路径
GIVEN 用户位于录音作答页
AND 系统已发起麦克风权限申请
WHEN 用户拒绝授权
THEN 系统展示权限说明和重新授权入口
AND 禁止用户继续录音提交
AND 页面保留返回题目详情的路径

AC4：重复进入录音页的幂等处理
GIVEN 用户已处理过麦克风权限弹窗
WHEN 用户再次进入同一录音作答页
THEN 系统根据当前授权状态决定是否直接进入可录音态或展示重新授权引导
AND 不重复弹出无意义的权限说明层 [假设]

AC5：权限接口异常处理
GIVEN 用户进入录音作答页
WHEN 客户端权限查询失败
THEN 系统展示“无法获取麦克风权限状态，请稍后重试”提示 [假设]
AND 录音按钮保持不可点击
AND 用户可重新进入页面后再次尝试

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S02 — 录音控件

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S02`

**upstream_refs**:
- persona: P1
- journey_stage: J3
- scenarios: [S1, S2]
- kpi_alignment: [K2, K6]

**User Story（英文）**:
> As an IELTS learner, I want to start, stop, and rerecord my answer, so that I can control the recording before submission.

**Acceptance Criteria**：

AC1：录音控件的可用状态
GIVEN 用户已授予麦克风权限
WHEN 用户进入录音作答页
THEN 系统展示开始录音、停止录音和重录相关控件
AND 未开始录音前仅开始录音按钮可点击

AC2：开始与停止录音
GIVEN 用户位于录音作答页
AND 当前处于可录音状态
WHEN 用户点击开始录音按钮
THEN 系统进入录音中状态
AND 停止录音按钮变为可点击
AND 在录音结束前提交按钮保持不可点击

AC3：停止录音后生成待提交音频
GIVEN 用户正在录音
WHEN 用户点击停止录音按钮
THEN 系统结束当前录音
AND 生成一条待提交的音频数据
AND 若录音数据有效则提交按钮变为可点击

AC4：重录行为
GIVEN 用户已生成一条待提交音频
WHEN 用户点击重录按钮
THEN 系统清除当前待提交音频
AND 页面回到可开始录音状态
AND 不保留上一次待提交音频用于提交

AC5：录音控件异常处理
GIVEN 用户已进入录音作答页
WHEN 客户端录音组件初始化失败或录音中断
THEN 系统展示通用失败提示
AND 当前录音作废
AND 用户可重新点击开始录音再次尝试

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S03 — 无效录音校验

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S03`

**upstream_refs**:
- persona: P1
- journey_stage: J3
- scenarios: [S1]
- kpi_alignment: [K2, K6]

**User Story（英文）**:
> As an IELTS learner, I want the miniapp to block empty or invalid recordings, so that I do not submit unusable audio by mistake.

**Acceptance Criteria**：

AC1：无效录音时按钮不可点击
GIVEN 用户位于录音作答页
AND 当前不存在有效待提交音频
WHEN 用户查看提交按钮
THEN 按钮处于不可点击状态
AND 页面提示需先完成有效录音 [假设]

AC2：过短或损坏录音校验
GIVEN 用户已结束一段录音
WHEN 系统检测该录音为空、时长低于最小门槛或文件损坏
THEN 系统判定该录音为无效
AND 展示对应提示文案 [假设]
AND 不允许创建评分任务

AC3：有效录音后允许提交
GIVEN 用户已生成通过本地校验的录音
WHEN 用户查看提交按钮
THEN 按钮处于可点击状态
AND 用户可进入音频上传流程

AC4：重复点击提交的幂等处理
GIVEN 用户已点击提交按钮并触发上传流程
WHEN 用户在同一待提交音频上再次点击提交
THEN 系统不创建重复上传请求
AND 按钮进入处理中态或保持不可点击

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S04 — 音频上传与对象存储落点

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S04`

**upstream_refs**:
- persona: P1
- journey_stage: J3
- scenarios: [S1, S3]
- kpi_alignment: [K2, K6, K9]

**User Story（英文）**:
> As an IELTS learner, I want my validated recording to be uploaded safely, so that the system can use it for scoring without losing my attempt.

**Acceptance Criteria**：

AC1：上传触发范围
GIVEN 用户已完成一段通过本地校验的录音
AND 当前题目 `promptId` 有效
WHEN 用户点击提交按钮
THEN 系统上传当前音频文件
AND 将音频落到对象存储或等效持久化介质
AND 上传请求关联当前 `promptId` 和用户会话标识

AC2：上传中的页面状态
GIVEN 用户已触发音频上传
WHEN 上传尚未完成
THEN 页面展示处理中状态
AND 提交按钮保持不可点击
AND 用户不可再次发起同一录音的重复上传

AC3：上传成功后的结果
GIVEN 用户已触发音频上传
WHEN 上传成功
THEN 系统返回可用于后续评分的音频引用标识 `audioRef`
AND 页面进入评分任务创建前的处理中态

AC4：上传失败时的恢复
GIVEN 用户已触发音频上传
WHEN 上传接口超时、系统错误或网络失败
THEN 系统展示“上传失败，请重试”提示 [假设]
AND 当前评分任务不创建
AND 用户可基于同一条待提交录音重新发起上传

AC5：取消或离页的处理
GIVEN 用户已进入上传中状态
WHEN 用户关闭页面、切后台或网络中断
THEN 系统不展示已成功提交的结果页
AND 该次上传结果以服务端最终状态为准
AND 用户重新进入时可根据最终状态决定是否重试 [假设]

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S05 — 评分任务创建与处理中态

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S05`

**upstream_refs**:
- persona: P1
- journey_stage: J4
- scenarios: [S1, S3]
- kpi_alignment: [K1, K6, K7]

**User Story（英文）**:
> As an IELTS learner, I want the system to create a scoring task and show me that scoring is in progress, so that I know my answer is being processed.

**Acceptance Criteria**：

AC1：上传成功后创建评分任务
GIVEN 系统已拿到有效的 `audioRef`
AND 当前题目 `promptId` 和用户会话标识有效
WHEN 系统调用评分任务创建接口
THEN 系统创建一条评分任务记录
AND 返回唯一的 `taskId`
AND 页面进入评分处理中状态

AC2：处理中态展示
GIVEN 系统已成功创建评分任务
WHEN 用户停留在结果等待页面
THEN 页面展示评分处理中状态
AND 告知结果正在生成中
AND 页面不提前展示未完成的分数或 band 结果

AC3：重复创建任务的幂等处理
GIVEN 某条 `audioRef` 已成功创建过 `taskId`
WHEN 客户端因重试或网络抖动再次发起任务创建请求
THEN 系统不创建新的评分任务
AND 返回已有 `taskId` 或等效幂等结果

AC4：任务创建失败时的异常处理
GIVEN 系统已拿到有效的 `audioRef`
WHEN 评分任务创建接口返回系统错误或超时
THEN 页面展示评分任务创建失败提示
AND 不进入结果页展示态
AND 用户可重新尝试提交或刷新后重试 [假设]

AC5：业务规则不满足时的异常处理
GIVEN 系统已拿到有效的 `audioRef`
WHEN 后端判定该录音不满足评分业务规则
THEN 系统阻止创建评分任务
AND 展示明确的业务提示文案 [假设]
AND 用户可返回录音页重新作答

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S06 — 短轮询状态查询与结果聚合

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S06`

**upstream_refs**:
- persona: P1
- journey_stage: J4
- scenarios: [S1, S3]
- kpi_alignment: [K1, K6, K7]

**User Story（英文）**:
> As an IELTS learner, I want the miniapp to check scoring progress and load the final result automatically, so that I do not need to manually search for my score.

**Acceptance Criteria**：

AC1：短轮询查询评分状态
GIVEN 页面已拿到有效的 `taskId`
AND 当前评分任务仍未结束
WHEN 页面进入评分处理中态
THEN 系统按预设节奏发起短轮询查询 [假设]
AND 查询结果至少区分 `processing`、`success`、`failed` 三类状态

AC2：成功状态下聚合结果
GIVEN 页面已拿到有效的 `taskId`
WHEN 评分状态查询返回 `success`
THEN 系统拉取或接收结构化评分结果
AND 将总分、分维度结果与解释所需字段聚合为结果页可展示数据模型
AND 页面自动跳转到结果展示态

AC3：处理中状态的继续等待
GIVEN 页面已进入短轮询流程
WHEN 查询结果持续返回 `processing`
THEN 页面维持处理中展示
AND 不重复创建评分任务
AND 在达到超时阈值前不提示失败

AC4：查询幂等与重复进入处理
GIVEN 用户在同一 `taskId` 下重复打开等待页或刷新页面
WHEN 页面重新初始化
THEN 系统继续查询同一 `taskId` 的状态
AND 不创建新的评分任务
AND 最终仅以该 `taskId` 的终态结果为准

AC5：查询接口异常处理
GIVEN 页面已进入短轮询流程
WHEN 状态查询接口返回系统错误、网络失败或短时超时
THEN 系统允许在阈值内继续查询
AND 不直接丢弃当前评分任务
AND 当异常达到阈值后转入超时或失败兜底路径

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F2-S07 — 超时与失败兜底

**Story ID**：`EPIC-speaking-challenge-and-scoring-F2-S07`

**upstream_refs**:
- persona: P1
- journey_stage: J4
- scenarios: [S3]
- kpi_alignment: [K6, K7, K8]

**User Story（英文）**:
> As an IELTS learner, I want a clear fallback when scoring times out or fails, so that I know what happened and what I can do next.

**Acceptance Criteria**：

AC1：评分超时兜底
GIVEN 页面已对某个 `taskId` 执行状态查询
WHEN 评分结果在预设超时阈值内仍未返回成功终态
THEN 系统展示“结果生成较慢”状态
AND 提供继续等待、刷新结果或重新提交三种动作
AND 不让用户误以为本次作答已丢失

AC2：评分失败兜底
GIVEN 页面已对某个 `taskId` 执行状态查询
WHEN 评分状态返回 `failed`
THEN 系统展示失败态和对应提示文案 [假设]
AND 不展示不完整的分数或解释信息
AND 用户可返回录音页重新作答或重新提交

AC3：继续等待与刷新结果
GIVEN 用户位于超时兜底状态页
WHEN 用户点击继续等待或刷新结果
THEN 系统继续查询同一 `taskId` 的状态
AND 不创建新的评分任务

AC4：重新提交的限制
GIVEN 用户位于超时或失败状态页
WHEN 用户点击重新提交
THEN 系统允许用户基于新录音或重新上传重新发起流程
AND 旧 `taskId` 不再继续驱动当前结果页
AND 页面清晰区分新旧尝试 [假设]

AC5：系统级异常分类处理
GIVEN 用户已进入超时或失败兜底流程
WHEN 失败原因分别属于系统错误、业务规则不满足或频次限制
THEN 系统按不同原因展示不同提示 [假设]
AND 对系统错误提供重试路径
AND 对业务规则或频次限制提供明确说明与后续引导

**变更记录**：
- v1.0 (2026-05-08)：初版

---

### Feature F3 — Band-and-CEFR Result Card（来自 §2）

#### Story EPIC-speaking-challenge-and-scoring-F3-S01 — 总分与分维度展示

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S01`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [S1]
- kpi_alignment: [K1, K8]

**User Story（英文）**:
> As an IELTS learner, I want to see my total score and dimension scores clearly, so that I can understand the outcome of my attempt.

> AC 降级覆盖（≥3 条）
> 降级理由：本 Story 仅查询展示，无状态变更 / 无三方调用 / 无敏感字段

**Acceptance Criteria**：

AC1：结果页默认展示分数卡片
GIVEN 用户已拿到一条成功的结构化评分结果
WHEN 结果页完成加载
THEN 页面展示总分和分维度分数卡片
AND 分数展示使用统一格式和顺序 [假设]

AC2：字段展示规范
GIVEN 用户查看结果页分数卡片
WHEN 系统返回评分结果字段
THEN 页面按规范展示 `overallScore` 和各维度分数
AND 缺失字段展示 `—`
AND 不展示内部评分原始字段

AC3：空状态与异常保护
GIVEN 用户进入结果页
WHEN 结果数据缺失或结构不完整
THEN 页面不展示错误分数
AND 展示通用失败提示或引导刷新结果
AND 不出现空白结果卡片

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F3-S02 — IELTS band 与 CEFR 对照

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S02`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [S1, S4]
- kpi_alignment: [K1, K8]

**User Story（英文）**:
> As an IELTS learner, I want to see my IELTS band and CEFR reference together, so that I can map the result to a level I recognize.

**Acceptance Criteria**：

AC1：band 与 CEFR 对照展示
GIVEN 用户已拿到成功的结构化评分结果
AND 当前结果存在可映射的等级区间
WHEN 结果页完成加载
THEN 页面展示 IELTS band 结果
AND 同时展示对应的 CEFR 参考等级

AC2：映射规则来源一致
GIVEN 页面需展示 band 与 CEFR 对照
WHEN 系统读取映射规则
THEN 映射结果使用当前生效的统一映射口径
AND 同一分数在同一版本口径下返回一致的展示结果

AC3：无可映射结果时的处理
GIVEN 页面需展示 band 与 CEFR 对照
WHEN 当前结果缺少映射区间或映射规则不可用
THEN 页面不展示错误等级
AND 展示通用参考说明 [假设]
AND 结果页其他分数信息仍可正常查看

AC4：映射接口或规则异常处理
GIVEN 页面需展示 band 与 CEFR 对照
WHEN 映射服务返回系统错误或规则加载失败
THEN 页面进入降级展示
AND 提示当前对照信息暂不可用 [假设]
AND 不阻塞结果页整体展示

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F3-S03 — 分维度解读卡

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S03`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [S1]
- kpi_alignment: [K3, K8]

**User Story（英文）**:
> As an IELTS learner, I want to read dimension-level explanations and basic next-step advice, so that I know what to improve after seeing my score.

**Acceptance Criteria**：

AC1：展示分维度解读卡
GIVEN 用户已进入成功结果页
AND 当前评分结果包含可用于解释的维度字段
WHEN 页面加载解读区域
THEN 页面展示分维度解读卡
AND 每张解读卡至少包含维度名称、当前表现说明和基础建议

AC2：建议内容与分数区间匹配
GIVEN 页面需生成分维度解读卡
WHEN 系统读取解释模板
THEN 当前维度的说明和建议与对应分数区间匹配
AND 同一区间下返回一致的解释文案版本

AC3：解释内容缺失时的降级
GIVEN 页面需展示分维度解读卡
WHEN 某个维度缺少匹配的解释模板
THEN 系统为该维度展示通用建议文案
AND 不影响其他维度卡片正常展示

AC4：重复进入结果页的幂等性
GIVEN 用户基于同一条成功结果重复打开结果页
WHEN 页面再次加载分维度解读卡
THEN 系统展示与该结果版本一致的解释内容
AND 不因重复打开生成新的评分结果

AC5：解释模板加载异常
GIVEN 页面需展示分维度解读卡
WHEN 解释模板服务或配置读取失败
THEN 页面展示通用解读占位内容 [假设]
AND 页面整体仍可继续展示分数与导流入口
AND 记录一次解释加载失败事件供后续排查

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F3-S04 — 边界分数通用解释回退

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S04`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [S4]
- kpi_alignment: [K8]

**User Story（英文）**:
> As an IELTS learner, I want a fallback explanation when my score falls into a boundary range, so that I still get a usable interpretation instead of a blank area.

**Acceptance Criteria**：

AC1：边界分数命中通用解释回退
GIVEN 用户已进入成功结果页
AND 当前分数落在边界区间或缺少精确匹配模板
WHEN 系统生成结果解释
THEN 系统使用通用等级解释模板填充对应区域
AND 页面不出现空白解释模块

AC2：回退解释的文案边界
GIVEN 系统已进入通用解释回退逻辑
WHEN 页面展示回退解释内容
THEN 文案明确该解释为参考性说明
AND 不将回退解释表述为官方等级结论

AC3：非边界分数不误用回退模板
GIVEN 用户结果存在精确匹配模板
WHEN 系统生成结果解释
THEN 系统优先使用精确匹配模板
AND 不展示通用回退模板内容

AC4：回退模板缺失时的异常处理
GIVEN 当前结果既缺少精确模板也缺少通用回退模板
WHEN 页面尝试展示解释信息
THEN 页面隐藏缺失的解释模块或展示最小通用提示 [假设]
AND 结果页其他模块仍保持可用

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F3-S05 — 解释性质说明

**Story ID**：`EPIC-speaking-challenge-and-scoring-F3-S05`

**upstream_refs**:
- persona: P1
- journey_stage: J5
- scenarios: [S4, S5]
- kpi_alignment: [K8, K9]

**User Story（英文）**:
> As an IELTS learner, I want the result page to clarify that the score is a practice reference, so that I do not mistake it for an official score.

**Acceptance Criteria**：

AC1：结果页展示解释性质说明
GIVEN 用户已进入成功结果页
WHEN 页面展示分数和解读信息
THEN 页面展示“练习参考，非官方成绩”性质说明
AND 该说明在结果页可见范围内可被用户看到 [假设]

AC2：导流前保留性质说明语境
GIVEN 用户已查看结果页
WHEN 用户浏览导流入口区域
THEN 结果页仍保留性质说明
AND 不出现暗示官方认证或正式出分的误导性文案

AC3：说明缺失时的失败保护
GIVEN 结果页加载成功
WHEN 性质说明配置缺失或加载失败
THEN 页面使用默认通用说明文案 [假设]
AND 不允许结果页在无性质说明的情况下发布展示

**变更记录**：
- v1.0 (2026-05-08)：初版

---

### Feature F4 — Website Handoff CTA（来自 §2）

#### Story EPIC-speaking-challenge-and-scoring-F4-S01 — 结果页 CTA 渲染

**Story ID**：`EPIC-speaking-challenge-and-scoring-F4-S01`

**upstream_refs**:
- persona: P1
- journey_stage: J6
- scenarios: [S5]
- kpi_alignment: [K3]

**User Story（英文）**:
> As an IELTS learner, I want to see a next-step CTA that matches my score context, so that I know where to go after reading my result.

**Acceptance Criteria**：

AC1：按结果分层展示 CTA
GIVEN 用户已进入成功结果页
AND 当前结果具备可用于分层的等级信息
WHEN 页面渲染导流区域
THEN 系统展示与当前分层匹配的 CTA 入口
AND CTA 文案与当前用户场景保持一致

AC2：无匹配分层时的默认 CTA
GIVEN 用户已进入成功结果页
WHEN 当前结果缺少明确分层或未命中分层规则
THEN 系统展示默认 CTA 入口
AND 默认 CTA 仍指向可用的 website 承接页

AC3：重复进入结果页的幂等展示
GIVEN 用户基于同一条结果重复打开结果页
WHEN 页面再次渲染导流区域
THEN 系统展示同一组 CTA 规则结果
AND 不因重复进入生成新的归因点击记录

AC4：CTA 配置异常处理
GIVEN 页面需渲染导流区域
WHEN CTA 规则配置缺失或加载失败
THEN 页面展示默认备用 CTA [假设]
AND 不影响分数和解释区域展示
AND 记录一次 CTA 配置异常事件

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F4-S02 — 深链参数透传

**Story ID**：`EPIC-speaking-challenge-and-scoring-F4-S02`

**upstream_refs**:
- persona: P1
- journey_stage: J6
- scenarios: [S5]
- kpi_alignment: [K3, K5]

**User Story（英文）**:
> As an IELTS learner, I want the website to receive the right context when I tap the CTA, so that the landing page can continue from my result instead of treating me as a random visitor.

**Acceptance Criteria**：

AC1：点击 CTA 时透传上下文参数
GIVEN 用户位于成功结果页
AND 当前 CTA 存在可用的目标承接页
WHEN 用户点击 CTA
THEN 系统向 website 透传来源场景、题目标识、分层区间和渠道标记等上下文参数
AND 不透传不必要的原始敏感分数字段 [假设]

AC2：参数透传成功后的跳转
GIVEN 用户点击 CTA
WHEN 参数构造成功
THEN 系统跳转到目标 website 承接页
AND website 可基于透传参数命中匹配的承接模块

AC3：参数透传的幂等处理
GIVEN 用户连续点击同一 CTA
WHEN 第一次点击已触发跳转
THEN 系统仅以第一次点击的参数为准发起跳转
AND 不重复触发多次跳转请求

AC4：参数构造失败时的异常处理
GIVEN 用户点击 CTA
WHEN 参数构造失败、参数校验不通过或跳转前校验失败
THEN 系统进入默认承接页降级逻辑
AND 记录一次参数缺失或构造失败事件
AND 用户不看到空白或 404 页面

AC5：承接页不支持部分参数时的处理
GIVEN 用户点击 CTA
WHEN website 仅识别部分透传参数
THEN 跳转仍应成功
AND 未识别参数被安全忽略
AND 不影响基础承接页展示

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F4-S03 — 归因埋点

**Story ID**：`EPIC-speaking-challenge-and-scoring-F4-S03`

**upstream_refs**:
- persona: P2
- journey_stage: J6
- scenarios: [S5]
- kpi_alignment: [K3, K5]

**User Story（英文）**:
> As a growth operator, I want click and arrival attribution around the handoff CTA, so that we can measure which result contexts drive website conversion.

**Acceptance Criteria**：

AC1：点击事件记录
GIVEN 用户位于结果页导流区域
WHEN 用户点击任一 CTA
THEN 系统记录一次点击事件
AND 事件至少包含 `ctaId`、`promptId`、`scoreBucket`、`sourceChannel` 和时间戳

AC2：到达事件记录
GIVEN 用户已从小程序点击 CTA 跳转 website
WHEN website 成功识别来源参数并完成页面加载
THEN 系统记录一次到达事件
AND 到达事件与点击事件可通过归因标识关联

AC3：重复点击的幂等与限制
GIVEN 用户在短时间内重复点击同一 CTA
WHEN 系统处理归因埋点
THEN 系统按既定规则记录或合并事件 [假设]
AND 不因为重复点击造成明显失真的转化统计

AC4：埋点异常处理
GIVEN 用户点击 CTA 或到达 website
WHEN 埋点服务返回系统错误或超时
THEN 不阻塞用户跳转和承接页访问
AND 系统记录本地失败日志或补偿标记 [假设]
AND 用户界面不展示技术报错

**变更记录**：
- v1.0 (2026-05-08)：初版

---

#### Story EPIC-speaking-challenge-and-scoring-F4-S04 — 参数缺失降级

**Story ID**：`EPIC-speaking-challenge-and-scoring-F4-S04`

**upstream_refs**:
- persona: P1
- journey_stage: J6
- scenarios: [S5]
- kpi_alignment: [K3]

**User Story（英文）**:
> As an IELTS learner, I want a safe default landing experience if some handoff parameters are missing, so that I still reach a usable website page.

**Acceptance Criteria**：

AC1：参数缺失时进入默认承接页
GIVEN 用户从结果页点击 CTA
WHEN 系统发现部分透传参数缺失、为空或不合法
THEN 系统跳转到默认承接页
AND 默认承接页仍提供可继续浏览的 website 内容

AC2：降级跳转不影响用户访问
GIVEN 用户命中默认承接页降级逻辑
WHEN 页面加载完成
THEN 页面不出现 404、空白页或技术错误提示
AND 用户仍可继续浏览题库或工具入口

AC3：参数缺失事件记录
GIVEN 用户命中默认承接页降级逻辑
WHEN 跳转完成
THEN 系统记录一次参数缺失事件
AND 事件可区分是参数缺失、参数格式错误还是目标页不可用 [假设]

AC4：重复命中降级逻辑的幂等处理
GIVEN 用户多次点击同一降级 CTA
WHEN 系统重复执行默认承接页跳转
THEN 每次跳转都进入可用的默认承接页
AND 不因重复降级导致循环跳转

**变更记录**：
- v1.0 (2026-05-08)：初版

## §4 Story-level Estimation

| Story ID | Size | Points | Units | Effort | Complexity | Confidence | Notes |
|---|:---:|---:|---:|---:|:---:|:---:|---|
| EPIC-speaking-challenge-and-scoring-F1-S01 | M | 3 | 2-4 | 1-2 days | Medium | High | 首页入口规则与无题兜底 |
| EPIC-speaking-challenge-and-scoring-F1-S02 | S | 2 | 1-2 | 0.5-1 day | Low | High | 纯展示列表 |
| EPIC-speaking-challenge-and-scoring-F1-S03 | L | 5 | 4-8 | 2-4 days | High | Medium | 题目状态校验与开始链路 |
| EPIC-speaking-challenge-and-scoring-F1-S04 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 中断恢复边界待定 |
| EPIC-speaking-challenge-and-scoring-F1-S05 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 无题降级与备用题范围 |
| EPIC-speaking-challenge-and-scoring-F2-S01 | M | 3 | 2-4 | 1-2 days | Medium | High | 权限申请与拒绝引导 |
| EPIC-speaking-challenge-and-scoring-F2-S02 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 录音交互状态切换 |
| EPIC-speaking-challenge-and-scoring-F2-S03 | S | 2 | 1-2 | 0.5-1 day | Low | Medium | 本地录音有效性校验 |
| EPIC-speaking-challenge-and-scoring-F2-S04 | L | 5 | 4-8 | 2-4 days | High | Medium | 上传落对象存储与离页处理 |
| EPIC-speaking-challenge-and-scoring-F2-S05 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 任务幂等创建 |
| EPIC-speaking-challenge-and-scoring-F2-S06 | L | 5 | 4-8 | 2-4 days | High | Medium | 短轮询节奏与结果聚合 |
| EPIC-speaking-challenge-and-scoring-F2-S07 | L | 5 | 4-8 | 2-4 days | High | Medium | 超时/失败多路径兜底 |
| EPIC-speaking-challenge-and-scoring-F3-S01 | S | 2 | 1-2 | 0.5-1 day | Low | High | 结果卡片基础展示 |
| EPIC-speaking-challenge-and-scoring-F3-S02 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 映射口径待定 |
| EPIC-speaking-challenge-and-scoring-F3-S03 | L | 5 | 4-8 | 2-4 days | High | Medium | 解读模板与建议内容 |
| EPIC-speaking-challenge-and-scoring-F3-S04 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 边界分数回退规则 |
| EPIC-speaking-challenge-and-scoring-F3-S05 | S | 2 | 1-2 | 0.5-1 day | Low | High | 参考性质说明 |
| EPIC-speaking-challenge-and-scoring-F4-S01 | M | 3 | 2-4 | 1-2 days | Medium | Medium | CTA 分层规则 |
| EPIC-speaking-challenge-and-scoring-F4-S02 | L | 5 | 4-8 | 2-4 days | High | Medium | 深链参数安全透传 |
| EPIC-speaking-challenge-and-scoring-F4-S03 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 点击/到达归因链路 |
| EPIC-speaking-challenge-and-scoring-F4-S04 | M | 3 | 2-4 | 1-2 days | Medium | Medium | 默认承接页降级 |

## §5 Engineering Notes

| Story ID | 可能涉及系统 / 集成点 | 主要复杂点 | 潜在风险点 | refinement 重点 |
|---|---|---|---|---|
| EPIC-speaking-challenge-and-scoring-F1-S01 | Miniapp Frontend, Challenge BFF | 首页入口规则与数据返回时机 | 可参与题判定不一致 | 首页入口与题目状态口径统一 |
| EPIC-speaking-challenge-and-scoring-F1-S02 | Miniapp Frontend, Challenge BFF | 默认排序与字段缺失处理 | 列表字段不全导致 UI 空洞 | 默认排序、空状态文案 |
| EPIC-speaking-challenge-and-scoring-F1-S03 | Miniapp Frontend, Challenge BFF | 题目状态与开始链路一致性 | 已下线题目仍可进入录音页 | `promptId` 生命周期与可参与状态 |
| EPIC-speaking-challenge-and-scoring-F1-S04 | Miniapp Frontend, Session Storage/BFF | 中断恢复保存位置与过期逻辑 | 恢复数据污染或恢复到失效题目 | 恢复 TTL、入口位置、恢复到详情还是录音页 |
| EPIC-speaking-challenge-and-scoring-F1-S05 | Miniapp Frontend, Challenge BFF | 备用题策略 | 展示到无评分支持题目 | 历史题范围与备用入口规则 |
| EPIC-speaking-challenge-and-scoring-F2-S01 | Miniapp Frontend, Client Permission API | 权限状态机 | 授权状态读取异常 | 首次授权、拒绝后再授权路径 |
| EPIC-speaking-challenge-and-scoring-F2-S02 | Miniapp Frontend, Recording Module | 录音状态切换与重录 | 录音中断或内存占用异常 | 控件状态机、最大录音时长 |
| EPIC-speaking-challenge-and-scoring-F2-S03 | Miniapp Frontend | 有效录音门槛 | 最短时长和静音检测未定 | 最小录音时长、大小、损坏文件判定 |
| EPIC-speaking-challenge-and-scoring-F2-S04 | Upload Gateway, Object Storage | 先上传后建任务的边界 | 离页/断网导致状态不一致 | 上传幂等键、对象存储保留策略 |
| EPIC-speaking-challenge-and-scoring-F2-S05 | Scoring Job Service, API Gateway | 任务创建幂等性 | 多次建任务或业务规则不一致 | `audioRef` 与 `taskId` 绑定策略 |
| EPIC-speaking-challenge-and-scoring-F2-S06 | Scoring Job Service, AI Assessment Service | 短轮询节奏与结果聚合 | 轮询频率过高、结果结构不完整 | 轮询间隔、最大次数、终态字段模型 |
| EPIC-speaking-challenge-and-scoring-F2-S07 | Scoring Job Service, Miniapp Frontend | 超时与失败分类 | 用户误解为提交丢失 | 超时阈值、失败类型映射、重试策略 |
| EPIC-speaking-challenge-and-scoring-F3-S01 | Miniapp Frontend | 分数字段规范化展示 | 结构化结果字段缺失 | overall 与 dimension 字段标准 |
| EPIC-speaking-challenge-and-scoring-F3-S02 | Mapping Service, Assessment BFF | band/CEFR 映射口径版本化 | 映射误导用户 | 固定映射还是服务端规则 |
| EPIC-speaking-challenge-and-scoring-F3-S03 | Interpretation Engine, Content Config | 解释模板匹配 | 模板缺口导致空白或误导 | 模板版本、分数区间、基础建议粒度 |
| EPIC-speaking-challenge-and-scoring-F3-S04 | Interpretation Engine | 边界分数回退规则 | 边界解释与正式模板冲突 | 回退优先级与缺模板处理 |
| EPIC-speaking-challenge-and-scoring-F3-S05 | Miniapp Frontend, Content Config | 参考性质说明的一致性 | 被误解为官方成绩 | 默认说明文案与展示位置 |
| EPIC-speaking-challenge-and-scoring-F4-S01 | Miniapp Frontend, Growth Config | CTA 分层规则 | 错配承接页降低点击率 | 低分/中分/高分 CTA 规则 |
| EPIC-speaking-challenge-and-scoring-F4-S02 | Handoff Link Builder, Website Router | 深链参数安全传递 | 参数缺失、敏感分值泄露 | 传 bucket 不传 raw score，匿名/登录承接 |
| EPIC-speaking-challenge-and-scoring-F4-S03 | Data / Analytics, Website BFF | 点击到达归因 | 埋点丢失但用户已跳转 | click/arrival 关联键与补偿策略 |
| EPIC-speaking-challenge-and-scoring-F4-S04 | Website Router, Landing Matcher | 默认承接页降级 | 降级跳转循环或空白页 | 缺参分类与默认页选择逻辑 |

## §6 Non-functional Requirements

- **Performance**：评分结果平均返回时长目标为 `<=20s`，短轮询链路需支持用户在等待态下稳定刷新，不得出现页面卡死。
- **Availability**：上传、任务创建、状态查询、CTA 跳转四条主链路均需定义失败态和恢复路径。
- **Retry strategy**：上传失败、状态查询失败允许有限次重试；重复提交、重复建任务必须幂等。
- **Logging / Monitoring**：关键日志至少覆盖 `promptId`、`audioRef`、`taskId`、`ctaId`、`sourceChannel`、trace id、失败类型。
- **Security / Compliance**：音频数据保存周期、脱敏策略、授权文案必须在研发评审前明确；结果页不得暗示官方成绩。
- **Data limits**：需定义录音最小时长、最大文件大小和轮询最大次数。
- **Compatibility**：需覆盖主流小程序客户端版本下的麦克风权限处理差异。

## §7 Capacity Summary

Total Features: 4  
Total Stories: 21  
Total Estimated Units: 50 – 100 units  
Total Estimated Effort: 25 – 50 days  
Suggested Sprint Count: 3 – 5  
Suggested Team Count: 1 product + 2 to 3 engineers + 1 QA  
Main Complexity Drivers: 录音上传与评分异步链路、band/CEFR 映射与解释模板、website 深链透传与归因  
Main Assumptions: MVP 采用短轮询；音频先落对象存储；结果 CTA 以分层区间而非原始分数驱动  

Capacity 对比校验:
  Solution Brief Phase-level: 50 – 100 units（来自 §6 Epic 合计）
  Product Planner Story-level: 50 – 100 units（本 §7 汇总）
  偏差: 0%

  结论:
    □ 偏差 ≤ 30%：合理范围
    □ 偏差 > 30% 上偏：⚠️ Story 拆细后超出 Solution 粗估
    □ 偏差 > 30% 下偏：⚠️ Story 实际复杂度低于 Solution 预估
    □ N/A（A/C 模式 — 无 Solution Brief）

## §8 Estimation Disclaimer

> 以上估算属于 PRD 阶段 planning-level estimation；仅用于范围判断、资源预估和优先级决策；不代表研发最终承诺；最终单位估算和任务拆分以 Eng Reviewer / Task Planner refinement 为准。

## §9 Open Questions

来自 Value Frame（status=open 条目）:
- V-OQ1: 小程序首期的挑战机制最小版本是什么：单题挑战、每日挑战、连续打卡，还是榜单竞赛（status=open, owner=PM）
- V-OQ2: 评分结果是否直接展示完整 IELTS band descriptor 解释，还是先展示简化版结论再展开详情（status=open, owner=PM）
- V-OQ3: IELTS band 与 CEFR 对照表采用固定映射还是内部解释版映射（status=open, owner=PM + Eng）
- V-OQ4: website 承接页的首期目标是题库浏览、AI 工具试用，还是直接会员/产品购买转化（status=open, owner=PM）
- V-OQ5: 语音数据的保存周期、授权提示和可复用范围如何定义（status=open, owner=PM + Eng）
- V-OQ6: 小程序评分返回的目标时延能否稳定控制在 20 秒内（status=open, owner=Eng）
- V-OQ7: 首期是否只覆盖指定简化题库，还是同时支持自由题目扩展（status=open, owner=PM）

来自 Solution Brief（status=open 条目）:
- S-OQ1: 评分返回前是否允许用户离开页面并稍后查看结果（status=open, owner=PM + Eng）
- S-OQ2: 音频存储是否需要做脱敏或分级保留策略（status=open, owner=Eng + Compliance）
- S-OQ3: 小程序到 website 是否需要账号打通后再跳转，还是允许匿名承接（status=open, owner=PM + Eng）
- S-OQ4: 承接页首版是否允许直接出现价格和购买 CTA，还是先用工具试用承接（status=open, owner=PM + Growth）
- S-OQ5: 本 Epic 整体规模偏 L+，是否在第一次研发评审拆分为里程碑迭代（status=open, owner=Eng + PM）

本 PRD 新增:
- P-OQ1: 有效录音的最小时长、最大文件大小以及静音/损坏文件判定阈值如何定义（status=open, owner=PM + Eng）
- P-OQ2: 短轮询的间隔、最大查询次数和超时阈值是否采用统一默认值，还是按渠道/机型动态调整（status=open, owner=Eng）
- P-OQ3: 上传成功但任务创建失败时，是否允许直接复用同一 `audioRef` 重新建任务（status=open, owner=Eng）

## §10 Future Extension

- 结果页增加更细粒度提分建议与示例回答能力
- 支持用户在离开页面后稍后回看评分结果历史
- 在 website 承接页实现按 score bucket 的更精细化内容推荐和试用路径

## §11 已沉淀规则索引

- Layer 1：TOC 小程序定位为轻量口语挑战与评分入口，不扩展为重课程销售阵地
- Layer 2：MVP 评分链路采用短轮询查询结果，避免首版引入复杂回调
- Layer 2：音频先上传并落对象存储，再创建评分任务，确保失败排查和可控重试
- Layer 2：website 深链默认透传 score bucket，不透传原始敏感分数
- Layer 3：结果页必须展示“练习参考，非官方成绩”性质说明

## §12 PRD-level Changelog

- v1.0 (2026-05-08-0400)：基于 Solution Brief `speaking-challenge-and-scoring` 首版拆解 Feature、User Story、AC、Story-level Estimation 与 Engineering Notes；F2 因 FCS > 10 先经 Story Splitter 校正为 7 条 Story 后再落盘。
