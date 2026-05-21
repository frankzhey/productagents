---
name: ado-work-item-publish-spec
description: Azure DevOps Boards Work Item 发布规范。用于 Work Item Publisher 将 PM approved PRD 中的 Epic / Feature / User Story / AC 发布到 BCChina Azure DevOps project，包含字段映射、PM confirmation gate、Iteration/Area Path、tag 幂等、dry-run、create/update/stale/block 规则。v1.1 新增本地 ado-mapping.json + 发布历史落盘规范，幂等搜索升级为"本地优先 → ADO 回查"两阶段。
version: 1.1.0
updated: 2026-05-19
maintainer: @frankzhey
applies-to: [work-item-publisher]
---

# ADO Work Item Publish Spec

本 SKILL 是 **Work Item Publisher** 发布 Azure DevOps Boards work items 的唯一规范。它只定义发布规则，不负责重写 PRD。

---

## §1 发布前置条件

PRD 必须由 PM 明确确认，frontmatter 必须包含：

```yaml
status: approved
pm_confirmation:
  status: approved
  confirmed_by: PM
  confirmed_at: {YYYY-MM-DD-HHmm}
  confirmation_note: "PRD is confirmed"
```

任一字段缺失或 `status != approved` 时，发布阻塞。

---

## §2 输入与目标

### 2.1 输入

Work Item Publisher 必须让 PM 输入 PRD 准确名称或文件名，并从本地路径查找：

```text
Project/*/PRD/*/
```

禁止默认使用最近 PRD。

### 2.2 ADO 目标

Organization 固定：

```yaml
organization: BCChina
```

PM 必填：

```yaml
ado_project: {ADO Project}
```

PM 可选：

```yaml
iteration_path: {ADO Project}\{Iteration}
area_path: {ADO Project}\{Area}
```

默认值：

| 字段 | Create 默认 | Update 默认 |
|---|---|---|
| Iteration Path | `{ADO Project}` | PM 未提供则保留已有值 |
| Area Path | `{ADO Project}` | PM 未提供则保留已有值 |

若 PM 显式提供 Iteration Path / Area Path，create 和 update 都写入本次提供值。

---

## §3 PRD 解析规则

### 3.1 Epic

来源：

- frontmatter `epic_id`
- frontmatter `epic_name`
- §1 Epic Definition

ADO Work Item Type：

```text
Epic
```

Title：

```text
{epic_id} - {epic_name}
```

### 3.2 Feature

来源：

- §2 Feature List
- `Feature ID`
- `Feature Name`
- `Description`
- `Value`

ADO Work Item Type：

```text
Feature
```

Feature stable ID：

```text
{epic_id}-{Feature ID}
```

Title：

```text
{Feature ID} - {Feature Name}
```

### 3.3 User Story

来源：

- §3 User Stories and AC
- `Story ID`
- Story heading title
- User Story
- upstream_refs
- Acceptance Criteria
- 变更记录

ADO Work Item Type：

```text
User Story
```

Title：

```text
{Story ID} - {Story title}
```

---

## §4 字段映射

| PRD 内容 | ADO 字段 |
|---|---|
| Epic title | `System.Title` |
| Feature title | `System.Title` |
| Story title | `System.Title` |
| ADO Project | `System.TeamProject` |
| Iteration Path | `System.IterationPath` |
| Area Path | `System.AreaPath` |
| PRD managed markdown | `System.Description` 中的受控区块 |
| Acceptance Criteria | `Microsoft.VSTS.Common.AcceptanceCriteria`；若字段不可用，则写入 Description 受控区块 |
| Story estimate / units | `Microsoft.VSTS.Scheduling.StoryPoints` 或团队可用估算字段；字段不可用时写入 Description |
| KPI alignment / source project / source file | Tags |

---

## §5 Tags 与幂等 Key

### 5.1 必写 tags

Epic：

```text
prd-epic-id:{epic_id}
prd-source-project:{project}
prd-source-file:{prd_filename}
```

Feature：

```text
prd-feature-id:{epic_id}-{Feature ID}
prd-epic-id:{epic_id}
prd-source-project:{project}
prd-source-file:{prd_filename}
```

User Story：

```text
prd-story-id:{Story ID}
prd-feature-id:{epic_id}-{Feature ID}
prd-epic-id:{epic_id}
prd-source-project:{project}
prd-source-file:{prd_filename}
```

