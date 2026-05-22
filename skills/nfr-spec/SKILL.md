---
name: nfr-spec
description: Non-Functional Requirements 写作规范——8 类 NFR 行业基线 3 档候选库 + Tier ID 化（PERF/AVAIL/CAP/DATA/COMPL/RETN/REGION-T1/T2/T3）+ 业务类型推荐档位 + Value Frame 自动抽取业务背景映射 + Scope 差异化输入清单（project-wide / epic-scoped）+ NFR 间依赖校验 + 跨 PM 协作落盘规则。NFR Architect 在产出 NFR Targets 前必须 Read 本文件。
version: 1.1.0
updated: 2026-05-22
maintainer: @frankzhey
applies-to: [nfr-architect]
---

# Non-Functional Requirements 写作规范

本 SKILL 是 **NFR Architect** 产出 NFR Targets 的唯一规范。它定义 8 类 NFR 的行业基线候选档位、业务类型推荐映射、NFR 间依赖校验规则与跨 PM 协作落盘格式。NFR Architect 在 Step 2 候选生成前必须显式 Read 本文件。

> NFR Architect 是**完全可选**调用：PM 可选择跳过，下游 agent 仅 flag 不阻塞。  
> NFR 是跨知识领域（业务/合规/技术）决策点的集中收口，避免分散到 Solution / IT Architect 各处。

---

## §1 章节锚点（必须按此顺序输出）

| § | 章节 | 强制 |
|---|---|---|
| §0 | NFR Brief（一段话摘要 + 业务背景 + 推荐档位）| ✅ |
| §1 | Performance SLA | ✅ |
| §2 | Availability SLA | ✅ |
| §3 | Capacity（用户量 + 数据量）| ✅ |
| §4 | Data Sensitivity & Security | ✅ |
| §5 | Compliance & Regulation | ✅ |
| §6 | Data Retention & Archive | ✅ |
| §7 | Geo & Region | ✅ |
| §8 | NFR Inter-dependency Check | ✅ |
| §9 | Open Questions（缺失项 / 待 Tech Lead 校准）| ⭕ |
| §10 | Changelog | ✅ |

---

## §2 8 类 NFR × 3 档行业基线候选库（强制 · 唯一权威）

> AI 在 NFR Architect Step 2 生成候选时必须**严格按本表 3 档输出**，不得自创档位。  
> PM 在 Step 3 选档时可选"低/中/高/其他"，"其他"必须 PM 自定义值并标 `custom`。

> **v1.1 Tier ID 化**：每档新增稳定 ID（PERF-T1/T2/T3 等），供 Solution / Architecture / PRD / Eng Review 跨文档 trace。

### 2.1 Performance SLA

| Tier ID | 档位 | API p95 | 异步任务延迟 | 适用场景 |
|---|---|---|---|---|
| **PERF-T1** | 低 | ≤ 1000ms | ≤ 60s | TOB 内部工具 / 后台运营 |
| **PERF-T2** | 中 | ≤ 500ms | ≤ 30s | TOC 一般业务（默认推荐） |
| **PERF-T3** | 高 | ≤ 200ms | ≤ 10s | TOC 极致体验 / 实时交互 |

### 2.2 Availability SLA

| Tier ID | 档位 | 月度可用性 | 年度停机预算 | 适用场景 |
|---|---|---|---|---|
| **AVAIL-T1** | 低 | 99.5% | ~3.65 天 | 内部工具 / 后台 |
| **AVAIL-T2** | 中 | 99.9% | ~8.76 小时 | TOC 一般业务（默认推荐） |
| **AVAIL-T3** | 高 | 99.99% | ~52.6 分钟 | 金融级 / 关键业务（必含多 AZ） |

### 2.3 Capacity（用户量 + 数据量）

| Tier ID | 档位 | DAU | MAU | 峰值并发 | 单条数据 | 年增长 |
|---|---|---|---|---|---|---|
| **CAP-T1** | 低 | < 1k | < 10k | < 100 QPS | ≤ 1MB | ≤ 50GB |
| **CAP-T2** | 中 | 1k–10k | 10k–100k | 100–1k QPS | ≤ 10MB | ≤ 500GB |
| **CAP-T3** | 高 | 10k–100k | 100k–1M | 1k–10k QPS | ≤ 100MB | ≤ 5TB |

