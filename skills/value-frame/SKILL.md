---
name: value-frame
description: Value Frame 写作规范——Brief 五要素 / Hypothesis 假设格式 / KPI Tree 三类要求 / Roadmap+Epic 列表结构 / Open Questions 状态分类。Discovery 阶段产出 Value Frame 时必须 Read 本文件。
version: 1.0.0
updated: 2026-05-08
maintainer: @frankzhey
applies-to: [value-architect]
---

# Value Frame 写作规范

本 SKILL 定义 Value Frame 的章节结构、字段要求、ID 规则与禁止事项，由 Value Architect 在 Gate 3（全文校对）前显式 Read 并执行。

> Value Frame 是项目入口工件，不属于任何特定 Epic，是多个 Epic 的共同上游。

---

## §1 章节锚点（必须按此顺序输出）

| § | 章节 | 强制 / 可选 |
|---|---|---|
| §0 | 调研 Summary 索引 | 仅 Mode 2 必填；Mode 1 跳过 |
| §1 | Brief（5 要素） | ✅ 必须 |
| §2 | Value Hypothesis（≥3 条） | ✅ 必须 |
| §3 | KPI Tree（≥1 North Star + ≥1 Guardrail） | ✅ 必须 |
| §4 | Roadmap with Phases（含 Epic 列表） | ✅ 必须 |
| §5 | Open Questions（含 status） | ✅ 必须（即使为空也保留区块） |
| §6 | 已沉淀规则索引 | ✅ 必须（推动 Rules 更新的条目） |
| §7 | 变更记录（changelog） | ✅ 必须 |

---

## §2 Brief 五要素（强制）

```
## §1 Brief

- **背景**：[业务 / 市场 / 内部环境，≥50 字]
- **当前问题**：[用户 / 业务 / 运营具体痛点，≥3 条]
- **为什么现在做**：[时机 / 风口 / 资源 / 战略对齐 — 必须具体]
- **目标用户**：[1–3 类核心角色 + 简短描述]
- **业务价值**：[直接业务收益 — 收入 / 成本 / 体验 / 合规]
```

**强制要求**：
- "为什么现在做"必须具体（不能写"用户体验差"这类空话），需要含时机依据（如"3Ups 上线带来流量入口" / "Q2 战略立项" / "竞品 X 已抢先 6 个月"）
- "目标用户"必须用具体角色名，不能用"所有用户"
- "业务价值"必须可衡量（指向某个 KPI 或业务指标）

---

## §3 Value Hypothesis 格式（≥3 条）

每个假设必须采用以下格式：

```
**假设 H{N}**：如果 [我方行为]，则 [用户/业务结果]，因为 [原因]

- **用户价值**：[用户能感知到的具体改善]
- **业务价值**：[可衡量的业务指标改善 — 引用 KPI ID]
- **验证信号**：[上线后看什么数据来判断假设成立]
```

**强制要求**：
- 至少 3 条假设
- "如果...则...因为..."三段必须齐全
- 验证信号必须可观察（如"DAU > 1000 / 一次通过率 > 85%"）
- 禁止含糊假设（如"提升体验"无法证伪）

---

## §4 KPI Tree（强制）

格式：

```
| Type | KPI ID | Name | Target | Definition / 来源 |
|------|--------|------|--------|-------------------|
| North Star | K1 | ... | >= X | [口径定义 + 数据来源] |
| Leading | K2 | ... | ... | ... |
| Leading | K3 | ... | ... | ... |
| Guardrail | K4 | ... | <= Y | [防止顾此失彼] |
```

**强制要求**：
- ≥1 个 **North Star**（核心北极星，反映核心价值）
- ≥1 个 **Guardrail**（防止顾此失彼，如"PII 暴露事件 = 0" / "页面加载 > 3s 比例 < 5%"）
- KPI ID（K1 / K2 / ...）**跨阶段稳定**，下游 Solution Brief 与 PRD 的 Story 都用此 ID 引用对齐
- 每个 KPI 必须有 Target（具体数值或区间）
- 每个 KPI 必须有 Definition（口径定义）+ Source（数据来源）
- 禁止空泛指标（如"提升用户体验"无法量化）

---

## §5 Roadmap with Phases（强制）

格式：

