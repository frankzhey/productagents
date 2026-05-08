---
project: spk2challenge-miniprogram
created: 2026-05-08-0000
maintainer: "@frankzhey"
mode: 2
strategy_pillar: ai_assessment
upstream_snapshot: 无（Value 是项目入口）
status: draft
gate_log:
  - gate: 1
    status: passed
    confirmed_at: 2026-05-08
  - gate: 2
    status: passed
    confirmed_at: 2026-05-08
  - gate: 3
    status: passed
    confirmed_at: 2026-05-08
skills_loaded:
  - skills/market-research/SKILL.md
  - skills/value-frame/SKILL.md
---

## §0 调研 Summary 索引

- [competitor-shortlist.md](../Research/competitor-shortlist.md)
- [competitor-tangzhaoxue.md](../Research/competitor-tangzhaoxue.md)
- [competitor-prepex-ielts.md](../Research/competitor-prepex-ielts.md)

## §1 Brief

- **背景**：现有 TOC 小程序已经具备用户触达基础，但缺少一个足够轻、足够高频、适合移动端碎片化使用的口语练习入口。对于雅思备考用户来说，口语练习既需要低门槛启动，也需要即时反馈才能形成持续参与。与此同时，团队希望通过真实用户作答持续沉淀口语语料，增强 AI 评分能力，并把小程序作为前置流量入口，引导用户进入 website 使用更完整的 AI 测评和提分工具，最终形成从练习、评测到付费转化的闭环。
- **当前问题**：
  - 现有 TOC 触点缺少一个专注口语、适合移动端高频使用的练习场景，用户难以形成稳定使用习惯。
  - 口语练习如果没有即时评分和提分建议，用户很难感知价值，完成率和复练率都会偏低。
  - 小程序和 website 之间的产品分工还不够清晰，缺少一个明确的导流链路去承接更高价值的 AI 工具和付费转化。
  - 当前如果没有持续真实作答输入，AI 评分能力和语料资产都难以稳定迭代。
  - 用户在备考过程中通常需要 band score 解释和能力映射，但若评分结果不对齐 IELTS band 与 CEFR，对结果的理解和信任会打折。
- **为什么现在做**：公司当前重点正从 TOB 向 TOC 和 mobile operations 转移，小程序是重要渠道，适合承接高频、轻量、即时反馈的使用场景。口语挑战练习天然适合移动端，能够在更短周期内验证用户参与、留存和导流效果。与此同时，ai_assessment 是当前战略归属，本期如果尽早建立“移动端口语作答 -> AI 评分 -> website 深度承接”的闭环，就能更快验证真实用户需求、积累训练数据，并为后续 website 的 AI 工具集合和商业化提供稳定入口。
- **目标用户**：
  - 备考雅思的 TOC 考生，尤其是希望先用轻量方式开始练习的用户。
  - 偏移动端、碎片化练习的年轻用户，希望随时做一题、立刻拿到反馈。
  - 对 AI 评分、提分建议和结构化学习工具有兴趣，并可能进一步购买完整测评服务的用户。
- **业务价值**：
  - 提升小程序内口语练习参与率、复练率和留存，为 TOC 渠道建立稳定活跃入口。
  - 持续沉淀真实口语作答数据，支持 AI 评分能力和反馈质量迭代。
  - 为 website 导入高意向用户，提升题库、AI 工具和提分服务的付费转化。
  - 通过 IELTS band 与 CEFR 对齐结果展示，增强评分结果可理解性和用户信任度。

## §2 Value Hypothesis

**假设 H1**：如果我们在小程序里提供“简化口语题 + 一键录音作答 + 即时 AI 评分”，则用户会更愿意开始第一次口语练习并完成提交，因为移动端进入成本更低，且评分反馈能立即让用户感知练习价值。

- **用户价值**：用户可以在较短时间内完成一次真实口语作答，并快速知道自己大致处于什么水平。
- **业务价值**：提升口语作答启动率和提交完成率，支撑 K1、K2。
- **验证信号**：首次进入口语挑战页到成功提交语音的转化率达到目标；新用户首次作答完成率稳定提升。

**假设 H2**：如果我们将评分结果对齐 IELTS band，并补充 CEFR 对比和提分建议，则用户会更信任 AI 评分结果并更愿意继续复练，因为结果更容易理解，也更能和他们熟悉的考试目标建立映射。

- **用户价值**：用户不仅拿到分数，还能知道自己的水平对应什么能力区间，以及下一步该怎么提分。
- **业务价值**：提升复练率、结果页停留时长和 website 导流点击率，支撑 K1、K3。
- **验证信号**：查看评分结果后二次作答率提升；结果页到 website 的点击转化率达到目标。

