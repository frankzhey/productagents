---
name: eng-review-spec
description: Engineering Review 写作规范——章节锚点 / Scope Challenge 三问 / Complexity Smell 检测 / Service Boundary / Blast Radius 五维评估 / Sequence 强制覆盖 / ERD / API / Error / Retry / Logging / NFR / Task Planning Readiness / AC 合规校验输出格式。Eng Reviewer 在产出评审前必须 Read 本文件。
version: 1.0.0
updated: 2026-05-19
maintainer: @frankzhey
applies-to: [eng-reviewer]
---

# Engineering Review 写作规范

本 SKILL 定义 Engineering Review 的章节结构、强制字段、Scope Challenge / Blast Radius / AC 合规校验等评审动作的统一规范。Eng Reviewer 在产出评审前必须显式 Read 本文件并按章节锚点执行。

> 与 ac-writing-spec / value-frame / solution-design / project-context-loader 并列，是 Eng-Reviewer 唯一的输出合规权威来源。

---

## §1 章节锚点（必须按此顺序输出）

| § | 章节 | 强制 / 可选 | 说明 |
|---|---|---|---|
| §0 | Scope Challenge ⭐ 先行 | ✅ 必须 | 最小变更集 + Complexity Smell（见 §2） |
| §1 | Review Scope | ✅ 必须 | 范围 / 输入 / 假设 / 不在范围内 |
| §2 | Feature / Epic Context | ✅ 必须 | Epic Name / 业务目标 / 用户价值 / 流程摘要 |
| §3 | High-level Architecture Design | ✅ 必须 | 整体架构 + 组件位置 + 同步/异步边界 |
| §4 | High-level Components Architecture | ✅ 必须 | 组件职责 / 关系 / 调用方向 / 数据流 |
| §5 | System Interaction Flow | ✅ 必须 | user → channel → gateway → service → data |
| §6 | Service Boundary Table | ✅ 必须 | Service / Component / Owns / Does NOT Own / Notes |
| §7 | Key Technical Decisions（含 Blast Radius） | ✅ 必须 | 见 §4 Blast Radius |
| §8 | Sequence Diagrams | ✅ 必须 | ≥1 happy + ≥1 failure + 必要 edge |
| §9 | Database ERD / Data Model | ✅ 必须 | 实体 / 关系 / ownership / 状态字段 / 时间字段 |
| §10 | API Document | ✅ 必须 | 见 §6 API 强制字段 |
| §11 | Error Handling Strategy | ✅ 必须 | 见 §7 Error 强制清单 |
| §12 | Retry Strategy | ✅ 必须 | 可重试接口 / 退避 / 幂等 / 补偿 |
| §13 | Logging & Monitoring | ✅ 必须 | trace id / 关键日志 / 告警点 / 指标 |
| §14 | Non-functional Requirements | ✅ 必须 | performance / reliability / security / observability |
| §15 | Risks / Open Questions | ✅ 必须 | 技术 / 集成 / 数据 / 依赖 / 未决 / 含 AC 合规 flag |
| §16 | Implementation Recommendation | ✅ 必须 | FE / BE / Integration / QA / DevOps 建议 |
| §17.0 | AC 合规校验 ⭐ 阻塞性 | ✅ 必须 | 见 §8 AC 合规校验输出格式 |
| §17 | Task Planning Readiness | ✅ 必须 | Story 级 Domain / Dependency / Complexity / Risk |
| §18 | Wiki Publishing Metadata | ✅ 必须 | epic name / page type |

---

## §2 §0 Scope Challenge（先行 · 强制）

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

## §3 §6 Service Boundary Table（强制格式）

```markdown
| Service | Component | Owns | Does NOT Own | Notes |
|---|---|---|---|---|
| ICS | speaking-engine | 语音文件上传、评分调用 | 用户身份认证（IOC 负责） | 内部 service |
| Mini Program | client-app | UI 渲染、本地状态 | 评分计算（后端负责） | 用户入口 |
```

强制要求：
- 每个 Service 必须显式列出 Owns 与 Does NOT Own 双列
- 跨系统集成点必须出现在两个 Service 行的 Notes 中
- 禁止只写"内部服务"等模糊描述，必须落到 BCChina 真实系统名（IOC admin / ICS / IVAP / OLM / Post test / 3Ups / Mini Program 等）

