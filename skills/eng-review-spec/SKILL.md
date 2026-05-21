---
name: eng-review-spec
description: Engineering Review 写作规范 v2.0——纯评审动作（不再写设计）。章节锚点 8 类评审动作：§0 Scope Challenge / §2 Architecture Challenge Checklist（消费 IT Architect 产出）/ §3 Blast Radius / §4 NFR Verification（消费 NFR LATEST）/ §5 Capacity 偏差 / §6 AC 合规（消费 ac-writing-spec）/ §7 Task Planning Readiness / §X Coverage Verification（警示）。两类 Refinement Request 模板（Architecture / NFR）。Eng Reviewer 在产出评审前必须 Read 本文件。
version: 2.0.0
updated: 2026-05-19
maintainer: @frankzhey
applies-to: [eng-reviewer]
---

# Engineering Review 写作规范 v2.0（纯评审版）

本 SKILL 定义 Engineering Review 的**纯评审动作**章节结构、Architecture Challenge Checklist、Blast Radius 五维评估、NFR Verification、AC 合规校验、Coverage Verification（警示）等评审动作的统一规范。

> **v2.0 核心变化**：
> - **删除 13 项设计动作**（C2 / C3 / ERD / API / Error / Retry / Logging / NFR 等已下放到 IT Architect / NFR Architect / Product Planner）
> - **新增 8 类纯评审动作**（评审 IT Architect 产出、NFR 产出、PRD AC、Coverage Matrix）
> - **新增反向 Refinement Request**（评审发现问题 → 输出 RR → PM Confirm 后触发 IT Architect / NFR Architect refinement）

Eng Reviewer 在产出评审前必须显式 Read 本文件并按章节锚点执行。

---

## §1 章节锚点（v2.0 重新定义为评审动作）

| § | 章节 | 强制 | 说明 |
|---|---|---|---|
| §0 | Scope Challenge | ✅ | 最小变更集 + Complexity Smell（独占评审动作）|
| §1 | Review Scope | ✅ | 范围 / 输入文档清单 / 假设 / 不在范围内 |
| §2 | Architecture Challenge Checklist ⭐ v2.0 新增 | ✅ | 消费 IT Architect 三层架构产出，按 §6 题库逐项挑战 |
| §3 | Blast Radius Assessment（爆炸半径）| ✅ | 评审 IT Architect ADR + Key Decisions，五维评估 |
| §4 | NFR Verification ⭐ v2.0 新增 | ✅ | 校验 IT Architect QAS 是否覆盖 NFR LATEST 全部 8 类 |
| §5 | Capacity 偏差核验 | ✅ | PRD §7 Capacity vs Solution §6 Phase-level Workload 偏差 |
| §6 | AC 合规校验（按 ac-writing-spec §1-§3）⭐ 阻塞性 | ✅ | 消费 PRD §3 Stories+AC，逐 Story 校验格式 + 覆盖 + 强制规则 |
| §7 | Task Planning Readiness | ✅ | 每个 Story 的 Domain / Dependency / Complexity / Risk |
| §X | Coverage Verification（警示性 · v2.0 新增） | ✅ flag | 评审 PRD §X Coverage Matrix，Solution Risk → AC 追溯 + 8 类场景维度覆盖率 |
| §8 | Risks / Open Questions | ✅ | 技术 / 集成 / 数据 / 依赖 / 未决 + 各章节 flag 汇总 |
| §9 | Architecture Refinement Request ⭐ v2.0 新增 | ⭕ 仅有时 | 反向能力：评审发现 IT Architect 设计问题 |
| §10 | NFR Refinement Request ⭐ v2.0 新增 | ⭕ 仅有时 | 反向能力：评审发现 NFR Targets 问题 |
| §11 | Wiki Publishing Metadata | ✅ | epic name / page type / source |

---

## §2 §0 Scope Challenge（独占评审动作 · 先行强制）

### 2.1 最小变更集三问（必答）

| 问题 | 要求 |
|---|---|
| 最小可行变更集 | 必须改动的系统 / 服务 / 数据表 |
| 哪些可以推迟 | 可延迟功能点或技术优化 |
| 哪些看似必须可绕过 | 临时方案替代的复杂集成 |