**假设 H3**：如果我们把小程序设计成“挑战练习”而不是单纯工具页，则用户 retention 会更高，因为挑战任务、连续练习和轻量目标感更适合移动端高频使用。

- **用户价值**：用户更容易坚持练习，不会因为缺乏目标感而中断备考。
- **业务价值**：提升 7 日留存和周活跃作答人数，支撑 K1、K4。
- **验证信号**：参与挑战的用户相较非挑战用户，7 日留存和周作答次数更高。

**假设 H4**：如果我们把小程序定位为轻量练习入口，并把更全题库、更多 AI 能力、解题锦囊和提分建议承接到 website，则能够提升高意向用户的付费转化，因为用户会先在小程序获得低门槛体验，再进入 website 获取更完整的价值。

- **用户价值**：用户先低成本验证产品有效性，再按需要进入更完整、更专业的备考体系。
- **业务价值**：提高小程序到 website 的导流效率和付费转化率，支撑 K3、K5。
- **验证信号**：完成口语评分后的 website 导流率和导流后付费转化率达到目标。

## §3 KPI Tree

| Type | KPI ID | Name | Target | Definition / 来源 |
|------|--------|------|--------|-------------------|
| North Star | K1 | 周口语有效作答人数 | >= 3,000 / week | 定义：每周在小程序内完成至少 1 次有效口语提交并成功拿到评分结果的去重用户数。来源：小程序作答提交日志 + 评分成功日志。 |
| Leading | K2 | 首次作答完成率 | >= 45% | 定义：进入口语挑战页的新用户中，最终完成录音提交并成功获得评分结果的比例。来源：页面访问埋点 + 录音提交成功事件 + 评分返回事件。 |
| Leading | K3 | 结果页到 website 导流点击率 | >= 18% | 定义：查看口语评分结果页的用户中，点击进入 website AI 测评工具集合入口的比例。来源：结果页曝光埋点 + 跳转点击埋点。 |
| Leading | K4 | 7日复练率 | >= 30% | 定义：首次完成口语作答后的用户，在 7 天内再次完成至少 1 次有效口语提交的比例。来源：用户作答行为日志。 |
| Leading | K5 | website 导流后付费转化率 | >= 6% | 定义：从小程序结果页进入 website 的用户中，在 14 天内完成指定 AI 测评产品购买的比例。来源：小程序导流标记 + website 用户归因 + 订单数据。 |
| Guardrail | K6 | 评分结果成功返回率 | >= 97% | 定义：成功提交录音后，系统在目标时限内返回可展示评分结果的比例。来源：录音提交日志 + 评分服务返回日志。 |
| Guardrail | K7 | 评分结果平均返回时长 | <= 20s | 定义：从用户提交口语录音到结果页拿到完整分数与建议的平均耗时。来源：提交时间戳 + 结果返回时间戳。 |
| Guardrail | K8 | 用户投诉率 | <= 1.5% | 定义：每 100 次有效口语作答中，因评分错误、结果不可理解、页面异常等原因产生客服投诉或差评反馈的比例。来源：客服工单 + 反馈表单 + 应用内投诉记录。 |
| Guardrail | K9 | 隐私与录音数据事故数 | = 0 | 定义：任何与用户录音、个人信息、未授权数据使用相关的安全或合规事故数量。来源：安全审计、客服升级记录、法务/运维事故记录。 |

## §4 Roadmap with Phases

| Phase ID | Name | Timeline | 涵盖 Epic（标题 + 一句话） | KPI 对齐 |
|----------|------|----------|---------------------------|----------|
| MVP | mobile-speaking-challenge | 2026 Q2-Q3 | E1 speaking-challenge-and-scoring：小程序口语作答、AI 评分、IELTS band/CEFR 解释与 website 导流一体交付（v2 迭代吞并原 E3 提分建议与原 E9 评分可信度） | K1, K2, K3, K6, K7, K8, K9 |
| Phase 2 | retention-and-conversion | 2026 Q3-Q4 | E2 challenge-retention-loop：增强连续挑战、任务和复练留存机制，并含最小口语作答运营看板 / E3 website-conversion-path：强化题库、AI 能力、锦囊内容与付费承接 | K1, K3, K4, K5, K8 |
| Future | expanded-ai-assessment-hub | 2027+ | E4 multi-skill-assessment-entry：从口语扩展到更多测评入口 / E5 personalized-study-routing：基于作答结果推荐个性化学习路径（与 E4 合并可能性见 OQ8） / E6 cross-platform-learning-account：打通小程序与 website 的学习身份与进度 | K1, K4, K5 |

### Epic List

