---
name: it-architecture-spec
description: IT Architecture 写作规范——三层架构（Layer 1 Context & Business / Layer 2 Solution / Layer 3 Component & Data）+ C4 4 层映射 + TOGAF 4 域覆盖 + ADR 标准模板 + QAS 接口契约（消费 NFR LATEST）+ 可视化产出规则（7 强 4 可选 + fireworks-tech-graph 调用）+ 跨电脑协作（wiki-pull + maintainer 标识）。IT Architect 在产出 Architecture 前必须 Read 本文件。
version: 1.2.0
updated: 2026-05-19
maintainer: @frankzhey
applies-to: [it-architect]
---

# IT Architecture 写作规范

本 SKILL 是 **IT Architect** 产出三层架构的唯一规范。它融合 C4 模型 + TOGAF 4 域，定义章节锚点、必画图清单、ADR 模板、QAS 接口契约、可视化产出规则与跨电脑协作约定。IT Architect 在 Step 3 产出前必须显式 Read 本文件。

> v1.1 采用 v3.7 简化协作模式：落盘到 `Project/{project}/Architecture/{epic}/`，所有权通过 frontmatter `maintainer` 字段标识。

---

## §1 章节锚点（必须按此顺序输出）

| § | 章节 | 强制 |
|---|---|---|
| §0 | Architecture Brief（一页摘要 · 目标状态 + 关键设计意图）| ✅ |
| §1 | **Layer 1: Context & Business Architecture** | ✅ |
| §1.1 | 业务上下文 | ✅ |
| §1.2 | 用户与外部 actors（角色 / Persona / 量级 / 地域） | ✅ |
| §1.3 | C1 System Context Diagram（必画 SVG）| ✅ |
| §1.4 | 业务能力地图（Capability Map） | ✅ |
| §1.5 | 合规与法规约束 | ✅ |
| §2 | **Layer 2: Solution Architecture** | ✅ |
| §2.1 | C2 Container Diagram（必画 SVG）| ✅ |
| §2.2 | Service Boundary Table（Owns / Does NOT Own）| ✅ |
| §2.3 | Deployment Topology（必画 SVG）| ✅ |
| §2.4 | Runtime Stack（语言 / 框架 / 中间件） | ✅ |
| §2.5 | 集成方案 + Sequence（必画 ≥1 happy + ≥1 failure SVG）| ✅ |
| §2.6 | Observability + CICD + Release Strategy | ✅ |
| §2.7 | Quality Attribute Scenarios (QAS · 消费 NFR LATEST) | ✅ |
| §3 | **Layer 3: Component & Data Architecture** | ✅ |
| §3.1 | C3 Component Diagram（按需 SVG）| ⭕ |
| §3.2 | ERD / Logical Data Model（必画 SVG）| ✅ |
| §3.3 | Data Flow Diagram（必画 SVG）| ✅ |
| §3.4 | API Contracts（OpenAPI 摘要） | ✅ |
| §3.5 | Data Contract / Schema Evolution | ✅ |
| §3.6 | Data Lineage & Retention | ✅ |
| §3.7 | Security Architecture（auth flow + 轻量 STRIDE） | ✅ |
| §7 | **Cross-cutting Concerns（5 类）** | ✅ |
| §7.1 | Security Architecture（与 §3.7 互补：组件级 vs 横切级） | ✅ |
| §7.2 | Quality Attribute Scenarios (QAS · 横切汇总) | ✅ |
| §7.3 | Cost View（关键决策的成本维度） | ⭕ |
| §7.4 | Reliability & DR Strategy | ✅ |
| §7.5 | Operability（运维交付清单） | ✅ |
| §8 | **Architecture Decision Records (ADR ≥3 条 · 独立 adr/ 子目录)** | ✅ |
| §9 | Migration & Implementation Plan | ⭕（refinement 场景必填） |
| §10 | Risks & Open Questions | ✅ |
| §11 | Handoff to Eng Reviewer + Product Planner | ✅ |
| §12 | Changelog | ✅ |

---

## §2 三层架构 × C4 × TOGAF 融合矩阵（核心理论）

