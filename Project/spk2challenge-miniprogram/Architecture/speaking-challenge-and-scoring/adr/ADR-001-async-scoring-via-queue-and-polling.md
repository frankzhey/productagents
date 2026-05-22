---
adr_id: ADR-001
slug: async-scoring-via-queue-and-polling
status: proposed
created: 2026-05-22-1500
maintainer: "@frankzhey"
related_epics: [speaking-challenge-and-scoring]
supersedes: null
superseded_by: null
tags: [async, scoring, queue, polling]
---

# ADR-001: 异步评分通过队列与短轮询实现

## Status
proposed

## Context
用户提交录音后，AI 评分耗时不可稳定落在同步 API 响应窗口内。Value 与 NFR 要求评分平均 <=20s、最大 <=30s，同时 Solution 明确 MVP 默认短轮询，不引入复杂 callback。

## Decision
评分链路采用异步队列 + worker pool 执行 AI scoring，小程序以短轮询查询 `scoring_task.status` 与最终结果。

## Architecture Principle Applied
Async by Default；Failure-first Design；Observable Workflow。

## Consequences

### Positive
- 解耦用户提交与 AI 评分耗时，避免同步请求超时。
- 队列可吸收峰值并支持 worker 横向扩容。
- taskId 状态机天然支持失败恢复、投诉排查和 K6/K7 监控。

### Negative
- 前端需要处理中状态和短轮询策略。
- 需要设计队列积压、worker 崩溃、任务 timeout 的运维机制。

### Risks
- AI vendor SLA 未确认时，<=20s 平均耗时存在落地风险。

## Alternatives Considered

### Alternative 1: 同步评分
- Pros: 前端交互简单，API 一次返回结果。
- Cons: AI 耗时不稳定，容易超过 API timeout，无法吸收峰值。
- 否决原因: 不满足异步 AI scoring 的可靠性和恢复要求。

### Alternative 2: WebSocket / Server Push
- Pros: 用户可实时接收结果，减少轮询请求。
- Cons: 小程序、后端连接保持和运维复杂度更高，MVP 不需要。
- 否决原因: Solution 明确 MVP 不引入复杂 callback / push。

## References
- Solution §7.1 技术方向
- NFR §1 Performance SLA
