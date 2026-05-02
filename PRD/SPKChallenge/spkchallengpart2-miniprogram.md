# PRD - spkchallengpart2-miniprogram

## 1. Feature Summary

本需求面向备考 IELTS 的 TOC 学生，在微信小程序中搭建一个 Part 2 口语活动入口，完成微信授权、题目展示、录音上传、等待结果、查看 CEFR 结果的完整闭环，同时为后续评分与 AI 素材采集沉淀标准化数据。

核心目标：

- 提供低门槛、单次提交的活动式口语参与路径
- 通过 `unionId` 建立用户、录音文件、提交记录和结果记录的唯一关联
- 用等待页和结果页提升提交完成率与结果查看率
- 为运营后台提供活动开关、Banner 切换和题库素材配置能力

## 2. Product / Opportunity Brief

### 当前问题

- 当前缺少一个适合微信小程序传播的轻量级口语活动入口，无法高效沉淀 Part 2 口语音频样本。
- 如果没有一次性提交、防重复和状态机控制，活动样本质量和运营转化数据会失真。
- 用户在提交后如果没有明确等待与结果反馈，容易流失，分享传播动力也会下降。

### 为什么现在做

- 小程序是 TOC 触达的重要渠道，适合通过活动式入口快速收集有效样本。
- 当前需求可直接复用历史小程序口语活动模式，风险可控、上线速度快。
- 该活动既可作为素材采集入口，也可作为后续口语测评、转化运营和 CEFR 反馈体验的前置能力。

### 目标用户

- 备考 IELTS 的 TOC 学生

### 业务价值

- 增加有效 `unionId` 级别的活动参与人数
- 增加可用 mp3 口语样本沉淀量
- 建立小程序活动闭环，为后续评分服务和活动复用提供稳定骨架
- 通过等待页与结果页增强用户回流与分享传播

## 3. Value Hypothesis

### 假设

- 如果活动入口足够直接，且首页 CTA 与状态反馈足够清晰，用户会更愿意完成一次完整录音提交。
- 如果提交后明确告知评估 SLA，并提供分享入口，用户对等待期的焦虑会降低，活动传播会增强。
- 如果结果页以 CEFR 等级、等级背景图和鼓励文案呈现，用户会更愿意回访和进一步探索 IELTS 相关内容。

### 用户价值

- 快速进入活动，不需要复杂学习成本
- 清楚知道自己当前处于未提交、待结果还是已出分状态
- 在结果返回后看到直观、鼓励性的口语水平反馈

### 业务价值

- 提高漏斗转化：授权 -> 开始录音 -> 成功上传 -> 查看结果
- 形成活动配置化能力，支持未来复用到更多口语活动
- 为后续 Eng Reviewer 和 Task Planner 提供明确状态机、边界和异步结果链路基础

### 验证信号

- 首页 CTA 点击率
- 录音页完成率
- 成功上传率
- 结果页查看率
- 分享触发率

## 4. KPI Tree

### North Star

- 有效活动提交数（按 `unionId + campaignId` 去重）

### Leading Indicators

- 微信授权成功率
- 入口 CTA 点击率
- 录音完成率
- 上传成功率
- 等待页访问率
- 结果页访问率
- 分享触发率

### Guardrails

- 重复提交率保持在可控范围内
- 上传失败率低于约定阈值
- 无结果等待超时用户比例受控
- 活动结束配置切换后不允许新用户误入提交流程

## 5. Epic

### Epic Name

- spkchallengpart2-miniprogram

### Context

在微信小程序中搭建一个轻量级 IELTS Speaking Part 2 活动，用户通过微信授权进入后，随机抽取一张图片题，完成最长 2 分钟录音并上传，系统将音频和 `unionId` 绑定，同步进入等待结果状态。待异步评分结果回传后，用户可查看 CEFR 结果页。整个活动必须满足单用户单活动仅一次成功提交、防重复提交、活动结束配置化、等待态和结果态可区分。

## 6. Feature List

| Feature Name | Description | Value |
|---|---|---|
| F1. Identity and Entry State Management | 管理微信授权、`unionId` 绑定、首页双 CTA 状态机和活动结束配置 | 保证用户身份可信、首页行为一致、活动状态可控 |
| F2. Recording and Submission Flow | 管理题目随机、录音、重录、播放、上传和单次提交控制 | 提供高完成率的核心录音提交流程 |
| F3. Waiting and Sharing Experience | 提供等待结果固定文案和分享能力 | 降低等待焦虑，提升传播转化 |
| F4. Result Lifecycle and CEFR Experience | 管理异步结果回传、状态流转和 CEFR 结果展示 | 让用户看到可理解的最终反馈，并支持后续复用 |

## 7. User Stories and AC

### Feature: F1. Identity and Entry State Management

#### Story 1: WeChat authorization and unionId campaign identity binding

Epic: spkchallengpart2-miniprogram  
Feature: F1. Identity and Entry State Management