| 三架构维度 | 对应 C4 层 | 对应 TOGAF 域 | 主要产物 |
|---|---|---|---|
| Layer 1 业务上下文 | C1 System Context | Business Architecture | C1 图 + Stakeholder + Capability Map |
| Layer 2 应用与系统 | C2 Container + Deployment | Application + Technology Architecture | C2 图 + Service Boundary + Deployment + Runtime + Integration |
| Layer 3 组件与数据 | C3 Component + Data Flow | Data Architecture + Application Detail | C3 + ERD + Data Flow + API + Security |
| Cross-cutting | — | TOGAF Phase G/H | Security / QAS / Cost / Reliability / Operability |
| ADR | — | TOGAF ADR | Status / Context / Decision / Consequence / Alternatives |

---

## §3 必画 SVG 清单（7 强 + 4 可选）

> AI 在 Step 3 章节产出时必须按本表触发 `fireworks-tech-graph` SKILL 生成对应 SVG。

| # | 层 | 章节 | 类型 | 强制 | fireworks-tech-graph 类型 | 默认风格 |
|---|---|---|---|---|---|---|
| 1 | L1 | §1.3 C1 System Context | C1 | ✅ | system_context | claude-official |
| 2 | L1 | §1.4 业务能力地图 | Capability | ⭕ | capability_map | claude-official |
| 3 | L2 | §2.1 C2 Container | C2 | ✅ | container | claude-official |
| 4 | L2 | §2.3 Deployment Topology | Deployment | ✅ | deployment | claude-official |
| 5 | L2 | §2.5 Sequence — Happy Path | Sequence | ✅ | sequence | claude-official |
| 6 | L2 | §2.5 Sequence — Failure Path | Sequence | ✅ | sequence | claude-official |
| 7 | L2 | §2.6 Observability Data Flow | Data Flow | ⭕ | data_flow | claude-official |
| 8 | L3 | §3.1 C3 Component（复杂容器） | C3 | ⭕ | component | claude-official |
| 9 | L3 | §3.2 ERD | ERD | ✅ | erd | claude-official |
| 10 | L3 | §3.3 Data Flow | Data Flow | ✅ | data_flow | claude-official |
| 11 | L3 | §3.7 Security Auth Flow | Sequence | ⭕ | sequence | claude-official |

**7 强制** = C1 + C2 + Deployment + 2 Sequence + ERD + Data Flow  
**4 可选** = Capability Map + Observability DF + C3 + Security Auth

---

## §4 落盘结构（v3.7 简化模式）

```text
Project/{project}/Architecture/{epic-slug}/
├── LATEST.md                                ← 指针: current: {epic-slug}-architecture-{stamp}.md
├── {epic-slug}-architecture-{stamp}.md     ← Markdown 主文档
├── diagrams/                                ← SVG 子文件夹
│   ├── layer1-c1-system-context.svg        ← 强制
│   ├── layer1-business-capability-map.svg  ← 可选
│   ├── layer2-c2-container.svg              ← 强制
│   ├── layer2-deployment-topology.svg       ← 强制
│   ├── layer2-sequence-happy-path.svg       ← 强制
│   ├── layer2-sequence-failure-path.svg     ← 强制
│   ├── layer2-observability-data-flow.svg   ← 可选
│   ├── layer3-c3-component-{name}.svg       ← 按需
│   ├── layer3-erd.svg                       ← 强制
│   ├── layer3-data-flow.svg                 ← 强制
│   └── layer3-security-auth-flow.svg        ← 可选
├── diagrams-manifest.json                   ← SVG 来源 / 风格 / 状态追踪
├── adr/                                     ← Architecture Decision Records
│   ├── ADR-001-{slug}.md
│   ├── ADR-002-{slug}.md
│   └── ADR-003-{slug}.md
└── refinement-requests-to-pm/               ← 反向 RR（可选 · 仅有时）
    └── upstream-refinement-{stamp}.md
```

---

## §5 diagrams-manifest.json 结构

```json
{
  "project": "spk2challenge-miniprogram",
  "epic_slug": "speaking-challenge-and-scoring",
  "architecture_ref": "Project/.../Architecture/{epic-slug}/LATEST.md",
  "default_style": "claude-official",
  "maintainer": "@ITArch",
  "diagrams": [
    {
      "id": "layer1-c1-system-context",
      "layer": 1,
      "section_in_md": "§1.3",
      "type": "system_context",
      "style": "claude-official",
      "svg_path": "diagrams/layer1-c1-system-context.svg",
      "generated_at": "2026-05-19-1430",
      "generated_by": "fireworks-tech-graph v1.0",
      "status": "draft | approved"
    }
  ]
}
```

