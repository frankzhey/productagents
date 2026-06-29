---
name: solution-design
description: Solution Brief 写作规范 v1.7——纯业务方案（不写技术选型 / 不写 engineering notes）。v1.7 接入 story-splitting-spec，Solution 阶段的 Feature List 必须按 Feature=系统能力、输入→处理→输出闭环、尽量不跨系统、每 Feature 预估 ≥3 Story 等 Feature Gate 生成；User Story / AC 详细拆解仍由 Product Planner / Story Splitter 承接。
version: 1.7.0
updated: 2026-06-29
maintainer: @frankzhey
applies-to: [solution-architect]
---

# Solution Brief 写作规范 v1.7

本 SKILL 定义 Solution Brief 的章节结构、ID 体系、Feature List 格式、Journey/Process/流程难点/T-shirt/EXP/NFR Reference/Story List 标准，由 Solution Architect 在产出 brief 前显式 Read 并执行。

> **v1.7 核心变化**：
> - Feature List 的颗粒度规则接入 `skills/story-splitting-spec/SKILL.md`
> - Solution 阶段必须先判断 Feature 是否是"系统能力"，并检查名称、业务闭环、系统边界、Story 数量与后台二级菜单边界
> - Solution 仍不写完整 Story AC；Story 详细拆分与 AC 由 Product Planner / Story Splitter 承接

> **v1.6 核心变化（在 v1.4 基础上）**：
> - **§7 重写为 Technology Expectations to IT Architect**：结构化 EXP-{n} ID + 优先级 must/should/nice + 来源/理由；删除 v1.4 §7.1 技术方向 / §7.3 Layer 1 引用回填等子节
> - **三份产出独立 + 无回路**：IT Architect 单向消费 Solution §7 EXP，**不回写 Solution**；删除任何"回填 Layer 1 引用"机制；Solution 进入审核后无 tech-refined 状态
> - **§7.2 ITQ 改为待澄清问题**：保留"待 IT Architect 回答的问题清单"作为 ITQ-{n}，但澄清结论由 IT Architect 在 Architecture LATEST / ADR 中给出，Solution 不回填
> - **§7.3 Architecture 引用指针**：仅作为只读指针，不回填详细架构内容
> - **严禁技术选型**：EXP 描述只能是"能力 / 约束 / 期望"，不能写"使用 RabbitMQ"等具体技术选择（详见 §9 强制规则）

> **v1.4 沉淀保持不变**：
> - §5 流程难点与 PRD 拆解提示（BP-H/U/E）
> - §8 NFR Reference（引用 NFR LATEST · 不重写）
> - Step 0.5 PM-AI 协作 4 阶段 14 项分级
> - 严禁画完整架构图 / ERD / API / 触发 fireworks-tech-graph（IT Architect 职责）

---

## §1 章节锚点（v1.4 重构 · 必须按此顺序输出）

| § | 章节 | 强制 / 可选 | v1.4 变化 |
|---|---|---|---|
| §0 | 上游引用（Value Frame 摘要） | ✅ 必须 | — |
| §1 | Epic 定义（Name + Stable ID + Context + Scope In/Out） | ✅ 必须 | — |
| §2 | Feature List ⭐ 核心交付 | ✅ 必须 | — |
| §3 | User Journey | ✅ 必须 | — |
| §4 | Business Process Flow | ✅ 必须 | — |
| **§5** | **流程难点与 PRD 拆解提示** ⭐ v1.4 替代 GWT | ✅ 必须 | 新章节（见 §X 详细规范） |
| §6 | Phase-level Workload（T-shirt 映射） | ✅ 必须 | — |
| **§7** | **Technology Expectations to IT Architect** ⭐ v1.6 重写 | ✅ 必须 | 结构化 EXP-{n} + must/should/nice + ITQ-{n} 待澄清问题 + Architecture 引用指针（只读） |
| **§8** | **NFR Reference** | ✅ 必须 | 仅引用 NFR LATEST，不重写 |
| §9 | Story List 预览（标题 + Stable ID 占位） | ✅ 必须 | 编号下移（原 §8） |
| §10 | Open Questions（含 Value 继承） | ✅ 必须 | 编号下移（原 §9） |
| §11 | 跨团队评审记录 | ⭕ 评审后填写 | 编号下移（原 §10） |
| §12 | 已沉淀规则索引 | ✅ 必须 | 编号下移（原 §11） |
| §13 | 变更记录 | ✅ 必须 | 编号下移（原 §12） |

