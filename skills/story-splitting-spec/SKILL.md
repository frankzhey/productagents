---
name: story-splitting-spec
description: Feature / User Story 拆分规范 v1.1。用于 Solution Architect、Product Planner、Story Splitter 在拆 Feature、拆 Story、评估 FCS、复用 Stable ID、输出 Jira-ready Story 前加载。定义 Feature=系统能力、User Story=用户行为、Feature Gate、Story 拆分优先级、FCS、Story 数量、Story Points→Man-day→Units 估算映射、特殊独立 Story 场景、输出契约与质量检查；AC 细节引用 ac-writing-spec。
metadata:
  version: 1.1.0
  updated: 2026-06-29
  maintainer: "@frankzhey"
  applies-to: [solution-architect, product-planner, story-splitter]
---

# Feature / User Story 拆分规范

本 SKILL 是 Feature / User Story 拆分的单一规则源。Agent 只负责编排；什么是合格 Feature、什么时候拆 Story、Story 输出结构与质量门禁都以本文为准。

> AC 的格式、覆盖项、状态机、按钮置灰、表单校验、写法模板与信息缺口处理，统一引用 `skills/ac-writing-spec/SKILL.md`。

---

## §1 加载顺序

拆分 Feature / Story 前必须加载：

1. `skills/story-splitting-spec/SKILL.md`
2. `skills/ac-writing-spec/SKILL.md`
3. `Project/{project}/Rules/{project}-rules.md`（如存在）
4. `Project/{project}/Solution/{epic-slug}/LATEST.md`（如存在，用于复用 Feature ID、Story ID、Persona、BP-X）

---

## §2 Feature 定义：系统能力

Feature 是系统提供的一组完整能力，必须能表达相对独立的用户价值或业务价值。

### 2.1 Feature Gate（强制）

每个候选 Feature 必须通过以下检查：

| 检查项 | 规则 |
|---|---|
| 名称可独立理解 | 只看 Feature 名就知道要做什么；禁止"报告优化"、"流程优化"这类模糊名称 |
| 业务闭环完整 | 至少包含输入 → 处理 → 输出 |
| 尽量不跨系统 | 跨用户系统 / AI 系统 / 支付系统 / 后台系统时，优先拆为多个 Feature |
| Story 数量合理 | 每个 Feature 下至少 3 个 User Story；超过 10 个 Story 时优先拆 Feature |
| 后台菜单边界 | 后台系统中独立二级菜单默认可作为一个 Feature |
| 价值可表达 | 必须能写出用户价值或业务价值，不能只写"提升体验" |

### 2.2 Feature 拆分反例与正例

错误：

```text
Feature：支付
```

正确：

```text
Feature 1：会员下单系统
- 创建订单
- 展示价格
- 优惠券

Feature 2：支付渠道集成
- Stripe / PayPal / 微信支付

Feature 3：支付结果处理
- 回调
- 状态更新
- 发放会员
```

### 2.3 Solution 阶段 Feature List 要求

Solution Brief 的 Feature List 必须包含：

| 字段 | 要求 |
|---|---|
| Feature ID | `F1`, `F2`, `F3`，一经确认永不重排 |
| Name | 通过 §2.1 名称检查 |
| Description | ≥30 字，不重复标题 |
| Value | 明确用户价值或业务价值 |
| 预估 Story 数 | range，如 3–5 / 4–6 |
| T-shirt | 按 `solution-design` 的统一映射 |
| 关联 Persona | 引用 Solution §3 中已定义的 P1/P2 |
| 主要复杂度驱动 | 具体说明，如"第三方 AI 服务 + 异步任务 + 结果展示状态" |

禁止孤立 Feature：§2 Feature List 中的每个 Feature 必须在 Journey 或流程难点中被引用。

---

## §3 User Story 定义：用户行为

User Story 是某类用户，在某个场景下，为了某个目标，希望完成的动作。

标准格式：

```text
As a [user role], I want to [action], so that [benefit].
```

每个 Story 必须具备业务闭环：