---

## §4 §7 Blast Radius（爆炸半径）五维评估（强制 · 每个决策必填）

### 4.1 评估维度

| 维度 | 审查问题 |
|---|---|
| 系统范围 | 最多波及哪些系统？（列举 service 名） |
| 用户范围 | 最坏情况影响多少用户？（全量 / 渠道 / 角色） |
| 数据范围 | 是否会写错、丢失或不一致？涉及哪些表？ |
| 恢复成本 | 能否快速回滚？回滚代价？ |
| 隔离能力 | feature flag / canary 可否限制爆炸半径？ |

### 4.2 输出格式（每个决策附加）

```text
决策 K-{n}: [决策描述]

爆炸半径评估:
  影响系统: [列出]
  影响用户: [范围]
  数据风险: [有 / 无，如有则说明]
  回滚能力: [可快速回滚 / 需人工干预 / 不可回滚]
  隔离方案: [feature flag / canary / 无]
  风险等级: [Low / Medium / High]
```

### 4.3 升级规则

风险等级 = **High** → 必须在 §15 Risks 中显式列出，并标注需要架构师或 domain owner 确认。

---

## §5 §8 Sequence Diagrams（强制覆盖）

- 至少 2 个，最多 5 个
- 必须包含：≥1 个 happy path + ≥1 个 failure path
- 每个 sequence 必须说明：actor / request / response / async event / callback / failure point

涉及以下场景额外补充：
- 文件上传 + AI 异步评分回调
- 一次性提交限制 + 幂等性
- WeChat unionId 绑定与 fallback

---

## §6 §10 API Document 强制字段

每个关键 API 必须包含：

| 字段 | 要求 |
|---|---|
| API Name | 业务可读名 |
| Endpoint | 完整路径 |
| Method | GET / POST / PUT / DELETE / PATCH |
| Purpose | 一句话业务目的 |
| Auth | 认证方式 |
| Permission | 权限模型 |
| Request Fields | 字段名 + 类型 + 必填性 + 校验 |
| Response Fields | 字段名 + 类型 + 含义 |
| Error Model | 标准错误码 + 描述 |
| Retry | 重试策略 |
| Timeout | 超时阈值 |

---

## §7 §11 Error Handling 强制清单

必须显式说明以下 8 类失败的处理：

- [ ] 输入校验失败
- [ ] 外部服务失败
- [ ] 超时
- [ ] 网络异常
- [ ] 重复提交
- [ ] 回调重复
- [ ] 状态不一致
- [ ] 部分成功

---

## §8 §17.0 AC 合规校验输出格式（阻塞性 · 引用 ac-writing-spec）

### 8.1 前置加载

```text
Read skills/ac-writing-spec/SKILL.md
```

把该 SKILL 的 §1（格式强制规范）/ §2（覆盖规范）/ §3（额外强制规则）作为 AC 合规校验清单。

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

**8.2.2 覆盖规范（按 ac-writing-spec §2）**

- 操作流程类 Story：覆盖 A-1 / A-2 / A-8 / B-4 / B-6 五类
- 列表查询类 Story：覆盖 A-2 / A-3 / A-4 / B-1 / A-5 / A-6 / A-7 七类

**8.2.3 额外强制（按 ac-writing-spec §3）**

- C-1 状态机覆盖
- B-3 按钮置灰覆盖
- B-2 表单校验五要素

### 8.3 输出格式（强制）

```text
AC 合规校验:
  Story 总数: X 个
  合规 Story 数: X 个
  不合规 Story 数: X 个

  不合规明细:
    Story [ID] - [Story 名称]:
      ❌ 格式问题: [如 "AC2 使用了 → 连写"]
      ❌ 覆盖缺失: [如 "操作类未覆盖 A-8 弹窗关闭"]
      ❌ 强制规则: [如 "B-3 按钮置灰未覆盖"]
      建议: 退回 Product Planner 按 ac-writing-spec §X.Y 修正

  结论: 【全部合规 / X 个 Story 不合规需返工】
```

### 8.4 不合规处理