---

## §2 Stable ID 体系（核心 - 跨文件稳定）

| 工件 | 前缀 | 规则 | 跨阶段使用 |
|---|---|---|---|
| Feature | `F1`, `F2`, `F3` | 永不重排，删除走退役 | Product Planner Story ID 基础 `EPIC-{slug}-F{N}-S{M}` |
| Persona | `P1`, `P2` | 永不重排 | Product Planner Story `upstream_refs.persona` 引用 |
| Journey Stage | `J1`, `J2` | 永不重排 | Product Planner Story `upstream_refs.journey_stage` 引用 |
| Process Path | `BP-H{n}`, `BP-U{n}`, `BP-E{n}` | 永不重排 | PRD §X Coverage Matrix `Source Path` 列引用（v1.4） |
| Technology Expectation | `EXP-1`, `EXP-2` | 永不重排（v1.6 新增）| IT Architect ADR / Architecture trace；PRD §5 Engineering Notes 间接引用（IT Architecture LATEST 已 trace） |
| IT Open Question | `ITQ-1`, `ITQ-2` | 永不重排（v1.6 新增）| IT Architect 在 Architecture LATEST / ADR 中给出结论；Solution 不回填 |
| Open Question | `S-OQ1`, `S-OQ2`（S 前缀避免与 Value V- 混淆） | — | PRD §9 propagate |

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
- 产出 Feature List 前必须加载 `skills/story-splitting-spec/SKILL.md`，并按其 §2 执行 Feature Gate
- 每个 Feature 必须是可独立理解的系统能力，至少包含输入 → 处理 → 输出
- 每个 Feature 尽量不跨系统；跨用户系统 / AI 系统 / 支付系统 / 后台系统时优先拆 Feature
- 每个 Feature 预估 Story 数默认 ≥3；少于 3 必须记录 PM override 理由
- 后台系统中独立二级菜单默认可作为一个 Feature
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

## §7 §5 流程难点与 PRD 拆解提示（v1.4 替代 GWT）

> **v1.4 替换动机**：GWT 是 AC 级形式化测试用例（QA/Engineer 视角），错位放在 Solution 阶段（Plan 视角）。  
> v1.4 改为 **"对 PRD 拆解的结构化指引"**，由 Product Planner 在 PRD §3 拆解 + §X Coverage Matrix 追溯。

### §5 章节格式（强制）

```markdown
## §5 流程难点与 PRD 拆解提示

| Path ID | 类型 | 所属 Feature | 流程 / 难点 | 对 PRD 的拆解提示 |
|---|---|---|---|---|
| BP-H1 | happy | F1 | 用户提交录音并成功进入评分 | PRD 拆"提交录音"+"上传成功"+"评分中状态" 3 个 Story |
| BP-U1 | unhappy | F1 | 上传中断 / 网络失败 / 重试 | 拆上传失败 AC + 重试 AC + 幂等 AC |
| BP-U2 | unhappy | F2 | 评分服务超时 / 回调失败 | 拆评分等待 + 超时提示 + 异步查询 + 后台补偿 |
| BP-U3 | unhappy | F3 | 用户重复提交同一题目 | 拆前端限制 + 后端幂等 + 历史结果回看 |
```

### Path ID 命名规则（跨阶段稳定）

| 前缀 | 类型 | 说明 |
|---|---|---|
| `BP-H{n}` | happy | Happy path · 主流程闭环 |
| `BP-U{n}` | unhappy | Unhappy path · 关键失败场景 |
| `BP-E{n}` | edge | Edge case · 罕见但需覆盖 |