### 2.4 Data Sensitivity & Security

| Tier ID | 档位 | 数据类型 | 加密要求 | 适用场景 |
|---|---|---|---|---|
| **DATA-T1** | 低 | 业务普通（无 PII / 财务）| TLS in-transit | 公开内容 / 配置数据 |
| **DATA-T2** | 中 | 含 PII（个人信息）| TLS + at-rest 加密 + 字段级加密 | 用户名 / 手机 / 身份证 / 学习记录 |
| **DATA-T3** | 高 | 含金融 / 健康 | TLS + at-rest + 字段级 + HSM 密钥管理 | 支付 / 健康数据（IELTS 业务通常不到此档） |

### 2.5 Compliance & Regulation

| Tier ID | 档位 | 等保级别 | 数据出境 | 行业标准 | 适用场景 |
|---|---|---|---|---|---|
| **COMPL-T1** | 低 | 无特殊（自评 1 级）| 不限 | 无 | 内部工具 |
| **COMPL-T2** | 中 | 等保二级 | 不出境 | 教育部备案 | TOC 教育业务（默认推荐 IELTS） |
| **COMPL-T3** | 高 | 等保三级 | 受限出境 | GDPR / 教育部备案 / 行业专项 | 跨境业务 / 监管敏感 |

### 2.6 Data Retention & Archive

| Tier ID | 档位 | 保留期 | 归档策略 | 适用场景 |
|---|---|---|---|---|
| **RETN-T1** | 低 | 1 年 | 1 年后删除 | 临时数据 / 缓存 |
| **RETN-T2** | 中 | 3 年 | 1 年热 + 2 年冷存 | 教育评分记录（默认推荐）|
| **RETN-T3** | 高 | 5 年+ | 1 年热 + 4 年冷 + 归档冷储 | 财务 / 监管要求 |

### 2.7 Geo & Region

| Tier ID | 档位 | 区域 | 数据驻留 | 适用场景 |
|---|---|---|---|---|
| **REGION-T1** | 低 | 仅大陆 | 大陆境内 | 国内业务（默认推荐 IELTS）|
| **REGION-T2** | 中 | 大陆 + 港澳 | 大陆 + 港澳境内 | 大陆+港澳业务 |
| **REGION-T3** | 高 | 全球 | 多 region 数据合规 | 跨境业务（需高档合规配合）|

> Tier ID 引用约定：下游 agent 在 ADR / EXP / AC / Coverage Matrix 中可直接用 `PERF-T2` 等 ID 引用本表档位，免去重复粘贴数值。

### 2.8 业务量级总览（综合）

> 把 2.3 Capacity 进一步抽象为一行总览，供 IT Architect Deployment Topology 直接消费。

```yaml
business_scale:
  level: low | medium | high
  dau: {value or range}
  peak_qps: {value or range}
  growth_yoy: {percentage}
```

---

## §3 业务类型 × 推荐档位映射表

> AI 在 NFR Architect Step 2 生成候选时，会基于 PM 在 Step 1 输入的"业务类型 + 敏感度"自动**预填推荐档位**，PM 可改选。

| 业务类型 | Perf | Avail | Capacity | Sens | Compl | Retention | Geo |
|---|---|---|---|---|---|---|---|
| TOC 极致体验（如实时考试） | 高 | 高 | 高 | 中 | 中 | 中 | 低 |
| TOC 一般业务（如 IELTS Mock） | **中** | **中** | **中** | **中** | **中** | **中** | **低** |
| TOC 简单业务（如内容浏览） | 中 | 中 | 中 | 低 | 中 | 低 | 低 |
| TOB 业务 | 低 | 中 | 低 | 中 | 中 | 高 | 低 |
| 内部工具 | 低 | 低 | 低 | 低 | 低 | 低 | 低 |
| 金融 / 健康 | 高 | 高 | 中 | 高 | 高 | 高 | 低/中 |
| 跨境业务 | 中 | 中 | 中 | 中 | 高 | 中 | 高 |

> **默认推荐**（粗体）：TOC 一般业务全部中档，适用于 BCChina 大部分 Mock test 类 Epic。

---

## §3.5 从 Value Frame 自动抽取业务背景（v1.1 新增 · NFR Architect Step 1 复用）

