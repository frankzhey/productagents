# ProductPortfolio — AI Agent 协作工作流

> BCChina 三段式 PM + 工程交付 Agent 框架  
> 更新时间：2026-05-22（**v3.8 完整落地 · PR1+PR2+PR3+PR4 全部就绪**：Value → NFR Architect（project-wide）→ Solution Architect（纯业务方案 · §7 EXP）→ IT Architect / Product Planner（并行 · 单向消费 · 无 patch 回写）→ UX / Eng Reviewer（反向 RR 仅到 IT/NFR · 不到 Solution）→ Task → Wiki / Work Item Publisher；落地清单：nfr-spec v1.1 / value-frame v1.2 / nfr-architect v1.1 / solution-design v1.6 / solution-architect v2.5 / wiki-publisher v3.3 / it-architect v1.5 / it-architecture-spec v1.3 / **product-planner v4.8** / **eng-reviewer v4.1**）

---

## 仓库定位

本仓库管理 BCChina 从 **Discovery → Plan → Deliver → 工程评审 → 任务拆分 → Wiki / Work Item 发布** 的全流程 AI Agent 协作体系。所有 Agent 遵循 `.github/copilot-instructions.md` 全局规则，通过 **SKILL（写作规范）+ instructions（文件级 contract）+ agents（流程编排）** 三层架构串联。

当前仓库已扩展为 **PM 文档链路 + 工程设计图形资产链路**：Solution 阶段在遇到复杂系统边界、AI scoring、异步评分、Mini program + backend、website handoff 等场景时，可通过 `fireworks-tech-graph` 生成发布级 SVG 技术图，用于 Engineering Review 和 Wiki 发布。

---

## 三段式 PM 工作流（v3.8 · NFR 前置 + 三份产出独立 + 无回路）

```
Discovery                Plan                                          Deliver
────────────         ──────────────────────                       ───────────────────
Value Architect      NFR Architect ⭐ v3.8 前置                       Product Planner
(market-research +   [scope=project-wide]                              (ac-writing-spec v1.1 +
 value-frame v1.2)   (nfr-spec v1.1 · 5 步流程)                         project-context-loader)
                     ①wiki-pull Value                                  Step 0 五步协议 +
                     ②AI 抽取 3 项业务背景候选                          wiki-pull NFR + Architecture
                     ③PM review/修正                                   §6 NFR Reference + §X Coverage Matrix
                     ④AI 生成 8 类 × 3 档（Tier ID）
                     ⑤PM 4 选 1 + 依赖校验 + 落盘
       │                            │                                       │
       ▼                            ▼                                       │
Project/{p}/Value/        Project/{p}/NFR/project-wide/                     │
LATEST.md                 LATEST.md (Wiki publish)                           │
       │                            │                                       │
       └────────┬───────────────────┘                                       │
                ▼                                                            │
Solution Architect (v2.5 · 纯业务方案 · 完全去技术化)                       │
  (solution-design v1.6 + project-context-loader)                            │
  Step 0.3 wiki-pull project-wide NFR → §7 EXP 推导                          │
  §5 流程难点 BP-X / §6 业务 Workload                                        │
  §7 Technology Expectations to IT Architect ⭐                              │
     - 7.1 EXP-{n} + must/should/nice + 来源/理由                            │
     - 7.2 ITQ-{n} 待澄清问题                                                │
     - 7.3 Architecture 引用指针（只读 · 不回填）                            │
  §8 NFR Reference（引用 NFR LATEST + Tier ID）                              │
  ❌ 严禁技术选型 / engineering notes / 回填 Architecture                   │
                                    │                                       │
                                    ▼ (Solution LATEST published)           │
        ┌───────────────────────────┴────────────────────────┐              │
        ▼ 并行 · 独立 · 不回路                                ▼              │
NFR Architect (epic-scoped · 补强)                  IT Architect ⭐ v1.4   │
nfr-spec §5.5 补强模式：                            (it-architecture-spec   │
 只问 PM 需 override 的类目                          v1.3 + fireworks-tech- │
 effective_nfr = project-wide ⊕ overrides            graph + project-context-│
                                                    loader + nfr-spec)      │
                                                  ⭐ 单向消费 Solution §7 EXP/ITQ
                                                  Layer 1/2/3 + C4 + TOGAF  │
                                                  + ADR（trace EXP）+ 7 SVG │
                                                  ❌ 不回写 Solution        │
        │                                                    │              │
        ▼                                                    ▼              │
Project/{p}/NFR/{epic}/LATEST.md              Project/{p}/Architecture/     │
                                              {epic}/LATEST.md              │
   │                                                    │                   │
   └─── Wiki publish ──┐                  Wiki publish ─┘                   │
                       ▼                                                    │
              （Product Planner 平等 wiki-pull 三份独立产出）◄────────────────┘
                                                                  │
                                                                  ▼
                                                         PM Confirm Gate
                                                    status: approved +
                                               pm_confirmation.status: approved
                                                                  │
                          ┌───────────────────────────────────────┤
                          ▼                                       ▼
                   UX Prototyper                       Eng Reviewer (v4.1 纯评审)
                   (UX 文档+HTML)                       消费 Value + NFR + Solution +
                          │                            Architecture + PRD（五段式）
                          │                            8 类评审动作 +
                          │                            反向 RR 只到 IT Architect /
                          │                            NFR Architect（不到 Solution）
                          │                                       │
                          ▼                                       ▼
                ┌──────────────────────────────────────────────────────┐
                │              Wiki Publisher (v3.2 → v3.3)            │
                │  /{project}                                ← Value 主页│
                │  /{project}/project-wide-nfr               ← NFR(全局)│
                │  /{project}/{epic}-solution                ← Solution │
                │  /{project}/{epic}-PRD                     ← PRD merged│
                │  /{project}/{epic}-PRD/nfr                 ← NFR(epic)│
                │  /{project}/{epic}-PRD/architecture        ← 架构     │
                │  /{project}/{epic}-PRD/architecture/adr-*  ← ADR      │
                │  /{project}/{epic}-PRD/ui-prototype        ← UX       │
                │  /{project}/{epic}-PRD/engineering-review  ← Eng      │
                │  /{project}/{epic}-PRD/task-planning       ← Task     │
                │  + 协作元数据 + frontmatter YAML 保真 + SVG Attachment │
                └──────────────────────────────────────────────────────┘
                                                                  │
                                                                  ▼
                ┌──────────────────────────────────────────────────────┐
                │            Work Item Publisher (v1.1)                │
                │  BCChina / {ADO Project}                             │
                │  Epic → Feature → User Story + AC                    │
                │  两阶段幂等：本地 mapping 优先 + ADO 回查兜底         │
                │  落盘 ado-mapping.json + ado-publish-history/         │
                └──────────────────────────────────────────────────────┘
```