> Path ID 由 PRD §X Coverage Matrix 引用 (`Source Path` 列)；ID 一旦发布永不变更。

### 强制要求

- **至少 1 条 happy path** (BP-H1)
- **3–5 条 unhappy path** (BP-U1 ~ BP-U5)
- 每条标注所属 Feature ID（追溯 §2 Feature List）
- 每条给 PRD 拆解提示（具体到"应拆几个 Story / 应覆盖哪些 AC 类型"）

### 与下游的接口契约

- **Product Planner v4.6**：PRD §3 按 Feature 拆 Story 时，对照每条 BP-X 拆 AC；§X Coverage Matrix 必须追溯每条 BP-X → AC
- **Eng Reviewer v4.0**：§X Coverage Verification 校验追溯完整性（警示性）

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
- 必须输出 Epic 合计行（Product Planner §7 Capacity Summary 偏差校验依赖此值）

---

## §9 §7 Technology Expectations to IT Architect（v1.6 重写）

> **v1.6 重大变化**：删除 v1.4 §7.1 技术方向 / §7.2 关键技术约束 / §7.3 Layer 1 引用回填等子节。  
> Solution 完全去技术化，§7 只产出**对 IT Architect 的业务期望 / 约束 / 待澄清问题** —— 结构化 EXP-{n} ID + must/should/nice 优先级 + 来源。  
> IT Architect **单向消费** Solution §7（wiki-pull）→ 在 Architecture LATEST / ADR 中给出技术决策；**不回写 Solution**。

### §7 章节格式（强制）

```markdown
## §7 Technology Expectations to IT Architect

> ⚠️ Solution Architect 不写技术选型。完整架构（三层 / C2 / C3 / ERD / API / ADR / 7 强制 SVG）
> 由 **IT Architect** 产出，路径：`Project/{project}/Architecture/{epic-slug}/LATEST.md`
> 
> 本章节传达：① 业务对 IT 的硬期望（must） / 重要期望（should） / 加分项（nice）；
> ② 待 IT Architect 在 ADR 中澄清的问题（ITQ-{n}）；
> ③ Architecture LATEST 引用指针（只读 · 不回填）。

### 7.1 Expectations 清单（EXP-{n}）

| EXP ID | 优先级 | 描述 | 来源 / 理由 |
|---|:---:|---|---|
| **EXP-1** | must | 评分必须可追溯到 prompt 版本 | 业务合规 + 用户投诉处理（Value §1 Brief） |
| **EXP-2** | must | 团队主要熟 .NET，BE 优先 .NET 实现 | 团队能力约束（Step 0.5 PM 输入） |
| **EXP-3** | must | 评分需支持异步处理 | NFR PERF-T2（API p95 ≤ 500ms 难以同步达成） |
| **EXP-4** | should | 复用现有 IOC 用户体系（unionId） | 减少账号体系重复 + 现有基础设施 |
| **EXP-5** | should | 录音文件保留 ≥ 3 年 | NFR RETN-T2（教育评分记录） |
| **EXP-6** | nice | 移动端可离线缓存历史报告 | 增强体验，非必需 |

### 7.2 Open Questions to IT Architect（ITQ-{n}）

> 待 IT Architect 在 Architecture LATEST / ADR 中给出结论。Solution 不回填，结论以 IT Architecture 为准。

| ITQ ID | 问题 | 期望结论形式 |
|---|---|---|
| **ITQ-1** | AI 评分异步回调失败的兜底策略？ | ADR：明确重试 / 死信队列 / 人工 fallback 的边界 |
| **ITQ-2** | unionId 绑定幂等性如何保证？ | Layer 3 Component：给出幂等 key 设计 + 冲突处理 |
| **ITQ-3** | 短轮询频率与服务端 LongConnection 取舍？ | ADR：性能 vs 实现复杂度权衡 |

### 7.3 Architecture LATEST 引用指针（只读 · v1.6 不回填）

| 项 | 值 |
|---|---|
| Architecture LATEST | `Project/{project}/Architecture/{epic-slug}/LATEST.md` |
| Wiki | `/{project}/{epic-slug}-PRD/architecture` |
| 状态 | ✅ 已产出 / ⏳ 待 IT Architect 启动 / ❌ 缺失 |
| 关联 EXP / ITQ | IT Architect 必须在 ADR 中 trace 本 Solution 每条 EXP-{n} / ITQ-{n} 的处理方式 |

> **v1.6 显式声明**：本指针为**只读**。IT Architect 完成后不回写 Solution 文件，PM 通过 wiki 查看 Architecture 详情。如 Architecture 给出的方案与 Solution §7 EXP 冲突，由 Eng Reviewer 在评审时发"反向 Refinement Request to IT Architect"（不发到 Solution）。
```

