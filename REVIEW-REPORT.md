# productagentsV3 机制 Review Report

> 范围：12 个 Agent + 12 个 SKILL + 3 个 instructions + README/copilot-instructions，对照你给的 PM 工作流（6 阶段）与 6 条原则，评估**效率**与**产出质量**。
> 日期：2026-05-30

---

## 一、总评（一句话）

**底座架构是对的，PRD 技术骨干质量很高，但"Agent 只编排、质量定义全在 SKILL"这条核心原则在几个承重点上被破坏了，加上若干 tool 配置已过期会导致运行时报错。** 当前系统能产出高质量 PRD，但**可维护性、一致性、防跳过（防漏 Gate）三个维度有明显欠债**。

### 评分卡

| 维度 | 评分 | 说明 |
|---|---|---|
| 架构分层（instructions/SKILL/agent 三层） | ★★★★★ | 概念清晰，落盘+LATEST 指针+upstream_snapshot 继承设计优秀 |
| PRD 技术骨干质量（value→solution→nfr→arch→ac→eng-review） | ★★★★★ | ID 可追溯、AC 强制 G/W/T、DoD 阻塞式，QA 与 AI coding 可直接消费 |
| 原则 1：Agent 单一职责，质量定义全在 SKILL | ★★☆☆☆ | **多处破坏**：PRD 模板、Story 拆分、NFR 档位库、出图管线都内嵌在 Agent |
| 原则 2：继承链完整 | ★★★☆☆ | value→solution→PRD 链干净；但"Eng 拿到完整 PRD"已被 v4.8 改成"PRD 只引用、合并发生在发布态"，原文已不成立 |
| 原则 3：WIKI 为最终落地 + Panel 审批 | ★★☆☆☆ | Wiki 路径规范强，但"必须发布到 Wiki"与"Panel 审批"**都是软提示、非阻塞** |
| 原则 4：planner 粗估 / eng-reviewer 细估 | ★★★★★ | 完全落实 |
| 原则 5：MP/Figma 贯穿 solution→planner | ★★★☆☆ | 流程上贯穿，但抽取逻辑在两个 Agent 重复、且 MP 读取工具未授予 |
| 原则 6：四个 Agent 支持 refinement | ★★★★★ | 全部落实（ux-prototyper / task-planner 例外，无 refinement） |
| 运行时健壮性（tool 配置） | ★★☆☆☆ | **多个 Agent 的 tools 列表仍指向已被合并删除的旧 Wiki/WIT 工具** |

---

## 二、原则逐条合规

**原则 1 — Agent 单一职责，"什么是合格输出"全在 SKILL：多处违反。**
- `product-planner.agent.md`（50KB）≈40% 是 PRD 输出模板（§1–§X 表结构、AC 分级降级表、Story Size 映射表），**没有 `prd-spec` SKILL 承接** —— 这是最严重的一处，PRD 整体 schema 实际住在 Agent 里。
- `nfr-architect.agent.md` Step 2.A 把"8 类 × 3 档"的**整张 NFR 基线档位库内联成 ASCII 表**，而它自己声明"档位库由 SKILL 提供"——自相矛盾。
- `wiki-publisher.agent.md`（36KB）内嵌整套 PNG 发布管线伪代码 + 渲染排错知识；一天内改了 6 个版本（3.3.0→3.3.6）全是渲染 bug，证明这套逻辑不该住在编排器里。
- `story-splitter` 的 FCS 计分表、`it-architect` 的 SVG/PNG 渲染约束、`task-planner` / `ux-prototyper` 的整套输出 schema 都内嵌在 Agent，且**无对应 SKILL**。
- 反面样板（做对了的）：`work-item-publisher.agent.md` 反复写"具体规则见 SKILL，本 agent 不重复展开"——**这才是目标形态，其余 Agent 应照此重构。**

**原则 2 — 继承链：value→solution→PRD 干净，Eng 端已偏离原文。**
- value→solution（强校验 Selected Epic ∈ Value Roadmap）、value+solution→PRD（自动抽取 Solution §1/§2/§5/§6/§9 + Value KPI）都正确落实。
- 但 v4.8 把 Solution/NFR/Architecture 改成**三份独立产出**，PRD 只"引用" LATEST，Eng Reviewer 要自己 wiki-pull 5 份上游。"Eng 拿到完整 PRD"只在**发布态合并页**短暂成立，PRD 源文件本身不再自包含。原则文字需要更新，或恢复 PRD 自包含。