As a TOC student preparing for IELTS, I want to authorize with WeChat before participating, so that the mini program can identify my campaign identity securely.

AC:

AC1：授权成功并建立活动身份  
GIVEN 用户进入口语 Part 2 活动入口页  
AND 当前活动处于 `active` 状态  
AND 用户当前活动身份尚未绑定 `unionId`  
WHEN 用户点击任一入口 CTA  
AND 用户在微信授权弹窗中同意授权  
THEN 系统获取用户的 `unionId`  
AND 系统为当前活动建立或绑定唯一参与者身份  
AND 当前小程序会话进入已识别状态  
AND 用户可继续进入后续活动流程

AC2：用户拒绝微信授权时不可参与  
GIVEN 用户进入口语 Part 2 活动入口页  
AND 用户当前活动身份尚未绑定 `unionId`  
WHEN 用户点击任一入口 CTA  
AND 用户在微信授权弹窗中拒绝授权  
THEN 系统不创建活动参与记录  
AND 用户停留在入口页  
AND 系统提示“需先完成微信授权后才能参与活动” [假设]

AC3：已绑定用户重复进入时复用既有身份  
GIVEN 用户当前活动身份已绑定有效 `unionId`  
WHEN 用户再次进入活动入口页  
THEN 系统复用该用户既有活动身份  
AND 系统不创建重复的参与者身份记录  
AND 用户继续使用当前活动状态进入后续流程

AC4：微信授权异常时允许用户重试  
GIVEN 用户已触发微信授权  
WHEN 微信授权接口超时或返回系统错误  
THEN 系统不创建活动参与记录  
AND 系统提示“授权失败，请稍后重试” [假设]  
AND 用户可在入口页再次发起授权

#### Story 2: Entry CTA state machine and event-ended behavior

Epic: spkchallengpart2-miniprogram  
Feature: F1. Identity and Entry State Management

As a TOC student preparing for IELTS, I want the entry CTA to reflect my current participation status, so that I always see the correct next action.

AC:

AC1：入口页根据活动状态和提交状态展示正确 CTA  
GIVEN 用户已完成活动身份识别  
WHEN 系统加载活动入口页  
THEN 系统基于 `activity_status` 与 `submission_status` 组合决定两个 CTA 的文案、可点击状态与跳转行为  
AND 当 `activity_status=active` 且 `submission_status=not_submitted` 时，两个 CTA 文案分别为“立即参与”和“开始口语测评”，且均可点击并进入录音页  
AND 当 `activity_status=active` 且 `submission_status=pending_result` 时，两个 CTA 均展示“等待测评结果”，且均不可导航到其他页面  
AND 当 `activity_status=active` 且 `submission_status=result_ready` 时，两个 CTA 均展示“查看结果”，且点击后进入结果页  
AND 当 `activity_status=ended` 且 `submission_status=not_submitted` 时，两个 CTA 均展示“谢谢参与，活动结束”，且均处于不可点击状态  
AND 当 `activity_status=ended` 且 `submission_status=not_submitted` 时，入口 Banner 切换为活动结束 Banner  
AND 对于已提交用户，活动结束后仍按 `submission_status` 展示“等待测评结果”或“查看结果”路径

AC2：等待结果状态下 CTA 不允许导航  
GIVEN 用户已完成提交  
AND 当前 `submission_status=pending_result`  
WHEN 用户点击任一入口 CTA  
THEN 系统不执行页面跳转  
AND 系统继续停留在当前入口页  
AND 页面持续展示“等待测评结果”

AC3：活动结束时未提交用户被阻止进入录音流程  
GIVEN 用户已完成活动身份识别  
AND 当前 `activity_status=ended`  
AND 当前 `submission_status=not_submitted`  
WHEN 用户查看入口页  
THEN 两个 CTA 均处于不可点击状态  
AND 页面展示活动结束 Banner  
AND 用户无法进入录音页

AC4：状态查询失败时不允许错误导航  
GIVEN 用户进入活动入口页  
WHEN 活动状态或提交状态查询失败  
THEN 系统不执行录音页或结果页跳转  
AND 系统提示“活动状态加载失败，请稍后重试” [假设]  
AND 用户可留在入口页重试刷新

### Feature: F2. Recording and Submission Flow

#### Story 3: Random question assignment and recording start flow

Epic: spkchallengpart2-miniprogram  
Feature: F2. Recording and Submission Flow

As a TOC student preparing for IELTS, I want to receive one random Part 2 prompt and start recording immediately, so that I can complete my speaking submission in the campaign flow.

AC:

AC1：录音页为用户分配一题随机题目  
GIVEN 用户已完成活动身份识别  
AND 当前活动可参与  
AND 当前 `submission_status=not_submitted`  
WHEN 用户进入录音页  
THEN 系统从 38 道图片题库中随机分配 1 道题给当前录音尝试  
AND 页面展示该题对应的图片题面  
AND 系统为本次录音尝试保存唯一 `questionId`  
AND 用户在同一次未上传的录音尝试内重复进入录音页时，继续看到同一 `questionId`