### 5.2 推荐 tags

```text
kpi:{KPI ID}
prd-version:{created}
prd-confirmed-at:{confirmed_at}
```

---

## §6 幂等搜索规则（v1.1 两阶段：本地优先 → ADO 回查）

### 6.1 两阶段查找

```text
For each level (Epic / Feature / User Story):

  Stage A — 本地 mapping 优先（快速路径）：
    读取 Project/{project}/PRD/{epic-slug}/ado-mapping.json
    按 PRD stable ID 查找：
      命中 → 取已记录的 ADO ID
              校验该 ADO ID 当前是否仍在目标 ADO project
                ├─ 是 → 直接使用，Source = "local-mapping"
                └─ 否 → mapping 已失效，进入 Stage B（并清理失效条目）
      未命中 → 进入 Stage B

  Stage B — ADO 回查（兜底路径）：
    调用 ado/search_workitem 在指定 ADO project 内按 tag 搜索
      tag 规则见下方 6.2 表
    命中 → 取 ADO ID，Source = "ado-search"，并把命中结果写回本地 mapping
    0 命中 → Action = create，Source = "new"
```

### 6.2 搜索 tag 表

| Level | 搜索 tag |
|---|---|
| Epic | `prd-epic-id:{epic_id}` |
| Feature | `prd-feature-id:{epic_id}-{Feature ID}` |
| User Story | `prd-story-id:{Story ID}` |

### 6.3 搜索结果处理

| 搜索结果 | 动作 |
|---|---|
| 0 条（Stage A + Stage B 均未命中） | `create` |
| 1 条，类型匹配 | `update` |
| 1 条，类型不匹配 | `block` |
| 多条 | `block`，要求 PM / ADO owner 清理重复项 |
| 找到但不在目标 ADO project | `block`，禁止跨 project 更新 |
| 本地 mapping 与 ADO 回查结果不一致 | `block`，要求 PM 显式选择以本地或 ADO 为准；选定后清理另一方 |

---

## §7 Update 规则

Update 时只更新 PRD 管理字段。

### 7.1 PRD managed block

Description 中必须维护受控区块：

```md
<!-- PRD_MANAGED_START -->
Source PRD: {local path}
PRD Confirmed At: {confirmed_at}

{Epic / Feature / Story content from PRD}
<!-- PRD_MANAGED_END -->
```

更新时只替换 `PRD_MANAGED_START` 和 `PRD_MANAGED_END` 之间的内容。区块外内容必须保留。

### 7.2 必须更新

- Title：PRD stable ID 相同但标题变化时更新
- PRD managed block
- Acceptance Criteria
- Tags
- Parent link：父级不一致时 dry-run 标注并在 publish 时修正

### 7.3 有条件更新

| 字段 | 规则 |
|---|---|
| Iteration Path | PM 本次显式提供才更新；未提供则保留已有值 |
| Area Path | PM 本次显式提供才更新；未提供则保留已有值 |
| Estimate / Story Points | 字段可用且 PRD 有值时更新；否则写入 Description |

### 7.4 禁止覆盖

- Assignee
- State
- Reason
- Comments / Discussion
- Links（除 PRD 层级 parent-child link）
- 团队手写的 Description 区块外内容
- 开发中新增的 implementation notes

---

## §8 Create 规则

Create 时必须写入：

- Work Item Type
- Title
- Description PRD managed block
- Acceptance Criteria（Story 必须）
- Tags
- Area Path
- Iteration Path
- Parent-child relation

Create 顺序：

1. Epic
2. Features
3. User Stories
4. Parent-child links

若 Feature create 失败，其下 Story 必须跳过并标记 `blocked: parent feature failed`。

---

## §9 Stale 规则

如果 ADO 中存在相同 `prd-source-project` + `prd-source-file` 的 Story tag，但当前 PRD 中不再存在该 Story ID：

```text
Action = stale-candidate
```

默认不删除、不关闭、不改 state。发布报告中提示 PM 后续人工确认。

如 PM 明确要求处理 stale work items，也只能追加 tag：

```text
prd-stale-candidate:true
```

禁止自动删除 ADO work item。

---

## §10 dry-run 输出（v1.1 增列）

正式 publish 前必须输出：

