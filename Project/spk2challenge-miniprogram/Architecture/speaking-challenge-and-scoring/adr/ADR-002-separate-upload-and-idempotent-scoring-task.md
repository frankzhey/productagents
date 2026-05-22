---
adr_id: ADR-002
slug: separate-upload-and-idempotent-scoring-task
status: proposed
created: 2026-05-22-1500
maintainer: "@frankzhey"
related_epics: [speaking-challenge-and-scoring]
supersedes: null
superseded_by: null
tags: [upload, idempotency, task-state]
---

# ADR-002: 上传与评分任务分离并使用幂等任务创建

## Status
proposed

## Context
Solution 规则要求音频先上传并落对象存储，再创建评分任务；无效录音不得进入评分链路；重复请求必须复用既有 taskId 或等效结果。

## Decision
Upload Gateway 负责音频上传、服务端有效性校验和 `audioRef` 生成；Scoring Job Service 基于 `userId + challengeId + audioRef + clientRequestId` 派生 `idempotencyKey` 创建或复用评分任务。

## Architecture Principle Applied
Single Responsibility；Idempotency by Design；State Explicitness。

## Consequences

### Positive
- 上传失败、音频无效、任务创建失败可以分别定位。
- 重复提交、断网重试和刷新重提不会重复消耗 AI 评分资源。
- `voice_submission` 与 `scoring_task` 分离有利于审计和补偿。

### Negative
- 多一个状态边界，需要处理 upload complete 但 task create failed 的补偿。
- 需要定义临时上传缓存清理策略。

### Risks
- 幂等键字段如果选取不当，可能误复用或重复建任务。

## Alternatives Considered

### Alternative 1: 上传接口直接触发评分
- Pros: 接口数量少，链路表面简单。
- Cons: 无法清晰区分上传有效性与评分任务，失败排查困难。
- 否决原因: 不满足 Solution 的上传与任务分离规则。

### Alternative 2: 前端生成 taskId 并直接上传到评分服务
- Pros: 后端编排少。
- Cons: 暴露内部评分边界，权限、审计和幂等难控制。
- 否决原因: 不符合服务边界和数据安全要求。

## References
- Solution §12 已沉淀规则索引
- Architecture §3.2 ERD