AC2：开始录音后启动 2 分钟倒计时并自动停止  
GIVEN 用户已进入录音页  
AND 页面已展示当前题目图片  
WHEN 用户点击开始录音  
THEN 系统调用设备录音能力开始录音  
AND 页面展示 2 分钟倒计时  
AND 当倒计时归零时，系统自动停止录音  
AND 录音结果进入录音后处理态

AC3：用户可在倒计时结束前手动停止录音  
GIVEN 用户正在录音中  
WHEN 用户点击停止录音  
THEN 系统立即停止本次录音  
AND 页面不再继续倒计时  
AND 录音结果进入录音后处理态

AC4：录音权限被拒绝或录音能力不可用时给出反馈  
GIVEN 用户已进入录音页  
WHEN 用户点击开始录音  
AND 设备麦克风权限被拒绝或录音能力不可用  
THEN 系统不开始录音  
AND 系统提示“请开启麦克风权限后再试”或“当前设备暂不支持录音” [假设]  
AND 用户可在录音页再次尝试开始录音

#### Story 4: Post-recording review, playback, and re-record confirmation

Epic: spkchallengpart2-miniprogram  
Feature: F2. Recording and Submission Flow

As a TOC student preparing for IELTS, I want to review, replay, or re-record my audio after stopping, so that I can decide whether to submit my final attempt.

AC:

AC1：停止录音后展示后处理操作  
GIVEN 用户已完成一次录音停止  
WHEN 页面进入录音后处理态  
THEN 页面展示“重新录音”“播放”“上传录音”三个操作  
AND 用户可播放当前录音  
AND 用户可选择重新录音  
AND 用户可继续进入上传提交流程

AC2：用户确认重新录音时删除上一段录音  
GIVEN 用户已完成一次录音停止  
AND 页面已展示当前录音的后处理操作  
WHEN 用户点击“重新录音”  
AND 用户在确认弹窗中点击 Confirm  
THEN 系统删除上一段未提交录音  
AND 系统关闭确认弹窗  
AND 系统重新开始新的录音流程  
AND 新录音不复用上一段音频文件

AC3：用户取消重新录音时保留上一段录音  
GIVEN 用户已完成一次录音停止  
AND 页面已展示当前录音的后处理操作  
WHEN 用户点击“重新录音”  
AND 用户在确认弹窗中点击 Cancel  
THEN 系统关闭确认弹窗  
AND 系统保留上一段未提交录音  
AND 用户仍可播放或上传当前录音

AC4：用户通过关闭按钮退出重新录音确认弹窗时保留上一段录音  
GIVEN 用户已完成一次录音停止  
AND 页面已展示当前录音的后处理操作  
WHEN 用户点击“重新录音”  
AND 用户在确认弹窗中点击右上角关闭按钮  
THEN 系统关闭确认弹窗  
AND 系统保留上一段未提交录音  
AND 用户仍可播放或上传当前录音

AC5：录音播放失败时不影响重新录音或上传  
GIVEN 用户已完成一次录音停止  
WHEN 用户点击“播放”  
AND 当前录音文件播放失败  
THEN 系统提示“录音播放失败，请重试” [假设]  
AND 系统保留当前录音文件  
AND 用户仍可选择“重新录音”或“上传录音”

#### Story 5: Upload final audio, persist metadata, and create the single submission

Epic: spkchallengpart2-miniprogram  
Feature: F2. Recording and Submission Flow

As a TOC student preparing for IELTS, I want to upload my final audio once, so that my campaign submission is stored and sent into the scoring lifecycle.

AC:

AC1：没有可提交录音时上传按钮不可点击  
GIVEN 用户位于录音后处理态  
AND 当前不存在已停止且可提交的录音文件  
WHEN 用户查看“上传录音”按钮  
THEN 按钮处于不可点击状态  
AND 用户不能发起上传

AC2：存在可提交录音时上传按钮可点击  
GIVEN 用户位于录音后处理态  
AND 当前存在已停止且可提交的录音文件  
WHEN 用户查看“上传录音”按钮  
THEN 按钮处于可点击状态

AC3：上传成功后创建唯一活动提交并进入待出分  
GIVEN 用户已完成活动身份识别  
AND 当前 `submission_status=not_submitted`  
AND 当前存在可提交的录音文件  
WHEN 用户点击“上传录音”按钮  
THEN 系统上传音频文件为 mp3  
AND 系统保存与 `unionId` 关联的文件元数据  
AND 文件元数据至少包含 `campaignId`、`unionId`、`questionId`、`fileUrl`、`durationSeconds`、`fileSize`、`submittedAt`  
AND 系统创建当前活动唯一提交记录  
AND 系统为该提交创建评分处理任务  
AND 用户进入等待结果页