```text
| Level | PRD ID | Action | Existing ADO ID | Title | Iteration Path | Area Path | AC Target | Source | Notes |
```

| 列 | 取值 |
|---|---|
| Action | `create` / `update` / `block` / `stale-candidate` / `no-op` |
| AC Target | `standard`（Microsoft.VSTS.Common.AcceptanceCriteria） / `description-fallback`（字段不可用） / `n/a`（Epic/Feature 无 AC） |
| Source | `local-mapping`（本地 mapping 命中） / `ado-search`（回查 ADO 命中） / `new`（首次发布） |

任何 `block` 存在时，禁止进入 publish。

---

## §11 发布报告

发布后输出：

```text
| Level | PRD ID | ADO Action | ADO ID | URL | Status | Notes |
```

并附：

```yaml
organization: BCChina
ado_project: {ADO Project}
iteration_path: {final iteration path}
area_path: {final area path}
source_prd: {local PRD path}
status: success | dry-run-only | blocked | partial-success
```

---

## §12 异常处理

| 异常 | 处理 |
|---|---|
| PRD 未找到 | 让 PM 重新输入准确名称 |
| PRD 多命中 | 列表让 PM 选择 |
| PRD 未 approved | 阻塞，要求回 Product Planner 完成 PM Confirm Gate |
| ADO project 不存在或无权限 | 阻塞，返回 project / 权限检查建议 |
| 缺少 ADO 写入工具 | 只允许 dry-run，正式 publish 阻塞 |
| tag 多命中 | 阻塞，要求人工清理 |
| work item 类型不匹配 | 阻塞，不自动转换类型 |
| parent link 冲突 | dry-run 标记，publish 时按 PRD 层级修正 |

---

## §13 强制规则

必须：
- 以 PRD stable ID tags 做幂等
- 先 dry-run，后 publish confirmed
- update 只更新 PRD managed block
- create/update 都保持 Epic -> Feature -> User Story 层级
- Iteration / Area 在 create 时空值进入 project 根路径
- Iteration / Area 在 update 时空值不覆盖已有值
- **每完成一个 Work Item 立即写回本地 mapping**（防止中途失败无法续传 · v1.1）
- **发布完成后必须落盘 mapping + history report + 回写 PRD frontmatter `ado_published` 块**（v1.1）
- **ADO 写入工具不可用时只允许 dry-run，status: `dry-run-only`**（v1.1）

禁止：
- 发布 draft PRD
- 自动删除 stale work item
- 跨 ADO project 更新相同 tag 的 work item
- 使用 title 作为唯一幂等 key
- 覆盖团队手工维护字段
- **跳过本地 mapping/history 落盘**（v1.1 落盘是审计追溯依据）
- **只信任本地 mapping 不做 ADO 回查**（mapping 可能因人工在 ADO 端清理而失效，必须按 §6.1 两阶段执行）

---

## §14 本地 mapping 与发布历史落盘（v1.1 新增 · 强制）

### 14.1 落盘路径

```text
Project/{project}/PRD/{epic-slug}/
├── ado-mapping.json                                    ← 单文件，本次更新条目 + 历史条目共存
├── ado-publish-history/
│   ├── ado-publish-2026-05-19-1430.md                  ← 每次发布生成独立时间戳文件
│   ├── ado-publish-2026-05-19-1845.md
│   └── ...
└── LATEST.md, {epic-slug}-prd-{stamp}.md               ← 既有 PRD 文件不变
```

### 14.2 ado-mapping.json 结构

