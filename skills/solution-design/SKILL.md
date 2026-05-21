---
name: solution-design
description: Solution Brief 写作规范 v1.4——业务方案为主（不再包含详细技术架构）。§5 改为"流程难点与 PRD 拆解提示"（替代 GWT，给 PRD 拆解指引）；§7 Technology Direction 瘦身为方向 + 约束 + 待 IT Architect 问题清单（详细架构由 IT Architect 产出）；§8 新增 NFR Reference（引用 NFR LATEST，不重写）；Step 0.5 PM-AI 协作 4 阶段（22→14 项）。Plan 阶段产出 Solution Brief 时必须 Read 本文件。
version: 1.4.0
updated: 2026-05-19
maintainer: @frankzhey
applies-to: [solution-architect]
---

# Solution Brief 写作规范 v1.4

本 SKILL 定义 Solution Brief 的章节结构、ID 体系、Feature List 格式、Journey/Process/流程难点/T-shirt/NFR Reference/Story List 标准，由 Solution Architect 在产出 brief 前显式 Read 并执行。

> **v1.4 核心变化**：
> - **§5 GWT Top 3-5 → 流程难点与 PRD 拆解提示**（不再写形式化测试用例，转为对 PRD 拆解的结构化指引）
> - **§7 Tech High-level 四段式 → Technology Direction（瘦版）**（详细架构由 IT Architect 产出，本章节仅给方向 + 约束 + 待 IT Architect 问题）
> - **§8 新增 NFR Reference**（引用 NFR LATEST，不重写 NFR 详情）
> - **删除"复杂边界触发 fireworks-tech-graph"段落**（可视化职责完全下放 IT Architect）
> - **Step 0.5 PM-AI 协作 4 阶段**（14 项分级输入 · NFR 已移除）

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
| **§7** | **Technology Direction & Open Questions（瘦版）** | ✅ 必须 | v1.4 瘦身（见 §X 详细规范） |
| **§8** | **NFR Reference** ⭐ v1.4 新增 | ✅ 必须 | 仅引用 NFR LATEST，不重写 |
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
| Scenario (GWT) | `S1`, `S2` | 永不重排（与 Story S 编号互不冲突，因为有完整前缀区分） | Product Planner Story `upstream_refs.scenarios` 引用 |
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

## §9 §7 Technology Direction & Open Questions（v1.4 瘦版）

> **v1.4 重大变化**：详细技术架构（C2 Container / C3 Component / ERD / API / Deployment / 7 强制 SVG / ADR ≥3 条）**全部下放到 IT Architect**。  
> Solution §7 仅保留 3 个子节：**方向 + 约束 + 待 IT Architect 问题清单**。

### §7 章节格式（强制）

```markdown
## §7 Technology Direction & Open Questions

> ⚠️ 本章节不包含完整架构。完整三层架构、Container 图、API 契约、ERD、ADR 等
> 由 **IT Architect** 产出，路径：`Project/{project}/Architecture/{epic-slug}/LATEST.md`

### 7.1 技术方向（≤3 句）
- 主要的同步 / 异步边界（如：评分走异步队列 + 短轮询前端）
- 主要存储选型方向（如：MySQL + 对象存储）
- 主要集成方向（如：复用 IOC 用户体系，新建 ICS 评分通道）

### 7.2 关键技术约束
[来自 Step 0.5 PM 输入的硬约束]
- 必须用: {例 .NET / Spring}
- 不能用: {例 Python / Go}
- 现有基础设施: {例 K8s / ELK / Datadog}
- 团队能力: {例 BE 团队熟 .NET，FE 团队熟 React/Vue}

### 7.3 引用 IT Architect Layer 1（refinement 时回填）
> 待 IT Architect 产出后回填此区块的引用：
> - Layer 1 §1.3 C1 System Context: 见 Architecture LATEST §1.3
> - Layer 1 §1.4 业务能力地图: 见 Architecture LATEST §1.4
> - Architecture Wiki URL: /{project}/{epic-slug}-PRD/architecture

### 7.4 待 IT Architect 回答的问题清单
- Q1: AI 评分异步回调失败时的兜底策略？
- Q2: unionId 绑定的幂等性如何保证？
- Q3: ...
```

### 强制要求

- 必须 4 个子节全部输出（即使某节简短也保留）
- **禁止**画完整 C2 / C3 / ERD / Sequence 图（这是 IT Architect 职责）
- 7.3 区块在首版可留空（refinement 时回填）
- 7.4 必须 ≥1 个问题给 IT Architect

### 与 IT Architect 的接口契约

- Solution §7.4 问题清单是 IT Architect 启动时的"PM 期望回答清单"
- IT Architect Refinement 完成后 → PM 触发 Solution Architect refinement → 回填 §7.3 引用
- **删除 v1.0 的"复杂边界触发 fireworks-tech-graph"段落**（职责完全下放 IT Architect，由 it-architecture-spec/SKILL.md §3 7 强制 + 4 可选 SVG 规则统一管理）

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
- §5 流程难点 ≥1 happy (BP-H1) + 3-5 unhappy (BP-U1..)，每条标 Feature + 拆解提示（v1.4）
- §6 T-shirt 与 Unit Range 严格按映射
- §7 Technology Direction 4 子节全输出（瘦版 · v1.4）
- §8 NFR Reference 必填，含路径 + 状态 + 摘要或调用提示（v1.4 新增）
- §9 Story List 每个 Story 有 Stable ID
- §10 OQ 必须 propagate Value 所有 status=open 条目

禁止：
- Feature ID 重排（任何场景）
- §2 Feature List 出现未在 §3 Journey / §5 流程难点关联的孤立 Feature
- 越权写完整 Story AC（Product Planner 职责）
- **v1.4 严禁画完整架构图 / ERD / API**（IT Architect 职责）
- **v1.4 严禁触发 fireworks-tech-graph 生图**（IT Architect 职责）
- **v1.4 严禁在 §8 重写 NFR 详细字段**（NFR Architect 职责，仅引用）
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

阶段 ⑤ 总确认 + 落 frontmatter
  AI 把 14 项内容结构化落到 Solution Brief frontmatter
  PM 总确认 → 进入 §1-§13 Solution Brief 产出
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
| 1.4.0 | 2026-05-19 | **重大重构 v1.4**：§5 GWT Top 3-5 → **流程难点与 PRD 拆解提示**（Path ID BP-H/U/E + 拆解提示 + Coverage Matrix 接口）；§7 Tech High-level 四段式 → **Technology Direction 瘦版**（方向 + 约束 + 引用 IT Architect Layer 1 + 待 IT Architect 问题清单）；**§8 新增 NFR Reference**（引用 NFR LATEST · 不重写）；**删除"复杂边界触发 fireworks-tech-graph"段落**（职责完全下放 IT Architect）；§9-§13 编号下移；新增 §15 Step 0.5 PM-AI 协作 4 阶段模板（22 项→14 项 · NFR 移除 · PM 负担 -36%）。|
| 1.0.0 | 2026-05-08 | 初版。从 solution-architect.agent v1.0 抽离 Solution Brief 章节锚点 + Stable ID 体系 + Feature List 表格 + Journey/Process/GWT/T-shirt/Tech high-level/Story List 预览的格式标准与强制规则。 |