### EXP / ITQ 命名规则（跨阶段稳定）

| 前缀 | 类型 | 命名 | 说明 |
|---|---|---|---|
| `EXP-{n}` | Expectation | 单调递增 | 业务期望 / 约束。一经发布永不变更；删除走退役 |
| `ITQ-{n}` | IT Open Question | 单调递增 | 待 IT Architect 澄清；结论由 IT Architect 在 Architecture LATEST / ADR 中给出 |

### 强制要求

- **§7.1 EXP 清单**：
  - **≥1 条 must**（Epic 必有 1 项业务侧硬期望，否则 Solution 不应启动 IT Architect）
  - 每条 EXP 描述 **≥10 字**，来源/理由必须有具体依据（如"来自 Step 0.5 PM 输入 / Value §1 Brief / NFR Tier ID / 团队能力"）
  - 优先级三选一：`must` / `should` / `nice`
  - EXP-{n} ID 一经发布永不变更，删除走退役归档
- **§7.2 ITQ 清单**：
  - 可为空（如无待 IT 澄清的问题）
  - 每条 ITQ 必须给"期望结论形式"（不能是开放式提问）
- **§7.3 Architecture 引用**：状态字段必填（已产出 / 待启动 / 缺失）

### 禁止

- ❌ **EXP 中写技术选型**（如"使用 RabbitMQ" / "采用 Redis Stream"）→ 只能写"需要异步队列能力" / "需要消息重试与死信能力"
- ❌ **EXP 中写组件设计 / API 设计 / ERD 字段**（IT Architect 职责）
- ❌ **回填 Architecture LATEST 内容到 §7.3**（v1.6 不回路：IT → Solution 单向，无 patch）
- ❌ **EXP 引用未在本 §7.1 定义的 ID**（必须先在表中定义）
- ❌ **画完整 C2 / C3 / ERD / Sequence 图**（IT Architect 职责）
- ❌ **触发 fireworks-tech-graph 生图**（IT Architect 职责）

### 与 IT Architect 的接口契约（v1.6）

- IT Architect 启动时 wiki-pull Solution LATEST → 读取 §7.1 EXP 全表 + §7.2 ITQ → 在 Architecture LATEST 各 Layer 章节 / ADR 中 trace 每条 EXP / ITQ 的处理（消费规则由 `skills/it-architecture-spec/SKILL.md` 定义）
- IT Architect 发现 EXP 业务期望间矛盾（如 EXP-1 must 与 EXP-3 must 冲突）→ 发"反向 Refinement Request to PM"，**不发到 Solution Architect**
- 三份产出（Solution / NFR / Architecture）彼此独立，无 patch、无 refine 反向触发；同步通过 Wiki Publisher 的协作元数据（Architecture last_published_at vs Solution 引用 timestamp）

---

## §9.5 §8 NFR Reference（v1.4 新增）

> **v1.4 新增章节**：NFR Targets 由 **NFR Architect** 独立产出，Solution §8 仅做引用。

### §8 章节格式（强制）

