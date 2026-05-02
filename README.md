# V2 修改预览（Review 用）

> 创建时间：2026-04-28
> 目的：把 AC 规则抽象到 SKILL + 清理 instructions 与 agent 重复内容
> 状态：**预览版，未应用到 live 文件**。Review 通过后才覆盖到 `PM agents/` 根目录。

---

## 一、文件清单

| V2 文件 | 对应原文件 | 行数变化 | 类型 |
|---------|----------|---------|------|
| `skills/ac-writing-spec/SKILL.md` | （新建） | +290 行 | 🆕 NEW |
| `instructions/product.instructions.md` | `../instructions/product.instructions.md`（303 行） | ~110 行（-193） | ✂️ 瘦身 |
| `product-planner.agent.md` | `../product-planner.agent.md`（v2.3.0，1004 行） | ~530 行（-474） | ✂️ 瘦身 |
| `story-splitter.agent.md` | `../story-splitter.agent.md`（v2.0.0，440 行） | ~290 行（-150）| ✂️ 瘦身 + 修复 |
| `eng-reviewer.agent.md` | `../eng-reviewer.agent.md`（v2.0.0，414 行） | ~430 行（+16） | 📈 增强 |

**总变化**：净减 **~711 行**，重复内容降为单一来源。

---

## 二、本次假设的 5 个决策默认值

由于你还没确认决策点，V2 文件采用了我推荐的默认值。如需调整请告诉我，我修改对应文件即可。

| # | 决策 | 默认值 | 影响文件 |
|---|------|--------|---------|
| 1 | 编号体系 | **A/B/C 跨章节锚点 + ①②③④⑤ 章节内序号** 混合 | SKILL.md §2 表格 |
| 2 | instructions 保留范围 | 按方案 3.2 列表（保留文件级 contract，删 agent 行为细节） | product.instructions.md 全文 |
| 3 | 依赖加载位置 | **Step 0「依赖加载」前置**（确定性更高） | product-planner.agent.md「在执行任何任务前」第 3 条 |
| 4 | eng-reviewer AC 合规校验 | **加入 Section 17.0 强制阻塞性校验** | eng-reviewer.agent.md Section 17.0 |
| 5 | 版本号策略 | **3 个 agent 一起升 minor**（v2.4.0 / v2.1.0 / v2.1.0），SKILL 起始 v1.0.0 | 各 agent frontmatter |

---

## 三、关键改动 highlights

### 🎯 改动 1：AC 规则单一来源

**Before**：3 个 agent 各自维护 AC 规则，story-splitter 还在用旧的连写格式。

**After**：
- `skills/ac-writing-spec/SKILL.md` 是唯一权威（290 行）
- product-planner / story-splitter / eng-reviewer 都通过 `Read` 引用同一份
- story-splitter B-3 旧格式（`WHEN A 或 B → THEN C` / `AND WHEN`）已彻底修复为多行新格式

### 🎯 改动 2：instructions 与 agent 重复消除

**Before**：`product.instructions.md` 的 PRD 结构、AC 规范、禁止事项与 `product-planner.agent.md` 大量重复（约 200 行重复内容）。

**After**：
- instructions 只保留**文件级 contract**（什么是合格的 PRD 文档）
- agent 只保留**行为流程**（Mode A/B/C / Step 0-11 / Quality Gate / Rule Sedimentation）
- AC 详情完全移到 SKILL
- instructions 末尾新增 §8「不在本文件范围」明确指向其他文件

### 🎯 改动 3：编号体系统一

**Before**：
- product-planner 用 `①②③④⑤`（仅章节内可读）
- story-splitter 用 `A-1 / A-2 / B-3 / C-1`（跨章节追溯）
- 同一规则两套编号，沟通混乱

**After**：
- A/B/C 编号作为跨章节追溯锚点（A 类操作覆盖 / B 类字段状态 / C 类业务约定）
- ①②③④⑤ 仅作为同一章节内的可读性序号
- 两者并存互不冲突

### 🎯 改动 4：eng-reviewer 升级为 AC 质量哑评审 → 强校验

**Before**：eng-reviewer 没有 AC 标准，会被动接收不合规 AC 并继续工程评审。

**After**：
- 新增 Section 17.0 AC 合规校验（强制阻塞性）
- 检查格式 / 覆盖 / 强制规则三大维度
- 不合规必须 flag 到 Section 15 Risks，建议 PM 让 Product Planner 重跑 Quality Gate

### 🎯 改动 5：Copilot 加载链路明确化

由于 Copilot 不支持 SKILL description 自动触发，三个 agent 都在「执行前规则」加了**强制 Read 语句**：

```
强制依赖加载（不可跳过）：必须先 Read：
- PM agents/skills/ac-writing-spec/SKILL.md
- ...
```