```text
用户动作 → 系统处理 → 用户反馈
```

---

## §4 Story 拆分优先级

拆 Story 时按以下顺序判断。命中高优先级时，优先按高优先级拆。

| 优先级 | 拆分维度 | 强制规则 |
|---:|---|---|
| 1 | 用户目标 | 一个 Story 只解决一个用户目标，如查看报告 / 提交作文 / 下载报告 |
| 2 | 用户动作 | 一个核心动作一个 Story，如保存草稿 / 提交 / 分享 |
| 3 | 业务闭环 | 每个 Story 都要有用户动作 → 系统处理 → 用户反馈 |
| 4 | 状态变化 | draft / processing / success / fail 等不同状态优先独立 Story 或独立 AC；复杂状态机必须独立 Story |
| 5 | 权限角色 | 学员 / 老师 / 运营 / 管理员等不同角色不混在同一 Story |
| 6 | 核心数据对象 | 报告 / 模考 / 作文 / 用户等核心实体不同，优先拆开 |
| 7 | 复杂逻辑 | 独立规则 / 独立计算 / 独立校验 / 独立状态机必须拆 |
| 8 | 接口/API | 第三方接口 / 异步任务 / AI 服务 / 长耗时任务必须拆；发送 / 接收 / 呈现默认拆为 3 个 Story |
| 9 | 异常流 | 超时 / 失败 / 重试 / 网络异常 / 权限不足必须独立考虑 |
| 10 | 埋点关键路径 | 提交 / 支付 / 转化 / 分享 / AI 调用必须单独可追踪 |

禁止按前端 / 后端 / 数据库这种技术层横切拆 Story，除非该技术边界本身就是用户可感知或业务可验收的独立能力。

---

## §5 Feature Complexity Score（FCS）

在开始拆 Story 前，每个 Feature 必须先完成 FCS 评估。

### 5.1 计分规则

| 复杂度因子 | 得分 | 说明 |
|---|:---:|---|
| 跨系统集成（每个额外系统） | +2 | 如 Mini program + Speakup + IOC admin，每个系统 +2 |
| 异步流程 / AI 评分回调 / callback | +3 | 有异步链路、排队、结果回传 |
| 文件上传 / 音频录制 | +2 | 上传 + 进度 + 失败重试 |
| WeChat 登录 / unionId 绑定 | +2 | 鉴权 + 绑定 + 解绑场景 |
| UI 状态复杂（>3 种页面状态） | +2 | Loading / Empty / Error / Waiting / Result 等 |
| 数据建模复杂（ERD >3 实体） | +2 | 有复杂实体关系或状态字段 |
| 一次性提交 / 幂等性要求 | +1 | 限制重复提交，需要幂等保障 |
| 边界场景 >5 个 | +2 | 失败路径、网络异常、超时等 |
| 新页面数量（每增 1 页） | +1 | 超过 2 个新页面时开始计分 |
| 多渠道差异 | +2 | Mini program / website / 3Ups 等行为不同 |
| Touch points 数据埋点 | +1 | 需集成埋点 |
| CEFR 等级映射 / 结果页逻辑 | +1 | 结果展示逻辑复杂 |

### 5.2 触发规则

| FCS 得分 | 行动规则 |
|:---:|---|
| >10 | 必须调用 Story Splitter 拆分，禁止 Product Planner 直接手动拆 |
| 6–10 | 建议调用 Story Splitter，Product Planner 可自行判断 |
| <6 | Story Splitter 可选，Product Planner 可自行拆 |

### 5.3 输出格式

```text
Feature: [Feature 名称]
FCS 得分: XX 分
触发原因:
  - [因子名] → +X 分
  - [因子名] → +X 分
结论: 【必须拆分 / 建议拆分 / 可自行拆分】
```

---

## §6 Story 数量与粒度

| Feature 规模 | Story 数量 |
|---|:---:|
| 小型 Feature（FCS <6） | 3–4 个 |
| 中型 Feature（FCS 6–10） | 4–6 个 |
| 大型 Feature（FCS >10） | 6–10 个；超过 10 个必须回拆 Feature |