### 2.2 Complexity Smell 5 条触发

满足任一条件 → 必须显式 Flag：

- 📁 变更文件数 > 8 个
- 🔧 新增服务数 > 2 个
- 🔗 集成点 > 3 个
- 🔄 数据迁移 + 新功能同时交付
- 📦 跨越 3 个以上渠道（Mini program / Website / 3Ups）行为不同

### 2.3 输出格式（强制）

```text
Scope Challenge:
  最小变更集:
    必须改动: [系统 / 服务 / 表]
    可延迟:   [功能点 / 优化项]
    可绕过:   [复杂依赖的替代方案]

  Complexity Smell: [无 / ⚠️ 已 Flag]
    （Flagged 时附）
    触发原因: [条件描述]
    建议: 拆分 Phase 或与 PM 对齐缩减范围后再进入详细评审

  结论: 【范围合理，继续评审 / 建议缩减后重新对齐】
```

---

## §3 §1 Review Scope（v2.0 强化输入文档清单）

```markdown
## §1 Review Scope

### 1.1 评审范围
{本次评审覆盖的 Epic / Feature / 系统}

### 1.2 输入文档清单（v2.0 强化）
| 来源 | 路径 | timestamp | status |
|---|---|---|---|
| Value Frame | /{project} (Wiki) 或 Project/{p}/Value/LATEST.md | {timestamp} | ✅ synced / ⚠️ stale |
| Solution Brief | /{project}/{epic}-solution (Wiki) 或 Project/{p}/Solution/{epic}/LATEST.md | {timestamp} | ✅ synced / ⚠️ stale |
| PRD | /{project}/{epic}-PRD (Wiki merged) 或 Project/{p}/PRD/{epic}/LATEST.md | {timestamp} | ✅ approved / ⚠️ draft |
| **IT Architecture** ⭐ v2.0 必拉 | /{project}/{epic}-PRD/architecture 或 Project/{p}/Architecture/{epic}/LATEST.md | {timestamp} | ✅ 存在 / ❌ 缺失 |
| **NFR LATEST** ⭐ v2.0 必拉 | /{project}/{epic}-PRD/nfr 或 Project/{p}/NFR/{scope}/LATEST.md | {timestamp} | ✅ 存在 / ❌ 缺失 |
| UX (可选) | /{project}/{epic}-PRD/ui-prototype | {timestamp} | ✅ / ⭕ |

### 1.3 关键假设
{评审基于哪些前置假设}

### 1.4 不在范围内
{明确边界外的内容}
```

---

## §4 §2 Architecture Challenge Checklist（v2.0 新增 · 核心评审动作）

> 这是 Eng Reviewer v4.0 的**核心新动作**：评审 IT Architect 三层架构产出，按结构化题库逐项挑战。

### 4.1 题库结构（按 IT Architect 三层 + Cross-cutting + ADR）

#### A. Layer 1 挑战（业务上下文与边界）

- [ ] §1.3 C1 System Context 是否覆盖所有外部 actor（含微信 / AI vendor / 支付 / CDN / 短信）？
- [ ] §1.2 Persona 列表是否完整（含运营 / 管理员等内部角色）？
- [ ] §1.4 业务能力地图是否完整？
- [ ] §1.5 合规约束是否有对应技术应对？

#### B. Layer 2 挑战（应用与系统架构）

- [ ] §2.1 C2 Container 是否覆盖所有 Solution §2 Feature？
- [ ] §2.2 Service Boundary 是否有"孤立 Owner"（无人负责的组件）？
- [ ] §2.3 Deployment Topology 是否含多渠道 / 多 Region（按 NFR Geo）？
- [ ] §2.4 Runtime Stack 是否符合技术栈约束？
- [ ] §2.5 集成方案：同步/异步边界是否清晰？第三方接口是否含错误码映射？
- [ ] §2.5 Sequence：是否有 ≥1 happy + ≥1 failure？
- [ ] §2.6 Observability 是否含 trace + metric + log 三件套？
- [ ] §2.7 QAS 是否覆盖 NFR LATEST 全部 8 类（详见 §5 NFR Verification）？