> **目的**：v3.8 工作流把 NFR Architect 前移到 Solution 之前，本节定义 NFR Architect Step 1 如何从 **Value Frame** 自动派生 3 项业务背景（业务类型 / 业务敏感度 / 业务场景关键词），减少 PM 手填，PM 仅做 review + 修正。

### §3.5.1 抽取映射表（Value 章节 → NFR Step 1 字段）

| NFR Step 1 字段 | Value Frame 来源 | 推导规则 |
|---|---|---|
| **业务类型** | §1 Brief "目标用户" + "业务价值"（value-frame v1.2 用户量级 / 用户地域枚举） | 用户量级 > 10w + 业务价值含"实时/即时" → `TOC 极致体验`；用户量级 1k–10w + 教育/Mock → `TOC 一般业务`（默认）；用户量级 < 1k + 内部 → `内部工具`；TOB 关键词 → `TOB 业务`；含支付/健康 → `金融 / 健康`；地域含海外 → `跨境业务` |
| **业务敏感度** | §1 Brief "数据敏感度"（value-frame v1.2 枚举）+ Gate 2 Q6 合规要求 | 数据敏感度=强合规 → `金融级`；=PII + 教育合规 → `教育合规`（默认 IELTS）；=PII 一般 → `一般业务`；=公开 → `公开内容` |
| **业务场景关键词** | §1 Brief "当前问题" + §4 Roadmap MVP Epic value_statement | 取最具技术指向性的一句（如"AI 评分异步处理 / 实时考试 / 运营数据看板"），≤30 字 |

### §3.5.2 抽取结果呈现给 PM 的格式（强制）

NFR Architect Step 1 必须先呈现"AI 抽取候选"+ "Value 引用证据"，PM 仅做接受/修改/重写三选一：

```text
基于 Value Frame /{project} 自动抽取的业务背景候选：

1. 业务类型: TOC 一般业务  ← AI 抽取
   依据: §1 Brief 用户量级=1-10万 + 业务价值含"教育评分"
   [接受 / 修改 / 重写]

2. 业务敏感度: 教育合规     ← AI 抽取
   依据: §1 Brief 数据敏感度=PII + Gate 2 Q6 合规要求="教育部备案"
   [接受 / 修改 / 重写]

3. 业务场景关键词: AI 评分异步处理 + 短轮询  ← AI 抽取
   依据: §4 Roadmap MVP Epic "speaking-challenge-and-scoring"
   [接受 / 修改 / 重写]
```

### §3.5.3 fallback 行为

- Value Frame 缺失或字段不完整 → fallback 到旧版 Step 1（PM 手填 3 项），并在 `§9 OQ` 标记 `[Value 字段缺失，建议补 Value 后 refinement]`
- value-frame SKILL 版本 < 1.2（无枚举字段） → AI 仍尝试从自由文本推导，但置信度标 ⚠️，让 PM 强校对

### §3.5.4 与 Step 1 的接口契约

- NFR Architect agent v1.1+ 在 Step 1 调用本 §3.5：先 wiki-pull / read Value Frame → 调用本节抽取 → 呈现候选 → PM 三选一
- 抽取来源记录到 `frontmatter.business_context.extracted_from` 字段（含 Value 引用路径 + 字段 + 时间戳）

---

## §4 NFR 间依赖校验规则（强制 · 落盘前自检）

某些 NFR 选项有内在依赖，PM 选择后 AI 必须校验，发现冲突立即提示 PM 重选。

| 依赖规则 | 校验逻辑 | 冲突处理 |
|---|---|---|
| 99.99% SLA → 必须多 AZ 部署 | Avail = 高 ⇒ Geo 选项不能为单点 | 提示 PM "99.99% 必须配多 AZ，是否调整 Geo？" |
| 含 PII / 财务 → 至少等保二级 | Sens = 中/高 ⇒ Compl 必须 ≥ 中 | 提示 PM "PII 数据必须等保二级，自动升 Compl" |
| 数据出境 → 高档合规 | Geo = 高（含出境）⇒ Compl 必须 = 高 | 提示 PM "出境必须 GDPR / 高合规" |
| 高峰值并发 → 高可用性 | Capacity peak_qps > 1k QPS ⇒ Avail 必须 ≥ 中 | 提示 PM "高并发需 ≥99.9% 可用性" |
| 5 年+ 保留 → 中/高 容量 | Retention = 高 ⇒ Capacity growth_yoy 必须按高档预估 | 提示 PM "长保留期需评估存储成本" |
| 极致性能 (p95 ≤ 200ms) + 高峰值 | Perf = 高 + Capacity = 高 ⇒ 必须 §9 OQ 列"性能优化策略" | 强制补 OQ |