AC4：每个候选人在同一活动中只能提交一次  
GIVEN 用户在当前 `campaignId` 下已存在成功提交记录  
WHEN 用户再次尝试上传录音  
THEN 系统不创建新的提交记录  
AND 系统不创建新的评分处理任务  
AND 系统提示“每位用户在本活动中仅可提交一次”  
AND 用户看到当前已存在的等待结果或结果状态

AC5：上传系统错误时不得暴露为成功提交  
GIVEN 用户当前存在可提交的录音文件  
WHEN 用户点击“上传录音”按钮  
AND 音频上传失败或评分任务创建失败  
THEN 系统不向用户展示提交成功  
AND 系统提示“提交失败，请稍后重试” [假设]  
AND 系统不允许产生重复的成功提交记录  
AND 用户可基于当前录音重新尝试上传

AC6：上传并发触发时不得生成重复提交  
GIVEN 用户当前存在可提交的录音文件  
WHEN 用户在短时间内重复点击“上传录音”按钮  
THEN 系统最多只接受一次成功提交  
AND 系统不创建重复的提交记录  
AND 系统提示“提交处理中，请勿重复操作” [假设]

### Feature: F3. Waiting and Sharing Experience

#### Story 6: Waiting page and social sharing while result is pending

Epic: spkchallengpart2-miniprogram  
Feature: F3. Waiting and Sharing Experience

As a TOC student preparing for IELTS, I want a waiting page with clear expectation and sharing options, so that I know when to expect my result and can share the campaign.

AC:

AC1：等待页展示固定文案与分享入口  
GIVEN 用户已成功完成活动提交  
AND 当前 `submission_status=pending_result`  
WHEN 用户进入等待结果页  
THEN 页面展示固定文案“评估时间：评估结果通常在 1 周内返回，请耐心等待”  
AND 页面展示分享给好友入口  
AND 页面展示分享到朋友圈入口

AC2：结果已返回时不再停留等待页  
GIVEN 用户已成功完成活动提交  
WHEN 用户进入等待结果页  
AND 当前 `submission_status=result_ready`  
THEN 系统自动跳转到结果页  
AND 用户不再看到等待中的固定文案

AC3：分享能力失败或取消时不影响等待状态  
GIVEN 用户处于等待结果页  
WHEN 用户触发分享给好友或分享朋友圈  
AND 分享能力不可用、分享失败或用户主动取消  
THEN 页面保持在等待结果页  
AND 当前提交状态不发生变化  
AND 用户仍可继续等待结果返回

### Feature: F4. Result Lifecycle and CEFR Experience

#### Story 7: Async scoring callback ingestion and lifecycle state transition

Epic: spkchallengpart2-miniprogram  
Feature: F4. Result Lifecycle and CEFR Experience

As a TOC student preparing for IELTS, I want my submitted audio to move to a result-ready state when scoring completes, so that I can view my CEFR outcome without manual follow-up.

AC:

AC1：系统维护完整的提交生命周期状态并在回调成功后流转  
GIVEN 当前活动采用异步评分回调架构  
WHEN 系统处理提交流程与评分回调流程  
THEN 系统维护 `submission_status` 的完整状态值集合  
AND `submission_status=not_submitted` 表示当前 `campaignId` 下无成功提交记录  
AND `submission_status=pending_result` 表示音频与提交记录已保存成功，正在等待评分回调  
AND `submission_status=result_ready` 表示评分结果已成功写入并可展示  
AND 当用户成功提交音频时，系统将 `submission_status` 从 `not_submitted` 更新为 `pending_result`  
AND 当系统收到与当前提交匹配且校验通过的评分回调时，系统将 `submission_status` 从 `pending_result` 更新为 `result_ready`

AC2：回调成功时写入结果并建立结果关联  
GIVEN 当前提交处于 `submission_status=pending_result`  
AND 系统收到包含有效 `submissionId` 或等效关联键的评分回调  
AND 回调载荷包含有效 CEFR 结果  
WHEN 系统完成回调校验  
THEN 系统写入该提交对应的评分结果  
AND 评分结果与当前提交记录建立唯一关联  
AND 系统将该提交更新为 `submission_status=result_ready`

AC3：评分系统报错或结果持久化失败时保持待出分  
GIVEN 当前提交处于 `submission_status=pending_result`  
WHEN 系统处理评分回调时发生接口超时、系统错误或结果持久化失败  
THEN 系统不写入结果页可见数据  
AND 系统保持该提交为 `submission_status=pending_result`  
AND 系统记录可追踪的错误日志用于后续排查  
AND 系统允许评分侧按约定策略重试 [假设]