```markdown
## §8 NFR Reference

> ⚠️ 本 Epic 的 NFR Targets 由 **NFR Architect** 独立产出。本章节仅引用，不重写。

### 8.1 NFR LATEST 引用

| 项 | 值 |
|---|---|
| 路径 | `Project/{project}/NFR/{epic-slug}/LATEST.md` |
| 回退路径 | `Project/{project}/NFR/project-wide/LATEST.md`（Epic 级缺失时） |
| Wiki | `/{project}/{epic-slug}-PRD/nfr` 或 `/{project}/project-wide-nfr` |
| 状态 | ✅ 已产出 / ❌ 未产出（建议调用 NFR Architect）|

### 8.2 关键摘要（NFR 已产出时填）

> 8 类档位摘要（仅一行总结，详细见 NFR LATEST §0 NFR Brief）

| NFR | 档位 | 关键值 |
|---|:---:|---|
| 性能 SLA | 中 | API p95 ≤ 500ms / 异步 ≤ 30s |
| 可用性 SLA | 中 | 99.9% |
| 容量 | 中 | DAU 10k / 峰值 1k QPS |
| 数据安全 | 中 | 含 PII |
| 合规 | 中 | 等保二级 + 教育部备案 |
| 保留 | 中 | 3 年 |
| 地域 | 低 | 仅大陆 |
| 业务量级 | medium | — |

### 8.3 NFR 未产出时的提示

> ❌ NFR LATEST 不存在。建议调用 NFR Architect 产出 NFR Targets：
>   - 推荐时机：Solution 完成后 / IT Architect 启动前
>   - 调用 agent：NFR Architect v1.0
```

### 强制要求

- §8.1 必填（路径 + 状态）
- §8.2 NFR 已产出时必填；未产出时填 §8.3 提示
- **禁止**在 §8 重写 NFR 详细字段（仅引用 + 摘要）

---

## §10 §9 Story List 预览格式

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
Read skills/story-splitting-spec/SKILL.md
```

加载位置：solution-architect.agent.md 的 **§Step 0 启动协议** 之后、§产出 Solution Brief 之前必须先 Read 本文件与 story-splitting-spec。

---

## §13 强制规则

必须：
- 章节锚点严格按 §1 顺序产出
- Stable ID 体系（F / P / J / BP / **EXP / ITQ** v1.6 / S-OQ）跨阶段稳定，禁止重排
- §2 Feature List 每 Feature 通过 `story-splitting-spec` Feature Gate，并含 Description + Value + T-shirt + 关联 Persona
- §3 Journey 每 Stage 含 Persona × Action × Touchpoint
- §4 Process Flow ≥1 Happy + ≥1 Unhappy
- §5 流程难点 ≥1 happy (BP-H1) + 3-5 unhappy (BP-U1..)，每条标 Feature + 拆解提示（v1.4）
- §6 T-shirt 与 Unit Range 严格按映射
- **§7 Technology Expectations（v1.6 重写）**：§7.1 EXP ≥1 条 must + 优先级三选一 + 来源/理由具体；§7.2 ITQ 可空，但每条必给期望结论形式；§7.3 Architecture 引用指针含状态字段
- §8 NFR Reference 必填，含路径 + 状态 + 摘要或调用提示
- §9 Story List 每个 Story 有 Stable ID
- §10 OQ 必须 propagate Value 所有 status=open 条目

禁止：
- Feature ID 重排（任何场景）
- §2 Feature List 出现名称模糊、缺输入→处理→输出闭环、跨系统大杂烩或少于 3 个预估 Story 且无 PM override 的 Feature
- §2 Feature List 出现未在 §3 Journey / §5 流程难点关联的孤立 Feature
- 越权写完整 Story AC（Product Planner 职责）
- **v1.4 严禁画完整架构图 / ERD / API**（IT Architect 职责）
- **v1.4 严禁触发 fireworks-tech-graph 生图**（IT Architect 职责）
- **v1.4 严禁在 §8 重写 NFR 详细字段**（NFR Architect 职责，仅引用）
- **v1.6 严禁在 §7 EXP 中写技术选型 / 组件设计 / API / ERD 字段**（必须是"能力 / 约束 / 期望"语义）
- **v1.6 严禁回填 §7.3 Architecture LATEST 详细内容**（IT → Solution 单向，无 patch）
- **v1.6 严禁 EXP-{n} / ITQ-{n} ID 重排或复用退役编号**
- 跳过 §9 Story List 预览
- T-shirt 估算偏离 §6 统一映射

---

## §15 Step 0.5 PM-AI 协作模板（v1.4 新增）

> Solution Architect Step 0.5 PM 输入采集协议。NFR 移除后从 22 项缩减为 14 项（PM 负担 -36%）。

### 15.1 4 阶段流程

```text
阶段 ① AI 先出 9 项初稿（30 秒）
  AI 基于 Value Frame + Persona 模板 + copilot-instructions 系统清单
  一次性产出 9 项初稿草案：
    1. Persona 详细画像（基于 Value Q3 角色展开）
    2. Scope In（基于 Value Epic 范围拟）
    3. 端到端主流程
    4. Happy Path
    5. Unhappy Path 列表
    6. 失败处理预期（与 Unhappy Path 配对）
    7. 外部系统候选（基于 copilot-instructions 系统清单勾选式）
    8. MoSCoW 优先级（基于 Feature 拟）
    9. 部分 NFR 提示（仅"是否调用 NFR Architect" 而非 NFR 详细字段）