---

## §5 NFR Brief 输出模板（§0 章节强制格式）

```markdown
## §0 NFR Brief

- **项目**: {project}
- **范围**: {epic 或 project-wide}
- **业务类型**: {Step 1 PM 输入}
- **业务敏感度**: {Step 1 PM 输入}
- **业务场景关键词**: {Step 1 PM 输入}

### 选定档位摘要

| NFR | 档位 | 关键值 |
|---|:---:|---|
| Performance SLA | 中 | API p95 ≤ 500ms / 异步 ≤ 30s |
| Availability SLA | 中 | 99.9% |
| Capacity | 中 | DAU 10k / 峰值 1k QPS / 单文件 ≤ 10MB |
| Data Sensitivity | 中 | PII（含手机 / 身份证）|
| Compliance | 中 | 等保二级 + 教育部备案 |
| Retention | 中 | 3 年（1 热 + 2 冷） |
| Geo | 低 | 仅大陆 |

### 依赖校验结果

- ✅ 全部通过 / ⚠️ X 项冲突已处理（详见 §8）

### 缺失项（标 [待 Tech Lead 校准]）

- {如有缺失项列出}
```

---

## §5.5 Scope 差异化输入清单（v1.1 新增 · project-wide vs epic-scoped）

> **目的**：v3.8 NFR 前置后，NFR Architect 可在两个时点启动：①Value 后立即（scope=project-wide）/ ②Solution 后补强（scope={epic-slug}）。本节定义两类启动的输入差异 + 8 类档位是否需要重选。

### §5.5.1 启动时点与输入差异

| 启动时点 | scope | 强制上游 | 可选上游 | PM 输入负担 |
|---|---|---|---|---|
| Value 后立即 | `project-wide` | Value LATEST（wiki-pull） | — | 极简（3 项 AI 抽取 + 8 类 4 选 1） |
| Solution 后补强 | `{epic-slug}` | Value LATEST + project-wide NFR LATEST + Solution LATEST | — | 差异化（仅问 8 类中**需要 Epic 级 override** 的几项） |

### §5.5.2 补强模式规则（epic-scoped 必读）

NFR Architect 启动 epic-scoped 时，检测到 `project-wide` NFR LATEST 已存在 → 进入**补强模式**：

| 8 类 NFR | epic 可否 override | 默认行为 | 触发 override 的常见场景 |
|---|:---:|---|---|
| Performance SLA | ✅ 可 override | 继承 project-wide | Epic 含实时交互 → 升 PERF-T3 |
| Availability SLA | ⚠️ 谨慎 override | 继承 project-wide | Epic 是关键收入业务 → 升 AVAIL-T3（需评估多 AZ 成本） |
| Capacity | ✅ 可 override | 继承 project-wide | Epic DAU 或并发显著高于全局 → 升 CAP-T3 |
| Data Sensitivity | ✅ 可 override | 继承 project-wide | Epic 含支付 / 健康 → 升 DATA-T3 |
| Compliance | ⚠️ 谨慎 override | 继承 project-wide | Epic 含跨境 → 升 COMPL-T3 |
| Retention | ✅ 可 override | 继承 project-wide | Epic 含财务凭证 → 升 RETN-T3 |
| Geo | ❌ 不应 override | 强制继承 | Geo 是全局合规决定，Epic 不应独立选 |

### §5.5.3 补强模式 PM 交互（精简）

补强模式只问"有哪些 NFR 需要 Epic 级 override"，不重做 8 类全部确认：