AC4：评分结果不满足业务规则时不得错误出分  
GIVEN 当前提交处于 `submission_status=pending_result`  
WHEN 系统收到评分回调  
AND 回调缺失必要字段或 `cefrLevel` 不在 `A1`、`A2`、`B1`、`B2`、`C1`、`C2` 范围内  
THEN 系统不写入正式结果  
AND 系统保持该提交为 `submission_status=pending_result`  
AND 系统记录异常结果日志  
AND 系统返回失败处理结果给评分回调方 [假设]

AC5：重复回调或并发回调不会产生重复结果  
GIVEN 当前提交已处理过一次成功评分回调  
WHEN 系统再次收到相同评分任务的重复回调或并发回调  
THEN 系统不创建第二条结果记录  
AND 系统不重复变更已完成的结果数据  
AND 当前提交继续保持 `submission_status=result_ready`

#### Story 8: CEFR result page presentation with level assets and encouragement copy

Epic: spkchallengpart2-miniprogram  
Feature: F4. Result Lifecycle and CEFR Experience

As a TOC student preparing for IELTS, I want to see a complete CEFR result page after scoring, so that I can understand my level and feel encouraged to continue practicing.

AC:

AC1：结果页按 CEFR 等级展示对应内容  
GIVEN 用户当前 `submission_status=result_ready`  
AND 当前提交已存在有效 CEFR 结果  
WHEN 用户进入结果页  
THEN 页面展示 CEFR 等级图片  
AND CEFR 等级支持 `A1`、`A2`、`B1`、`B2`、`C1`、`C2` 六个有效值  
AND 页面根据当前 CEFR 等级展示对应背景资源  
AND 页面展示固定文案链接“IELTS and the CEFR”

AC2：结果页从对应等级的文案池中随机展示一条鼓励文案  
GIVEN 用户当前 `submission_status=result_ready`  
AND 当前 `cefrLevel` 已确定  
WHEN 用户进入结果页  
THEN 系统在该 `submissionId` 首次生成结果页内容时，从当前 `cefrLevel` 对应的 5 条鼓励文案池中随机选择 1 条  
AND 系统保存该 `submissionId` 对应的结果文案  
AND 用户后续再次进入结果页时，系统持续展示同一条已保存文案  
AND 所选文案仅来自当前等级对应的文案池  
AND 不得展示其他等级的鼓励文案

AC3：结果数据异常时不展示错误等级页  
GIVEN 用户尝试进入结果页  
WHEN 当前结果缺失 `cefrLevel`、`cefrLevel` 值不在 `A1` 至 `C2` 范围内，或对应资源缺失  
THEN 系统不展示正式结果页  
AND 系统提示“结果暂时不可用，请稍后重试” [假设]  
AND 用户可返回入口页或稍后再次查看

AC4：未出分用户访问结果页时按当前状态重定向  
GIVEN 用户尝试进入结果页  
WHEN 当前 `submission_status=not_submitted`  
THEN 系统将用户引导回录音页入口  
AND 用户不可查看结果内容

AC5：等待中的用户访问结果页时按当前状态重定向  
GIVEN 用户尝试进入结果页  
WHEN 当前 `submission_status=pending_result`  
THEN 系统将用户引导到等待结果页  
AND 用户不可查看结果内容

## 8. Estimation

| Story | Story Size | Story Points | Estimated Units | Estimated Effort | Complexity | Confidence | Estimation Notes |
|---|---:|---:|---:|---:|---|---|---|
| Story 1 | S | 2 | 1 - 2 | 0.5 - 1 day | Medium | Medium | 涉及微信授权链路、`unionId` 绑定和授权失败处理 |
| Story 2 | M | 3 | 2 - 4 | 1 - 2 days | Medium | Medium | 双 CTA 共享状态机，活动状态与提交状态组合较多 |
| Story 3 | M | 3 | 2 - 4 | 1 - 2 days | Medium | Medium | 题库随机、录音权限、计时器和手动停止逻辑 |
| Story 4 | M | 3 | 2 - 4 | 1 - 2 days | Medium | Medium | 需要覆盖播放、重录确认弹窗和本地录音状态一致性 |
| Story 5 | L | 5 | 4 - 8 | 2 - 4 days | High | Medium | 上传、元数据落库、单次提交、防重和评分任务创建耦合 |
| Story 6 | S | 2 | 1 - 2 | 0.5 - 1 day | Low | High | 等待态和分享能力相对简单，但依赖状态正确性 |
| Story 7 | L | 5 | 4 - 8 | 2 - 4 days | High | Low | 异步回调、幂等、异常分类和状态机一致性风险较高 |
| Story 8 | M | 3 | 2 - 4 | 1 - 2 days | Medium | Medium | CEFR 等级映射、资源匹配和文案池选择 |

## 9. Notes for Engineering

### Story-level Notes

