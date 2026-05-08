---
name: market-research
description: 市场竞品调研规范——基于 Project Name 自动发现竞品 / 5 字段竞品速览 / PM 选定 1-2 家深度对标 / 调研结果落盘格式。Value Architect Mode 2 入口必须 Read 本文件。
version: 1.0.0
updated: 2026-05-08
maintainer: @frankzhey
applies-to: [value-architect]
---

# 市场竞品调研规范（Discovery 阶段通用）

本 SKILL 定义"基于 Project Name 自动发现竞品 + 浅扫 + 深度对标"的标准流程，由 Value Architect 在 Mode 2（AI 自动调研）下显式 Read 并执行。

---

## §1 输入要求

| 输入项 | 说明 | 是否必须 |
|---|---|---|
| Project Name | PM 拟做的项目名（kebab-case，如 `ielts-b2b`） + 一句话项目描述 | ✅ |
| 战略归属 | ops_excellence / test_delivery / ai_assessment | ✅ |
| 调研重点 | PM 关心的 1–3 个核心能力维度（如 "AI 评分" / "B 端管理后台" / "支付集成"） | ⭕ 推荐 |
| 已知竞品（可选） | PM 已了解的对标产品名或 URL | ⭕ |
| 地域偏好 | 国内 / 海外 / 全部 | ⭕ 默认全部 |

---

## §2 三步调研流程（强制执行顺序）

### Step 1：竞品发现（Pan Search）

基于 Project Name + 战略归属 + 调研重点，使用 web search 工具（`web/search` / `web/fetch`）执行三类搜索：

| 搜索维度 | 关键词构造 | 期望发现 |
|---|---|---|
| **直接竞品** | `{项目领域} + {核心能力} + {产品类型}` | 同领域同用户群同价值主张 |
| **间接竞品** | `{用户痛点} + 替代方案 / 同类工具` | 形态不同但解决同类问题 |
| **跨地域竞品** | 国内 + 海外混合搜索 | 不同市场成熟度的对标 |

输出：候选竞品列表（5–10 个），含名称 + URL + 一句话识别理由。

> 如 `web/search` 不可用 → 提示 PM 提供候选清单（PM 输入 5+ 个竞品 URL），跳过 Step 1 直接进入 Step 2。

### Step 2：5 字段竞品速览（每竞品一行）

对 Step 1 候选清单中每个竞品产出一行速览，落盘到 `Project/{project}/Research/competitor-shortlist.md`：

```markdown
# Competitor Shortlist — {project}

> Created: {YYYY-MM-DD-HHmm}
> Project: {project}
> Strategy: {pillar}
> Discovery Keywords: {keywords used in Step 1}

| # | 竞品名称 | 核心能力 | 主要优势 | 产品速览 | 整体评价 | URL |
|---|---|---|---|---|---|---|
| 1 | Competitor A | 1) ... 2) ... 3) ... | 1) ... 2) ... | [≤30 字定位] | [适合借鉴 / 不适合] | https://... |
| 2 | Competitor B | ... | ... | ... | ... | ... |
| 3 | ... | ... | ... | ... | ... | ... |
```

**字段强制要求：**

| 字段 | 内容标准 |
|---|---|
| **核心能力** | 3–5 项核心能力（动词短语，如"AI 口语自动评分" / "考前批量发码" / "数据看板回看"） |
| **主要优势** | 与同类相比的差异化点（≤3 条，强调差异化非通用能力） |
| **产品速览** | 一句话定位（≤30 字，包含目标用户 + 核心价值主张） |
| **整体评价** | 适合借鉴 / 部分借鉴 / 不适合（含一句话理由） |
| **URL** | 官网或主要产品页（可访问的最相关 URL） |

### Step 3：PM 选定 1-2 家深度对标

呈现 Step 2 表格给 PM，由 PM 选定 1–2 家进入深度对标。对每家选定竞品产出独立的 5 段式 Summary，落盘到 `Project/{project}/Research/competitor-{name}.md`：

#### Section 1：产品速览
- 产品名称 / 公司 / 一句话定位
- 核心用户群（具体到行业 / 职能 / 体量）
- 主要使用场景（≥3 个典型场景）
- 商业模式（订阅 / 流量 / 一次性付费 / 免费 等）

#### Section 2：核心能力
- 列出 5–10 个核心能力（按重要性排序）
- 每个能力配 1 句描述（避免空泛 - 必须可观察可验证）

格式：
```
1. 能力名称 — [一句具体描述，含触发条件 / 输入 / 输出]
2. ...
```

#### Section 3：优势定位
- 与同类产品差异化点（3–5 条）
- 每条注明：① 优势性质（技术 / 数据 / 生态 / 价格 / 体验）；② 难复制度（高 / 中 / 低）

#### Section 4：不足之处
- 已知痛点 / 用户反馈差评 / 缺失能力（3–5 条）
- 每条注明来源（用户评论 / 自身测试 / 行业分析）

#### Section 5：整体评价（最重要）
- **适合借鉴**：针对我方场景的具体设计 / 流程 / 字段（条目化）
- **不适合借鉴**：哪些做法在我方场景会失败（条目化 + 原因）
- **推荐截取的核心能力清单**：从 Section 2 中筛选出我方可复用的能力（≤5 项），为 Value Architect Gate 2 提供输入

---

## §3 落盘规范

```
Project/{project}/Research/
├── competitor-shortlist.md             # Step 2 浅扫结果（单文件 + 表格）
├── competitor-{name-1}.md              # Step 3 深度对标
└── competitor-{name-2}.md              # Step 3 深度对标
```

`{name}` 为 kebab-case 形式（如 `competitor-ielts-online.md`）。

---

## §4 数据来源与可验证性

调研内容必须满足：

- **可追溯**：每个 Section 的关键论断附引用源（URL / 报告名 / 测试日期）
- **可观察**：能力描述基于公开可验证的产品功能，禁止凭空推断
- **避免广告语**：不使用厂商营销话术，需要转译为客观描述
- **不确定标注**：信息不足时显式标 `[待确认]`，不强行编造

---

## §5 Gate 与 Handoff

### Gate（PM 确认）
- **Step 2 浅扫产出后**：PM 选 1–2 家深度对标 → 进入 Step 3
- **Step 3 深度对标产出后**：PM 确认"推荐截取核心能力清单"是否符合我方场景 → 进入 Value Architect Gate 2

### Handoff 到 Value Architect
深度对标的"推荐截取核心能力清单"作为 Value Architect Gate 2 的核心输入，PM 在 Gate 2 决定哪些能力进入"我方核心能力"。

---

## §6 调用方式（Copilot 环境）

```
Read skills/market-research/SKILL.md
```

加载位置：value-architect.agent.md 的 **Mode 2 入口**，在 Step 1 竞品发现之前必须先 Read 本文件。

---

## §7 强制规则

必须：
- Step 1 → Step 2 → Step 3 顺序执行，禁止跳过
- Step 2 五字段全部填写，不允许任一字段留空
- Step 3 五段式全部产出，不允许只写"整体评价"
- 关键论断必须附引用源
- 信息不足显式标 `[待确认]`

禁止：
- 凭空编造竞品（必须 web search 或 PM 提供）
- 用厂商营销话术替代客观描述
- 跳过 PM 选定环节，agent 自行决定深度对标对象
- 不落盘只在对话中输出

---

## §8 版本变更

| 版本 | 日期 | 变更 |
|------|------|------|
| 1.0.0 | 2026-05-08 | 初版。三步调研流程（Pan Search → 5 字段速览 → 1-2 家深度对标）+ 5 段式深度对标格式 + 落盘规范 + 引用强制要求。 |