---

## 四、Diff 摘要（按文件）

### 📄 product-planner.agent.md（v2.3.0 → v2.4.0）

**删除（约 470 行）**：
- AC 格式强制规范（约 50 行）→ 移到 SKILL §1
- AC 覆盖规范（操作类 5 类 + 列表类 7 类，约 80 行）→ 移到 SKILL §2
- AC 额外强制规则（状态机 / 按钮置灰 / 表单校验，约 60 行）→ 移到 SKILL §3
- AC 自主补全分级（约 100 行）→ 移到 SKILL §5
- 与 instructions 重复的 PRD 结构详细描述（约 100 行）→ instructions 已有
- 通用禁止事项重复部分（约 30 行）→ instructions 已有
- 估算映射表的冗余说明（约 50 行）→ 保留表格，删重复说明

**新增（约 30 行）**：
- 「在执行任何任务前」第 3 条：强制依赖加载
- Step 6 引用 SKILL §1-§5
- Step 10.5 Quality Gate 引用 SKILL §1
- 输出结构 §7 末尾 SKILL 引用
- 强制规则末尾 instructions 引用

### 📄 story-splitter.agent.md（v2.0.0 → v2.1.0）

**删除（约 160 行）**：
- AC 分类写法强制规范（A 类 + B 类约定，约 70 行）→ 移到 SKILL §2
- AC 写法模板（约 60 行）→ 移到 SKILL §4
- 状态机强制覆盖规则 C-1（约 10 行）→ 移到 SKILL §3.1
- **按钮置灰规则强制覆盖 B-3 旧版连写格式**（约 15 行）→ **修复后**移到 SKILL §3.2 多行新格式

**新增（约 10 行）**：
- 「执行前规则」第 4 条：强制依赖加载
- 「AC 写作规范引用」表格指向 SKILL §1-§5
- 质量检查清单引用 SKILL 章节锚点
- 禁止事项加 3 条新限制
- 版本变更记录

### 📄 eng-reviewer.agent.md（v2.0.0 → v2.1.0）

**新增（约 16 行净增）**：
- 「执行前规则」第 7 条：强制 Read SKILL
- 工作方式新增 Step 8 AC 合规校验
- **Section 17.0 AC 合规校验**（强制阻塞性，约 80 行）
- 强制规则与禁止事项各加 2 条
- 版本变更记录

### 📄 instructions/product.instructions.md（瘦身）

**删除（约 220 行）**：
- BCChina 详细系统语境
- PRD 章节展开内容（agent 自带）
- 详细 AC 规范（移到 SKILL）
- Copilot 输出要求
- 默认输出风格

**保留**（重写为简洁版）：
- 输出语言规则
- Epic / Feature / Story 层级
- AC 基础要求 + SKILL 引用
- PRD 必含章节骨架（仅标题）
- 总体产品原则
- 通用禁止事项
- 「不在本文件范围」对照表

---

## 五、Review 步骤建议

1. **先读 SKILL.md**（这是新增最重要的文件）
2. **再读 product.instructions.md**（确认瘦身后是否还能 cover 文件级要求）
3. **diff product-planner.agent.md** 与原文件，确认删除部分是否都已移到 SKILL，新增的 reference 是否到位
4. **diff story-splitter.agent.md** 与原文件，特别注意 B-3 修复
5. **diff eng-reviewer.agent.md** 与原文件，主要看 Section 17.0 的合理性

---

## 六、应用到 Live 文件的步骤

Review 通过后告诉我，我会执行：

```
1. cp V2/skills/ac-writing-spec/SKILL.md → PM agents/skills/ac-writing-spec/SKILL.md
2. cp V2/instructions/product.instructions.md → PM agents/instructions/product.instructions.md
3. cp V2/product-planner.agent.md → PM agents/product-planner.agent.md
4. cp V2/story-splitter.agent.md → PM agents/story-splitter.agent.md
5. cp V2/eng-reviewer.agent.md → PM agents/eng-reviewer.agent.md
6. （可选）archive V2/ folder 或保留作为版本快照
```

---

## 七、需要你确认的事项

请逐项 review 后告诉我：

- [ ] SKILL.md 内容是否完整？编号体系是否接受？
- [ ] product.instructions.md 瘦身是否过度？还有哪些必须留？
- [ ] product-planner.agent.md 删除项是否都接受？还有哪些不该删？
- [ ] story-splitter.agent.md B-3 修复是否符合预期？
- [ ] eng-reviewer.agent.md Section 17.0 AC 合规校验是否要这么严格（阻塞性）？还是仅 warning？
- [ ] 5 个决策默认值是否需要调整？

确认后我执行最终覆盖。
