# ProductPortfolio — AI Agent 协作工作流

> BCChina 三段式 PM + 工程交付 Agent 框架  
> 更新时间：2026-05-19（v3.0 多 project 并行 + Wiki 路径重构）

---

## 仓库定位

本仓库管理 BCChina 从 **Discovery → Plan → Deliver → 工程评审 → 任务拆分 → Wiki 发布** 的全流程 AI Agent 协作体系。所有 Agent 遵循 `.github/copilot-instructions.md` 全局规则，通过 **SKILL（写作规范）+ instructions（文件级 contract）+ agents（流程编排）** 三层架构串联。

当前仓库已扩展为 **PM 文档链路 + 工程设计图形资产链路**：Solution 阶段在遇到复杂系统边界、AI scoring、异步评分、Mini program + backend、website handoff 等场景时，可通过 `fireworks-tech-graph` 生成发布级 SVG 技术图，用于 Engineering Review 和 Wiki 发布。

---

## 三段式 PM 工作流（v3.0 · 多 project 并行）

```
Discovery                 Plan                            Deliver
────────────          ─────────────                  ─────────────
Value Architect   →   Solution Architect         →   Product Planner
(market-research +    (solution-design SKILL +       (ac-writing-spec SKILL +
 value-frame SKILL)    project-context-loader)        project-context-loader)
                      ┌──────────────────────┐       ┌──────────────────────┐
                      │ Step -1: PM 选 1 或 │      │ Step 0: PM 选 1 或 │
                      │   多个 Epics from   │       │   ALL Epics from   │
                      │   §4 Roadmap        │       │   Solution Brief   │
                      └──────────────────────┘       └──────────────────────┘
       │                     │                                    │
       ▼                     ▼                                    ▼
Project/{p}/Value/    Project/{p}/Solution/              Project/{p}/PRD/
LATEST.md             {epic}/LATEST.md                   {epic}/LATEST.md
                                                                  │
                          ┌───────────────────────────────────────┤
                          ▼                                       ▼
                   UX Prototyper                       Eng Reviewer (v3.1)
                   (UX 文档+HTML)                       mode:
                          │                          ┌──local: 本地拉取
                          │                          ├──wiki-fallback
                          │                          │   （临时缓存）
                          │                          └──manual-input
                          │                            (eng-review-spec SKILL +
                          │                             ac-writing-spec SKILL +
                          │                             project-context-loader)
                          │                                       │
                          │                                       ▼
                          │                                Project/{p}/EngReview/
                          │                                {epic}/LATEST.md
                          │                                       │
                          │                                       ▼
                          │                                 Task Planner
                          │                                       │
                          ▼                                       ▼
                ┌──────────────────────────────────────────────────────┐
                │              Wiki Publisher (v3.0)                   │
                │  /{project}              ← Value 主页                │
                │  /{project}/{epic}-solution         ← Solution       │
                │  /{project}/{epic}-PRD              ← PRD merged     │
                │  /{project}/{epic}-PRD/ui-prototype       ← UX       │
                │  /{project}/{epic}-PRD/engineering-review ← Eng      │
                │  /{project}/{epic}-PRD/task-planning      ← Task     │
                └──────────────────────────────────────────────────────┘
```

> **v3.0 核心变化**：
> - 所有非 Value 阶段 agent 启动时**强制**经过 `project-context-loader` 五步协议（询问 project name + 一致性校验 + Epic List 列出 + PM 确认）
> - Eng Reviewer 使用 `local` / `wiki-fallback`（临时缓存）/ `manual-input`
> - Wiki 路径以 `/{project}` 为主页，全部带 `-solution` / `-PRD` 命名后缀
> - Solution Architect 支持从 Value §4 Roadmap 单选 / 多选 / ALL，且每个 Epic 独立产出 Solution Brief
> - Product Planner 支持单 Epic 或 ALL 全选批处理（每 Epic 独立 PRD）
> - 新增 `skills/project-context-loader` 与 `skills/eng-review-spec` 两个 SKILL，Eng Reviewer 改为薄编排
> - Knowledge Retriever 在 Epic Kickoff 时单独调用一次，生成 `context-memo.md` 供后续所有 Agent 共享。

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

### Stage 3：Product Planner（Deliver）

推荐输入路径是 Source B：基于 Solution Brief 产出 PRD。

```text
Project={project}
Selected Epic={epic-slug}
Value Frame Ref=Project/{project}/Value/LATEST.md
Solution Brief Ref=Project/{project}/Solution/{epic}/LATEST.md
Magic Patterns editor_id={可选，推荐}
Figma file_id={可选}
```

Product Planner 读取 Solution §2 Feature List 和 §8 Story List Preview，接管 Stable Feature ID / Story ID，并进一步拆解为：

```text
Feature → User Story → Acceptance Criteria → Estimation → Engineering Notes → NFR
```

Magic Patterns 可在 Product Planner 阶段继续作为 Story / AC 细化输入，用于识别页面状态、字段、按钮、校验和异常反馈。若 Solution 或设计稿更新，Product Planner 进入 refinement，同步更新 `PRD/{epic}/LATEST.md`。