> **v3.8 核心变化（vs v3.7）**：
> - **NFR 前置到 Solution 之前**：NFR Architect [scope=project-wide] 推荐在 Value 发布后立即启动；Solution Architect Step 0.3 wiki-pull NFR LATEST 作为 §7 EXP 推导的强信号源
> - **三份产出彼此独立 + 无回路**：Solution / NFR / Architecture 各自落盘各自 publish，IT Architect **单向消费** Solution §7 EXP/ITQ 但**不回写 Solution**；如发现冲突走"反向 RR to PM"，不打扰 Solution Architect
> - **Solution 完全去技术化（solution-design v1.6）**：§7 重写为 "Technology Expectations to IT Architect"——结构化 `EXP-{n}` ID + 优先级 `must`/`should`/`nice` + 来源/理由；删除 v1.4 §7.1 技术方向 / §7.2 技术约束 / §7.3 Layer 1 引用回填等子节；§7 严禁技术选型（"使用 RabbitMQ"等），只能写"能力 / 约束 / 期望"
> - **NFR Architect 5 步流程（v1.1）**：①wiki-pull Value → ②AI 从 Value Frame 自动抽取 3 项业务背景候选（nfr-spec §3.5 映射表）→ ③PM review + 接受/修改/重写三选一 → ④AI 生成 8 类 × 3 档（每档带 Tier ID：PERF-T1/T2/T3 等）→ ⑤PM 4 选 1 + 依赖校验 + 落盘；epic-scoped 走补强模式（只问 override 项）
> - **Value Frame v1.2 枚举化**：§1 Brief 目标用户新增 3 项枚举（用户量级范围 / 用户地域 / 数据敏感度），为 NFR 自动抽取提供结构化输入
> - **Wiki 路径与 v3.2 保持一致**：Architecture / ADR / NFR 子页结构不变；Wiki Publisher v3.3 等待 PR4 落地
> 
> **v3.0 / v3.7 沉淀保持不变**：所有非 Value 阶段 agent 必走 `project-context-loader` 五步协议；Wiki 路径 `/{project}` 主页 + `-solution` / `-PRD` 命名后缀；Solution / Product Planner 支持单 Epic / 多选 / ALL 批处理；PM Confirm Gate 后才允许 Work Item Publisher；Knowledge Retriever 仅 Epic Kickoff 调用一次产出 `context-memo.md`

---

## PM 使用工作流（审核版）

### Stage 0：Project Kickoff（可选）

项目启动或 Epic 背景复杂时，先调用 **Knowledge Retriever** 检索 ADO Wiki 历史页面，生成：

```text
Project/{project}/context-memo.md
```

后续 Value / Solution / PRD / UX / Eng Review 直接读取该缓存，不重复触发历史检索。

### Stage 1：Value Architect（Discovery）

Value 层的核心目的是 **做价值判断**：通过看清竞品的核心能力和解决的痛点，决定我方是否要做、为什么我们做、给谁做、价值假设是什么。

PM 先选择输入模式：

| 模式 | 适用场景 | 产出 |
|---|---|---|
| Mode 1：PM 文字调研输入 | PM 已完成调研，有文字总结或片段 | `Research/research-summary.md` + `Value/LATEST.md` |
| Mode 2：竞品 URL 调研 | PM 提供 ≥3 个竞品 URL，agent 结构化整理（**当前仓库不依赖 web search 自动发现竞品**） | `Research/competitor-shortlist.md` + `Research/competitor-{name}.md` + `Value/LATEST.md` |

Mode 1 中，PM 调研内容建议按 6 段式 Summary 提供，但不强制全部填写：

```text
产品速览 / 核心能力 / 解决的痛点 / 优势定位 / 不足之处 / 整体评价
```

缺失或不确定内容可留空或标 `[待确认]`，由 Value Architect 在 Gate 2 / Gate 3 中补问。

Mode 2 中，Value Architect 加载 `market-research`，执行：

```text
读取 PM 提供的竞品 URL → 竞品速览（核心能力 + 解决的痛点 两列并列）
→ PM 选 1-2 家深度对标（6 段式 Summary）
```

Value 阶段必须经过：

```text
Gate 1：调研 Summary 校对（Mode 2 必走，Mode 1 skipped）
Gate 2：PM 价值判断必答四问（全部必答，强约束）
  Q1：要解决的核心痛点是什么？
  Q2：为什么是我们做？
  Q3：目标用户是什么？
  Q4：价值假设是什么？
Gate 3：逐段确认 Value Frame（Brief 含"为什么是我们做"字段，来自 Gate 2 Q2）
```

> Gate 2 四问必须全部回答完毕才能进入 Gate 3，禁止留空或仅以 `[待确认]` 跳过。

### Stage 2：Solution Architect（Plan）

PM 从 `Value/LATEST.md` 的 Roadmap / Epic List 中选择 1 个或多个 Epic，启动 Solution Architect：

```text
Project={project}
Selected Epic(s)={epic-slug 或 [epic-slug...]}
Value Frame Ref=Project/{project}/Value/LATEST.md
Magic Patterns editor_id={可选，推荐}
Figma file_id={可选}
```

Solution Architect 会校验所有选中 Epic 是否来自 Value Roadmap，并基于 Value + Magic Patterns / Figma 草稿逐个产出 Solution Brief。多选只增强编排能力，不改变产物颗粒度：每个 Epic 都会独立执行 Quality Gate、独立落盘到 `Project/{project}/Solution/{epic}/...md`，并独立更新 `LATEST.md`。核心输出包括 Feature List、User Journey、Business Process Flow、GWT、Workload、Tech High-level 和 Story List Preview。

Solution 支持反复 refinement：当 Magic Patterns 草稿更新、PM 调整 Feature、Value 上游更新或跨团队 review 返回时，可 patch 当前 `Solution/{epic}/LATEST.md`，或在 PM 明确要求时创建新版本。

复杂系统边界或工程评审场景下，Solution 会调用 `fireworks-tech-graph` 生成发布级 SVG/PNG target 技术图，默认路径：

```text
Project/{project}/Solution/Engdesign/{epic-slug}-engdesign/
```

### Stage 1.5：NFR Architect [scope=project-wide]（v3.8 前置 · 推荐 Value 后立即启动）

Value Frame 发布到 Wiki 后立即启动 NFR Architect（推荐）。NFR Architect 通过 wiki-pull 读取 Value，AI 基于 nfr-spec §3.5 抽取 3 项业务背景候选，PM 仅做 review/修正三选一即可。

**5 步流程（NFR Architect v1.1）**

```text
Project={project}
Scope=project-wide  ← 默认
Value Frame Ref=wiki-pull /{project}
```

①wiki-pull Value Frame → ②AI 按 nfr-spec §3.5 抽取业务类型/敏感度/场景关键词 3 项候选 → ③PM review + 接受/修改/重写三选一 → ④AI 生成 8 类 × 3 档候选（每档带稳定 Tier ID：PERF-T1/T2/T3 / AVAIL-T1/T2/T3 / CAP-T1/T2/T3 / DATA-T1/T2/T3 / COMPL-T1/T2/T3 / RETN-T1/T2/T3 / REGION-T1/T2/T3）→ ⑤PM 4 选 1 + nfr-spec §4 依赖校验 + 落盘 `Project/{project}/NFR/project-wide/LATEST.md` → Wiki publish `/{project}/project-wide-nfr`。