- 全部合规 → 进入 §17 Task Planning Readiness
- 任一不合规 → **必须** 在 §15 Risks 中输出 AC 合规 flag：

```text
🚨 AC 合规风险:
  不合规 Story: [列表]
  建议: PM 让 Product Planner 重新跑 Quality Gate，修复后再交工程评审
  工程影响: AC 不合规 → 估算偏差 / QA 用例不完整 / 上线风险升高
```

---

## §9 §17 Task Planning Readiness（强制字段）

针对每个 Story 输出：

| 字段 | 必填值 |
|---|---|
| Recommended Domains | FE / BE / Integration / Data / QA / DevOps 中选 |
| Main Dependencies | 列出依赖的系统/组件/数据 |
| Main Complexity Drivers | 复杂度来源 |
| Suggested Breakdown Hints | 拆分建议 |
| Suggested Estimation Risk Level | Low / Medium / High |

并在 Story 级输出之外补充 **Refinement Notes for Task Planner**：

- 哪些任务适合先拆
- 哪些模块可并行
- 哪些依赖会影响 final unit
- 哪些需要系统 owner / domain expert 参与

---

## §10 §18 Wiki Publishing Metadata

```text
- Epic Name: {epic-name}
- Project: {project}
- Page Type: engineering-review
- 发布路径建议: /{project}/{epic-slug}-PRD/engineering-review
```

> 注：路径规则统一由 Wiki Publisher 按其路径表执行；此处仅为 metadata 引导。

---

## §11 Quality Gate（落盘前自检 — 阻塞性）

**SKILL 合规**
- [ ] §0–§18 章节锚点齐全
- [ ] §0 Scope Challenge 三问全答 + Complexity Smell 已检测
- [ ] §6 Service Boundary 含 Owns / Does NOT Own 双列
- [ ] §7 Key Decisions 每个含 Blast Radius 五维
- [ ] §8 Sequence ≥1 happy + ≥1 failure
- [ ] §10 API 字段齐全（11 项）
- [ ] §11 Error 覆盖 8 类失败
- [ ] §17.0 AC 合规校验已执行；不合规已在 §15 flag
- [ ] §17 每个 Story 含 5 字段（Domain / Dep / Complexity / Hint / Risk）
- [ ] §18 含 epic_name / project / page_type

**上游一致性**
- [ ] 加载了 PRD + Solution + Value 三段式（任一缺失已在 §1 标注）
- [ ] PRD §7 Capacity Summary 与 Solution §6 Phase-level 偏差已核验
- [ ] PRD §9 OQ 已逐条回执（关闭 / 追踪 / 升级）
- [ ] Story upstream_refs（persona / scenario / kpi）在 Solution / Value 中可定位

**Mode 合规（来自 eng-reviewer agent）**
- [ ] `local` / `wiki-fallback` / `manual-input` 之一已显式记录在 §1 Review Scope

---

## §12 强制规则

必须：
- 先 Read 本 SKILL 与 `skills/ac-writing-spec/SKILL.md` 再产出
- §6 Service Boundary 必须 Owns / Does NOT Own 双列
- §7 每个 Key Decision 必须 Blast Radius 五维评估
- §8 必须 ≥1 happy + ≥1 failure
- §17.0 必须先于 §17 执行；不合规必 §15 flag
- 落盘时 frontmatter 必须含 `skills_loaded`、`upstream_snapshot`、`mode`

禁止：
- 跳过 §0 Scope Challenge 直接进 §1
- §7 Decision 无 Blast Radius 五维
- §17.0 不合规但不在 §15 flag
- 凭记忆评 AC（必须先 Read ac-writing-spec）
- 越权写产品决策（如重新定义 Feature 范围）

---

## §13 变更记录

| 版本 | 日期 | 变更 |
|---|---|---|
| 1.0.0 | 2026-05-19 | 初版。抽取自 eng-reviewer.agent.md v2.2.0 的章节结构、Scope Challenge、Blast Radius、§17.0 AC 合规校验输出格式、§17 Task Planning Readiness 字段清单、Quality Gate 自检；与 ac-writing-spec / value-frame / solution-design / project-context-loader 形成 SKILL 矩阵；Eng Reviewer agent 改为薄编排（Mode 编排 + SKILL 加载 + Handoff）。 |
