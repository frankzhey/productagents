---
name: solution-design
description: Solution Brief 写作规范——Stable Feature ID / Feature List 表格 / Journey 三栏 / Process Flow 标准 / GWT Top 5 / T-shirt Workload 映射 / Tech high-level 四段式 / Story List 预览。Plan 阶段产出 Solution Brief 时必须 Read 本文件。
version: 1.0.0
updated: 2026-05-08
maintainer: @frankzhey
applies-to: [solution-architect]
---

# Solution Brief 写作规范

本 SKILL 定义 Solution Brief 的章节结构、ID 体系、Feature List 格式、Journey/Process/GWT/T-shirt/Tech high-level/Story List 标准，由 Solution Architect 在产出 brief 前显式 Read 并执行。

---

## §1 章节锚点（必须按此顺序输出）

| § | 章节 | 强制 / 可选 |
|---|---|---|
| §0 | 上游引用（Value Frame 摘要） | ✅ 必须 |
| §1 | Epic 定义（Name + Stable ID + Context + Scope In/Out） | ✅ 必须 |
| §2 | Feature List ⭐ 核心交付 | ✅ 必须 |
| §3 | User Journey | ✅ 必须 |
| §4 | Business Process Flow | ✅ 必须 |
| §5 | GWT Top 3–5 | ✅ 必须 |
| §6 | Phase-level Workload（T-shirt 映射） | ✅ 必须 |
| §7 | Tech High-level（四段式） | ✅ 必须 |
| §8 | Story List 预览（标题 + Stable ID 占位） | ✅ 必须 |
| §9 | Open Questions（含 Value 继承） | ✅ 必须 |
| §10 | 跨团队评审记录 | ⭕ 评审后填写 |
| §11 | 已沉淀规则索引 | ✅ 必须 |
| §12 | 变更记录 | ✅ 必须 |

---

## §2 Stable ID 体系（核心 - 跨文件稳定）

| 工件 | 前缀 | 规则 | 跨阶段使用 |
|---|---|---|---|
| Feature | `F1`, `F2`, `F3` | 永不重排，删除走退役 | Product Planner Story ID 基础 `EPIC-{slug}-F{N}-S{M}` |
| Persona | `P1`, `P2` | 永不重排 | Product Planner Story `upstream_refs.persona` 引用 |
| Journey Stage | `J1`, `J2` | 永不重排 | Product Planner Story `upstream_refs.journey_stage` 引用 |
| Scenario (GWT) | `S1`, `S2` | 永不重排（与 Story S 编号互不冲突，因为有完整前缀区分） | Product Planner Story `upstream_refs.scenarios` 引用 |
| Open Question | `S-OQ1`, `S-OQ2`（S 前缀避免与 Value V- 混淆） | — | PRD §18 propagate |

**强制规则**：
- ID 一经 PM 确认（评审通过）→ 永不变更
- 删除 → 编号退役，**不复用**，归档到 `{epic-slug}-archived.md`
- 新增 → 取当前最大编号 + 1

---

## §3 §1 Epic 定义格式

```markdown
## §1 Epic 定义

- **Epic Name**：[标题]
- **Epic Stable ID**：`EPIC-{slug}`
- **Context**（≤300 字）：本 Epic 在项目中的位置、本期目标、不在范围内的相关功能
- **Scope In**（继承自 Value Frame Phase）：[列表]
- **Scope Out**（继承 + 本 brief 进一步明确）：[列表]
```

---

## §4 §2 Feature List 格式（核心交付）

```markdown
| Feature ID | Name | Description | Value | 预估 Story 数 | T-shirt | 关联 Persona | 主要复杂度驱动 |
|---|---|---|---|---:|:---:|---|---|
| F1 | ... | [≥30 字] | [用户/业务价值] | 4–6 | M | P1, P2 | [具体复杂度] |
| F2 | ... | ... | ... | 3–5 | S | P1 | ... |
```

