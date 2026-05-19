---
name: project-context-loader
description: 多 project 并行场景下，下游 agent（Solution / Product Planner / Eng Reviewer / Wiki Publisher）启动时如何识别 project、校验一致性、加载上游产出的统一规范。本 SKILL 是所有"非 Value 阶段"agent 进入正式工作前的强制前置步骤。
version: 1.1.0
updated: 2026-05-19
maintainer: @frankzhey
applies-to: [solution-architect, product-planner, eng-reviewer, wiki-publisher]
---

# Project Context Loader（多 project 并行下的统一上下文加载规范）

本 SKILL 解决"多个 project 并行时，下游 agent 如何知道读取哪个 project"的问题。所有非 Value 阶段的 agent 启动时必须显式 Read 本文件并按 §2 协议执行。

---

## §1 适用范围

| Agent | 何时引用 |
|---|---|
| **Solution Architect** | Step -1：进入 Solution Brief 产出前 |
| **Product Planner** | Step 0：进入 PRD 产出前 |
| **Eng Reviewer** | Step 0：进入工程评审前 |
| **Wiki Publisher** | 进入发布模式判定前 |

Value Architect 不引用本 SKILL（它是 project 的入口，负责创建 project 而非加载）。

---

## §2 五步协议（强制顺序执行）

### Step P1：询问 Project Name

固定话术：

> 请输入本次工作的 **project name**（kebab-case，与 Value Architect 阶段命名保持一致）：

PM 输入后转 Step P2。

### Step P2：Project 存在性校验

执行：

```text
检查 Project/{project}/Value/LATEST.md 是否存在
```

| 结果 | 处理 |
|---|---|
| ✅ 存在 | 进入 Step P3 |
| ❌ 不存在 | 进入 **不一致循环**（见 §3） |

### Step P3：加载项目级常量

无条件加载（缺失则跳过该条，但要在响应中告知 PM）：

| 文件 | 用途 | 缺失处理 |
|---|---|---|
| `Project/{project}/Value/LATEST.md` → 指向文件 | 战略/KPI/Roadmap | **必须存在**，不存在阻塞 |
| `Project/{project}/Rules/{project}-rules.md` | 项目永久规则（业务/工程/AC 三层） | 缺失 → 在响应中提示 "项目规则文件缺失，本次产出未应用项目级 Rules" |
| `Project/{project}/context-memo.md` | Epic 级历史缓存 | 缺失 → 静默跳过 |

### Step P4：列出可选范围（按 agent 类型差异化）

| Agent | 列出内容 | 来源 |
|---|---|---|
| Solution Architect | **Epic List**（来自 Value §4 Roadmap） | Value LATEST §4 表格；支持单选 / 多选 / ALL，但每个 Epic 独立产出 Solution Brief |
| Product Planner | **Value Epic List + Solution / PRD 状态** | Value LATEST §4 Epic List 为 canonical source；扫描 `Project/{project}/Solution/*/LATEST.md` 与 `Project/{project}/PRD/*/LATEST.md` 补状态 |
| Eng Reviewer | **本地 PRD Epic List 或 Wiki fallback 例外流程** | 优先扫描 `Project/{project}/PRD/*/LATEST.md`；若本地没有目标 PRD，允许按 Eng Reviewer agent 进入 `wiki-fallback` 或 `manual-input` |
| Wiki Publisher | 不适用（由输入文件 frontmatter 决定） | — |

列出格式（统一）：

```text
项目 {project} 下当前已有以下 {scope}：

| # | Epic Slug | Epic Name | 状态 | LATEST 路径 |
|---|---|---|---|---|
| 1 | epic-a | Epic A 名称 | draft / approved | Project/{project}/{scope}/epic-a/LATEST.md |
| 2 | epic-b | Epic B 名称 | draft / approved | Project/{project}/{scope}/epic-b/LATEST.md |

请选择本次要操作的 Epic（单选 / 多选 / 全选；具体限制见各 agent 选择规则）。
```

### Step P5：PM 选择确认

| Agent | 选择规则 |
|---|---|
| Solution Architect | **单选 1 个 / 多选多个 / 可选 ALL**（多选只是批量编排；每个 Epic 仍独立产出 1 份 Solution Brief + LATEST） |
| Product Planner | **单选 1 个 / 选"全部"**（选全部 = 循环每个已展开 Solution 的 Epic 各产出一份独立 PRD；未展开 Solution 的 Epic 只能单选后由 PM 确认是否跳过 Solution） |
| Eng Reviewer | **单选 1 个 Epic 或进入 Wiki fallback / manual-input 例外流程** |
| Wiki Publisher | 不适用 |

PM 选定后，转回原 agent 的正式工作流，并把以下变量带入下游：

```text
- project: {project}
- selected_epic: {epic-slug}（单选时）
- selected_epics: [list]（Solution Architect 多选 / ALL，或 PP 全选时）
- value_ref: Project/{project}/Value/LATEST.md
- rules_ref: Project/{project}/Rules/{project}-rules.md（可选）
- context_memo_ref: Project/{project}/context-memo.md（可选）
```

### Step P5-E：Eng Reviewer 例外流程

Eng Reviewer 与其它下游 agent 不同：工程评审可能发生在本地 PRD 尚未同步、但 Wiki 已发布合并 PRD 的场景。