```text
项目 {project} 已有 project-wide NFR LATEST（{stamp}）。
本 Epic {epic-slug} 是否需要在以下 8 类中 override 任何档位？

  □ Performance (当前继承 PERF-T2)
  □ Availability (当前继承 AVAIL-T2)
  □ Capacity (当前继承 CAP-T2)
  □ Data Sensitivity (当前继承 DATA-T2)
  □ Compliance (当前继承 COMPL-T2)
  □ Retention (当前继承 RETN-T2)
  □ Geo (当前继承 REGION-T1 · 不建议 override)

PM 可输入 "none"（全继承）或选要 override 的项进入精细化档位选择。
```

### §5.5.4 落盘字段差异

```yaml
# project-wide NFR
scope: project-wide
nfr_targets: { ... 8 类全量 ... }

# epic-scoped NFR（补强模式）
scope: {epic-slug}
inherits_from: Project/{project}/NFR/project-wide/LATEST.md
overrides:
  performance: { level: high, api_p95_ms: 200, ... }  # 仅列 override 项
  # 其余 7 类隐式继承 project-wide
```

### §5.5.5 下游引用规则

Solution / IT Architect / Product Planner / Eng Reviewer 引用 epic-scoped NFR 时，必须**先 merge project-wide + epic overrides** 再使用：

```text
effective_nfr = project-wide.nfr_targets ⊕ epic-scoped.overrides
```

Wiki Publisher 在发布 epic-scoped NFR 时，建议同时渲染 effective_nfr 摘要（详见 wiki-publisher.agent.md v3.3）。

---

## §6 跨电脑协作落盘规则（v3.7 物理隔离 + frontmatter maintainer 标识）

### 6.1 本地落盘（运行 NFR Architect 的电脑 · 不论 PM 还是 NFR 专家）

```text
Project/{project}/NFR/{scope}/
├── LATEST.md
└── {project}-{scope}-nfr-{YYYY-MM-DD-HHmm}.md

scope 取值:
  - project-wide  ← 项目级（跨 Epic 共享）
  - {epic-slug}   ← Epic 级（覆盖项目级）
```

> v3.7 简化：不再使用独立 `NFR-Workspace/`，统一落到 `Project/{project}/NFR/` 子目录。  
> **物理隔离保证**：每个人在自己电脑工作，本身就不会跨电脑冲突。  
> **所有权标识**：通过 frontmatter `maintainer` 字段（如 `@PM-A` 或 `@NFRArch`）声明，不靠文件系统强制。

### 6.2 Wiki 发布路径（唯一对外通道）

| scope | Wiki 路径 |
|---|---|
| project-wide | `/{project}/project-wide-nfr` |
| {epic-slug} | `/{project}/{epic-slug}-PRD/nfr` |

### 6.3 frontmatter 强制字段

```yaml
---
project: {project}
scope: project-wide | {epic-slug}
nfr_version: {YYYY-MM-DD-HHmm}
created: {YYYY-MM-DD-HHmm}
maintainer: "@{nfr-architect-name}"
status: draft | approved
upstream_snapshot:
  value_wiki_path: /{project}
  solution_wiki_path: /{project}/{epic-slug}-solution  # Epic 级才有
  value_pulled_at: {YYYY-MM-DD-HHmm}
  solution_pulled_at: {YYYY-MM-DD-HHmm}
skills_loaded:
  - skills/project-context-loader/SKILL.md
  - skills/nfr-spec/SKILL.md
business_context:
  type: {TOC 极致 / TOC 一般 / TOB / 内部工具 / 金融健康 / 跨境}
  sensitivity: {金融级 / 教育合规 / 一般业务 / 公开内容}
  scenario_keyword: {一句话}
nfr_targets:
  performance:
    level: low | medium | high | custom
    api_p95_ms: 500
    async_max_s: 30
  availability:
    level: low | medium | high
    sla: "99.9%"
  capacity:
    level: low | medium | high | custom
    dau: 10000
    mau: 100000
    peak_qps: 1000
    file_max_mb: 10
    growth_yoy_gb: 500
  data_sensitivity:
    level: low | medium | high
    fields: ["phone", "id_card"]
  compliance:
    level: low | medium | high
    dpi_level: "等保二级"
    regulations: ["教育部备案"]
  retention:
    level: low | medium | high
    years: 3
    archive_policy: "1y_hot_2y_cold"
  geo:
    level: low | medium | high
    regions: ["mainland_china"]
dependency_check:
  status: passed | warnings | blocked
  warnings: []
---
```