**强制要求**：
- 每个 Feature 必须有 Description ≥30 字（不是标题重复）
- Value 必须明确用户价值或业务价值（不能写"提升体验"）
- 预估 Story 数为 range（如 4–6）
- T-shirt 必须严格符合 §7 映射
- 关联 Persona 必须引用 §3 中已定义的 P1/P2/...
- 主要复杂度驱动必须具体（如"涉及第三方 NFC 集成 + 异步回调"，不能写"逻辑复杂"）

---

## §5 §3 User Journey 格式

```markdown
| Persona ID | Stage ID | Stage | Action | Touchpoint | Emotion |
|---|---|---|---|---|---|
| P1 | J1 | Entry | 登录后台进入 IDV List | List 页 | 找到待复核项 |
| P1 | J2 | Review | 打开 Details 比对三源 | Details 弹窗 | 判定信心 |
| P1 | J3 | Decision | 填备注 + Pass/Fail/Override | Details 弹窗 | 完成闭环 |
| P2 | J4 | ... | ... | ... | ... |
```

**强制要求**：
- 至少覆盖 Entry / Action / Decision / Result 四类阶段中的 ≥3 类
- 每个 Stage 含 Persona × Action × Touchpoint
- Touchpoint 必须是具体页面 / 弹窗 / 组件名
- Emotion 必须可感知（如"找到待复核项" / "犹豫不决"），不写"流畅"

---

## §6 §4 Business Process Flow 格式

```markdown
### Happy Path
[文字描述 + Mermaid 泳道图]

### Unhappy Path 1：[场景名]
[文字描述]

### Unhappy Path 2：[场景名]
[文字描述]
```

**每条 Path 必须明确**：
- 触发点（什么操作启动）
- 关键决策点（哪些分支判断）
- 系统边界（本系统 vs 外部依赖）
- 异常恢复（如何回到 happy 或终止）

**强制要求**：至少 1 Happy + 1 Unhappy

---

## §7 §5 GWT Top 3–5 格式

```markdown
| Scenario ID | Type | Persona | Name | 关联 Stage | 关联 Feature |
|---|---|---|---|---|---|
| S1 | happy | P1 | 三源一致通过 | J1, J2, J3 | F1 |
| S2 | unhappy | P1 | 三源不一致需 Override | J2, J3 | F1, F2 |
| S3 | unhappy | P1 | 证件照不清晰 | J2 | F1 |
| S4 | failure | P1 | 网络异常读取失败 | J2 | F1 |
| S5 | edge | P2 | 无权限访问 | J1 | F1 |
```

**Type 取值**：`happy` / `unhappy` / `failure` / `edge`

**每条 Scenario 必须**用多行 GWT 格式描述（遵循 `skills/ac-writing-spec/SKILL.md` §1）：

```
### S1：三源一致通过

GIVEN 用户已登录后台并具备 IDV 复核权限
AND IDV List 中存在状态为 `pending` 的记录
WHEN 用户点击某条 Pending 记录的 Details 按钮
AND `Facial Comparison` / `MPS 3-factor` / `NFC` 三源结果均为 `Pass`
THEN 系统在 Details 弹窗显示三源一致 Pass
AND 用户可点击 `Confirm Pass` 完成复核
```

**强制要求**：≥3 条，含 ≥1 happy + ≥1 unhappy

---

## §8 §6 Phase-level Workload + T-shirt 强制映射

```markdown
| Feature | T-shirt | Unit Range | Effort Range | 主要复杂度驱动 |
|---|:---:|---:|---:|---|
| F1 | M | 10–20 units | 5–10 days | ... |
| F2 | S | 5–10 units | 2.5–5 days | ... |
| F3 | L | 20–40 units | 10–20 days | ... |
| **Epic 合计** | — | **35–70 units** | **17.5–35 days** | — |
```

### T-shirt → Unit Range 强制映射（统一 · 跨 agent 一致）

| T-shirt | Unit Range | Effort Range（1 unit = 0.5 day） |
|:---:|---:|---:|
| **S** | 5–10 units | 2.5–5 days |
| **M** | 10–20 units | 5–10 days |
| **L** | 20–40 units | 10–20 days |
| **XL** | 40–80 units | 20–40 days |

