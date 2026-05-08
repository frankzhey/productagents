# ProductPortfolio — AI Agent 协作工作流

> BCChina 三段式 PM + 工程交付 Agent 框架  
> 更新时间：2026-05-08

---

## 仓库定位

本仓库管理 BCChina 从 **Discovery → Plan → Deliver → 工程评审 → 任务拆分 → Wiki 发布** 的全流程 AI Agent 协作体系。所有 Agent 遵循 `.github/copilot-instructions.md` 全局规则，通过 **SKILL（写作规范）+ instructions（文件级 contract）+ agents（流程编排）** 三层架构串联。

---

## 三段式 PM 工作流（v3.0）

```
Discovery                 Plan                       Deliver
────────────          ─────────────              ─────────────
Value Architect   →   Solution Architect    →    Product Planner
(market-research +    (solution-design SKILL)    (ac-writing-spec SKILL)
 value-frame SKILL)                              ├─ Story Splitter (子)
       │                     │                            │
       ▼                     ▼                            ▼
Project/{p}/Value/     Project/{p}/Solution/       Project/{p}/PRD/
LATEST.md              {epic}/LATEST.md            {epic}/LATEST.md
                                                          │
                          ┌───────────────────────────────┤
                          ▼                               ▼
                   UX Prototyper                  Eng Reviewer (v2.2)
                   (UX 文档+HTML)                  三段式上下文加载
                          │                       §17.0 AC 合规校验
                          │                               │
                          │                               ▼
                          │                         Task Planner
                          │                               │
                          ▼                               ▼
                ┌──────────────────────────────────────────────┐
                │       Wiki Publisher (v2.1)                  │
                │  standard mode  /  merged mode（PRD 三合一）  │
                └──────────────────────────────────────────────┘
```

> Knowledge Retriever 在 Epic Kickoff 时单独调用一次，生成 `context-memo.md` 供后续所有 Agent 共享。

---

## Agent 清单

| Agent | 版本 | 职责 | 关键 SKILL | Handoff |
|---|---|---|---|---|
| **Knowledge Retriever** | — | Epic Kickoff 时检索 ADO Wiki 历史，生成 `context-memo.md` | — | Product Planner / UX / Eng |
| **Value Architect** | v2.0.0 | Discovery 入口：市场调研 + Value Frame（Brief/Hypothesis/KPI/Roadmap/OQ） | `market-research`, `value-frame` | Solution Architect |
| **Solution Architect** | v2.0.0 | Plan 中段：基于选定 Epic 产出 Solution Brief（Feature List / Journey / Process / GWT / Workload / Tech / Story List 预览） | `solution-design` | Product Planner |
| **Product Planner** | v4.1.0 | Deliver 终段：Epic→Feature→Story→AC + Estimation + NFR + Engineering Notes | `ac-writing-spec` | Story Splitter / UX / Eng / Wiki |
| **Story Splitter** | v2.2.0 | Feature 复杂度评估 (FCS) + Story 拆分 + AC 补全（PP 子 Agent） | `ac-writing-spec` | (返回 Product Planner) |
| **UX Prototyper** | v2.0.0 | UX 文档 + HTML 原型 | — | Eng Reviewer / Wiki |
| **Eng Reviewer** | v2.2.0 | 工程评审；强制加载 Value+Solution+PRD 三段式上下文；§17.0 AC 合规校验 | `ac-writing-spec` | Task Planner / Wiki |
| **Task Planner** | — | 任务拆分、估算、依赖识别 | — | Wiki Publisher |
| **Wiki Publisher** | v2.1.0 | 文档识别 + Wiki 发布；支持 **merged mode**（Value+Solution+PRD 合并为单页） | — | — |

---

## SKILL 清单

所有 SKILL 是"写作规范的单一来源"，由 agent 通过强制 Read 引用，不内化到 agent。

| SKILL | 版本 | 用途 | 被谁加载 |
|---|---|---|---|
| [skills/market-research/SKILL.md](skills/market-research/SKILL.md) | v1.0.0 | 竞品发现 + 5 字段速览 + 深度对标流程 | Value Architect (Mode 2) |
| [skills/value-frame/SKILL.md](skills/value-frame/SKILL.md) | v1.0.0 | Value Frame 章节锚点（§1 Brief / §2 Hypothesis / §3 KPI Tree / §4 Roadmap+Epic / §5 OQ）；Epic 颗粒度三判定 + 反模式 + Epic 自检矩阵 | Value Architect (Gate 3) |
| [skills/solution-design/SKILL.md](skills/solution-design/SKILL.md) | v1.0.0 | Solution Brief 章节锚点（Stable Feature ID / Feature List / Journey / Process / GWT Top / T-shirt Workload / Tech 四段式 / Story List 预览） | Solution Architect |
| [skills/ac-writing-spec/SKILL.md](skills/ac-writing-spec/SKILL.md) | v1.0.0 | AC 写作规范（GIVEN/WHEN/THEN 多行 / A 类操作 / B 类字段 / C 类业务）；编号体系唯一权威 | Product Planner / Story Splitter / Eng Reviewer |

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
  market-research/SKILL.md         ← 竞品调研规范
  value-frame/SKILL.md             ← Value Frame 写作规范
  solution-design/SKILL.md         ← Solution Brief 写作规范
  ac-writing-spec/SKILL.md         ← AC 写作规范（PM agents 唯一权威）

Project/                           ← 项目级落盘根目录
  {project}/
    Rules/{project}-rules.md       ← 项目永久规则（业务/工程/AC 三层）
    context-memo.md                ← Epic 级历史缓存
    Research/                      ← 调研材料（market-research 产出）
    Value/
      LATEST.md                    ← 指针 → 当前 canonical Value 文件
      value-architect-{stamp}.md
    Solution/
      {epic-slug}/
        LATEST.md                  ← 指针 → 当前 canonical Solution 文件
        {epic-slug}-solution-brief-{stamp}.md
    PRD/
      {epic-slug}/
        LATEST.md                  ← 指针 → 当前 canonical PRD 文件
        {epic-slug}-prd-{stamp}.md

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
6. **强制依赖加载** — agent 在 Step 0 / Gate 前置 Read SKILL，确保 Copilot 加载链路确定性

---

## 指令优先级（强制）

1. 用户当前明确要求
2. `.github/copilot-instructions.md`（全局）
3. `.github/instructions/*.instructions.md`（局部）
4. `.github/agents/*.agent.md`（角色）
5. `skills/*/SKILL.md`（写作规范）

---

## Wiki 发布路径（ADO `Product-Portfolio.wiki`）

| 文档类型 | 路径 | 模式 |
|---|---|---|
| PRD（独立） | `/{epic-name}` | standard |
| PRD（含上游 snapshot） | `/{epic-name}` | **merged**（拼接 Value + Solution + PRD） |
| UX | `/{epic-name}/ui-prototype` | standard |
| Engineering Review | `/{epic-name}/engineering-review` | standard |
| Task Planning | `/{epic-name}/task-planning` | standard |

---

## 版本记录

| 日期 | 变更 |
|---|---|
| 2026-05-08 | README 同步至 v3.0 三段式架构：新增 Value Architect / Solution Architect / 4 个 SKILL；Wiki Publisher v2.1 merged mode；Project/ 目录约定 + LATEST.md 指针 |
| 2026-05-02 | V2 工作流架构文档化 |
| 2026-04-28 | V2 重构：AC 抽象到 SKILL、instructions 瘦身、Eng Reviewer §17.0 合规校验 |
| 2026-04-16 | 初始 Agent 体系建立 |