Epic 级 NFR 可在 Solution 后通过**补强模式**启动（scope={epic-slug}），按 nfr-spec §5.5 只问 PM 需 override 的类目，effective_nfr = project-wide ⊕ overrides。

### Stage 2.5：IT Architect（架构前置 · Solution 后并行 · v3.7 沉淀）

Solution Brief 发布到 Wiki 后，PM 启动 IT Architect。它单向消费 Solution §7 EXP/ITQ + NFR LATEST + Value，产出 Architecture LATEST + ADR + 7 强制 SVG。**v3.8 重要**：IT Architect 产出**不回写 Solution**；结论以 Architecture LATEST / ADR 为准。

**IT Architect 启动指令**

```text
Project={project}
Selected Epic={epic-slug}
NFR Ref=wiki-pull /{project}/project-wide-nfr (+ epic 补强 if any)
Solution Brief Ref=wiki-pull /{project}/{epic-slug}-solution
```

产出三层架构 + ADR + 强制 SVG 图：

```text
Layer 1 Context & Business / Layer 2 Solution / Layer 3 Component & Data
+ Cross-cutting 5 类
+ ADR ≥3 条（每条 trace Solution §7 EXP-{n} 处理方式）
+ 7 强制 SVG（通过 fireworks-tech-graph）+ 4 可选
+ QAS 接口契约（消费 NFR LATEST Tier ID）
+ 反向 RR to PM（如 EXP 业务期望冲突 · 不到 Solution Architect）
```

落盘 `Project/{project}/Architecture/{epic-slug}/LATEST.md`，SVG 同步落到 `diagrams/`。**v3.8 严禁回写 Solution §7.3**（单向消费 · 无 patch）。

### Stage 3：Product Planner（Deliver · v4.7）

推荐输入路径是 Source B：基于 Solution Brief 产出 PRD。Product Planner 在 Step 0 项目协议之后，Step 2.2 / 2.3 会 wiki-pull NFR LATEST 与 Architecture LATEST 作为引用，**不再原创 NFR 详细字段或完整架构**。

```text
Project={project}
Selected Epic={epic-slug}
Value Frame Ref=Project/{project}/Value/LATEST.md
Solution Brief Ref=Project/{project}/Solution/{epic}/LATEST.md
NFR Ref=（wiki-pull · 由 Step 2.2 自动检测；缺失走软 Gate 三选一）
Architecture Ref=（wiki-pull · 由 Step 2.3 自动检测；缺失走软 Gate 三选一）
Magic Patterns editor_id={可选，推荐}
Figma file_id={可选}
```

Product Planner 读取 Solution §2 Feature List 和 §8 Story List Preview，接管 Stable Feature ID / Story ID，并进一步拆解为：

```text
§1 Epic Definition → §2 Feature List → §3 User Stories + AC（⭐核心交付）
→ §4 Estimation → §5 Engineering Notes（引用 Architecture）
→ §6 NFR Reference（引用 NFR LATEST · 不原创）
→ §7 Capacity Summary → §8 Disclaimer → §9 OQ 聚合 → §10 Future 补充
→ §X Coverage Matrix（v4.6 新增·必填：追溯 Solution §5 每条 BP-X Path ID → AC，含 8 类场景维度自检）
→ §11 Rules 索引 → §12 Changelog
```

AC 写作严格按 `ac-writing-spec v1.1` §1-§3 + §3.5 **8 类场景维度索引**（happy / unhappy / failure / edge / permission / state / retry / empty-expired-duplicate）。

Magic Patterns 可在 Product Planner 阶段继续作为 Story / AC 细化输入，用于识别页面状态、字段、按钮、校验和异常反馈。若 Solution / Architecture / NFR / 设计稿更新，Product Planner 进入 refinement，同步更新 `PRD/{epic}/LATEST.md`。

PRD 进入发布前必须经过 PM Confirm Gate。PM 明确回复 `PRD is confirmed` 后，Product Planner 写入：

```yaml
status: approved
pm_confirmation:
  status: approved
  confirmed_by: PM
  confirmed_at: {YYYY-MM-DD-HHmm}
  confirmation_note: "PRD is confirmed"
```

只有 `pm_confirmation.status: approved` 的 PRD 才允许交给 Work Item Publisher 发布到 Azure DevOps Boards。

### Stage 4：Handoff（交付延展）

| 下游 Agent | 输入 | 输出 |
|---|---|---|
| UX Prototyper | PRD + Solution + 设计稿 | UX 文档 / HTML Prototype |
| Eng Reviewer（v4.0 纯评审） | Value + Solution + IT Architecture + NFR + PRD | Engineering Review（8 类评审动作 + 两类反向 RR） |
| Task Planner | PRD + Eng Review | 可执行开发任务拆分 |
| Wiki Publisher（v3.2） | Value / Solution / PRD / UX / Eng / Task / **Architecture / ADR / NFR / RR** | ADO Wiki standard / merged / SVG Attachment 发布 |
| Work Item Publisher（v1.1） | PM approved PRD + PM 指定 ADO Project / Iteration Path / Area Path | ADO Boards Epic / Feature / User Story + AC（两阶段幂等） |

PM 推荐使用顺序（v3.7）：

```text
Knowledge Retriever（可选 · Epic Kickoff）
→ Value Architect
→ Solution Architect
→ 发布 Solution 到 Wiki
→ NFR Architect（可选） + IT Architect（推荐） 并行启动，发布 NFR / Architecture 到 Wiki
→ Product Planner（wiki-pull NFR + Architecture 作为引用 + PM Confirm Gate）
→ UX Prototyper / Eng Reviewer（v4.0 纯评审 · 含两类反向 RR 回路）
→ Task Planner
→ Wiki Publisher（发布所有产物，含 SVG Attachment）
→ Work Item Publisher（PRD approved 后发布到 ADO Boards）
```

---

## Agent 清单