**强制要求**：
- T-shirt 与 Unit Range 严格按映射，禁止"M = 50 units"等偏离
- 必须输出 Epic 合计行（Product Planner §15 Capacity 偏差校验依赖此值）

---

## §9 §7 Tech High-level 四段式

```markdown
### 1. 架构图
[Mermaid 或 ASCII 文字描述]

### 2. 关键组件清单
| 组件 | 职责 | 归属服务 |
|---|---|---|

### 3. Service Interaction Flow
[2–5 个关键链路]
- 链路 1：用户登录 → SSO → IDV-Service → 第三方 NFC
- 链路 2：...

### 4. 主要 ADR（待研发评审确认）
- ADR-1：[决策点描述]，候选方案 A vs B，倾向 A 因为...
- ADR-2：...
```

**强制要求**：四段全部输出（即使某段简短也保留区块）

---

## §10 §8 Story List 预览格式

按 Feature 分组，每个 Story 仅给：标题 + 一句话描述 + Stable ID 占位：

```markdown
### F1 — [Feature Name]
- `EPIC-{slug}-F1-S01` — [Story 标题]：[一句话描述]
- `EPIC-{slug}-F1-S02` — ...

### F2 — [Feature Name]
- `EPIC-{slug}-F2-S01` — ...
```

**作用**：
- 跨团队评审用此清单确认范围一致性
- Product Planner 直接接走，每个 Story ID 已稳定，AC 由 Product Planner 补全

**强制要求**：
- 每个 Story 必须有 Stable ID（`EPIC-{slug}-F{N}-S{M}`）
- Story 编号在同一 Feature 内连续，不允许跳号

---

## §11 §9 Open Questions 格式（含 Value 继承）

```markdown
### 来自 Value Frame（继承）
| OQ ID | Question | Status | Owner |
|---|---|---|---|
| V-OQ1 | ... | open | PM |
| V-OQ2 | ... | closed | Eng（已于 [日期] 评审关闭） |

### 本 Solution 新增
| OQ ID | Question | Status | Owner |
|---|---|---|---|
| S-OQ1 | ... | open | Eng |
| S-OQ2 | ... | open | PM |
```

**强制要求**：
- Value 阶段所有 status=open 条目必须 propagate（前缀 `V-`）
- 本 Solution 新增条目前缀 `S-`
- 每条必须有 Status + Owner

---

## §12 调用方式

```
Read skills/solution-design/SKILL.md
```

加载位置：solution-architect.agent.md 的 **§Step 0 启动协议** 之后、§产出 Solution Brief 之前必须先 Read 本文件。

---

## §13 强制规则

必须：
- 章节锚点严格按 §1 顺序产出
- Stable ID 体系（F / P / J / S）跨阶段稳定，禁止重排
- §2 Feature List 每 Feature 含 Description + Value + T-shirt + 关联 Persona
- §3 Journey 每 Stage 含 Persona × Action × Touchpoint
- §4 Process Flow ≥1 Happy + ≥1 Unhappy
- §5 GWT ≥3 条含 ≥1 happy + ≥1 unhappy + 每条多行 GWT 格式
- §6 T-shirt 与 Unit Range 严格按映射
- §7 Tech high-level 四段全部输出
- §8 Story List 每个 Story 有 Stable ID
- §9 OQ 必须 propagate Value 所有 status=open 条目

禁止：
- Feature ID 重排（任何场景）
- §2 Feature List 出现未在 §3 Journey / §5 GWT 关联的孤立 Feature
- 越权写完整 Story AC（Product Planner 职责）
- 跳过 §8 Story List 预览
- T-shirt 估算偏离 §8 统一映射

---

## §14 版本变更

| 版本 | 日期 | 变更 |
|------|------|------|
| 1.0.0 | 2026-05-08 | 初版。从 solution-architect.agent v1.0 抽离 Solution Brief 章节锚点 + Stable ID 体系 + Feature List 表格 + Journey/Process/GWT/T-shirt/Tech high-level/Story List 预览的格式标准与强制规则。 |