```
| Phase ID | Name | Timeline | 涵盖 Epic（标题 + 一句话） | KPI 对齐 |
|----------|------|----------|---------------------------|----------|
| MVP | [Phase 名] | 2026Q3 | EPIC-A：[一句话 ≤30 字] / EPIC-B：[一句话 ≤30 字] | K1, K4 |
| Phase 2 | ... | 2026Q4 | EPIC-C：[一句话] | K2 |
| Future | ... | 2027+ | EPIC-D：[一句话] | K3 |
```

**强制要求**：
- 每个 Epic 标题必须是 **kebab-case**（如 `idv-result-review` / `b2b-quota-management`），可作为 `epic-slug` 直接给 Solution Architect 调用
- Epic 描述 **≤30 字**，仅说明该 Epic 的目标和范围边界
- **禁止**深入 Feature / Story（那是 Solution Architect 职责）
- 每个 Epic 必须 align 到 ≥1 个 KPI ID

### §5.1 Epic 颗粒度定义（强制）

一个 Epic 必须是**可独立交付的价值单元**，必须同时满足以下三条：

1. **端到端可感知**：上线后用户能完成一段端到端的真实任务，而不是只完成其中一步。
   - **基础设施例外**：账号、身份、权限、跨端打通等基础设施类 Epic，其端到端价值在于 "消除跨端/跨场景摩擦"，无须用户主动完成一项任务，但必须在 value_statement 中明确说明摩擦消除场景。
2. **一句话价值可表达**：可以用一句 "用户能 X，所以获得 Y" 完整说清楚。value_statement 主语必须是终端用户（考生 / 学员 / 客户），**禁止使用内部角色**（运营 / 数据 / 算法 / 客服）作为主语。
3. **独立 KPI 可度量**：可以独立指向 ≥1 个 KPI，且**满足以下两条 KPI 独立性硬约束**：
   - **子集约束**：该 Epic 的 KPI 集合不能是任一其他 Epic KPI 集合的子集。
   - **重叠率约束**：与任一其他 Epic 的 Leading/Guardrail KPI 重叠率 < 50%（North Star KPI 允许跨 Epic 共享，不计入重叠率）。
   - **持续 Guardrail 例外**：MVP Epic 的 Guardrail KPI（如评分成功率、时延、合规事故数）在 Phase 2 / Future 仍持续生效时，不视为新 Epic 与 MVP Epic 的 KPI 重叠，但 Phase 2 / Future Epic 必须有 ≥1 个**新增**的 Leading KPI 来证明独立价值。

额外约束：
- 推荐每个 Phase 内 Epic 数量 ≤ 3，MVP Phase 默认 1–2 个 Epic。
- 一个 Epic 内部允许跨多个功能模块（前端 / 后端 / 内容 / 数据），不要按层切 Epic。
- 如果两个或多个候选 Epic 必须一起上线才能讲清楚价值，则它们应当被合并为一个 Epic，剩余部分退化为 Feature 由 Solution Architect 拆解。

### §5.2 Epic 反模式（禁止）

下列拆法属于错误颗粒度，必须在 §4 落盘前消除：

- **反模式 A — 按功能层切**：如 “入口 Epic / 评分 Epic / 结果解释 Epic / 导流 Epic”。
- **反模式 B — 按页面切**：如 “首页 Epic / 详情页 Epic / 结果页 Epic”。
- **反模式 C — 按团队切**：如 “前端 Epic / 算法 Epic / 数据 Epic”。
- **反模式 D — 把 Feature 当 Epic**：如 "按钮文案配置 Epic" / "参数透传 Epic" / "评分置信度展示 Epic"。
- **反模式 E — 按内部受益方切**：value_statement 主语是 "运营 / 数据 / 算法 / 客服 / 法务" 等内部角色，**且必须依赖其他 Epic 产生数据/事件才能运转**（如 "data-ops Epic" / "质量监控 Epic" / "客服后台 Epic"）。这类能力应作为各业务 Epic 内的横切 Feature（埋点、监控、看板）下沉，而非独立 Epic。

### §5.3 Epic List 强制结构

§4 Roadmap 表之外，**必须**追加一张独立 Epic List 表，作为 Epic 的 canonical 定义来源：

```
### Epic List

| EPIC ID | EPIC name | value_statement | KPI 对齐 | Phase |
|---------|-----------|-----------------|----------|-------|
| E1 | speaking-challenge-and-scoring | 用户能在小程序内完成一次口语作答并立即获得对齐 IELTS band 的评分与下一步引导 | K1, K2, K3, K6, K7 | MVP |
| E2 | challenge-retention-loop | 用户能因连续挑战和回访提醒持续复练，从而提升留存 | K1, K4 | Phase 2 |
```