| Agent | 版本 | 职责 | 关键 SKILL | Handoff |
|---|---|---|---|---|
| **Knowledge Retriever** | — | Epic Kickoff 时检索 ADO Wiki 历史，生成 `context-memo.md` | — | Product Planner / UX / Eng |
| **Value Architect** | v2.6.0 | Discovery 入口（project 主入口）：启动主动扫描 project，竞品调研 + Gate 2 PM 必答 6 问（Q1-Q4 + **Q5 用户地域 + Q6 合规要求**）+ Value Frame | `market-research`, `value-frame` | Solution Architect |
| **Solution Architect** | v2.5.0 | Plan 中段（v1.6 完全去技术化）：Step 0.3 wiki-pull project-wide NFR → Step 0.5 PM-AI 协作 14 项 + 阶段⑤生成 EXP 候选 → 产出 §5 BP-X 流程难点 + **§7 Technology Expectations to IT Architect（结构化 EXP-{n} + must/should/nice + ITQ-{n} + Architecture 引用只读指针）** + §8 NFR Reference；❌ 严禁技术选型 / engineering notes / 回填 Architecture | `project-context-loader`, `solution-design v1.6` | IT Architect / Product Planner / NFR Architect / Eng Reviewer（并行 · 无回路） |
| **NFR Architect** ⭐ v3.7 | v1.1.0 | v3.8 前置 · Value 后立即启动：5 步流程（wiki-pull Value → AI 抽取 3 项业务背景候选 → PM review 三选一 → AI 生成 8 类 × 3 档 + Tier ID → PM 4 选 1 + 依赖校验 + 落盘）；scope 双轨（project-wide / epic-scoped 补强模式）；落盘 `Project/{p}/NFR/{scope}/` | `project-context-loader`, `nfr-spec v1.1` | Solution Architect (wiki-pull) / IT Architect / Product Planner / Eng Reviewer |
| **Product Planner** | **v4.8.0** | Deliver 终段：Step 0 五步协议 → Step 2.2 wiki-pull NFR + Step 2.3 wiki-pull Architecture（**v4.8 缺失走软 Gate 默认 B 继续 + 兜底警示**）→ Epic→Feature→Story→AC + **§5 Engineering Notes 改为引用 Architecture LATEST**（不再原创）+ **§6 NFR Reference 引用** + **§X Coverage Matrix 三向 trace**（v4.8：BP-X + NFR Tier + Architecture Container/ADR）+ PM Confirm Gate | `project-context-loader`, `ac-writing-spec v1.1` | Story Splitter / UX / Eng / Wiki / Work Item Publisher |
| **IT Architect** ⭐ v3.7 新增 | **v1.5.0** | 跨电脑共享：wiki-pull 白名单（Value / Solution / NFR / PRD / UX）→ 产出三层架构（C4 + TOGAF）+ ADR ≥3 + 7 强制 SVG（**v1.5 必须 inline-friendly：无 foreignObject / 无内联 style 块 / < 200KB**）+ **同步产 PNG 备份**到 `diagrams/png/` + 反向 RR to PM；落盘 `Project/{p}/Architecture/{epic}/`；handoff Wiki Publisher 内联发布（不再依赖 attachment） | `project-context-loader`, `it-architecture-spec v1.3`, `fireworks-tech-graph`, `nfr-spec v1.1` | Wiki / Eng Reviewer / Task Planner |
| **Story Splitter** | v2.2.0 | Feature 复杂度评估 (FCS) + Story 拆分 + AC 补全（PP 子 Agent） | `ac-writing-spec` | (返回 Product Planner) |
| **UX Prototyper** | v2.0.0 | UX 文档 + HTML 原型 | — | Eng Reviewer / Wiki |
| **Eng Reviewer** | **v4.1.0** | 纯评审 · 8 类评审动作 + **反向 RR 严格限定 v4.1**：仅向 IT Architect / NFR Architect 发 RR，**严禁向 Solution Architect 发**（Solution 问题改为 §8 Risks flag + 提请 PM）+ **兜底警示模式**（配合 PP v4.8 Step 2.3 软 Gate B · Architecture/NFR 缺失时 §2/§4/§X 走警示而非阻塞）+ §4 NFR Verification 双源校验（effective_nfr 合并）+ tools 对齐 ADO MCP v2 | `project-context-loader`, `eng-review-spec v2.0`, `ac-writing-spec v1.1` | Task Planner / Wiki / IT Architect / NFR Architect |
| **Task Planner** | — | 任务拆分、估算、依赖识别 | — | Wiki Publisher |
| **Wiki Publisher** | **v3.3.0** | v3.2 路径表（13 类 page_type）+ **协作元数据强制注入** + **frontmatter YAML 保真** + **v3.3 SVG 三级 fallback 内嵌发布**（Level 1 内联 SVG / Level 2 base64 data URI / Level 3 PNG · ADO MCP 无 attachment 上传工具 · 官方确认）+ tools 列表对齐 ADO MCP v2 命名（`wiki` + `wiki_upsert_page`）| `project-context-loader` | — |
| **Work Item Publisher** | v1.1.0 | 将 PM approved PRD 发布到 Azure DevOps Boards：**Step 0 强制 project-context-loader 五步协议** → 列本地 PRD Epic List → PM 单选 → 校验 `pm_confirmation.status: approved` → PM 指定 ADO Project + 可选 Iteration / Area → dry-run（**本地 mapping 优先 + ADO 回查兜底**）→ `publish confirmed` 后 create/update Epic / Feature / User Story / AC → 落盘 `ado-mapping.json` + `ado-publish-history/{stamp}.md` + 回写 PRD frontmatter `ado_published` | `project-context-loader`, `ado-work-item-publish-spec` | — |

---

## SKILL 清单

所有 SKILL 是"写作规范的单一来源"，由 agent 通过强制 Read 引用，不内化到 agent。