> v3.7 简化：**不存** `source_md_hash`，stale 检测改为"PM 手动触发"（PM 显式说"重生 X 图"才重生，简化运维）。

---

## §6 Markdown 引用 SVG 的标准 grammar

```markdown
### §2.1 C2 Container Diagram

![C2 Container](./diagrams/layer2-c2-container.svg)

> **图源**：`fireworks-tech-graph` (风格: claude-official)  
> **manifest**: `diagrams-manifest.json#layer2-c2-container`  
> **上次生成**: 2026-05-19-1430

**关键 Container**：
| Container | 职责 | 技术栈 | Owner |
|---|---|---|---|
| ... |
```

---

## §7 ADR 标准模板（每条 ADR 独立 .md 文件）

```markdown
---
adr_id: ADR-001
slug: async-scoring-via-queue
status: proposed | accepted | deprecated | superseded
created: 2026-05-19-1430
maintainer: "@ITArch"
related_epics: [speaking-challenge-and-scoring]
supersedes: null
superseded_by: null
tags: [async, scoring, queue]
---

# ADR-001: 异步评分通过消息队列实现

## Status
proposed | accepted | deprecated

## Context
[当前的技术约束 / 业务挑战，为什么需要这个决策]

## Decision
[做出的决策本身，一句话明确]

## Architecture Principle Applied（v1.1 隐式引用）
[遵循的架构原则，如：Async by Default / API First / Stateless Services]

## Consequences

### Positive
- ...

### Negative
- ...

### Risks
- ...

## Alternatives Considered（必填 ≥2 个备选）

### Alternative 1: 同步评分
- Pros: ...
- Cons: ...
- 否决原因: ...

### Alternative 2: WebSocket 推送
- Pros: ...
- Cons: ...
- 否决原因: ...

## References
- [相关 SKILL / 标准 / 文档链接]
```

> **强制规则**：每个 Epic 至少 3 条 ADR；每条 ADR 必须含 ≥2 个 Alternatives Considered；ADR 中必须含 `Architecture Principle Applied` 段落（v1.1 隐式融入设计原则）。

---

## §8 Quality Attribute Scenarios (QAS) 接口契约（消费 NFR LATEST）

IT Architect Layer 2 §2.7 和 Cross-cutting §7.2 的 QAS **直接消费** NFR Architect 产出的 `Project/{project}/NFR/{scope}/LATEST.md`。

### 8.1 QAS 生成规则

```text
For each NFR target in NFR LATEST.nfr_targets:
  生成对应 Quality Attribute Scenario:
    - Stimulus（触发场景）
    - Environment（环境条件）
    - Response（系统响应）
    - Response Measure（量化指标 ← 直接取 NFR 数值）
```

### 8.2 QAS 示例

```text
NFR §1 Performance SLA: api_p95_ms = 500

→ IT Architect §2.7 QAS-1:
  ID: QAS-1
  Source: 用户
  Stimulus: 提交评分请求
  Environment: 正常负载（≤1k QPS · 来自 NFR §3 Capacity 中档）
  Response: 系统返回评分结果
  Response Measure: p95 ≤ 500ms（来自 NFR §1 Performance 中档）
  Architecture Tactics: 异步队列 + 缓存 + Circuit Breaker
```

### 8.3 NFR 缺失时处理

- NFR LATEST 不存在 → §2.7 QAS 标 `[待 NFR Architect 产出后回填]`，进入 §10 OQ
- NFR LATEST 存在但部分字段缺失 → 该字段 QAS 标 `[待 NFR 校准]`

---

## §9 跨电脑协作规则（v3.7 简化模式）

### 9.1 wiki-pull 场景（IT Architect 在自己电脑，没有 PM 的本地工作区）

> IT Architect 通常是独立角色（另一个人在另一台电脑），通过 Wiki 拉取上游。

```text
启动指令: project={project}, epic={epic-slug}

白名单拉取:
  ado/wiki_get_page_content path="/{project}"                       → outputs/wiki-cache/{project}/value.md
  ado/wiki_get_page_content path="/{project}/{epic}-solution"       → outputs/wiki-cache/{project}/{epic}/solution.md
  ado/wiki_get_page_content path="/{project}/{epic}-PRD/nfr"        → outputs/wiki-cache/{project}/{epic}/nfr.md (如有)