#### C. Layer 3 挑战（组件与数据架构）

- [ ] §3.1 C3 Component 关键容器是否覆盖（AI 评分 / async pipeline 等复杂容器）？
- [ ] §3.2 ERD 是否含状态机字段 + 时间字段 + ownership？
- [ ] §3.3 Data Flow 是否含 retention 标注 + source/sink 明确？
- [ ] §3.4 API 是否含 error model + retry + timeout 三件套？
- [ ] §3.5 Data Contract 是否含 schema evolution 策略？
- [ ] §3.6 Data Lineage & Retention 是否符合 NFR §6？
- [ ] §3.7 Security Auth Flow 是否清晰（unionId / OAuth / token）？

#### D. ADR 挑战

- [ ] ADR ≥ 3 条？
- [ ] 每个 ADR 含 ≥2 Alternatives Considered？
- [ ] 每个 ADR 含 Architecture Principle Applied 段（隐式融入设计原则）？
- [ ] 关键决策（同步/异步 / 存储选型 / 集成方式）是否都有 ADR 覆盖？

#### E. Cross-cutting 挑战

- [ ] §7.1 Security：认证流 + 加密 + 权限边界是否齐？
- [ ] §7.2 QAS：是否覆盖 Solution NFR Targets 全部 8 类？
- [ ] §7.4 Reliability & DR：是否含容灾策略 + RTO/RPO？
- [ ] §7.5 Operability：运维交付清单（部署 / 监控 / 回滚）是否完整？

#### F. 必画 SVG 完整性

- [ ] 强制 7 张 SVG 全部生成（C1 / C2 / Deployment / 2 Sequence / ERD / Data Flow）？
- [ ] diagrams-manifest.json 已登记？

### 4.2 输出格式

```text
## §2 Architecture Challenge Checklist

### A. Layer 1 挑战
- [x] §1.3 C1 覆盖所有 actor: ✅ 通过
- [x] §1.2 Persona 完整: ⚠️ 缺运营角色 → §8 OQ-1

### B. Layer 2 挑战
- [x] §2.1 C2 覆盖 Feature: ✅ 通过
- [x] §2.2 Service Boundary 无孤立 Owner: ✅ 通过
- [x] §2.5 同步/异步边界: ⚠️ AI 评分异步回调失败兜底策略不清 → §8 OQ-2 → §9 Architecture RR

### C. Layer 3 挑战 ...
### D. ADR 挑战 ...
### E. Cross-cutting 挑战 ...
### F. 必画 SVG 完整性 ...

总体结论: 【全部通过 / X 项需 Architecture Refinement】
```

任何 ⚠️ → 必须在 §8 OQ 或 §9 Architecture RR 中显式列出。

---

## §5 §3 Blast Radius Assessment（独占评审动作）

> 评审 IT Architect ADR + Key Decisions，每条决策从 5 维度评估爆炸半径。

### 5.1 五维评估表

| 维度 | 审查问题 |
|---|---|
| 系统范围 | 最多波及哪些系统？（列举 service 名）|
| 用户范围 | 最坏情况影响多少用户？（全量 / 渠道 / 角色） |
| 数据范围 | 是否会写错、丢失或不一致？涉及哪些表？ |
| 恢复成本 | 能否快速回滚？回滚代价？ |
| 隔离能力 | feature flag / canary 可否限制爆炸半径？ |

### 5.2 输出格式（每个决策附加）

```text
ADR-001 异步评分通过消息队列实现

爆炸半径评估:
  影响系统: [ICS / MQ / 评分服务]
  影响用户: 全量 Mini Program 用户（评分功能不可用）
  数据风险: 评分记录可能丢失（如 MQ 故障）→ 必须配死信队列
  回滚能力: 可回滚到同步评分（性能下降但功能可用）
  隔离方案: feature flag 控制异步开关
  风险等级: Medium
```

### 5.3 升级规则

风险等级 = **High** → 必须在 §8 Risks 中显式列出，并在 §9 Architecture RR 中提出改进建议。