**强制要求**：
- `EPIC ID`：稳定排序号（E1, E2, ...），同一 Value Frame 内不允许跳号。
- `EPIC name`：与 §4 Roadmap 中使用的 kebab-case slug 完全一致，是 Solution Architect 启动指令的 `Selected Epic`。
- `value_statement`：必须使用 “用户能 X，所以获得 Y” 句式，且能通过 §5.1 三条判定。
- `KPI 对齐`：必须引用 §3 KPI Tree 中已定义的 ID。
- `Phase`：必须引用 §4 Roadmap 中已定义的 Phase ID。

### §5.4 Epic 合并自检（强制）

agent 在产出 §4 Roadmap 草稿后、提交 PM 确认前，必须执行以下五步：

1. 为每个候选 Epic 写出 `value_statement`，校验主语为终端用户（基础设施 Epic 例外见 §5.1.1）。
2. 检查是否存在两个或多个 Epic 共用同一 `value_statement`、或缺一不可才能讲清楚价值的情况。
3. 计算每对 Epic 的 Leading/Guardrail KPI 重叠率（North Star 不计），≥50% 即触发合并候选。
4. 命中 step 2 / 3 → 主动给 PM 输出 **合并建议**（合并后的 Epic 名 + 退化为 Feature 的清单 + 合并理由），不替 PM 拍板。
5. **依赖单向性检查**：对每个 Epic 问 "如果只上线这一个 Epic，其他 Epic 全部不上，用户能拿到 value_statement 描述的价值吗？" 答否（必须依赖某上游 Epic 的输出才能产生用户价值）→ 该 Epic 应作为上游 Epic 的 v2 增强或内部 Feature，不应独立成 Epic。**例外**：基础设施类 Epic（账号 / 身份 / 权限 / 跨端打通）可独立成立。
6. 命中任一反模式 A–E → 主动输出 **重命名建议**。

#### §5.4.1 Epic 自检矩阵（强制产出）

agent 在提交 PM 前，必须在 §4 内嵌一张 **Epic 自检矩阵**作为附录，便于 PM 一眼审核：

```
#### Epic 自检矩阵

| EPIC ID | 三判定（端到端 / 一句话 / 独立 KPI） | KPI 重叠扫描 | 反模式扫描（A-E）| 依赖单向性 | 结论 |
|---------|--------------------------------------|--------------|------------------|------------|------|
| E1 | ✅ ✅ ✅ | none | none | 独立 | pass |
| E3 | ✅ ✅ ⚠️ 与 E1 重叠 67% (K3,K4,K8) | hit | hit D | 单向依赖 E1 | merge → E1 v2 |
| E5 | ✅ ❌ 主语为运营 | — | hit E | 单向依赖 E1/E2 | demote to cross-cutting Feature |
```

**矩阵任一行结论非 `pass`**：
- agent 必须在 §4 输出对应合并/降级/重命名建议；
- 若 PM 显式 override（保留原拆分），必须在 §7 changelog 记录 override 理由，否则阻塞落盘。

---

## §6 Open Questions 状态分类（强制）

格式：

```
| OQ ID | Question | Status | Owner | 备注 |
|-------|----------|--------|-------|------|
| OQ1 | [问题描述] | open | PM | [需调研 / 需评审] |
| OQ2 | ... | closed | Eng | 已通过 [日期] 评审关闭 |
| OQ3 | ... | wontfix | PM | 不在本期范围 |
```

**Status 分类**：
- `open` — 未关闭，需后续阶段处理
- `closed` — 已关闭（含决议日期 + 决议方）
- `wontfix` — 不在范围内（含理由）

**强制要求**：
- 即使无 OQ 也保留区块（写 "本次调研中无 Open Questions"）
- 每个 open 条目必须有 Owner（指派人）
- 下游 Solution Brief / PRD 必须 propagate 所有 status=open 条目

---

## §7 ID 体系（与下游约定）

| 工件 | ID 前缀 | 跨阶段使用 |
|---|---|---|
| Hypothesis | `H1`, `H2` | 下游 Story 可引用 |
| KPI | `K1`, `K2` | 下游 Story `kpi_alignment: [K1]` 引用 |
| Phase | `MVP`, `phase2`, `future` | Solution Brief 必须明确归属 |
| Epic 排序号 | `E1`, `E2`（仅在 Value Frame 内排序，不跨阶段引用） | §4 Epic List 显示用 |
| Epic（canonical） | kebab-case slug（如 `idv-result-review`） | Solution Architect 启动指令的 `Selected Epic`，跨阶段唯一标识 |
| Open Question | `OQ1`, `OQ2`（Value 阶段无前缀） | 下游 propagate 时加前缀 `V-OQ1` |