阶段 ② PM 逐项矫正
  PM 看 AI 9 项初稿，对每项选：
    a) 接受 ✅ → 进入下一项
    b) 修改 ✏️ → 提供修正内容，AI 重新生成
    c) 重写 🔄 → AI 删除初稿，PM 直接给

阶段 ③ AI 列结构 + PM 填值（2 项）
  AI 列档位 B 的 2 项结构 / 候选清单：
    10. 核心业务规则维度（次数 / 有效期 / 资格 / 提交 / 失败计费 / 历史 / 权益 / 复现）
        → PM 逐维度填具体值
    11. 成功标准三层候选（用户侧 / 系统侧 / 业务侧）
        → PM 删除不适用 + 补业务侧细节

阶段 ④ PM 必填硬项（4 项）
  AI 不出稿，PM 直接输入档位 C 的 4 项：
    12. Scope Out（业务取舍 · AI 不能猜）
    13. 用户入口 / 触点（渠道现状 · AI 不知道）
    14. 第三方 vendor（具体合作商 · AI 容易编造）
    15. 技术栈 / 团队 / 时间 / 运营闭环约束（合并必填）
    
  任一缺失 → 软 Gate 标 [待确认] + 进入 §10 OQ
  
  注：NFR 已移除（独立到 NFR Architect agent · v1.0+）

阶段 ⑤ 总确认 + 落 frontmatter + 生成 EXP 候选清单（v1.6 新增）
  AI 把 14 项内容结构化落到 Solution Brief frontmatter
  AI 基于 14 项内容 + Step 0.3 wiki-pull 的 NFR LATEST，生成 §7.1 EXP 候选清单：
    - 第 13 项"用户入口 / 触点" → 可能产生 EXP（如"必须支持微信小程序生态"）
    - 第 14 项"第三方 vendor" → 产生 EXP（如"复用现有 IDV vendor"）
    - 第 15 项"技术栈 / 团队 / 时间 / 运营闭环约束" → 直接产生 EXP must（如"团队主要熟 .NET"）
    - NFR Tier ID（如 PERF-T2 / AVAIL-T2）→ 推导 EXP（如"评分需异步"基于 PERF-T2 API p95 不可同步达成）
  PM 总确认 EXP 候选清单（增/删/调优先级） → 进入 §1-§13 Solution Brief 产出