| SKILL | 版本 | 用途 | 被谁加载 |
|---|---|---|---|
| [skills/project-context-loader/SKILL.md](skills/project-context-loader/SKILL.md) | v1.1.0 | **多 project 并行下的统一上下文加载规范**（v3.0 新增）：询问 project name → 校验 Value LATEST → 加载 Rules/context-memo → 列 Epic List → PM 单选 / 多选 / 全选 / 例外流程；project 名不一致循环 ≤3 次 | Solution Architect / Product Planner / Eng Reviewer / Wiki Publisher |
| [skills/market-research/SKILL.md](skills/market-research/SKILL.md) | v1.2.0 | 竞品 URL 调研（PM 提供 URL）+ 竞品速览（核心能力 + 解决的痛点 两列并列）+ 6 段式深度对标 | Value Architect (Mode 2) |
| [skills/value-frame/SKILL.md](skills/value-frame/SKILL.md) | **v1.2.0** | Value Frame 章节锚点（§1 Brief 6 要素含"为什么是我们做" / §2 Hypothesis / §3 KPI Tree / §4 Roadmap+Epic / §5 OQ）；**v1.2 §1 Brief 目标用户新增 3 项枚举（用户量级范围 / 用户地域 / 数据敏感度），为 NFR 自动抽取提供结构化输入**；Epic 颗粒度三判定 + 反模式 + Epic 自检矩阵 | Value Architect (Gate 3) |
| [skills/solution-design/SKILL.md](skills/solution-design/SKILL.md) | **v1.6.0** | **v3.8 完全去技术化 + 独立产出**：§5 流程难点 BP-H/U/E + **§7 重写为 Technology Expectations to IT Architect（EXP-{n} + must/should/nice + 来源/理由 + ITQ-{n} 待澄清 + Architecture 引用只读指针）** + §8 NFR Reference + §15 Step 0.5 PM-AI 协作 14 项（阶段⑤生成 EXP 候选清单）；删除 v1.4 §7.1/7.2/7.3 技术方向/约束/Layer 1 回填等子节；明示三份产出独立 + 无 patch 回路 | Solution Architect |
| [skills/nfr-spec/SKILL.md](skills/nfr-spec/SKILL.md) ⭐ v3.7 | **v1.1.0** | **NFR 写作规范 + v3.8 前置支持**：8 类 NFR × 3 档行业基线候选库 + **Tier ID 化（PERF-T1/T2/T3 等）** + **§3.5 从 Value Frame 自动抽取业务背景映射** + **§5.5 Scope 差异化输入清单（project-wide vs epic-scoped 补强模式）** + 业务类型 × 推荐档位映射 + 6 条依赖校验 + 与 IT Architect QAS 接口契约 | NFR Architect |
| [skills/it-architecture-spec/SKILL.md](skills/it-architecture-spec/SKILL.md) ⭐ v3.7 新增 | **v1.3.0** | **IT 架构写作规范**：三层架构（Layer 1/2/3）+ C4 4 层映射 + TOGAF 4 域覆盖 + 7 强制 + 4 可选 SVG 清单（通过 fireworks-tech-graph）+ **v1.3 §3.5 SVG inline-friendly 三约束（无 foreignObject / 无行内 style 块 / < 200KB · 重生 ≤3 次）** + **§3.6 PNG 备份产出规则** + ADR 标准模板 + QAS 接口契约（消费 NFR LATEST）+ manifest.json schema 新增 inline_friendly / png_path / png_status 字段 + 反向 RR to PM 模板 | IT Architect |
| [skills/fireworks-tech-graph/SKILL.md](skills/fireworks-tech-graph/SKILL.md) | external | 生成发布级 SVG/PNG 技术图（layered architecture / data flow / sequence / component diagram 等），默认可配合 Claude Official style | Solution Architect / Eng Reviewer |
| [skills/ac-writing-spec/SKILL.md](skills/ac-writing-spec/SKILL.md) | **v1.1.0** | AC 写作规范（GIVEN/WHEN/THEN 多行 / A 类操作 / B 类字段 / C 类业务）+ **§3.5 8 类场景维度索引**（happy / unhappy / failure / edge / permission / state / retry / empty-expired-duplicate · v1.1 新增 · 与 PRD §X Coverage Matrix 接口契约） | Product Planner / Story Splitter / Eng Reviewer |
| [skills/eng-review-spec/SKILL.md](skills/eng-review-spec/SKILL.md) | **v2.0.0** | **Engineering Review 纯评审版**（v2.0 重构 · 删 13 设计动作）：章节锚点 = 8 类纯评审动作（§0 Scope / §2 Architecture Challenge Checklist 6 大类题库 / §3 Blast Radius / §4 NFR Verification 8 类校验 / §5 Capacity / §6 AC 合规 / §7 Task Readiness / §X Coverage Verification 警示）+ 两类反向 RR 模板（Architecture / NFR · PM Confirm Gate） | Eng Reviewer |
| [skills/ado-work-item-publish-spec/SKILL.md](skills/ado-work-item-publish-spec/SKILL.md) | v1.1.0 | **ADO Work Item 发布规范**：approved PRD 校验 / ADO Project + Iteration Path + Area Path / Epic-Feature-Story 映射 / **两阶段幂等（本地 mapping 优先 + ADO 回查兜底）** / dry-run（含 AC Target + Source 列）/ create-update-stale-block / PRD managed block / §14 `ado-mapping.json` + `ado-publish-history/` 落盘规范 + content_hash no-op 优化 | Work Item Publisher |

### Solution 技术图生成约定

当 Solution Brief 命中复杂系统边界或评审产出场景时，`solution-design` 会要求加载 `fireworks-tech-graph`：

1. Read [skills/fireworks-tech-graph/SKILL.md](skills/fireworks-tech-graph/SKILL.md)
2. 如使用 Claude 风格，Read [skills/fireworks-tech-graph/references/style-6-claude-official.md](skills/fireworks-tech-graph/references/style-6-claude-official.md)
3. 从 Solution Brief §7 Tech High-level 提取 layers、components、data flows、sequence scenarios
4. 生成 SVG，并在具备转换依赖时导出 PNG
5. 将图形路径回写到 Solution Brief §7

默认输出路径：

```text
Project/{project}/Solution/Engdesign/{epic-slug}-engdesign/
```

---

## 文件结构

```
.github/
  copilot-instructions.md          ← 全局规则（最高优先级）
  agents/
    knowledge-retriever.agent.md   ← Epic Kickoff 历史检索
    value-architect.agent.md       ← Discovery：Value Frame
    solution-architect.agent.md    ← Plan：Solution Brief（v1.4 业务方案聚焦）
    nfr-architect.agent.md         ← v3.7：非功能需求（4 步极简协议 · 跨 PM 共享）
    it-architect.agent.md          ← v3.7：三层架构 + ADR + 强制 SVG（wiki-pull · 跨电脑共享）
    product-planner.agent.md       ← Deliver：PRD（Epic→Feature→Story→AC · wiki-pull NFR + Architecture）
    story-splitter.agent.md        ← Story 拆分（PP 子 Agent）
    ux-prototyper.agent.md         ← UX 文档 + HTML 原型
    eng-reviewer.agent.md          ← v4.0 纯评审 + 两类反向 RR
    task-planner.agent.md          ← 任务拆分
    wiki-publisher.agent.md        ← v3.2 ADO Wiki 发布（含 architecture / adr / nfr / RR + SVG Attachment）
    work-item-publisher.agent.md   ← v1.1 ADO Boards Work Items 发布（两阶段幂等）
  instructions/
    product.instructions.md        ← PRD 文件级 contract
    engineering.instructions.md    ← 工程设计规范
    frontend.instructions.md       ← 前端/UI 规范

skills/
  project-context-loader/SKILL.md  ← v3.0 多 project 并行统一上下文加载（mini-SKILL）
  market-research/SKILL.md         ← 竞品调研规范
  value-frame/SKILL.md             ← Value Frame 写作规范
  solution-design/SKILL.md         ← Solution Brief 写作规范
  fireworks-tech-graph/             ← 发布级技术图生成 Skill（SVG/PNG）
    SKILL.md
    references/style-6-claude-official.md
    templates/
    scripts/
  ac-writing-spec/SKILL.md         ← AC 写作规范（PM agents 唯一权威）
  eng-review-spec/SKILL.md         ← v2.0 Engineering Review 纯评审写作规范（v3.7 重构）
  nfr-spec/SKILL.md                ← v1.0 NFR 8 类 × 3 档候选库（v3.7 新增）
  it-architecture-spec/SKILL.md    ← v1.1 IT 三层架构 + C4 + TOGAF 写作规范（v3.7 新增）
  ado-work-item-publish-spec/SKILL.md ← ADO Work Item 发布规范（tag 幂等 + dry-run）

Project/                           ← 项目级落盘根目录
  {project}/
    Rules/{project}-rules.md       ← 项目永久规则（业务/工程/AC 三层）
    context-memo.md                ← Epic 级历史缓存
    Research/                      ← 调研材料（market-research 产出）
    Value/
      LATEST.md                    ← 指针 → 当前 canonical Value 文件
      value-architect-{stamp}.md
    Solution/
      Engdesign/
        {epic-slug}-engdesign/      ← Solution / Eng Review 技术图资产（SVG / PNG target / manifest）
      {epic-slug}/
        LATEST.md                  ← 指针 → 当前 canonical Solution 文件
        {epic-slug}-solution-brief-{stamp}.md
    NFR/                           ← v3.7 新增 · NFR Architect 产出落盘
      {scope}/                     ← scope = project-wide 或 {epic-slug}
        LATEST.md                  ← 指针 → 当前 canonical NFR 文件
        nfr-{stamp}.md
    Architecture/                  ← v3.7 新增 · IT Architect 产出落盘
      {epic-slug}/
        LATEST.md                  ← 指针 → 当前 canonical Architecture 文件
        architecture-{stamp}.md
        adr/                       ← ADR ≥3（每条独立 .md）
        diagrams/                  ← 7 强制 SVG + 4 可选（fireworks-tech-graph 产出）
    PRD/
      {epic-slug}/
        LATEST.md                  ← 指针 → 当前 canonical PRD 文件
        {epic-slug}-prd-{stamp}.md
    EngReview/                     ← Eng Reviewer 产出落盘
      {epic-slug}/
        LATEST.md                  ← 指针 → 当前 canonical Eng Review 文件
        {epic-slug}-eng-review-{stamp}.md
    ado-mapping.json               ← v1.1 Work Item Publisher 本地幂等映射
    ado-publish-history/           ← v1.1 Work Item Publisher 发布历史审计
      {stamp}.md

outputs/                           ← v3.0 临时缓存目录（Wiki fallback / 手工输入）
  wiki-cache/{project}/{epic-slug}/   ← Eng Reviewer mode=wiki-fallback
  manual-input/{project}/{epic-slug}/ ← Eng Reviewer mode=manual-input

README.md
```