---

## §8 调用方式

```
Read skills/value-frame/SKILL.md
```

加载位置：value-architect.agent.md 的 **Gate 3（Value Frame 全文校对）入口**，在产出 §1–§7 之前必须先 Read 本文件。

---

## §9 强制规则

必须：
- §1 Brief 五要素全部具体化（禁空话）
- §2 Hypothesis ≥3 条，含完整"如果...则...因为..."格式 + 用户价值 + 业务价值 + 验证信号
- §3 KPI Tree ≥1 North Star + ≥1 Guardrail，每个有 ID + Target + Definition
- §4 Roadmap 每 Epic 是 kebab-case + ≤30 字描述 + ≥1 KPI 对齐
- §4 必须额外输出 §5.3 规定的 **Epic List** 表，含 EPIC ID / EPIC name / value_statement / KPI 对齐 / Phase 五列
- §4 必须额外输出 §5.4.1 规定的 **Epic 自检矩阵**，所有行结论为 `pass`，否则需 PM override + §7 changelog 记录
- 每个 Epic 必须通过 §5.1 三条颗粒度判定（端到端可感知 / 一句话价值 / 独立 KPI 含子集与重叠率约束），违反即阻塞落盘
- 每个非 MVP Epic 必须有 ≥1 个**新增** Leading KPI（持续生效的 MVP Guardrail 不计）
- §5 OQ 显式标 status

禁止：
- §1 "为什么现在做"留空话或泛泛而谈
- §2 假设无法证伪（如"提升用户体验"）
- §3 KPI 无 Target 或无 Definition
- §4 Roadmap 中 Epic 描述过深（侵入 Solution Architect 职责）
- §4 出现 §5.2 列出的任一 Epic 反模式（按功能层 A / 按页面 B / 按团队 C / Feature 当 Epic D / 按内部受益方 E）
- §4 出现两个 Epic 共享同一 value_statement、或缺一不可才有价值的情况
- §4 任意两个 Epic Leading/Guardrail KPI 重叠率 ≥ 50%
- §4 出现非基础设施类 Epic 单向依赖另一 Epic 的情况
- §4 Epic value_statement 主语为内部角色（运营 / 数据 / 算法 / 客服）
- §5 OQ 不标 status

---

## §10 版本变更

| 版本 | 日期 | 变更 |
|------|------|------|
| 1.3.0 | 2026-05-08 | §5.1 第 1 条新增基础设施 Epic 例外；§5.4 新增 Epic 自检矩阵 §5.4.1（EPIC ID / 三判定 / KPI 重叠扫描 / 反模式扫描 / 依赖单向性 / 结论），所有行需 `pass` 否则 PM override + changelog 记录；§9 必须清单同步加入自检矩阵与新增 Leading KPI 要求。 |
| 1.2.0 | 2026-05-08 | §5.1 第 2 条加 value_statement 主语必为终端用户、禁止内部角色；第 3 条引入 KPI 子集约束 + 重叠率 <50% 硬约束 + North Star 共享豁免 + 持续 Guardrail 例外；§5.2 新增反模式 E（按内部受益方切）；§5.4 新增 step 5 依赖单向性检查（基础设施类例外）；§9 禁止清单新增 KPI 重叠率 ≥50% / 单向依赖 / 内部角色主语三项。 |
| 1.1.0 | 2026-05-08 | 新增 §5.1 Epic 颗粒度定义（端到端可感知 / 一句话价值 / 独立 KPI）+ §5.2 Epic 反模式 + §5.3 Epic List 强制结构（EPIC ID / EPIC name / value_statement / KPI 对齐 / Phase）+ §5.4 Epic 合并自检；§7 ID 体系明确 slug 为 canonical、E1/E2 为排序号；§9 强制规则补充 Epic List 与颗粒度阻塞项及反模式禁止项。 |
| 1.0.0 | 2026-05-08 | 初版。从 value-architect.agent v1.0 抽离 Value Frame 章节锚点 + Brief 五要素 + Hypothesis 三段格式 + KPI Tree 三类要求 + Roadmap+Epic 列表 + OQ 状态分类。 |