---

## §6 §4 NFR Verification（v2.0 新增 · 核心评审动作）

> 校验 IT Architect §2.7 QAS 是否覆盖 NFR LATEST 全部 8 类。

### 6.1 NFR LATEST 加载

```text
优先级:
  1. Project/{project}/NFR/{epic-slug}/LATEST.md (Epic 级)
  2. Project/{project}/NFR/project-wide/LATEST.md (项目级回退)
  3. Wiki: /{project}/{epic-slug}-PRD/nfr (跨电脑 wiki-fallback)

缺失处理:
  NFR LATEST 不存在 → §4 输出 "NFR LATEST 缺失，本评审 NFR 部分基于 PRD §6 NFR 独立判断"
                    → §10 NFR Refinement Request: 建议 PM 调用 NFR Architect 产出
```

### 6.2 8 类 NFR 校验矩阵

| NFR 维度 | NFR LATEST 值 | IT Architect QAS 是否覆盖 | 一致性 | 备注 |
|---|---|---|---|---|
| §1 Performance SLA | p95 ≤ 500ms | ✅ QAS-1 引用 | ✅ | |
| §2 Availability SLA | 99.9% | ✅ QAS-2 引用 | ✅ | |
| §3 Capacity | DAU 10k / 1k QPS | ⚠️ QAS 未覆盖峰值 | ⚠️ | §8 OQ-3 |
| §4 Data Sensitivity | PII | ✅ Layer 3 §3.7 覆盖 | ✅ | |
| §5 Compliance | 等保二级 | ✅ Layer 1 §1.5 覆盖 | ✅ | |
| §6 Retention | 3 年 | ✅ Layer 3 §3.6 覆盖 | ✅ | |
| §7 Geo | 仅大陆 | ✅ Layer 2 §2.3 Deployment 单 region | ✅ | |
| §8 业务量级总览 | level: medium | ✅ §2.3 Deployment 按中档 | ✅ | |

### 6.3 不一致处理

- 任一 NFR 未被 IT Architect QAS 覆盖 → §8 OQ + §9 Architecture RR
- NFR 与 IT Architect 设计冲突（如 NFR 99.99% SLA 但 IT Arch 单点部署）→ §3 Blast Radius 标 High + §9 Architecture RR

### 6.4 输出格式

```text
## §4 NFR Verification

NFR LATEST: Project/{project}/NFR/{epic}/LATEST.md (last_synced: {stamp})
  ├─ ✅ 加载成功
  └─ ❌ 缺失 (回退到 PRD §6 或建议补做 → §10 NFR RR)

8 类 NFR 校验:
  [完整矩阵]

不一致项: X 项 → §8 OQ-3, OQ-4
不可达项: Y 项（如 SLA 与设计冲突）→ §9 Architecture RR
```

---

## §7 §5 Capacity 偏差核验

> PRD §7 Capacity Summary vs Solution §6 Phase-level Workload 偏差检测。

### 7.1 校验逻辑

```text
偏差 = (PRD Story-level 单位数 - Solution Phase-level 单位数) / Solution Phase-level 单位数

判定:
  |偏差| ≤ 30% → 合理
  |偏差| > 30% → flag + §8 OQ
```

### 7.2 输出格式

```text
## §5 Capacity 偏差核验

Solution §6 Phase-level Workload: 40-60 units
PRD §7 Story-level: 65-80 units
偏差: +35% (上偏)

结论: ⚠️ 超出 30% → §8 OQ-5: Story 拆细后超出 Solution 粗估，建议 Product Planner refinement 或重新评估 Phase 范围
```

---

## §8 §6 AC 合规校验（独占 · 阻塞性 · 引用 ac-writing-spec）

### 8.1 前置加载

```text
Read skills/ac-writing-spec/SKILL.md
```

把该 SKILL 的 §1（格式强制规范）/ §2（覆盖规范）/ §3（额外强制规则）/ §X 8 类场景维度索引（v1.1 新增） 作为 AC 合规校验清单。

### 8.2 三类合规检查（逐 Story 执行）

**8.2.1 格式合规（按 ac-writing-spec §1）**