---

## 三层架构核心设计

| 层 | 职责 | 谁定义 | 谁加载 |
|---|---|---|---|
| **instructions** | 文件级 contract（PRD 必含哪些章节、输出语言、禁止事项） | `.github/instructions/*.instructions.md` | 所有 agent |
| **SKILL** | 写作规范 / 流程模板（如 AC 怎么写、Value Frame 章节锚点） | `skills/*/SKILL.md` | 对应 agent 通过强制 Read |
| **agent** | 工作流编排（Step / Gate / Handoff / 落盘路径） | `.github/agents/*.agent.md` | Copilot 模式选择时加载 |

**关键决策：**

1. **AC 单一来源** — `ac-writing-spec` SKILL 为唯一权威，Product Planner / Story Splitter / Eng Reviewer 均通过强制 Read 引用
2. **三段式上游链** — Value Frame → Solution Brief → PRD 通过 frontmatter `upstream_snapshot` 引用 + `LATEST.md` 指针定位
3. **Wiki 合并发布** — PRD 含 `upstream_snapshot.value/solution` 时，Wiki Publisher 自动拼装单页（source 文件保持分离）
4. **Epic 颗粒度强约束** — `value-frame` SKILL §5 三判定 + 反模式 + 自检矩阵（阻塞性）
5. **context-memo 共享** — Knowledge Retriever 仅 Epic 启动调用一次，后续 agent 读文件而非重复查询 ADO
6. **发布级技术图外置** — `solution-design` 保持 Solution Brief contract，`fireworks-tech-graph` 作为独立 Skill 负责 SVG/PNG 技术图生成，避免把图形工具链塞进方案写作规范
7. **强制依赖加载** — agent 在 Step 0 / Gate 前置 Read SKILL，确保 Copilot 加载链路确定性
8. **多 project 并行（v3.0 新增）** — `project-context-loader` mini-SKILL 是除 Value Architect 外所有 agent 的强制前置：询问 project name → 校验 Value LATEST → 列 Epic List → PM 确认。不一致循环 ≤3 次，禁止凭 handoff 直接处理 project + epic
9. **Eng Reviewer 薄编排 + v4.0 纯评审** — 评审章节锚点抽离到 `eng-review-spec` SKILL；v4.0 删除 13 项设计动作（下放到 IT Architect / NFR Architect / Product Planner），保留 8 类纯评审动作 + 两类反向 RR（Architecture / NFR · 经 PM Confirm Gate 后回路）
10. **Wiki 路径项目化（v3.0 + v3.2 扩展）** — 以 `/{project}` 为 Value 主页，命名后缀 `-solution` / `-PRD` 严格强制；v3.2 扩展 13 类 page_type（含 architecture / adr-* / nfr / refinement-request）；frontmatter YAML 保真不剥离；Architecture / Eng Review 发布时自动重写 SVG 路径并同步上传 Attachment
11. **ADO Boards 发布幂等（v1.1）** — Work Item Publisher 只发布 `pm_confirmation.status: approved` 的 PRD；启动强制 `project-context-loader` 五步协议；以 `prd-epic-id` / `prd-feature-id` / `prd-story-id` tags 作为幂等 key；**两阶段幂等：先查本地 `ado-mapping.json`，再回查 ADO**；发布完成强制落盘 mapping + `ado-publish-history/{stamp}.md` + PRD frontmatter `ado_published` 回写
12. **架构 / NFR 前置 + 跨电脑共享（v3.7 新增）** — NFR Architect 与 IT Architect 作为两个独立 agent 在 Solution 之后、PRD 之前介入；通过 Wiki（而非本地 git）跨 PM / 跨电脑共享：启动 wiki-pull 上游 LATEST，结束 wiki-publish 自己的 LATEST；Product Planner 在 Step 2.2 / 2.3 wiki-pull 二者作为引用 source；缺失时走软 Gate 三选一询问 PM（continue / pause / wait）
13. **AC 8 类场景维度 + Coverage Matrix 强制（v3.7 新增）** — `ac-writing-spec v1.1` §3.5 统一 8 类场景维度（happy / unhappy / failure / edge / permission / state / retry / empty-expired-duplicate）；PRD §X Coverage Matrix 强制追溯 Solution §5 每条 BP-X Path ID → AC，含 prd_extension 自补；Eng Reviewer §X Coverage Verification 警示（未覆盖即提请 PM 决策）

---

## 当前示例项目状态

当前仓库内的主示例项目为 [Project/spk2challenge-miniprogram](Project/spk2challenge-miniprogram)，围绕 TOC 小程序口语挑战、AI 评分和 website 导流闭环展开。