```

### 15.2 14 项分级总览

| 档位 | 项数 | 输入类型 | 包含 |
|---|:---:|---|---|
| A | 9 | AI 先出 + PM 矫正 | Persona / Scope In / 主流程 / Happy Path / Unhappy Path / 失败处理 / 外部系统 / MoSCoW / NFR 调用提示 |
| B | 2 | AI 列结构 + PM 填值 | 业务规则维度 / 成功标准三层 |
| C | 4 | PM 必填硬项（无 AI 稿） | Scope Out / 触点 / 第三方 vendor / 技术栈+团队+时间+运营闭环 |
| 元数据 | — | 系统自动 | Project + Epic + maintainer 时间戳 |

### 15.3 frontmatter 写入约定

```yaml
pm_input_14:
  persona: [详细画像]
  scope_in: [...]
  scope_out: [...]
  e2e_flow: [...]
  happy_path: [BP-H1 大纲]
  unhappy_paths: [BP-U1, BP-U2, ...]
  failure_handling: { BP-U1: ..., BP-U2: ... }
  user_entries: [...]               # 渠道触点
  external_systems: [...]           # 内部系统勾选
  third_party_vendors: [...]
  business_rules:
    counts: ...
    validity: ...
    eligibility: ...
    submission: ...
    fail_costs: ...
    history: ...
    rights: ...
    redo: ...
  success_criteria:
    user_side: ...
    system_side: ...
    business_side: ...
  moscow: { must: [...], should: [...], could: [...] }
  constraints:
    tech_stack: { must_use: [...], cannot_use: [...] }
    team: [...]
    time: [...]
    ops_loop: [...]
  nfr_call:
    status: called | skipped
    nfr_latest_path: Project/{project}/NFR/{scope}/LATEST.md  # 如已调用
```

---

## §16 版本变更

| 版本 | 日期 | 变更 |
|------|------|------|
| 1.7.0 | 2026-06-29 | **接入 story-splitting-spec**：Solution 阶段 Feature List 按 Feature=系统能力执行 Feature Gate，新增名称可独立理解、输入→处理→输出闭环、尽量不跨系统、每 Feature 预估 ≥3 Story、后台二级菜单可作为 Feature 等规则；加载方式新增 `skills/story-splitting-spec/SKILL.md`，保持完整 Story AC 仍由 Product Planner 负责。 |
| 1.6.0 | 2026-05-22 | **v3.8 Solution 完全去技术化 + 独立产出 + 无回路**。①§7 重写为 **Technology Expectations to IT Architect**：结构化 EXP-{n} ID + 优先级 must/should/nice + 来源/理由具体；删除 v1.4 §7.1 技术方向 / §7.2 技术约束 / §7.3 Layer 1 引用回填等子节；②§7.2 ITQ-{n} 待澄清问题保留，但结论由 IT Architect 在 Architecture LATEST / ADR 中给出（Solution 不回填）；③§7.3 Architecture 引用指针明确为"只读 · 不回填"；④Stable ID 表新增 `EXP-{n}` / `ITQ-{n}` / `BP-{H/U/E}{n}` 三类；⑤§13 强制规则补"§7 EXP ≥1 must / 来源具体 / 严禁技术选型"；禁止规则补"严禁回填 §7.3 / 严禁 EXP/ITQ ID 重排"；⑥§15.1 Step 0.5 阶段 ⑤ 新增"生成 §7 EXP 候选清单"产出；⑦明确三份产出（Solution / NFR / Architecture）独立 + 无 patch 回路。 |
| 1.4.0 | 2026-05-19 | **重大重构 v1.4**：§5 GWT Top 3-5 → **流程难点与 PRD 拆解提示**（Path ID BP-H/U/E + 拆解提示 + Coverage Matrix 接口）；§7 Tech High-level 四段式 → **Technology Direction 瘦版**（方向 + 约束 + 引用 IT Architect Layer 1 + 待 IT Architect 问题清单）；**§8 新增 NFR Reference**（引用 NFR LATEST · 不重写）；**删除"复杂边界触发 fireworks-tech-graph"段落**（职责完全下放 IT Architect）；§9-§13 编号下移；新增 §15 Step 0.5 PM-AI 协作 4 阶段模板（22 项→14 项 · NFR 移除 · PM 负担 -36%）。|
| 1.0.0 | 2026-05-08 | 初版。从 solution-architect.agent v1.0 抽离 Solution Brief 章节锚点 + Stable ID 体系 + Feature List 表格 + Journey/Process/GWT/T-shirt/Tech high-level/Story List 预览的格式标准与强制规则。 |