| 检查项 | 标准 |
|---|---|
| 关键字大写 | GIVEN / WHEN / THEN / AND / BUT 大写独占行 |
| 无连写压缩 | 无 → / 单行连写 |
| 无非标语法 | 无 `AND WHEN` / `AND THEN` |
| 多场景独立 | 多场景拆为 AC1 / AC2 / AC3 |
| 字段名格式 | 反引号包裹 |
| 无 UI 视觉描述 | 无颜色 / 字号 / 布局 |
| 每条 AC 有标题 | `AC1：xxx` 格式 |

**8.2.2 覆盖规范（按 ac-writing-spec §2 + §X 8 类场景）**

- 操作流程类 Story：覆盖 A-1 / A-2 / A-8 / B-4 / B-6 五类
- 列表查询类 Story：覆盖 A-2 / A-3 / A-4 / B-1 / A-5 / A-6 / A-7 七类
- **v2.0 新增：8 类场景维度覆盖率 ≥ 6**（happy / unhappy / failure / edge / permission / state / retry / empty-expired-duplicate）

**8.2.3 额外强制（按 ac-writing-spec §3）**

- C-1 状态机覆盖
- B-3 按钮置灰覆盖
- B-2 表单校验五要素

### 8.3 输出格式（强制）

```text
## §6 AC 合规校验（阻塞性）

Story 总数: X 个
合规 Story 数: X 个
不合规 Story 数: X 个

不合规明细:
  Story [ID] - [Story 名称]:
    ❌ 格式问题: [如 "AC2 使用了 → 连写"]
    ❌ 覆盖缺失: [如 "操作类未覆盖 A-8 弹窗关闭"]
    ❌ 8 类场景维度: [如 "缺 retry 场景"]
    ❌ 强制规则: [如 "B-3 按钮置灰未覆盖"]
    建议: 退回 Product Planner 按 ac-writing-spec §X.Y 修正

结论: 【全部合规 / X 个 Story 不合规需返工】
```

### 8.4 不合规处理

- 全部合规 → 进入 §7 Task Planning Readiness
- 任一不合规 → **必须** 在 §8 Risks 中输出 AC 合规 flag：

```text
🚨 AC 合规风险:
  不合规 Story: [列表]
  建议: PM 让 Product Planner 重新跑 Quality Gate，修复后再交工程评审
  工程影响: AC 不合规 → 估算偏差 / QA 用例不完整 / 上线风险升高
```

---

## §9 §7 Task Planning Readiness（独占）

针对每个 Story 输出：

| 字段 | 必填值 |
|---|---|
| Recommended Domains | FE / BE / Integration / Data / QA / DevOps 中选 |
| Main Dependencies | 依赖的系统 / 组件 / 数据 |
| Main Complexity Drivers | 复杂度来源 |
| Suggested Breakdown Hints | 拆分建议 |
| Suggested Estimation Risk Level | Low / Medium / High |

并在 Story 级输出之外补充 **Refinement Notes for Task Planner**。

---

## §10 §X Coverage Verification（v2.0 新增 · 警示性）

> 评审 PRD §X Coverage Matrix（Solution Flow Risk → PRD AC 追溯 + 8 类场景维度覆盖率）。

### 10.1 校验逻辑

```text
读取 PRD §X Coverage Matrix
检查:
  1. Solution §5 流程难点与 PRD 拆解提示 每条 BP-X 是否在 Matrix 中找到，type=solution_risk？
  2. 每个 Feature 在 Matrix 中 8 类场景维度覆盖率 ≥ 6（推荐 ≥7）？
  3. 是否有 prd_extension 自补的场景？
```

### 10.2 输出格式（警示性 · 不阻塞）

