# Context Memo — Mini Program 口语练习引流与 AI 素材采集

> 生成时间: 2026-04-17
> 生成方式: Knowledge Retriever (ADO Wiki 检索)
> 有效范围: 本 Epic 内所有 Agent（Product Planner / UX / Eng Reviewer / Task Planner）
> 重要提醒: 本文件为 Epic 级缓存，后续 Agent 直接读取此文件，不再重复触发 ADO 搜索。

---

## 1. 参考页面清单

| 页面名称 | ADO 路径 | 相关度说明 |
|---|---|---|
| Mini Program IELTS Speaking Part 2 Campaign Submission and Result Experience | /mini-program-ielts-speaking-part-2-campaign-submission-and-result-experience | 与当前需求在渠道（Mini program）、目标（引流）、业务目标（采集口语音频）三项直接重叠。该页提供完整 PRD、状态流转和运营约束，可直接复用为本 Epic 的产品骨架。 |
| Engineering Review - Mini Program IELTS Speaking Part 2 Campaign Submission and Result Experience | /mini-program-ielts-speaking-part-2-campaign-submission-and-result-experience/engineering-review | 覆盖了 async scoring、callback 幂等、一次性提交、防重与日志追踪，和当前“素材采集+后续 AI 处理”强相关。可直接作为工程边界、错误处理和可观测性模板。 |
| Mini Program Speaking Part 2 UX 方案 | /mini-program-ielts-speaking-part-2-campaign-submission-and-result-experience/ui-prototype | 提供了首页/录音页/等待页/结果页的完整 UX 状态机，包含 loading/错误/成功等关键状态。非常适合当前引流活动场景下的最小可用交互闭环。 |
| PRD — IELTS Speaking Mock · WeChat Mini Program | /ielts-speaking-mock | 虽然目标更偏“完整 mock 评测”，但与当前需求在录音上传、等待态、结果页、CEFR 映射高度重合。可用于补全更细粒度 User Story、AC 与埋点指标定义。 |

---

## 2. 可复用 Patterns（核心）

### Product Patterns
- 漏斗型活动闭环：授权进入 -> 抽题录音 -> 最终提交 -> 等待态 -> 结果页 -> 分享传播。适用性：✅ 直接复用（当前目标即引流+素材采集）。
- 一次性最终提交（per unionId）控制重复参与，提升样本质量并避免刷量。适用性：✅ 直接复用。
- 活动态与用户态双维度渲染（进行中/结束 + 未提交/待评分/已出分）。适用性：⚠️ 需调整（若本期不出分，需将“已出分”替换为“已提交待通知”）。

### UX Patterns
- 首页双 CTA 但单一状态源，避免按钮行为不一致。适用性：✅ 直接复用。
- 录音页必须显式覆盖权限拒绝、录音中断、上传失败、重录确认。适用性：✅ 直接复用。
- 等待页固定文案 + 分享入口，降低焦虑并增强传播。适用性：✅ 直接复用。
- 结果页 CEFR 可视化+鼓励文案。适用性：⚠️ 需调整（若当前仅采集素材，可先用“提交成功/后续通知”页替代）。

### Engineering Patterns
- 架构边界：前端只做交互，后端持有状态机真相。适用性：✅ 直接复用。
- 评分链路异步化（queue + callback + reconciliation job）。适用性：⚠️ 需调整（若首期不做即时评分，可保留队列与回调接口，延后实时结果展示）。
- callback 幂等（submissionId + callbackId）与版本防乱序。适用性：✅ 直接复用。
- 上传与业务落库分离，需补偿任务处理“文件成功但业务失败”。适用性：✅ 直接复用。
- 可观测性三键：traceId + submissionId + unionId_hash。适用性：✅ 直接复用。

---

## 3. 系统边界速查

| 系统 | Owns | Does NOT Own | 历史决策备注 |
|---|---|---|---|
| Mini program | 页面交互、录音控制、上传触发、分享触发 | 状态真相、评分算法、最终结果写入 | 前端只渲染状态，不自行推断业务状态 |
| Campaign Backend/BFF | unionId 绑定、状态机、防重复提交、结果查询 | 微信底层能力、AI 模型内部逻辑 | 业务主键以 submissionId 串联全链路 |
| Upload/Storage | 文件接收、对象存储、文件元数据关联 | 业务状态流转、评分规则 | 文件记录与业务记录分离，失败要可补偿 |
| Scoring Service | 评分处理、CEFR 输出、callback 回传 | 活动规则、前台交互 | callback 必须可验签、可重试、可幂等 |
| IOC admin/运营配置 | 活动开关、Banner、题图/文案素材配置 | 用户前台流程、评分计算 | 活动结束态与素材切换由配置驱动 |
| Touch points | 埋点采集、漏斗分析 | 业务状态决策 | 仅用于分析，不反向驱动状态 |