Story 粒度规则：

- 每个 Story 必须可独立开发、独立测试、独立验收。
- 单 Story 最终 Units 必须为 1 / 3 / 5 / 8 之一。
- 单 Story 最大 8 units（4 man-days）；超过 8 units 必须继续拆 Story。
- 禁止把整个 Feature 写成 1 个超大 Story。
- 禁止模糊标题，如"优化流程"、"处理逻辑"。

### 6.1 Story Estimation 映射（强制）

User Story 估算必须采用 **Story Points → Man-day → Units** 三层映射。

| Story Points | Man-day | Units |
|---:|---:|---:|
| 1 | 0.5 day | 1 unit |
| 3 | 1.5 days | 3 units |
| 5 | 2.5 days | 5 units |
| 8 | 4 days | 8 units |

规则：

- `1 unit = 0.5 man-day`。
- Story Points 与 Units 数值保持一致，用于 ADO/Jira 等工具映射。
- Agent 先根据工作量 Matrix 评估 Story Points，再映射到 Man-day 与 Units。
- 单个 Story 的最终 Units 只允许 `1 / 3 / 5 / 8`。
- 禁止单个 Story 使用 Units range（如 `2–4 units`）。
- 禁止继续使用 XS / S / M / L / XL 作为 Story-level estimation 口径。
- 评估超过 8 units 时，必须继续拆 Story，不允许输出 13 / 16 / range。
- 可以保留 Complexity / Confidence / Estimation Drivers 说明评估依据。

工作量 Matrix 至少考虑：

- 用户动作复杂度
- 业务规则 / 计算 / 校验复杂度
- 状态机复杂度
- 第三方 / AI / 异步 / 长耗时任务
- 权限角色与数据对象边界
- 异常流、重试、幂等和埋点要求

---

## §7 必须独立成 Story 的场景

以下场景出现时必须显式拆出独立 Story：

- WeChat 登录 / unionId 绑定
- 文件上传 / 音频录制
- AI 评分提交
- Async callback / 结果等待状态
- CEFR 结果展示
- 一次性提交限制 / 幂等防重
- Touch points 埋点（AC 必须含事件类型、btn_id、触发时机、KPI）
- 错误恢复 / 重试 / 超时处理
- 异步链路每个节点：触发 → 中间节点 → 终态逐步拆分
- 边界用户群：特殊身份、特殊操作路径、特殊数量状态
- 通知 / 推送 / 邮件发送：需字段级模板规范
- 数据同步：需同步时机、失败重试、告警或跳过策略

---

## §8 Story 输出契约

Story Splitter 输出的每个 Story 必须包含：

```markdown
---
Story EPIC-{slug}-F{feature_no}-S{story_no}: [Story 名称]

upstream_refs（如 Solution Brief 存在则必填）:
  persona: P1
  journey_stage: J2
  scenarios: [BP-H1, BP-U1]
  kpi_alignment: [K1]

User Story（英文）:
  As a [user role], I want to [action], so that [benefit].

AC 覆盖类型（强制声明）:
  □ 完整覆盖（默认 · 操作类 5 类 / 列表类 7 类）
  □ 降级覆盖（仅适用：只读 + 无状态变更 + 无三方 + 无敏感字段 → ≥3 条 AC）
  降级理由（如降级则必填）: ...

Acceptance Criteria（中文 — 严格按 ac-writing-spec SKILL 执行）:
  AC1：主流程（Happy Path）
  GIVEN ...
  WHEN ...
  THEN ...

  AC2：失败 / 异常路径
  GIVEN ...
  WHEN ...
  THEN ...

Estimation 参考（Planning Level）:
  Story Points: 1 / 3 / 5 / 8
  Man-day: 0.5 / 1.5 / 2.5 / 4
  Units: 1 / 3 / 5 / 8
  Complexity: Low / Medium / High
  Confidence: Low / Medium / High
  Estimation Drivers: ...

变更记录：
  - v1.0 ({YYYY-MM-DD})：初版
---
```