```text
## §X Coverage Verification

Solution Flow Risk 追溯:
  BP-H1: ✅ 已追溯到 EPIC-{slug}-F1-S01 AC1-AC3
  BP-U1: ✅ 已追溯到 EPIC-{slug}-F1-S01 AC3-AC5
  BP-U2: ⚠️ 未追溯（仅在 OQ 中讨论，未拆 AC）→ §8 OQ-Coverage-1
  BP-U3: ✅ 已追溯

8 类场景维度覆盖率:
  Feature F1: 7/8 (缺 retry 场景) → §8 OQ-Coverage-2
  Feature F2: 6/8 (缺 retry + empty-expired) → ⚠️ flag
  Feature F3: 8/8 ✅

结论: ⚠️ Coverage Matrix 不完整（2 项追溯缺失 + F2 维度低）
  PM 决策:
    □ Accept Risk: PM 接受当前覆盖率，发布
    □ 退回 Product Planner refinement
```

> v2.0 决策：Coverage Verification 是**警示性**，不阻塞 PRD 发布；PM 可 accept risk 后续发布。

---

## §11 §8 Risks / Open Questions（汇总）

汇总各章节 flag：

- 技术风险 / 集成风险 / 数据风险 / 依赖风险 / 未决问题
- AC 合规 flag（§6 引入）
- Architecture Challenge flag（§2 引入）
- NFR Verification flag（§4 引入）
- Capacity 偏差 flag（§5 引入）
- Coverage 警示 flag（§X 引入）

---

## §12 §9 Architecture Refinement Request（v2.0 新增 · 反向能力）

> 评审发现 IT Architect 设计问题时，输出结构化 RR，PM Confirm 后触发 IT Architect refinement。

### 12.1 触发条件

- §2 Architecture Challenge 出现 ⚠️ 项
- §3 Blast Radius 出现 High 风险且需架构改动
- §4 NFR Verification 不一致或不可达

### 12.2 落盘 + 通知

```text
本地落盘:
  Project/{project}/EngReview/{epic-slug}/refinement-requests/architecture-refinement-{stamp}.md

Wiki 发布（可选）:
  /{project}/{epic-slug}-PRD/engineering-review/architecture-refinement-{stamp}

@ 通知:
  通过 IT Architect 产出 maintainer 字段（通常 @ITArch）定位
  群消息人工通知

PM Confirm Gate:
  ├─ Accept → 触发 IT Architect refinement → 重新发布架构
  └─ Reject → §8 Risks 标 "PM 不接受 Eng Review 反馈"
```

### 12.3 模板

```markdown
---
rr_id: ARCH-RR-{stamp}
project: {project}
epic: EPIC-{slug}
created: {YYYY-MM-DD-HHmm}
maintainer: "@EngReviewer"
target: "@ITArch"
severity: high | medium | low
status: open | pm-accepted | pm-rejected | resolved
---

# Architecture Refinement Request — {issue-slug}

## 评审发现问题
[Architecture Challenge / Blast Radius / NFR Verification 中发现的具体问题]

## 影响章节
- IT Architect §{X.Y}: {影响描述}
- ADR-NNN: {可能需要变更或新增}

## 评审建议（结构化 ≥1 候选方案）
方案 1: ...
方案 2: ...

## PM 决策栏（PM 填写）
- [ ] Accept → 触发 IT Architect refinement
- [ ] Reject → 接受当前 risk
- 决策理由: [PM 填]
```

---

## §13 §10 NFR Refinement Request（v2.0 新增 · 反向能力）

> 评审发现 NFR Targets 问题时，输出结构化 RR，PM Confirm 后触发 NFR Architect refinement 或新建。

### 13.1 触发条件

- NFR LATEST 不存在但本评审需要 NFR → 建议 PM 调用 NFR Architect
- NFR LATEST 与 IT Architect 设计严重不一致
- NFR Targets 与业务量级矛盾（如 §3 Capacity DAU 不匹配实际预期）

### 13.2 落盘 + 通知

```text
本地落盘:
  Project/{project}/EngReview/{epic-slug}/refinement-requests/nfr-refinement-{stamp}.md

Wiki 发布（可选）:
  /{project}/{epic-slug}-PRD/engineering-review/nfr-refinement-{stamp}

@ 通知:
  通过 NFR LATEST maintainer 字段（@NFRArch 或 @PM-xxx）定位
  群消息人工通知

PM Confirm Gate:
  ├─ Accept → 触发 NFR Architect refinement / 新建 → 重新发布 NFR LATEST
  └─ Reject → §8 Risks 标 "PM 不接受 NFR Refinement"
```