### Stage 4：Handoff（交付延展）

| 下游 Agent | 输入 | 输出 |
|---|---|---|
| UX Prototyper | PRD + Solution + 设计稿 | UX 文档 / HTML Prototype |
| Eng Reviewer | Value + Solution + PRD + UX | Engineering Review / 风险与实现建议 |
| Task Planner | PRD + Eng Review | 可执行开发任务拆分 |
| Wiki Publisher | Value / Solution / PRD / UX / Eng / Task | ADO Wiki standard / merged 发布 |

PM 推荐使用顺序：

```text
Knowledge Retriever（可选）
→ Value Architect
→ Solution Architect
→ Product Planner
→ UX Prototyper / Eng Reviewer
→ Task Planner
→ Wiki Publisher
```

---

## Agent 清单

| Agent | 版本 | 职责 | 关键 SKILL | Handoff |
|---|---|---|---|---|
| **Knowledge Retriever** | — | Epic Kickoff 时检索 ADO Wiki 历史，生成 `context-memo.md` | — | Product Planner / UX / Eng |
| **Value Architect** | v2.5.0 | Discovery 入口（project 主入口）：启动时主动扫描已有 project，竞品 URL 调研 + Gate 2 PM 必答四问 + Value Frame | `market-research`, `value-frame` | Solution Architect |
| **Solution Architect** | v2.2.0 | Plan 中段：Step -1 强制 project-context-loader → 列 Value §4 Epic List → PM 单选 / 多选 / ALL → 每 Epic 独立产出 Solution Brief；复杂边界下生成发布级技术图 | `project-context-loader`, `solution-design`, `fireworks-tech-graph` | Product Planner / Eng Reviewer |
| **Product Planner** | v4.3.0 | Deliver 终段：Step 0 强制 project-context-loader → 列 Value Epic List + Solution / PRD 状态 → PM 单选 / ALL 全选批处理 → Epic→Feature→Story→AC + Estimation + NFR + Engineering Notes | `project-context-loader`, `ac-writing-spec` | Story Splitter / UX / Eng / Wiki |
| **Story Splitter** | v2.2.0 | Feature 复杂度评估 (FCS) + Story 拆分 + AC 补全（PP 子 Agent） | `ac-writing-spec` | (返回 Product Planner) |
| **UX Prototyper** | v2.0.0 | UX 文档 + HTML 原型 | — | Eng Reviewer / Wiki |
| **Eng Reviewer** | v3.1.0 | 工程评审薄编排：Step 0 project-context-loader → Step 1 mode 判定（`local` / `wiki-fallback` 临时缓存 / `manual-input`）→ 产出落盘到 `Project/{p}/EngReview/{epic}/`；评审章节锚点 / Scope Challenge / Blast Radius / §17.0 AC 合规由 SKILL 定义 | `project-context-loader`, `eng-review-spec`, `ac-writing-spec` | Task Planner / Wiki |
| **Task Planner** | — | 任务拆分、估算、依赖识别 | — | Wiki Publisher |
| **Wiki Publisher** | v3.0.0 | v3.0 路径表：`/{project}` Value 主页 + `-solution` / `-PRD` 命名后缀 + 三级子页 UX / Eng / Task；支持 6 种 page_type；PRD 合并模式 | `project-context-loader` | — |

---

## SKILL 清单

所有 SKILL 是"写作规范的单一来源"，由 agent 通过强制 Read 引用，不内化到 agent。