Story ID 规则：

- 格式：`EPIC-{slug}-F{N}-S{M}`
- Solution Brief §9 Story List 已分配 ID 时必须复用，禁止重新编号。
- ID 一经发布永不变更；删除走退役，编号不复用。

Story Splitter 还必须输出：

1. FCS 评估结果
2. Story List
3. Dependencies
4. Suggested Sequence
5. Product Planner 对齐说明
6. Missing Information
7. Rule Sedimentation 建议

---

## §9 AC 引用规则

写每个 Story 的 AC 前，必须加载 `skills/ac-writing-spec/SKILL.md` 并按场景引用：

| 场景 | 引用 |
|---|---|
| 所有 AC 格式 | `ac-writing-spec` §1 |
| 操作流程类 Story | `ac-writing-spec` §2.1 |
| 列表查询类 Story | `ac-writing-spec` §2.2 |
| 状态机完整覆盖 | `ac-writing-spec` §3.1 |
| 按钮置灰 | `ac-writing-spec` §3.2 |
| 表单校验 | `ac-writing-spec` §3.3 |
| 操作触发 / 幂等 / 第三方错误 / 筛选 / 字段展示 / Export | `ac-writing-spec` §4 |
| 信息缺口处理 | `ac-writing-spec` §5 |

AC 降级判定四要素必须全部满足才允许降级：只读 + 无状态变更 + 无三方调用 + 无敏感字段。任一不满足，必须完整覆盖。

---

## §10 质量检查

输出前必须逐项自检：

| 检查项 | 标准 |
|---|---|
| Feature Gate | §2.1 全部通过；不通过则拆 Feature 或列 PM 确认问题 |
| FCS | 每个 Feature 均已评分 |
| Story 数量 | 每 Feature 3–10 个 Story |
| Stable ID | Story ID 格式正确且复用上游 ID |
| upstream_refs | B 模式下 persona / journey_stage / scenarios 必填 |
| User Story | As a / I want / so that 完整 |
| Story 闭环 | 用户动作 → 系统处理 → 用户反馈清晰 |
| AC 覆盖类型 | 完整覆盖 / 降级覆盖声明明确 |
| AC 格式 | 多行 GWT、大写关键字、无箭头连写 |
| Story Estimation | Story Points / Units 只能为 1 / 3 / 5 / 8；Man-day 按 1 unit = 0.5 day 映射 |
| 角色与系统边界 | 单 Story 不跨多角色 / 多外部系统集成 |
| 特殊场景 | 上传 / 回调 / AI / 埋点 / 重试 / 通知 / 同步等已独立处理 |
| 依赖关系 | 无隐式依赖 |
| Rule Sedimentation | 新业务规则、状态值、字段校验、共享边界已建议沉淀 |

---

## §11 禁止事项

- 禁止 Feature 名称模糊，或只写内部技术名。
- 禁止跨系统大杂烩 Feature。
- 禁止 Feature 少于 3 个 Story 仍直接进入 PRD，除非 PM 明确批准并记录原因。
- 禁止忽略 FCS 直接拆 Story。
- 禁止 Story 只有 happy path。
- 禁止 Story 使用 XS / S / M / L / XL 或 Units range 作为最终估算。
- 禁止把实现方案写成 User Story。
- 禁止凭记忆写 AC。
- 禁止 Product Planner / Story Splitter 重复维护本文规则；变更必须回到本 SKILL。

---

## §12 版本记录

| 版本 | 日期 | 变更 |
|---|---|---|
| 1.1.0 | 2026-06-29 | 新增 Story Estimation 映射：Story Points → Man-day → Units，`1 unit = 0.5 man-day`，Story Units 只允许 1 / 3 / 5 / 8，禁止 Size 与 Units range；超过 8 units 必须继续拆 Story。 |
| 1.0.0 | 2026-06-29 | 初版。合并 rule.txt 的 Feature=系统能力 / US=用户行为规则，以及 story-splitter v2.2 的 FCS、Story 数量、特殊场景、输出契约和质量检查。 |