---

## 4. 可复用技术决策

- 是否 async scoring: 历史方案为异步（提交后入队，回调落库）。当前适配建议：⚠️ 若首期重点是引流+素材采集，可先异步离线评分，不阻塞主流程。
- callback 策略: 历史方案为 callback 主导 + 后台 reconciliation。当前适配建议：✅ 直接复用，保留补偿与人工重放能力。
- 用户绑定方式（unionId）: 历史方案为微信授权后 unionId 唯一绑定参与记录。当前适配建议：✅ 直接复用。
- 文件上传策略: 历史方案为上传初始化凭证 + finalize 提交 + 音频与业务分表。当前适配建议：✅ 直接复用。
- 提交次数限制: 历史方案为 per unionId/per campaign 一次最终提交。当前适配建议：✅ 直接复用。
- CEFR 映射方式: 历史方案为 Band 到 CEFR 映射并结果页展示。当前适配建议：⚠️ 若首期无即时评分，先不前台展示 CEFR，仅保留字段与映射表。 

---

## 5. 历史踩坑 / 风险提醒

- unionId 获取链路不稳定：在不同授权场景可能拿不到稳定标识，当前同样适用，需要 fallback 策略与失败兜底页。
- 上传成功但 finalize 失败：会出现孤儿文件，当前同样适用，需补偿任务和清理策略。
- callback 重复/乱序：容易污染结果状态，当前同样适用，必须做幂等和版本检查。
- 等待态过长导致流失：用户不知道是否成功，当前同样适用，需可预期 SLA 文案与提醒机制。
- 活动结束态优先级错误：可能误伤已提交用户，当前同样适用，需状态优先级矩阵。

---

## 6. 当前 Epic 适用性说明

> 直接可复用：状态机、防重、上传链路、埋点框架。 
> 需要调整：实时出分和 CEFR 结果展示节奏（按“引流优先”可后置）。
> 不建议直接复用：重评测导向的复杂评分页面（首期会增加转化路径阻力）。

| Pattern | 适用性 | 备注 |
|---|---|---|
| unionId 唯一绑定 + 一次性提交 | ✅ 直接复用 | 与“搜集有效 AI 素材”目标完全一致 |
| 首页/录音/等待三段式漏斗 | ✅ 直接复用 | 最小化路径、提升提交转化 |
| async scoring + callback 幂等 | ⚠️ 需调整 | 可保留后台能力，前台暂不承诺即时出分 |
| CEFR 结果强曝光页 | ⚠️ 需调整 | 取决于首期是否上线评分能力 |
| 四维细分打分与长反馈 | ❌ 不适用 | 首期目标是引流采集，不宜增加交互负担 |
| 活动配置化（开关/Banner/题图） | ✅ 直接复用 | 便于快速运营迭代 |

---

## 7. 对下游 Agent 的建议

**Product Planner：**
- 先定义双 North Star：有效提交 unionId 数、可用音频样本数。
- Story 优先级建议：授权与身份 -> 录音上传 -> 提交防重 -> 等待态与分享 -> 可选结果页。
- AC 必须覆盖失败路径：授权失败、上传失败、重复提交、活动结束、回调延迟。

**UX Prototyper：**
- 优先打磨首屏 CTA 与录音页可完成率，减少分叉操作。
- 强化等待态可信反馈（预计时长、已提交确认、分享引导）。
- 若无即时评分，结果页改为“提交成功+后续通知+二次触达入口”。

**Eng Reviewer：**
- 先落状态机与幂等策略，再接入评分能力。
- 必须定义回调签名校验、重试次数、补偿任务、失败告警阈值。
- 强制日志字段：traceId、submissionId、unionId_hash、state_from/state_to、error_code。

**Task Planner：**
- 估算参考：历史同类 Epic 总量约 37–74 units，首期引流采集版可裁剪到约 20–35 units。
- 高风险任务优先拆分：一次性提交防重、上传 finalize 事务、状态机一致性、异常补偿。
- 外部依赖需单列任务：微信授权、对象存储、评分服务契约（如启用）。