| 阶段 | 当前文件 | 状态 |
|---|---|---|
| Value | [Project/spk2challenge-miniprogram/Value/LATEST.md](Project/spk2challenge-miniprogram/Value/LATEST.md) | `draft`，Gate 1-3 已通过，E1 已合并为端到端价值单元 |
| Solution E1 | [Project/spk2challenge-miniprogram/Solution/speaking-challenge-and-scoring/LATEST.md](Project/spk2challenge-miniprogram/Solution/speaking-challenge-and-scoring/LATEST.md) | 已 refinement，覆盖 K1/K2/K3/K6/K7/K8/K9、短轮询、幂等评分任务、score bucket 深链和 guardrails |
| Engdesign E1 | [Project/spk2challenge-miniprogram/Solution/Engdesign/speaking-challenge-and-scoring-engdesign/README.md](Project/spk2challenge-miniprogram/Solution/Engdesign/speaking-challenge-and-scoring-engdesign/README.md) | 已生成 4 张 Claude 风格 SVG；PNG export pending |
| PRD E1 | [Project/spk2challenge-miniprogram/PRD/speaking-challenge-and-scoring/LATEST.md](Project/spk2challenge-miniprogram/PRD/speaking-challenge-and-scoring/LATEST.md) | 已存在，后续可基于 refined Solution 继续同步 |

E1 `speaking-challenge-and-scoring` 的 Engdesign 资产包括：

- `layered architecture`
- `data flow`
- `sequence happy path`
- `component diagram`

这些资产位于 [Project/spk2challenge-miniprogram/Solution/Engdesign/speaking-challenge-and-scoring-engdesign](Project/spk2challenge-miniprogram/Solution/Engdesign/speaking-challenge-and-scoring-engdesign)，并由 `manifest.json`、`diagram-source-notes.md`、`export-status.md`、`validation-command.md` 跟踪来源和导出状态。

---

## 指令优先级（强制）

1. 用户当前明确要求
2. `.github/copilot-instructions.md`（全局）
3. `.github/instructions/*.instructions.md`（局部）
4. `.github/agents/*.agent.md`（角色）
5. `skills/*/SKILL.md`（写作规范）

---

## Wiki 发布路径（ADO `Product-Portfolio.wiki` · v3.0）

> v3.0 路径规则：以 `/{project}` 为 Value 主页，Solution / PRD 为二级子页（带 `-solution` / `-PRD` 命名后缀），UX / Eng / Task 为三级子页。

| 文档类型 | 路径 | 模式 |
|---|---|---|
| Value Frame | `/{project}` | standard（项目主页） |
| Solution Brief | `/{project}/{epic-slug}-solution` | standard |
| PRD（含上游 snapshot） | `/{project}/{epic-slug}-PRD` | **merged**（拼接 Value + Solution + PRD） |
| PRD（独立） | `/{project}/{epic-slug}-PRD` | standard |
| UX | `/{project}/{epic-slug}-PRD/ui-prototype` | standard（三级子页） |
| Engineering Review | `/{project}/{epic-slug}-PRD/engineering-review` | standard（三级子页） |
| Task Planning | `/{project}/{epic-slug}-PRD/task-planning` | standard（三级子页） |

> v2.x 旧路径 `/{epic-name}` 平铺已废弃。详细路径生成逻辑见 `agents/wiki-publisher.agent.md` v3.0。

---

## 版本记录