**原则 3 — WIKI 落地 + Panel 审批：规范在、强制缺。**
- Wiki 路径映射（v3.2 13 行 PATH_MAP，`-solution`/`-PRD` 后缀强制）很强。
- 但**没有任何 Gate 检查"最终版是否已发布到 Wiki"**，发布永远是可选 handoff；PM 可以 `status: approved` 后直接发 Boards，PRD 从未上 Wiki。
- **Panel 审批未被强制**：`panel_approved` 只是 frontmatter 枚举值，Solution Architect 明确"status ≠ panel_approved 时只警示、不阻塞"。工作流第 3 步"→ Panel 审批"实为软警告。

**原则 4 — 粗估/细估：完全落实。** product-planner §8 明确"planning-level，不代表研发承诺，最终以 Eng Reviewer/Task Planner 为准"。

**原则 5 — MP/Figma 贯穿：流程在，配置有坑。**
- Solution Architect 与 Product Planner 两阶段都读 MP/Figma，流程贯穿正确。
- 但抽取维度表在**两个 Agent 重复**（改一次要改两处）；且两个 Agent 的 `tools:` 里只有 `figma/*`，**没有 Magic Patterns 的 `read_files` 工具**，而正文却调用它——MP 能力"描述了但没接线"。

**原则 6 — 四角色 refinement：落实。** value/solution/product-planner/eng-reviewer 都有 Refinement 模式 + LATEST 检测 + 三段 diff。ux-prototyper、task-planner 缺 refinement，与其它 Agent 不一致。

---

## 三、效率问题（会拖慢 PM / 制造返工）

1. **每个 Agent 都重新追问 project name。** `project-context-loader` 强制每个非 Value Agent 重新确认项目名并"禁止信任 handoff 传入的 project+epic"。单项目线性会话里会出现 6+ 次冗余确认，而 handoff prompt 其实已带 `Project=`/`Epic=`。
2. **跨机 wiki-pull 全靠 PM 手动编排。** 三份独立产出 + 仅靠 Wiki 共享 = PM 要手动按序触发 publish→NFR→publish→IT→publish→planner pull，每步还有软 Gate 三选一，无自动编排。
3. **v4.8 软 Gate 默认"无架构/NFR 也继续出 PRD"**（默认 Option B，用 `[pending]` 占位）。更快，但**保证返工**：§5 trace、§X 覆盖矩阵被占位符填满，需后补，而 Eng Reviewer 只警示不阻塞。
4. **PNG/SVG 渲染链脆弱。** 当前 Level-1 需独立 Chrome headless + 固定尺寸 HTML wrapper 导出 PNG，经常退化到"⚠️ Missing PNG"。
5. **每个产物单独手动发布**，无批量 publish。
6. **版本号散乱。** README 标 v3.8 却引用 v4.8/v4.1，Stage 3 又写 v4.7，Handoff 表写"Eng Reviewer v4.0"。读起来要猜哪个是现行行为。

---

## 四、产出质量问题

1. **两个质量锚点都可被绕过**：Panel 审批未强制、Wiki 发布未强制 → PRD 可在 Value 仍是 draft、且从未上 Wiki 的情况下进 Boards。
2. **过期 tool 配置会运行时报错**（高危）：
   - `wiki-publisher` 的 tools 仍授予已被合并删除的 `wiki_create_or_update_page`/`wiki_get_page`/`wit_*`，正文却用新的 `wiki_upsert_page`——主写路径与附件上传错配。
   - `nfr-architect`/`it-architect` 仍用旧 `wiki_create_or_update_page`/`wiki_get_page_content` 做 wiki-pull。
   - solution/planner 引用 `read_files(editor_id)` 但未授予该工具。