| EPIC ID | EPIC name | value_statement | KPI 对齐 | Phase |
|---------|-----------|-----------------|----------|-------|
| E1 | speaking-challenge-and-scoring | 用户能在小程序内完成一次口语作答并立即拿到对齐 IELTS band 与 CEFR、含可信度说明与提分建议的评分，并被引导到 website 进一步提分 | K1, K2, K3, K6, K7, K8, K9 | MVP |
| E2 | challenge-retention-loop | 用户能因连续挑战、任务与回访提醒持续复练，从而提升 7 日留存 | K1, K4, K8 | Phase 2 |
| E3 | website-conversion-path | 用户能在 website 上使用更全题库、AI 能力与锦囊并完成付费转化 | K3, K5 | Phase 2 |
| E4 | multi-skill-assessment-entry | 用户能在同一入口下使用多科目 AI 测评，不限于口语 | K1, K5 | Future |
| E5 | personalized-study-routing | 用户能基于多科目评分结果拿到个性化推荐路径，从而减少盲练 | K4, K5 | Future |
| E6 | cross-platform-learning-account | 用户在小程序与 website 有统一学习身份与进度，从而跨端运用不脱节 | K4, K5 | Future |

## §5 Open Questions

| OQ ID | Question | Status | Owner | 备注 |
|-------|----------|--------|-------|------|
| OQ1 | 小程序首期的挑战机制最小版本是什么：单题挑战、每日挑战、连续打卡，还是榜单竞赛 | open | PM | 该决策会直接影响首页结构、任务设计和留存策略 |
| OQ2 | 评分结果是否直接展示完整 IELTS band descriptor 解释，还是先展示简化版结论再展开详情 | open | PM | 需平衡结果可理解性与页面复杂度 |
| OQ3 | IELTS band 与 CEFR 对照表采用固定映射还是内部解释版映射 | open | PM + Eng | 需要确认口径来源与展示风险，避免误导用户 |
| OQ4 | website 承接页的首期目标是题库浏览、AI 工具试用，还是直接会员/产品购买转化 | open | PM | 决定导流入口文案、落地页结构和转化埋点 |
| OQ5 | 语音数据的保存周期、授权提示和可复用范围如何定义 | open | PM + Eng | 涉及录音数据合规、训练使用边界和用户授权文案 |
| OQ6 | 小程序评分返回的目标时延能否稳定控制在 20 秒内 | open | Eng | 影响用户体验与 Guardrail KPI，可在方案阶段评估 |
| OQ7 | 首期是否只覆盖指定简化题库，还是同时支持自由题目扩展 | open | PM | 影响题库运营方式、评分稳定性和内容生产成本 |
| OQ8 | E5 personalized-study-routing 是否与 E4 multi-skill-assessment-entry 同期上线；如同期且 E4 必为 E5 提供评分数据源，则应合并为一个 Future Epic | open | PM | 来自 Epic 合并自检（SKILL §5.4），Future 阶段规划时重新评估 |

## §6 已沉淀规则索引

- Strategy pillar 固定为 ai_assessment。
- TOC 小程序定位为轻量口语挑战与评分入口，不扩展为重课程销售阵地。
- website 承接更全题库、更多 AI 能力、解题锦囊、提分建议和付费转化。
- 评分结果需对齐 IELTS band，并提供 IELTS band 与 CEFR 对照解释。
- 首期范围优先口语，不在 Value 阶段扩展到四科重建设。

## §7 变更记录（changelog）

- 2026-05-08-0000：首版 Value Frame 创建，完成 Mode 2 竞品调研、Gate 1-3 确认，并沉淀小程序与 website 分工边界。
- 2026-05-08-0200：按 SKILL v1.1.0 Epic 颗粒度规则 refine §4。原 E1–E4（miniapp-speaking-challenge / miniapp-voice-scoring / ielts-band-cefr-mapping / website-handoff-entry）合并为新 E1 `speaking-challenge-and-scoring`，作为一个可独立交付的价值单元。原 E5–E12 顺序重排为 E2–E9。Epic List 表新增 value_statement / KPI 对齐 / Phase 三列，满足 SKILL §5.3 强制结构。
- 2026-05-08-0300：按 SKILL §5.4 Epic 合并自检 review 全部 9 个 Epic，进一步顶件合并：原 E3 speaking-feedback-upgrade 与原 E9 scoring-trust-and-quality 合并进 E1（v2 迭代），E1 KPI 同步吞并 K8/K9；原 E5 speaking-data-ops 判定为反模式 C（按受益方切 Epic），退化为跨 Epic 横切 Feature：埋点与监控进入各业务 Epic，最小运营看板进入 E2。原 E4/E6/E7/E8 顺序重排为 E3/E4/E5/E6。Epic 总数从 9 减为 6。新增 OQ8 跟踪 E5 与 E4 同期上线时的二次合并可能。各 Epic 重新通过 §5.1 三判定。
