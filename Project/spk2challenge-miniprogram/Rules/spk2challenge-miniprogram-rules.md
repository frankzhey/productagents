# Project Rules

## Layer 1

- Strategy pillar: ai_assessment
- Target channel: TOC miniprogram
- Primary user: 可能考雅思的考生

## Layer 2

- MVP 口语评分链路默认采用短轮询查询结果，不在首版引入复杂 callback 回传。
- 短轮询默认每 2 秒查询一次评分任务状态，最长查询窗口 30 秒；超过后进入超时恢复路径。
- 音频先上传并落对象存储，再创建评分任务；上传成功与任务创建必须分离建模，便于失败排查与可控重试。
- website 深链默认透传来源场景和 score bucket，不透传原始敏感分数。
- website handoff 只允许透传 `sourceScene` / `scoreBucket` / `attributionId` / 签名参数。
- 评分任务创建必须支持幂等，重复请求复用既有 `taskId` 或等效结果，不得重复建任务。
- 评分任务状态至少覆盖 `created` / `queued` / `processing` / `succeeded` / `failed` / `timeout` / `cancelled`。

## Layer 3

- 结果页必须展示“练习参考，非官方成绩”性质说明。
- 评分超时与失败必须给出继续等待、刷新结果或重新提交的清晰恢复路径。
- 无效录音（空录音、过短、损坏）不得进入评分链路。
- score bucket 仅允许 `low` / `mid` / `high` / `unknown`，不得透传原始敏感分数。
- 评分结果解释必须记录 `mappingVersion` 和 `templateVersion`，用于追踪 IELTS band / CEFR / 建议模板口径。

## Pending Sedimentation

- 待 Value Frame 落盘后补充角色边界、数据范围、KPI 口径和战略约束。