| Story | Possible Systems | Integration Points | Main Complexity | Main Risks | Refinement Focus |
|---|---|---|---|---|---|
| Story 1 | Mini program, WeChat auth, Campaign Backend | 微信授权、`unionId` 获取 | 鉴权失败兜底与身份复用 | 无法拿到稳定 `unionId` | 授权失败页、匿名态回退策略 |
| Story 2 | Mini program, Campaign Backend, IOC admin config | 活动配置读取、用户状态查询 | 双 CTA 状态一致性 | Banner 和 CTA 状态不同步 | 状态优先级矩阵 |
| Story 3 | Mini program | 设备录音能力、题库配置 | 录音权限、随机题保持策略 | 重进页面导致题目漂移 | `questionId` 固定策略 |
| Story 4 | Mini program | 本地音频播放 | 重录确认三种关闭方式 | 文件状态错乱 | 本地临时文件生命周期 |
| Story 5 | Mini program, Upload/Storage, Campaign Backend, Scoring Queue | mp3 上传、元数据持久化、任务创建 | 原子性、一人一次、防重复点击 | 孤儿文件、重复提交 | finalize 策略、幂等键 |
| Story 6 | Mini program | 微信分享能力 | 等待态与分享共存 | 分享失败误影响状态 | 分享文案和埋点 |
| Story 7 | Campaign Backend, Scoring Service | 评分回调、结果落库 | 幂等、乱序、异常回调 | 错误出分或重复出分 | 回调签名、重试、补偿 |
| Story 8 | Mini program, Campaign Backend, Asset config | CEFR 结果查询、资源映射 | 资源与等级一一对应 | 等级资源缺失 | 资源清单冻结与随机文案策略 |

### System Interaction Flow

1. 用户进入微信小程序活动页。
2. 小程序触发微信授权并获取 `unionId`。
3. 小程序向 Campaign Backend 查询当前活动 `activity_status` 和用户 `submission_status`。
4. 若允许参与，用户进入录音页，Backend 返回或确认 `questionId`。
5. 用户完成录音后，小程序将 mp3 上传到 Upload/Storage，并提交文件元数据到 Backend。
6. Backend 创建提交记录与评分任务，将状态更新为 `pending_result`。
7. 用户进入等待页并可分享活动。
8. Scoring Service 完成评分后调用回调接口，将 `cefrLevel` 等结果回传给 Backend。
9. Backend 校验回调并落库结果，将状态更新为 `result_ready`。
10. 用户从首页或等待页进入结果页，查看 CEFR 等级结果。

### Service Boundary Table

| Service / Component | Owns | Does NOT Own | Notes |
|---|---|---|---|
| Mini program | 页面交互、录音控制、分享触发、状态渲染 | 活动状态真相、评分结果计算、最终持久化 | 前端只渲染，不自行推断业务状态 |
| Campaign Backend / BFF | `unionId` 绑定、状态机、防重复提交、结果查询 | 微信底层授权能力、AI 评分算法内部逻辑 | 建议以 `submissionId` 串联全链路 |
| Upload / Storage | 音频接收、对象存储、文件元数据关联 | 业务状态流转、评分规则 | 需补偿处理孤儿文件 |
| Scoring Service | 评分处理、CEFR 输出、回调回传 | 活动规则、前端页面逻辑 | 回调必须可幂等、可重试 |
| IOC admin / 运营配置 | 活动开关、Banner、题图素材配置 | 用户前台交互、评分计算 | 活动结束态由配置驱动 |

### Key Technical Decisions

| Topic | Decision | Reason |
|---|---|---|
| User identity | 使用微信授权后的 `unionId` 作为活动用户唯一标识 | 支撑一人一次提交和结果回查 |
| Submission constraint | 使用 `unionId + campaignId` 作为唯一成功提交约束 | 防刷量、防重复样本 |
| Scoring mode | 使用异步评分回调 | 用户等待期较长，不应阻塞主流程 |
| Storage strategy | 文件上传与业务提交分离，但提交成功需满足上传成功 + 元数据落库 + 评分任务创建 | 避免假成功和孤儿状态 |
| Idempotency | 上传和回调都需要幂等键 | 防止重复点击和重复回调 |
| Config mode | 活动结束 Banner 与入口禁用由后台配置驱动 | 便于运营控制活动周期 |

### Status Mapping

#### `activity_status`

| Status Value | Meaning | User-facing Behavior |
|---|---|---|
| `active` | 活动进行中 | 未提交用户可继续录音和上传 |
| `ended` | 活动结束 | 未提交用户不可再进入录音流程 |

#### `submission_status`

| Status Value | Meaning | Entry CTA | Allowed Page |
|---|---|---|---|
| `not_submitted` | 当前活动下还没有成功提交记录 | 立即参与 / 开始口语测评 | 入口页、录音页 |
| `pending_result` | 提交成功，等待评分回传 | 等待测评结果 | 入口页、等待页 |
| `result_ready` | 结果已回传且可展示 | 查看结果 | 入口页、结果页 |

### Open Questions for Engineering