| SKILL | 版本 | 用途 | 被谁加载 |
|---|---|---|---|
| [skills/project-context-loader/SKILL.md](skills/project-context-loader/SKILL.md) | v1.1.0 | **多 project 并行下的统一上下文加载规范**（v3.0 新增）：询问 project name → 校验 Value LATEST → 加载 Rules/context-memo → 列 Epic List → PM 单选 / 多选 / 全选 / 例外流程；project 名不一致循环 ≤3 次 | Solution Architect / Product Planner / Eng Reviewer / Wiki Publisher |
| [skills/market-research/SKILL.md](skills/market-research/SKILL.md) | v1.2.0 | 竞品 URL 调研（PM 提供 URL）+ 竞品速览（核心能力 + 解决的痛点 两列并列）+ 6 段式深度对标 | Value Architect (Mode 2) |
| [skills/value-frame/SKILL.md](skills/value-frame/SKILL.md) | v1.1.0 | Value Frame 章节锚点（§1 Brief 6 要素含"为什么是我们做" / §2 Hypothesis / §3 KPI Tree / §4 Roadmap+Epic / §5 OQ）；Epic 颗粒度三判定 + 反模式 + Epic 自检矩阵 | Value Architect (Gate 3) |
| [skills/solution-design/SKILL.md](skills/solution-design/SKILL.md) | v1.0.0 | Solution Brief 章节锚点（Stable Feature ID / Feature List / Journey / Process / GWT Top / T-shirt Workload / Tech 四段式 / Story List 预览）；复杂系统边界时触发发布级技术图规则 | Solution Architect |
| [skills/fireworks-tech-graph/SKILL.md](skills/fireworks-tech-graph/SKILL.md) | external | 生成发布级 SVG/PNG 技术图（layered architecture / data flow / sequence / component diagram 等），默认可配合 Claude Official style | Solution Architect / Eng Reviewer |
| [skills/ac-writing-spec/SKILL.md](skills/ac-writing-spec/SKILL.md) | v1.0.0 | AC 写作规范（GIVEN/WHEN/THEN 多行 / A 类操作 / B 类字段 / C 类业务）；编号体系唯一权威 | Product Planner / Story Splitter / Eng Reviewer |
| [skills/eng-review-spec/SKILL.md](skills/eng-review-spec/SKILL.md) | v1.0.0 | **Engineering Review 写作规范**（v3.0 新增）：章节锚点（§0–§18）/ Scope Challenge 三问 / Complexity Smell 5 触发 / Service Boundary 双列 / Blast Radius 五维 / Sequence ≥1 happy + ≥1 failure / API 11 字段 / Error 8 类 / §17.0 AC 合规输出格式 / §17 Task Planning Readiness | Eng Reviewer |

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
    solution-architect.agent.md    ← Plan：Solution Brief
    product-planner.agent.md       ← Deliver：PRD（Epic→Feature→Story→AC）
    story-splitter.agent.md        ← Story 拆分（PP 子 Agent）
    ux-prototyper.agent.md         ← UX 文档 + HTML 原型
    eng-reviewer.agent.md          ← 工程评审 + AC 合规
    task-planner.agent.md          ← 任务拆分
    wiki-publisher.agent.md        ← ADO Wiki 发布（standard/merged）
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
  eng-review-spec/SKILL.md         ← v3.0 Engineering Review 写作规范

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
    PRD/
      {epic-slug}/
        LATEST.md                  ← 指针 → 当前 canonical PRD 文件
        {epic-slug}-prd-{stamp}.md
    EngReview/                     ← v3.0 新增 · Eng Reviewer 产出落盘
      {epic-slug}/
        LATEST.md                  ← 指针 → 当前 canonical Eng Review 文件
        {epic-slug}-eng-review-{stamp}.md

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
9. **Eng Reviewer 薄编排（v3.1）** — 评审章节锚点、Scope Challenge、Blast Radius、§17.0 AC 合规输出格式抽离到 `eng-review-spec` SKILL；Eng Reviewer 只负责 `local` / `wiki-fallback` / `manual-input` 编排
10. **Wiki 路径项目化（v3.0 新增）** — 旧 `/{epic-name}` 平铺废弃；新规则以 `/{project}` 为 Value 主页，命名后缀 `-solution` / `-PRD` 严格强制

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
| 2026-05-19 | **Solution Architect v2.2 批量编排增强**。Solution 阶段支持从 Value §4 Roadmap 选择单个、多个或 ALL Epic；多选只增强编排能力，每个 Epic 仍独立产出 Solution Brief、独立 Quality Gate、独立落盘并维护 LATEST。`project-context-loader` 升级到 v1.1，同步 selected_epics / batch_selection 约定 |
| 2026-05-19 | **v3.0 多 project 并行 + Wiki 路径重构**。新增 `skills/project-context-loader` mini-SKILL（除 Value 外所有 agent 强制前置协议）；新增 `skills/eng-review-spec` SKILL（Eng Reviewer 改为薄编排）；Solution Architect v2.1 新增 Step -1 Epic List 选择；Product Planner v4.2 新增 ALL 全选批处理；Eng Reviewer v3.0 新增本地 / Wiki Fallback 临时缓存 / 手工输入 + 落盘 `Project/{p}/EngReview/`；Wiki Publisher v3.0 路径重构 `/{project}` 主页 + `-solution` / `-PRD` 命名后缀 + 三级子页；Value Architect v2.5 启动时扫描已有 project 防重名 |
| 2026-05-14 | Value 层重构：Mode 2 改为"竞品 URL 调研"（不依赖 web search）；竞品 Summary 升级为"核心能力 + 解决的痛点"两列并列 + 6 段式深度对标；Gate 2 升级为 PM 必答四问强制门；`value-frame` Brief 新增"为什么是我们做"字段 |
| 2026-05-14 | 增加 PM 使用工作流（审核版）；Mode 1 调研输入增加可选 5 段式 Summary 参考；`market-research` Step 2 浅扫表新增“不足之处”并统一“优势定位”口径 |
| 2026-05-13 | 引入 `fireworks-tech-graph` 独立 Skill；`solution-design` 增加复杂系统边界/评审产出的发布级图生成规则；E1 `speaking-challenge-and-scoring` Solution refinement，并生成 Engdesign SVG 资产 |
| 2026-05-08 | README 同步至 v3.0 三段式架构：新增 Value Architect / Solution Architect / 4 个 SKILL；Wiki Publisher v2.1 merged mode；Project/ 目录约定 + LATEST.md 指针 |
| 2026-05-02 | V2 工作流架构文档化 |
| 2026-04-28 | V2 重构：AC 抽象到 SKILL、instructions 瘦身、Eng Reviewer §17.0 合规校验 |
| 2026-04-16 | 初始 Agent 体系建立 |