### 13.3 模板（与 ARCH-RR 同款，target 字段改为 @NFRArch 或 @PM）

---

## §14 §11 Wiki Publishing Metadata

```text
- Epic Name: {epic-name}
- Project: {project}
- Page Type: engineering-review
- 发布路径建议: /{project}/{epic-slug}-PRD/engineering-review
- Source: local | wiki-fallback | manual-input
- maintainer: "@{EngReviewer-name}"
```

---

## §15 Quality Gate（落盘前自检 · 阻塞性）

**章节合规**
- [ ] §0–§11 章节锚点齐全
- [ ] §0 Scope Challenge 三问全答 + Complexity Smell 已检测
- [ ] §2 Architecture Challenge Checklist 6 大类（A-F）全部评估
- [ ] §3 Blast Radius 每个 Key Decision 含五维评估
- [ ] §4 NFR Verification 8 类 NFR 全部校验
- [ ] §6 AC 合规校验已执行；不合规已在 §8 flag
- [ ] §7 每个 Story 含 5 字段（Domain / Dep / Complexity / Hint / Risk）
- [ ] §X Coverage Verification 已执行（警示性）

**上游一致性**
- [ ] 加载了 Value + Solution + IT Architecture + NFR + PRD（任一缺失已在 §1 标注）
- [ ] PRD §15 Capacity 与 Solution §6 Phase-level 偏差已核验

**反向 RR 合规（v2.0 新增）**
- [ ] §2 Architecture Challenge / §3 Blast Radius High / §4 NFR 不一致项已转化为 §9 Architecture RR（如适用）
- [ ] NFR LATEST 缺失或不一致已转化为 §10 NFR RR（如适用）

**协作合规（v3.7）**
- [ ] frontmatter `maintainer` 字段已填写
- [ ] 没有修改 IT Architect / NFR / Value / Solution / PRD 文件（只读）

---

## §16 强制规则

必须：
- 先 Read 本 SKILL + `skills/ac-writing-spec/SKILL.md` 再产出
- §0 Scope Challenge 必须先行
- §2 Architecture Challenge Checklist 必须按 §4.1 6 大类全评
- §3 Blast Radius 每个 Key Decision 必须含五维
- §4 NFR Verification 必须按 §6.2 8 类校验
- §6 AC 合规校验必须先于 §7 Task Readiness
- §X Coverage Verification 必须执行（警示性）
- 不合规 / 不一致 / High 风险必须转化为 §9 / §10 Refinement Request

禁止：
- **凭记忆生成评审章节**（必须先 Read SKILL）
- **v2.0 严禁写设计动作**（不画 C2 / C3 / ERD / API / Error / Retry / Logging · 这些都在 IT Architect / NFR Architect 产出）
- **越权改 IT Architect / NFR / Value / Solution / PRD 文件**（只读）
- 跳过任一评审章节
- 反向 RR 不经 PM Confirm 直接触发上游 refinement

---

## §17 变更记录

| 版本 | 日期 | 变更 |
|---|---|---|
| 2.0.0 | 2026-05-19 | **重大重构 v2.0 纯评审版**：删除 13 项设计动作（C2 / C3 / ERD / API / Error / Retry / Logging / NFR 等已下放到 IT Architect / NFR Architect / Product Planner）；新增 8 类纯评审动作（Scope Challenge / Architecture Challenge Checklist / Blast Radius / NFR Verification / Capacity 偏差 / AC 合规 / Task Readiness / Coverage Verification）；新增两类反向 Refinement Request 模板（Architecture / NFR）+ PM Confirm Gate；§4 NFR Verification 消费 NFR LATEST；§2 Architecture Challenge 消费 IT Architect 三层产出；§6 AC 合规含 8 类场景维度（ac-writing-spec v1.1）。|
| 1.0.0 | 2026-05-19 | 初版。抽取自 eng-reviewer.agent.md v2.2.0 的章节结构、Scope Challenge、Blast Radius、§17.0 AC 合规校验输出格式。 |