- 回调接口的签名校验、失败响应格式和最大重试次数需在工程评审阶段确认。
- 分享链接的目标路径、分享素材和埋点字段需在 UX / Engineering Review 阶段定稿。

## 10. Business Process Flow

### Happy Path

1. 用户进入活动首页。
2. 用户点击 CTA 并完成微信授权。
3. 系统识别 `unionId`，判断活动进行中且用户未提交。
4. 用户进入录音页，系统随机展示 1 张 Part 2 题图。
5. 用户开始录音，最长 2 分钟，录音结束后可播放、重录或上传。
6. 用户点击上传，系统保存 mp3 与元数据，并创建唯一提交记录。
7. 页面进入等待结果页，展示 1 周内返回结果的固定文案。
8. 评分系统异步回调后，系统更新状态为 `result_ready`。
9. 用户点击首页 CTA 或等待页跳转进入结果页。
10. 结果页展示 CEFR 等级、背景图、固定链接和随机鼓励文案。

### Exception Path 1: 活动结束且用户未提交

1. 用户进入活动首页。
2. 系统返回 `activity_status=ended` 且 `submission_status=not_submitted`。
3. 页面展示活动结束 Banner。
4. 两个 CTA 统一展示“谢谢参与，活动结束”。
5. 用户无法进入录音页。

### Exception Path 2: 上传或回调异常

1. 用户完成录音并点击上传。
2. 若上传失败或任务创建失败，系统提示重试，不展示成功提交。
3. 若上传成功但评分结果迟迟未回传，用户保持在等待页。
4. 若回调缺失必要字段或 `cefrLevel` 非法，系统不出分并保留 `pending_result`。
5. 运营或技术通过补偿机制排查和重试。

## 11. GWT Scenarios

### Scenario 1: 首次参与并成功提交

GIVEN 用户首次进入活动页  
AND 当前活动处于 `active` 状态  
WHEN 用户完成微信授权、录音并上传 mp3  
THEN 系统创建唯一提交记录  
AND 用户进入等待结果页

### Scenario 2: 活动结束后未提交用户不可参与

GIVEN 用户进入活动页  
AND 当前活动处于 `ended` 状态  
AND 当前 `submission_status=not_submitted`  
WHEN 用户查看首页 CTA  
THEN 页面展示“谢谢参与，活动结束”  
AND 用户无法进入录音页

### Scenario 3: 重复上传被拦截

GIVEN 用户在当前 `campaignId` 下已有成功提交记录  
WHEN 用户再次尝试上传录音  
THEN 系统拒绝创建新的提交记录  
AND 页面提示每位用户仅可提交一次

### Scenario 4: 等待中的用户收到结果后查看结果页

GIVEN 用户当前 `submission_status=pending_result`  
WHEN 评分回调成功写入结果  
THEN 系统将状态更新为 `result_ready`  
AND 用户再次进入等待页时自动跳转到结果页

### Scenario 5: 结果异常时不展示错误等级

GIVEN 用户尝试查看结果页  
WHEN 当前结果缺失 `cefrLevel` 或资源缺失  
THEN 系统不展示正式结果页  
AND 用户看到稍后重试提示

## 12. Roadmap with Phases

### MVP

- 微信授权与 `unionId` 绑定
- 首页 CTA 状态机与活动结束配置
- 38 套题随机展示
- 录音、停止、重录、播放、上传
- 一人一次成功提交控制
- 等待页与分享入口
- 异步结果回调与 CEFR 结果页

### Phase 2

- 分享链路埋点和裂变来源追踪
- 结果页内容个性化固定策略
- 更丰富的活动配置项，例如题库分组与时间窗控制
- 回调补偿后台可视化

### Future Extension

- Part 1 / Part 3 活动扩展
- 多轮活动与多 campaign 复用
- 更细粒度口语反馈和学习推荐链接

## 13. User Journey / User Flow

| Stage | User Goal | User Action | System Feedback | Potential Pain Point |
|---|---|---|---|---|
| Entry | 判断是否可以参与 | 打开首页、点击 CTA | 展示当前状态对应文案 | 不知道自己为什么不能继续 |
| Authorization | 完成身份识别 | 同意微信授权 | 建立 `unionId` 活动身份 | 拒绝授权后无法继续 |
| Recording | 完成口语回答 | 查看题图、开始录音、停止录音 | 展示倒计时和后处理按钮 | 麦克风权限、时间压力 |
| Submission | 成功上传录音 | 点击上传 | 进入等待结果页 | 上传失败或担心重复提交 |
| Waiting | 确认已提交 | 查看固定文案、分享活动 | 告知 1 周内返回结果 | 等待过长焦虑 |
| Result | 查看测评结果 | 点击查看结果 | 展示 CEFR 等级和鼓励文案 | 结果异常或迟迟不出分 |

## 14. Non-functional Requirements

### Performance

- 首页状态查询响应需足够快，避免影响 CTA 判断。
- 录音页启动和停止操作需稳定，倒计时不可明显漂移。