---

## §7 与 IT Architect QAS 的接口契约

NFR Architect 落盘后，IT Architect 在 Layer 2 §2.7 QAS 中**直接消费**本 NFR LATEST：

```text
QAS 生成规则（IT Architect 侧）:
  For each NFR target:
    生成对应的 Quality Attribute Scenario:
      - Stimulus（触发场景）
      - Environment（环境条件）
      - Response（系统响应）
      - Response Measure（量化指标 ← 直接取 NFR 数值）
```

例如：

```text
NFR §1 Performance SLA: API p95 ≤ 500ms

→ IT Architect Layer 2 §2.7 QAS-1:
  Stimulus: 用户提交评分请求
  Environment: 正常负载（≤1k QPS）
  Response: 系统返回评分结果
  Response Measure: p95 ≤ 500ms（来自 NFR §1 Performance SLA 中档）
```

---

## §8 Quality Gate（落盘前自检 · 阻塞性）

NFR Architect 在落盘前必须执行：

- [ ] §0 NFR Brief 含完整 7 项摘要 + 依赖校验结果
- [ ] §1–§7 八类 NFR 全部填档（缺失项标 `[待 Tech Lead 校准]`）
- [ ] §8 NFR 依赖校验已执行，所有 warning 已记录或 PM 已 override
- [ ] frontmatter 含 `business_context` + `nfr_targets` + `dependency_check`
- [ ] frontmatter `upstream_snapshot.value_pulled_at` / `solution_pulled_at` 已记录（wiki-pull 时）
- [ ] `maintainer` 字段含 `@{nfr-architect-name}`
- [ ] 本地 LATEST.md 已更新
- [ ] **Wiki 发布前置**：可单独 Wiki Publisher 发布 NFR 页（不阻塞，但建议）

---

## §9 强制规则

必须：
- AI 候选档位严格按 §2 表，不得自创
- §8 依赖校验在 PM 选档后立即执行（不等到落盘）
- 缺失项必须明确标 `[待 Tech Lead 校准]` 进 §9 OQ
- 跨 PM 时通过 Wiki 共享，NFR Architect 不写 PM 本地工作区
- frontmatter 完整记录 upstream wiki paths（防止后续无法追溯）

禁止：
- AI 自创档位或越权改 PM 选定值
- 跳过 §8 依赖校验
- 凭记忆生成（必须先 Read 本 SKILL）
- frontmatter `maintainer` 字段为空或不准确（v3.7 唯一所有权标识，必填）
- 一次产出多个 scope（一次只产出 project-wide 或一个 epic，不并行）

---

## §10 变更记录

| 版本 | 日期 | 变更 |
|---|---|---|
| 1.1.0 | 2026-05-22 | **v3.8 NFR 前置 + Tier ID 化**。①§2.1-§2.7 每档新增稳定 Tier ID（PERF-T1/T2/T3 / AVAIL-T1/T2/T3 / CAP-T1/T2/T3 / DATA-T1/T2/T3 / COMPL-T1/T2/T3 / RETN-T1/T2/T3 / REGION-T1/T2/T3），供 Solution / Architecture / PRD / Eng Review 跨文档 trace；②新增 §3.5 "从 Value Frame 自动抽取业务背景"（含抽取映射表 + Step 1 接受/修改/重写交互 + fallback 行为）；③新增 §5.5 "Scope 差异化输入清单（project-wide vs epic-scoped）"（含两类启动时点输入差异 + 补强模式 PM 交互 + override 字段 + effective_nfr 合并规则）。 |
| 1.0.1 | 2026-05-19 | v3.7 简化协作模式：落盘路径由 `NFR-Workspace/{project}/{scope}/` 改回 `Project/{project}/NFR/{scope}/`；不再要求独立工作区；所有权改用 frontmatter `maintainer` 字段标识；物理隔离（不同电脑）天然防冲突。Quality Gate 严隔离自检改为 maintainer 字段必填自检。 |
| 1.0.0 | 2026-05-19 | 初版。8 类 NFR × 3 档行业基线候选库；业务类型 × 推荐档位映射；NFR 间依赖校验 6 条；NFR Brief 输出模板；与 IT Architect QAS 接口契约；Quality Gate 自检 8 条。 |