3. **占位符可通过自检**：§X 覆盖矩阵 Architecture/NFR 列全 `[pending]` 仍能过 Quality Gate 并发布，三向追溯形同虚设。
4. **AC SKILL 章节引用不一致**：`product.instructions` 写"§1-§3"、`product-planner` 写"§1-§5"、AC 合规检查写"§1-§3.5"——到底哪段权威说不清。
5. **engineering.instructions 与 eng-reviewer v4.1 角色冲突**：instructions 仍要求"先有高层架构图/ERD/API"，而 eng-reviewer v4.1 是"纯评审"且被明令禁止产出 C2/C3/ERD/API。Agent 加载的文件级契约要求它产出被禁止产出的东西。
6. **两个前端 SKILL（claud-frontend / premium-frontend-UI）功能重叠**，且都未接入 PRD/UX 交付链，是游离件；真正缺的是 UX-Prototype 交付规范。
7. **market-research 偏窄**：实为"竞品 URL 调研"，不含市场容量/趋势/分群，与工作流第 2 步"教育科技市场竞品调研报告"的期望有差距。

---

## 五、缺失的 SKILL（直接导致原则 1 被破坏）

| 缺失 SKILL | 当前住在哪 | 影响 |
|---|---|---|
| **`prd-assembly-spec`**（PRD 整体模板 + Epic→Feature→Story 结构 + 覆盖矩阵 + PM-Confirm Gate） | product-planner.agent.md | 最高优先级；PRD schema 住在 Agent |
| **`story-splitting-spec`**（FCS 计分 + INVEST + 垂直切片 + Story 数量规则） | story-splitter.agent.md + product-planner | 拆分质量无 SKILL 治理 |
| **`estimation-spec`**（T-shirt→unit 映射 + Story 估算 + Capacity） | 散在 solution-design §6 / product-planner / task-planner 三处 | 同一标准三份拷贝 |
| **`ux-prototype-spec`**（页面清单 + 组件状态 + AC↔屏映射） | ux-prototyper.agent.md（内联） | 无"合格 UX 原型"定义 |
| **`wiki-publish-spec` / `diagram-publish-spec`**（发布规则 + PNG 管线） | wiki-publisher.agent.md（36KB） | 多个 SKILL 依赖其契约却无 SKILL |

---

## 六、优先级建议（Roadmap）

**P0 — 止血（运行时 + 防跳过，1–2 天）**
1. 修正 `wiki-publisher`/`nfr-architect`/`it-architect` 的 tools 列表，对齐现行 `wiki_upsert_page` 等工具；给 solution/planner 补 Magic Patterns 读取工具。
2. 把"Wiki 发布"做成 PRD `approved` 前的**阻塞 Gate**；Panel 审批同样升级为阻塞（或显式确认放弃）。
3. 改 v4.8 默认：缺 Architecture/NFR 时默认**不**出占位 PRD，或占位 PRD 不允许过 Quality Gate / 不允许发布。

**P1 — 落实原则 1（抽 SKILL，1–2 周）**
4. 新建 `prd-assembly-spec`，把 product-planner 的模板/AC 分级/Size 映射搬过去，Agent 瘦身成纯编排（对标 work-item-publisher）。
5. 把 NFR 档位库从 nfr-architect 搬回 `nfr-spec`；把出图管线从 wiki-publisher/it-architect 搬到 `diagram-publish-spec`。
6. 新建 `story-splitting-spec`、`estimation-spec`、`ux-prototype-spec`；estimation 三处去重为一份。

**P2 — 效率与一致性（持续）**
7. 让 `project-context-loader` 在单会话内信任 handoff 的 project+epic，去掉重复确认。
8. 统一版本号叙事；统一 AC SKILL 章节引用；更新 engineering.instructions 以匹配 eng-reviewer v4.1 纯评审角色。
9. 合并两个前端 SKILL；扩充 market-research 增加市场容量/趋势/分群可选模块。
10. 给 ux-prototyper、task-planner 补 Refinement 模式。

---

## 七、值得肯定的设计（不要改）

- 三层架构 + LATEST 指针 + upstream_snapshot 继承。
- ID 体系（F/P/J/BP/EXP/ITQ/K/E/OQ）跨文件单向可追溯。
- `ac-writing-spec` 强制多行 GIVEN/WHEN/THEN + 8 维场景覆盖 ≥6 + 自动补全分级（🟢直填/🟡假设/🔴必问）——这是 QA 出 test case、研发 AI coding 的关键基石。
- value-frame 的 Epic 粒度数学（KPI 子集约束、重叠 <50%、依赖单向）可机器校验。
- solution/nfr/it-architect 的"去技术化"边界切分干净（业务 EXP / 仅目标 / 仅架构），三者无重叠。
- `work-item-publisher` 的"规则见 SKILL，不重复展开"是全系统应统一的范式。