### Compatibility

- 支持主流微信小程序环境。
- 对不支持录音或未授权麦克风的设备提供明确失败反馈。

### Data Limits

- 音频文件格式为 mp3。
- 最大录音时长为 2 分钟。
- 每个 `unionId + campaignId` 最多仅允许 1 次成功提交。

### Retry Strategy

- 授权失败允许用户重新授权。
- 上传失败允许基于当前录音重新提交。
- 评分回调失败由服务侧按约定次数重试，补偿策略待工程评审确认。

### Logging and Monitoring

- 关键日志需包含 `traceId`、`submissionId`、`unionId_hash`、`activity_status`、`submission_status`、`error_code`。
- 监控上传失败率、回调失败率、长时间 `pending_result` 数量。

### Security

- 仅存储业务所需用户标识与录音元数据。
- 结果查询必须基于当前用户绑定身份和提交记录。

## 15. Capacity Summary

| Metric | Value |
|---|---|
| Total Features | 4 |
| Total Stories | 8 |
| Total Estimated Units | 18 - 36 units |
| Total Estimated Effort | 9 - 18 days |
| Suggested Sprint Count | 2 - 3 sprints |
| Suggested Team Count | 1 cross-functional squad |
| Main Complexity Drivers | 微信授权、录音上传、单次提交、防重、异步回调、CEFR 结果展示 |
| Main Assumptions | 评分服务和活动配置后台可提供基础能力；结果页素材可按等级配置 |

## 16. Estimation Disclaimer

以上估算属于 PRD 阶段 planning-level estimation，仅用于范围判断、资源预估和优先级决策，不代表研发最终承诺。最终单位估算、任务拆分和排期以 Eng Reviewer / Task Planner refinement 为准。

## 17. Future Extension Ideas

- 增加分享后回流激励和渠道归因分析
- 为结果页提供更多 IELTS 学习资源跳转
- 将活动能力复用到更多 speaking mock campaign
- 支持多语言活动文案和多套运营素材配置

## Open Questions

详见同目录决策清单：`spkchallengpart2-miniprogram-decision-checklist.md`。

- 分享链接、分享标题、分享图片和来源埋点字段尚未确认。
- 评分回调的失败重试次数、人工补偿入口和异常告警阈值尚未确认。

## 待沉淀规则

- 同一活动内每个 `unionId` 仅允许 1 次成功提交；重复提交必须命中既有状态并拒绝创建新提交。
- 入口页两个 CTA 必须共用同一状态机，不允许出现文案或跳转行为不一致。
- 活动结束只阻止未提交用户发起新录音；已提交用户仍应按结果生命周期查看等待页或结果页。
- 音频提交成功必须同时满足 mp3 上传成功、元数据落库成功、评分任务创建成功。
- 异步评分回调必须具备幂等键；重复回调不得生成重复结果。
- 提交生命周期状态统一使用 `not_submitted`、`pending_result`、`result_ready`。
- 所有包含重新录音确认弹窗的 Story，必须分别覆盖 Confirm、Cancel、右上角关闭三种关闭方式。
- 所有涉及 CTA 的 Story，必须显式列出文案、可点击状态、点击后行为三项。
- 所有涉及 CEFR 展示的 Story，必须显式列出有效等级值集合、等级资源映射和文案池归属规则。
- 结果页鼓励文案必须在 `submissionId` 首次生成时随机一次，并在后续访问中保持固定。

## 已沉淀规则索引

- Layer 1 / 1.1 角色与权限：用户通过微信授权建立唯一 `unionId` 活动身份。
- Layer 1 / 1.3 业务流程约定：同一活动内每个 `unionId` 仅允许 1 次成功提交。
- Layer 1 / 1.3 业务流程约定：入口页两个 CTA 共用同一状态机。
- Layer 1 / 1.3 业务流程约定：活动结束仅阻止未提交用户发起新录音，已提交用户仍可查看等待或结果状态。
- Layer 2 / 2.1 集成模式：音频提交成功需满足上传成功、元数据落库成功、评分任务创建成功。
- Layer 2 / 2.2 异步与回调策略：提交生命周期状态统一为 `not_submitted`、`pending_result`、`result_ready`。
- Layer 2 / 2.2 异步与回调策略：异步评分回调必须具备幂等键。
- Layer 3 / 3.1 字段校验约定：涉及 CTA 的 Story 必须显式列出文案、可点击状态和点击后行为。
- Layer 3 / 3.2 状态机约定：CEFR 展示必须列出有效等级值集合与资源映射。
- Layer 3 / 3.4 通知与触发约定：结果页鼓励文案必须在 `submissionId` 首次生成时随机一次，并在后续访问中保持固定。
- Layer 3 / 3.4 通知与触发约定：重新录音确认弹窗必须分别覆盖 Confirm、Cancel、右上角关闭三种关闭方式。