```json
{
  "project": "spk2challenge-miniprogram",
  "epic_slug": "speaking-challenge-and-scoring",
  "ado_organization": "BCChina",
  "ado_project": "IELTS Mini Program",
  "schema_version": "1.1",
  "last_synced_at": "2026-05-19-1430",
  "epic": {
    "prd_id": "EPIC-speaking-challenge-and-scoring",
    "ado_id": 12345,
    "ado_url": "https://dev.azure.com/BCChina/IELTS%20Mini%20Program/_workitems/edit/12345",
    "title_at_publish": "EPIC-speaking-challenge-and-scoring - Speaking Challenge and Scoring",
    "iteration_path": "IELTS Mini Program",
    "area_path": "IELTS Mini Program",
    "last_synced_at": "2026-05-19-1430",
    "content_hash": "sha1:..."
  },
  "features": {
    "F1": {
      "prd_id": "F1",
      "ado_id": 12346,
      "ado_url": "...",
      "parent_ado_id": 12345,
      "last_synced_at": "2026-05-19-1430",
      "content_hash": "sha1:..."
    }
  },
  "stories": {
    "EPIC-speaking-challenge-and-scoring-F1-S01": {
      "ado_id": 12350,
      "ado_url": "...",
      "parent_ado_id": 12346,
      "ac_count": 5,
      "ac_target": "standard",
      "iteration_path": "IELTS Mini Program\\Sprint 2026-05",
      "area_path": "IELTS Mini Program\\Mini Program",
      "last_synced_at": "2026-05-19-1430",
      "content_hash": "sha1:..."
    }
  },
  "stale_candidates": [
    {
      "prd_id": "EPIC-speaking-challenge-and-scoring-F1-S99",
      "ado_id": 12399,
      "reason": "Story removed from PRD",
      "detected_at": "2026-05-19-1430"
    }
  ]
}
```

### 14.3 content_hash 计算规则

对每个 work item 的 PRD managed block 内容（Description 中 `<!-- PRD_MANAGED_START -->` 到 `<!-- PRD_MANAGED_END -->` 之间）+ AC 全文做 SHA1。

用途：
- 下次发布前对比 hash，**未变更的 Story 标 Action=no-op，不做 ADO 写入**（性能优化）
- dry-run 表 Notes 列显式标 `content unchanged` / `content changed`

### 14.4 发布历史报告格式（ado-publish-{stamp}.md）

```markdown
---
publish_at: 2026-05-19-1430
project: {project}
epic_slug: {epic-slug}
ado_organization: BCChina
ado_project: {ADO Project}
source_prd: Project/{project}/PRD/{epic-slug}/{file}.md
pm_confirmation_at: {YYYY-MM-DD-HHmm}
status: success | partial-success | blocked | dry-run-only
---

# ADO Publish History — {epic-slug} — {stamp}

## 发布摘要

- Created: N
- Updated: M
- No-op (content unchanged): K
- Block: B
- Stale candidates: S

## 明细

| Level | PRD ID | ADO Action | ADO ID | URL | AC Target | Source | Status | Notes |
|---|---|---|---|---|---|---|---|---|
| Epic | ... | create | 12345 | ... | n/a | new | success | |
| ... | | | | | | | | |

## Block 明细（如有）

[每条 block 的具体原因 + 建议]

## Stale Candidates（如有）

[已在 ADO 但不在当前 PRD 的 work items 列表，仅追加 `prd-stale-candidate:true` tag，不删除]
```

### 14.5 PRD frontmatter 回写

发布完成后必须回写 `Project/{project}/PRD/{epic-slug}/{file}.md` 与 LATEST 指向文件的 frontmatter：

```yaml
ado_published:
  last_publish_at: {YYYY-MM-DD-HHmm}
  ado_organization: BCChina
  ado_project: {ADO Project}
  work_item_count:
    epic: 1
    feature: N
    story: M
  status: success | partial-success | blocked | dry-run-only
  mapping_file: Project/{project}/PRD/{epic-slug}/ado-mapping.json
  last_history_report: Project/{project}/PRD/{epic-slug}/ado-publish-history/ado-publish-{stamp}.md
```

> 仅 v1.1 起的新发布回写此字段；旧 PRD 不强制 backfill。

---

## §15 版本变更记录

| 版本 | 日期 | 变更 |
|---|---|---|
| 1.1.0 | 2026-05-19 | **本地 mapping + 发布历史落盘**。§6 幂等搜索升级为两阶段（本地 mapping 优先 → ADO 回查兜底）；§10 dry-run 表新增 `AC Target` / `Source` 列；新增 §14 落盘规范（`ado-mapping.json` 结构 + `ado-publish-history/{stamp}.md` 格式 + PRD frontmatter `ado_published` 回写）；§13 强制规则新增"无写工具时只允许 dry-run"、"立即写回 mapping"、"完成后必须落盘"。 |
| 1.0.0 | 2026-05-19 | 初版。定义 approved PRD 到 Azure DevOps Boards 的映射、tag 幂等、dry-run、create/update/stale/block、Iteration Path / Area Path 默认与覆盖规则。 |
