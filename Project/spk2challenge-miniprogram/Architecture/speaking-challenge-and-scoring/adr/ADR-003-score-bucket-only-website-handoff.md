---
adr_id: ADR-003
slug: score-bucket-only-website-handoff
status: proposed
created: 2026-05-22-1500
maintainer: "@frankzhey"
related_epics: [speaking-challenge-and-scoring]
supersedes: null
superseded_by: null
tags: [handoff, privacy, attribution]
---

# ADR-003: Website 深链只透传 score bucket 与归因参数

## Status
proposed

## Context
小程序需要把高意向用户导流到 website，但评分结果含敏感学习数据。Solution 已定义 website 深链只透传来源场景和 score bucket，不透传原始敏感分数。

## Decision
小程序结果页调用 Handoff Attribution Service 生成 signed handoff URL，仅包含 source scene、score bucket、attribution id 和短 TTL 签名；website 根据 bucket 做默认承接或个性化入口。

## Architecture Principle Applied
Privacy by Design；Least Data Disclosure；Traceable Attribution。

## Consequences

### Positive
- 降低跨端传播原始分数的隐私风险。
- website 仍可按低敏分层承接不同用户意图。
- attribution id 支持 K3/K5 追踪而不泄露原始评分。

### Negative
- website 无法仅靠深链展示精确分数，需要另行授权或账号打通。
- score bucket 分层策略需要产品和增长共同治理。

### Risks
- 如果未来要求跨端展示完整历史结果，需要新增账号打通和权限校验设计。

## Alternatives Considered

### Alternative 1: 深链传原始 band score
- Pros: website 可直接展示更精确承接内容。
- Cons: 敏感学习结果跨端扩散，日志、URL、第三方分析都可能泄露。
- 否决原因: 不符合 NFR 数据安全和 Solution 边界。

### Alternative 2: 不传任何分层，仅固定落地页
- Pros: 最安全、实现最简单。
- Cons: 无法利用评分结果提升转化，影响 K3/K5。
- 否决原因: 业务价值不足，无法支撑导流优化。

## References
- Solution §4 Unhappy Path 5
- NFR §4 Data Sensitivity & Security