当 `Project/{project}/PRD/{epic-slug}/LATEST.md` 不存在时，Eng Reviewer 不应静默创建 PRD，也不应终止整个评审；必须让 PM 二选一：

```text
本地未找到 Project/{project}/PRD/{epic-slug}/LATEST.md。
请选择：
  Y. 到 ADO Wiki 的 /{project}/ 下寻找带 -PRD 标识的页面（mode=wiki-fallback）
  N. 手工粘贴 PRD 内容（mode=manual-input）
```

- `wiki-fallback`：只把 Wiki 内容临时缓存到 `outputs/wiki-cache/{project}/{epic-slug}/...`，不得回写 `Project/{project}/...`
- `manual-input`：只把 PM 粘贴内容临时缓存到 `outputs/manual-input/{project}/{epic-slug}/...`
- 两种例外流程落盘 Engineering Review 时都必须在 frontmatter 写明 `mode` 与 `source.cache_dir`

---

## §3 Project Name 不一致循环（强约束）

当 Step P2 校验失败（`Project/{project}/Value/LATEST.md` 不存在）时，执行：

```text
1. 扫描 Project/* 下已存在的所有项目（含 Value/LATEST.md 的才算"已建立项目"）
2. 列出给 PM：

   ⚠️ 项目 "{input}" 未找到 Value 产出。当前已建立的项目有：
     - project-a（Value 已建立：2026-05-08）
     - project-b（Value 已建立：2026-05-12）
     - ...

   请：
     A. 重新输入正确的 project name（必须与 Value Architect 阶段命名一致）
     B. 或先回 Value Architect 为 "{input}" 项目建立 Value 产出后再来

3. PM 输入新 project name → 回到 Step P2 重新校验
4. 重试上限 3 次。第 3 次仍失败 → 终止启动，提示 PM 检查项目命名规范
```

**禁止**：
- ❌ PM 输入不存在的 project name 时静默创建空目录
- ❌ 跳过校验直接进入工作流
- ❌ 自动猜测/纠正 project name（如 PM 输 "spk2-challenge" 自动找 "spk2challenge-miniprogram"）
- ❌ Product Planner 通过不存在 project 绕过 Value；独立 PRD 不得写入正式 `Project/{project}` 三段式目录

---

## §4 上游变更感知（refinement 场景）

当下游 agent 的当前 epic LATEST 已存在时（refinement 模式），加载完上游后必须比对 timestamp：

```text
比对：
  当前产出的 frontmatter.upstream_snapshot.value timestamp
  vs
  实际 Value/LATEST.md 指向文件的 timestamp

如果实际更新 > 当前 snapshot：
  提示 PM "Value Frame 已更新（{old-stamp} → {new-stamp}），是否需要基于新 Value 精炼本产出？"
  - PM 选"是" → 进入 refinement diff 流程
  - PM 选"否" → 保留当前 upstream_snapshot，在 §changelog 中追加 "已感知上游 X 变更但 PM 选择本次不同步"
```

对 Solution / PRD 上游同理（Eng Reviewer 应感知 PRD + Solution + Value 三段）。

---

## §5 Frontmatter 写入约定

凡通过本 SKILL 加载上下文的 agent，落盘 frontmatter 必须显式记录：

```yaml
skills_loaded:
  - skills/project-context-loader/SKILL.md
  - ... (其它本 agent 的写作规范 SKILL)
upstream_snapshot:
  value: Project/{project}/Value/value-architect-{stamp}.md
  solution: Project/{project}/Solution/{epic}/{epic}-solution-brief-{stamp}.md  # 如适用
  prd: Project/{project}/PRD/{epic}/{epic}-prd-{stamp}.md  # 如适用
project_loader:
  pm_confirmed_project: {project}
  pm_confirmed_epic: {epic-slug}  # 或 [list] 当 Solution Architect / PP 多选或全选
  batch_selection: single | multi | all  # 如适用
  loader_at: {YYYY-MM-DD-HHmm}
```

---

## §6 Quality Gate 自检

落盘前每个下游 agent 必须包含以下 3 条阻塞性检查：

- [ ] Step P1–P5 全部执行完毕，PM 已显式确认 project 与 epic 选择
- [ ] Step P2 校验通过（Value LATEST 存在）；未通过的不一致循环已记录到对话历史
- [ ] frontmatter 已写入 `project_loader.pm_confirmed_project` 与 `project_loader.pm_confirmed_epic`
- [ ] 多选 / 全选时已写入 `project_loader.batch_selection`，且每个被处理 Epic 独立通过本 agent 的落盘 Quality Gate

---

## §7 变更记录

| 版本 | 日期 | 变更 |
|---|---|---|
| 1.1.0 | 2026-05-19 | Solution Architect 选择规则从单选扩展为单选 / 多选 / ALL；明确多选只增强编排能力，每个 Epic 仍独立产出 Solution Brief、独立落盘并维护 LATEST。frontmatter 写入约定新增 `batch_selection`。 |
| 1.0.0 | 2026-05-19 | 初版。抽取多 project 并行场景下的 project 选择与一致性校验逻辑，被 Solution / Product Planner / Eng Reviewer / Wiki Publisher 共享引用。Value Architect 不引用（它是 project 入口）。 |