黑名单（严禁拉取）:
  ❌ /{project}/{epic}-PRD       (PRD merged 页，IT Architect 不消费 PRD 详情)
  ❌ /{project}/{epic}-PRD/ui-prototype / engineering-review / task-planning
  ❌ /{other-project}/*

校验 Wiki 协作元数据（Wiki Publisher v3.2 注入）:
  - status: synced ✅ → 继续
  - status: local_ahead ⚠️ → 阻塞 + 提示 "PM 本地超前 Wiki，请先让 PM 发布最新版"
```

### 9.2 local 场景（PM 自己跑 IT Architect · 罕见）

```text
启动指令: project={project}, epic={epic-slug}
本地存在 Project/{project}/Value/LATEST.md + Solution/{epic}/LATEST.md 时：
  - 直接读取本地（不需 wiki-pull）
  - frontmatter maintainer 标 @PM-{name}（不是 @ITArch）
```

### 9.2-bis manual-input 应急场景（v1.2 新增 · 第三 mode）

> 适用：Wiki 不可用 / Value 还未发布 / 紧急评审 / 离线工作。  
> 与 Eng Reviewer v3.1 wiki-fallback / manual-input 模式同款设计。

```text
PM 在 IT Architect 启动时选择"B. 手工粘贴"后：
  1. 按顺序粘贴 Value Frame / Solution Brief / NFR LATEST 内容
  2. 至少必须粘贴 Solution（无 Feature List 无法做架构 · 阻塞）
  3. IT Architect 临时缓存到:
     outputs/manual-input/{project}/{epic-slug}/
       ├── value.md      (如粘贴 · 否则缺失)
       ├── solution.md   (必须)
       └── nfr.md        (如粘贴 · 否则 QAS 标 [待 NFR 校准])
  
  ⚠️ 严禁回写本地 Project/{project}/Value 或 Solution 或 NFR（保 PM 私有工作区干净）

落盘 frontmatter:
  mode: manual-input
  source:
    type: manual-input
    cache_dir: outputs/manual-input/{project}/{epic-slug}/
    value: { provided: true | skip }
    solution: { provided: true | skip }
    nfr: { provided: true | skip }

§10 Risks 强制标:
  "IT-MANUAL: 上游为 PM 手工粘贴非 Wiki 权威版本，内容失真风险，
   建议 PM 后续发布正版到 Wiki 后通过 wiki-pull refinement 同步"
```

### 9.3 frontmatter 强制字段（v3.7 所有权标识）

```yaml
---
project: {project}
epic: EPIC-{slug}
created: {YYYY-MM-DD-HHmm}
maintainer: "@ITArch" | "@{pm-name}"   # ⭐ v3.7 必填
mode: local | wiki-pull
upstream_snapshot:
  value_wiki_path: /{project}                                       # wiki-pull 模式
  value_pulled_at: {YYYY-MM-DD-HHmm}                                # wiki-pull 模式
  solution_wiki_path: /{project}/{epic-slug}-solution               # wiki-pull 模式
  solution_pulled_at: {YYYY-MM-DD-HHmm}                             # wiki-pull 模式
  nfr_wiki_path: /{project}/{epic-slug}-PRD/nfr                     # 可选
  # local 模式则记录本地 LATEST.md 时间戳
status: draft | in_review | approved
skills_loaded:
  - skills/project-context-loader/SKILL.md
  - skills/it-architecture-spec/SKILL.md
  - skills/fireworks-tech-graph/SKILL.md
  - skills/nfr-spec/SKILL.md  # 如消费 NFR LATEST
---
```

---

## §10 反向 Refinement Request（IT Architect → PM）

当 IT Architect 评审 Value / Solution 发现问题时，输出结构化 RR：

### 10.1 落盘路径

```text
Project/{project}/Architecture/{epic-slug}/refinement-requests-to-pm/
└── upstream-refinement-{stamp}.md
```

### 10.2 RR 模板

```markdown
---
rr_id: RR-{stamp}
project: {project}
epic: EPIC-{slug}
created: {YYYY-MM-DD-HHmm}
maintainer: "@ITArch"
target_pm: "@PM-A"                       # 通过 Wiki Value/Solution 页 maintainer 字段定位
severity: high | medium | low
status: open | pm-accepted | pm-rejected
---

# Upstream Refinement Request — {issue-slug}

## 发现问题
[评审中发现的 Value / Solution 缺陷]

## 影响架构章节
- Layer 2 §2.X: {影响描述}
- ADR-NNN: {可能需要变更的决策}

## 建议
[结构化建议，含 ≥1 个候选方案]

## PM 决策栏（PM 填写）
- [ ] 接受 → 触发 Value / Solution refinement → 重新发布 Wiki
- [ ] 拒绝 → IT Architect 在 §10 Risks 标 "PM 不接受架构反馈"
- 决策理由: [PM 填]
```

### 10.3 通知通道

- Wiki 发布到 `/{project}/{epic-slug}-PRD/architecture/upstream-refinement-{stamp}`
- 群消息 @ target_pm（人工通知）

---

## §11 Quality Gate（落盘前自检 · 阻塞性）

**章节合规**
- [ ] §0–§12 章节锚点齐全
- [ ] Layer 1 / 2 / 3 三层完整
- [ ] Cross-cutting 5 类全填（§7.3 Cost 可选）
- [ ] §8 ADR ≥ 3 条，每条含 ≥2 Alternatives + Architecture Principle Applied 段

**必画图合规**
- [ ] 强制 7 张 SVG 全部生成（C1 / C2 / Deployment / 2 Sequence / ERD / Data Flow）
- [ ] 所有 SVG 在 diagrams-manifest.json 中登记
- [ ] Markdown 引用 grammar 正确（图源 + manifest ID + 上次生成时间）

**NFR 集成合规**
- [ ] §2.7 QAS 已消费 NFR LATEST.nfr_targets（如 NFR 存在）
- [ ] NFR 缺失时 QAS 标 `[待 NFR 校准]` 且进入 §10 OQ

**协作合规（v3.7）**
- [ ] frontmatter `maintainer` 字段已填写（@ITArch 或 @{pm-name}）
- [ ] wiki-pull 模式：frontmatter `upstream_snapshot` 含完整 wiki path + pulled_at
- [ ] wiki-pull 模式：Wiki 协作元数据 status=synced 校验通过
- [ ] 没有越权改 Value / Solution 文件（仅可读取，不可修改）

**落盘合规**
- [ ] 落盘到 `Project/{project}/Architecture/{epic-slug}/`
- [ ] LATEST.md 已更新
- [ ] adr/ 子目录含 ≥3 条 ADR
- [ ] diagrams/ 子目录含强制 7 张 SVG

修复 3 次仍不通过 → 告知用户哪些项无法自动修复。

---

## §12 强制规则

必须：
- 先 Read 本 SKILL + `fireworks-tech-graph/SKILL.md` 再产出
- 三层 + Cross-cutting + ADR 全部输出
- 强制 7 张 SVG 全部生成
- ADR ≥3 条，每条 ≥2 Alternatives + Architecture Principle Applied
- §2.7 QAS 必须基于 NFR LATEST（不能凭空创造）
- frontmatter `maintainer` 必填
- wiki-pull 模式：status=synced 校验

禁止：
- 凭记忆生成（必须先 Read SKILL）
- 跳过任一强制章节
- 强制 7 张 SVG 缺图（C3 等可选不算）
- ADR 缺 Alternatives Considered 段落
- 拉取 Wiki 黑名单路径（/{project}/{epic}-PRD 等）
- 越权改 Value / Solution / PRD 文件
- 一次产出多个 Epic 的 Architecture（一次只一个）

---

## §13 变更记录

| 版本 | 日期 | 变更 |
|---|---|---|
| 1.2.0 | 2026-05-19 | **新增 §9.2-bis manual-input 应急场景**（配合 it-architect agent v1.3 第三 mode）：PM 手工粘贴 Value/Solution/NFR → 缓存 outputs/manual-input/{p}/{epic}/ → 严禁回写 PM 本地 → §10 Risks 标 IT-MANUAL flag。frontmatter source 块扩展（type / cache_dir / value/solution/nfr provided 标识）。|
| 1.1.0 | 2026-05-19 | 初版 v1.1：三层架构 + C4 + TOGAF 融合矩阵；7 强制 + 4 可选 SVG 清单；ADR 模板（含 Architecture Principle Applied 段隐式融入设计原则）；与 NFR Architect QAS 接口契约；v3.7 简化协作模式（落盘到 `Project/{project}/Architecture/{epic}/` + frontmatter maintainer 标识）；wiki-pull 白名单 + Wiki 协作元数据 status 校验；反向 Refinement Request to PM 模板与通道；Cross-cutting 5 类（Security/QAS/Cost/Reliability/Operability）。|