| 日期 | 变更 |
|---|---|
| 2026-05-22 | **v3.8 PR4 落地 · 反向 RR 严格限定 + 一致性补齐**。①**Product Planner v4.7 → v4.8**：§5 Engineering Notes 重写为引用 Architecture LATEST（与 §6 NFR Reference 同构 · 不再原创）；§X Coverage Matrix 升级为**三向 trace**（Solution BP-X + NFR Tier ID + Architecture Container/ADR + AC 四列）；Step 2.3 Architecture 缺失软 Gate 默认从 A（等待）改为 B（继续 + 兜底警示，不阻塞）；职责边界明确"不修改 Solution/NFR/Architecture 文件"；强制规则补 §X 三向 trace 必填 + Architecture/NFR 整列不可删除。②**Eng Reviewer v4.0 → v4.1**：反向 RR 严格限定为**仅 Architecture / NFR**，显式禁止 Solution RR（Solution 问题改为 §8 Risks flag + §11 提请 PM 决策）；新增**兜底警示模式**配合 PP v4.8 Step 2.3 软 Gate B（Architecture/NFR 缺失时 §2/§4/§X 走警示性输出，不阻塞 PRD 发布）；§4 NFR Verification 双源校验（按 nfr-spec §5.5.5 effective_nfr 合并规则 merge project-wide + epic overrides）；§X Coverage Verification 三向 trace 校验；tools 列表对齐 ADO MCP v2（删除 4 项已 consolidate 旧工具）；强制规则 + 禁止规则同步补三条。 |
| 2026-05-22 | **v3.8 PR3 落地 · ADO Wiki 内联 SVG 发布 + SVG inline-friendly 约束**。背景：经 Option A 验证，ADO MCP Server 官方 Wiki toolset 仅 6 项能力（`wiki` consolidated 派发 + `wiki_upsert_page` + `search_wiki`），**无 attachment 上传工具**（feature request Issue #392 至今未实现 · Microsoft Learn 2026-05-13 确认）。①**Wiki Publisher v3.2 → v3.3**：tools 列表对齐 ADO MCP v2 命名（删除旧独立工具 `wiki_create_or_update_page` 等 6 项 + 不存在的 `core_get_identity_ids`）；§4-bis-③ 重写为**三级 fallback 内嵌策略**（Level 1 内联 SVG <200KB / Level 2 base64 data URI <1.5MB / Level 3 PNG fallback），图片仍出现在 Markdown 引用原位置（§1.3 / §2.1 等章节就地）；ADR 子页改用 `wiki_upsert_page`；manifest.json 改嵌入页面底部 `<details>` 区块；失败降级显式定义。②**IT Architect v1.4 → v1.5**：Step 3.2 强化 fireworks-tech-graph 传参约束（禁用 `<foreignObject>` / 行内 `<style>` 块 / 字体外部依赖 / < 200KB）；新增 Step 3.2.1 SVG 自检三项（grep / 体积），不通过自动重生 ≤3 次；同步产 PNG 备份到 `diagrams/png/{slug}.png`（1.5x DPR / 透明背景），转换依赖缺失时标 `png_status: pending`；Quality Gate / 强制规则同步。③**it-architecture-spec v1.2 → v1.3**：§3 加 §3.5 inline-friendly 三约束 + §3.6 PNG 备份规则；§4 落盘加 `diagrams/png/` 子目录；§5 manifest.json schema 加 6 个新字段（svg_size_kb / inline_friendly / inline_check.* / png_path / png_status / png_size_kb）；§11 Quality Gate 加两组自检；§12 强制规则补两条 + 禁止规则补一条（严禁 foreignObject）。 |
| 2026-05-22 | **v3.8 NFR 前置 + 三份产出独立 + 无回路（PR1 + PR2 已落地）**。①NFR Architect 从 Solution 后移到 Value 之后立即启动（推荐 project-wide）；②Solution 完全去技术化，§7 重写为 EXP-{n} + must/should/nice + ITQ-{n} 待澄清；删除 §7.1 技术方向 / §7.2 技术约束 / §7.3 Layer 1 回填等子节；③IT Architect 单向消费 Solution §7 但不回写（无 patch · 无 refine 回路）；④三份产出（Solution / NFR / Architecture）彼此独立，各自落盘 / 各自 wiki publish / Product Planner 平等 wiki-pull。落地文件：`nfr-spec` v1.0.1→**v1.1.0**（Tier ID 化 + §3.5 Value 自动抽取映射 + §5.5 Scope 差异化）/ `value-frame` v1.1.0→**v1.2.0**（§1 Brief 用户量级/地域/数据敏感度三项枚举化）/ `nfr-architect` v1.0.1→**v1.1.0**（4 步→5 步流程 + 补强模式 + Tier ID 落盘）/ `solution-design` v1.4.0→**v1.6.0**（§7 EXP-{n} 重写 + Stable ID 新增 EXP/ITQ/BP）/ `solution-architect` v2.4.0→**v2.5.0**（Step 0.3 wiki-pull NFR + §7 EXP 写作 + handoff 三选并行）。PR3（IT Architect v1.4 + it-architecture-spec v1.3 · 单向消费 + ADR trace EXP）/ PR4（Product Planner v4.8 + Eng Reviewer v4.1 + Wiki Publisher v3.3）待后续滚动落地。 |
| 2026-05-21 | **README v3.7 工作流顺序固化**。同步以下 agent / SKILL 版本：Product Planner v4.6→**v4.7**（Step 2.2 wiki-pull NFR / Step 2.3 wiki-pull Architecture，缺失走软 Gate 三选一），IT Architect v1.2→**v1.3**（handoff 增强 Wiki Publisher 发布 architecture / adr-* 子页 + SVG Attachment），it-architecture-spec v1.1→**v1.2**（C4 4 层映射 + TOGAF 4 域覆盖 + 跨电脑协作 wiki-pull + maintainer 标识）；工作流 ASCII 图加入 NFR Architect / IT Architect（Solution → 二者并行 → Product Planner）；新增 Stage 2.5 章节；推荐使用顺序加入 "发布 Solution → NFR/IT Architect → wiki-pull → Product Planner" 的关键路径；文件结构补充 `Project/{p}/NFR/` 与 `Project/{p}/Architecture/`；核心设计追加第 12 / 13 条 |
| 2026-05-19 | **v3.7 NFR + IT Architect 双独立 agent**。新增 `.github/agents/nfr-architect.agent.md` + `skills/nfr-spec/SKILL.md`（8 类 × 3 档行业基线候选库 + 业务类型推荐档位 + 跨 PM 协作落盘）；新增 `.github/agents/it-architect.agent.md` + `skills/it-architecture-spec/SKILL.md`（三层架构 + C4 + TOGAF + ADR + 7 强制 SVG + QAS 接口契约）；Eng Reviewer 重构 v4.0（删 13 设计动作 + 8 纯评审动作 + 两类反向 RR）；Solution v1.4（§5 流程难点 BP-X 替代 GWT / §7 Tech Direction 瘦版 / §8 NFR Reference）；Product Planner v4.6（§6 NFR 改为引用 + §X Coverage Matrix 强制）；Wiki Publisher v3.2（13 类 page_type + 协作元数据强制 + frontmatter YAML 保真 + SVG Attachment 同步） |
| 2026-05-19 | **Work Item Publisher v1.1 升级（多 project 并行 + 本地 mapping）**。Work Item Publisher 升级 v1.1：启动强制 `project-context-loader` 五步协议（防跨 project 误命中）；PRD 定位改为基于选定 epic-slug + LATEST.md，废弃跨 project 全局搜索；新增 Step 8 强制落盘 `ado-mapping.json` + `ado-publish-history/{stamp}.md` + PRD frontmatter 回写 `ado_published`；dry-run 升级两阶段幂等（本地 mapping 优先 + ADO 回查兜底），dry-run 表新增 `AC Target` / `Source` 列。SKILL ado-work-item-publish-spec v1.1：新增 §14 落盘规范 + §6 两阶段幂等 + §13 "无写工具时只允许 dry-run" 等强制规则。 |
| 2026-05-19 | **Work Item Publisher v1.0 新增**。新增 `.github/agents/work-item-publisher.agent.md` 与 `skills/ado-work-item-publish-spec/SKILL.md`；Product Planner v4.4 新增 PM Confirm Gate，PM 明确 `PRD is confirmed` 后写入 `pm_confirmation.status: approved`；Work Item Publisher 将 approved PRD 发布到 BCChina Azure DevOps Boards，支持 PM 指定 ADO Project、可选 Iteration Path / Area Path、tag 幂等、dry-run 与 `publish confirmed` 双阶段 |
| 2026-05-19 | **Solution Architect v2.2 批量编排增强**。Solution 阶段支持从 Value §4 Roadmap 选择单个、多个或 ALL Epic；多选只增强编排能力，每个 Epic 仍独立产出 Solution Brief、独立 Quality Gate、独立落盘并维护 LATEST。`project-context-loader` 升级到 v1.1，同步 selected_epics / batch_selection 约定 |
| 2026-05-19 | **v3.0 多 project 并行 + Wiki 路径重构**。新增 `skills/project-context-loader` mini-SKILL（除 Value 外所有 agent 强制前置协议）；新增 `skills/eng-review-spec` SKILL（Eng Reviewer 改为薄编排）；Solution Architect v2.1 新增 Step -1 Epic List 选择；Product Planner v4.2 新增 ALL 全选批处理；Eng Reviewer v3.0 新增本地 / Wiki Fallback 临时缓存 / 手工输入 + 落盘 `Project/{p}/EngReview/`；Wiki Publisher v3.0 路径重构 `/{project}` 主页 + `-solution` / `-PRD` 命名后缀 + 三级子页；Value Architect v2.5 启动时扫描已有 project 防重名 |
| 2026-05-14 | Value 层重构：Mode 2 改为"竞品 URL 调研"（不依赖 web search）；竞品 Summary 升级为"核心能力 + 解决的痛点"两列并列 + 6 段式深度对标；Gate 2 升级为 PM 必答四问强制门；`value-frame` Brief 新增"为什么是我们做"字段 |
| 2026-05-14 | 增加 PM 使用工作流（审核版）；Mode 1 调研输入增加可选 5 段式 Summary 参考；`market-research` Step 2 浅扫表新增“不足之处”并统一“优势定位”口径 |
| 2026-05-13 | 引入 `fireworks-tech-graph` 独立 Skill；`solution-design` 增加复杂系统边界/评审产出的发布级图生成规则；E1 `speaking-challenge-and-scoring` Solution refinement，并生成 Engdesign SVG 资产 |
| 2026-05-08 | README 同步至 v3.0 三段式架构：新增 Value Architect / Solution Architect / 4 个 SKILL；Wiki Publisher v2.1 merged mode；Project/ 目录约定 + LATEST.md 指针 |
| 2026-05-02 | V2 工作流架构文档化 |
| 2026-04-28 | V2 重构：AC 抽象到 SKILL、instructions 瘦身、Eng Reviewer §17.0 合规校验 |
| 2026-04-16 | 初始 Agent 体系建立 |
