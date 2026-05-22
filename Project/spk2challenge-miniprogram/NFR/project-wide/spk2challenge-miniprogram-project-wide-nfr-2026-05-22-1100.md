---
project: spk2challenge-miniprogram
scope: project-wide
nfr_version: 2026-05-22-1100
created: 2026-05-22-1100
maintainer: "@frankzhey"
status: draft
upstream_snapshot:
  value_wiki_path: /spk2challenge-miniprogram
  value_pulled_at: 2026-05-22-1100
skills_loaded:
  - skills/project-context-loader/SKILL.md
  - skills/nfr-spec/SKILL.md
business_context:
  type: TOC 一般业务
  sensitivity: 教育合规
  scenario_keyword: AI 评分异步处理
nfr_targets:
  performance:
    level: custom
    api_p95_ms: 500
    async_avg_s: 20
    async_max_s: 30
  availability:
    level: medium
    sla: "99.9%"
  capacity:
    level: medium
    dau: 10000
    mau: 100000
    peak_qps: 1000
    file_max_mb: 10
    growth_yoy_gb: 500
  data_sensitivity:
    level: medium
    fields: ["voice_recording", "learning_record", "user_identifier", "phone"]
  compliance:
    level: medium
    dpi_level: "等保二级"
    regulations: ["教育部备案"]
  retention:
    level: medium
    years: 3
    archive_policy: "1y_hot_2y_cold"
  geo:
    level: low
    regions: ["mainland_china"]
  business_scale:
    level: medium
    dau: "1k-10k"
    peak_qps: "100-1k"
    growth_yoy: "<=500GB"
dependency_check:
  status: passed
  warnings: []
project_loader:
  pm_confirmed_project: spk2challenge-miniprogram
  pm_confirmed_epic: project-wide
  batch_selection: single
  loader_at: 2026-05-22-1100
---

## §0 NFR Brief

- **项目**: spk2challenge-miniprogram
- **范围**: project-wide（项目级，供 Epic 级 NFR 缺失时回退使用）
- **业务类型**: TOC 一般业务
- **业务敏感度**: 教育合规
- **业务场景关键词**: AI 评分异步处理
- **上游 Value Wiki**: `/spk2challenge-miniprogram`
- **NFR 版本**: 2026-05-22-1100

### 选定档位摘要

| NFR | 档位 | 关键值 |
|---|:---:|---|
| Performance SLA | 其他 | API p95 <= 500ms / 异步评分平均 <= 20s / 异步最大 <= 30s |
| Availability SLA | 中 | 99.9% |
| Capacity | 中 | DAU 1k-10k / MAU 10k-100k / 峰值 100-1k QPS / 单文件 <= 10MB / 年增 <= 500GB |
| Data Sensitivity | 中 | 录音、学习记录、用户标识、手机号等 PII/教育数据 |
| Compliance | 中 | 等保二级 + 教育部备案，不做数据出境 |
| Retention | 中 | 3 年（1 年热 + 2 年冷） |
| Geo | 低 | 仅大陆，数据驻留大陆境内 |

### 依赖校验结果

- ✅ 全部通过。
- §4 Data Sensitivity = 中，§5 Compliance = 中，满足 PII 至少等保二级要求。
- §7 Geo = 低，不触发数据出境与 GDPR / 高合规依赖。
- §3 Capacity = 中，峰值上限为 1k QPS，不触发高并发强制提升可用性规则。
- §1 Performance 使用 PM 自定义目标，用于对齐 Value Guardrail K7（评分结果平均返回时长 <= 20s）。

### 缺失项（标 [待 Tech Lead 校准]）

- [待 Tech Lead 校准] 评分异步 SLA 的统计口径：平均 <= 20s 与最大 <= 30s 是否按全量任务、有效录音任务、还是排除第三方依赖故障后的任务计算。
- [待 Tech Lead 校准] 录音单文件大小上限、音频编码格式和最长录音时长。
- [待 Tech Lead 校准] 录音数据训练复用范围、授权粒度和删除请求处理机制。

## §1 Performance SLA

| 指标 | 目标 | 说明 |
|---|---:|---|
| API p95 | <= 500ms | 覆盖小程序页面查询、挑战题目获取、评分任务查询、结果页基础数据读取等同步 API。 |
| 异步评分平均返回时长 | <= 20s | 对齐 Value Guardrail K7，用于衡量用户提交有效录音到获得完整评分结果的平均耗时。 |
| 异步评分最大目标时长 | <= 30s | 超过该目标应进入“结果生成较慢 / 继续等待 / 刷新 / 稍后查看”恢复路径。 |
| 上传成功到任务创建 | <= 2s p95 | 音频上传完成后应快速创建或复用评分任务，避免用户卡在提交态。 |

说明：本项为 PM 自定义档位，基线参考中档 API p95 <= 500ms / 异步 <= 30s，并额外加入平均 <= 20s 以承接 Value K7。

## §2 Availability SLA

| 指标 | 目标 | 说明 |
|---|---:|---|
| 月度可用性 | 99.9% | TOC 一般业务推荐中档。 |
| 年度停机预算 | 约 8.76 小时 | 计划内维护需提前公告并避开高峰使用时段。 |
| 评分结果成功返回率 | >= 97% | 对齐 Value Guardrail K6；失败、超时、依赖异常需可追踪。 |

关键要求：评分链路必须区分上传成功、任务创建成功、评分处理中、评分成功、评分失败、评分超时等状态，避免用户无法判断结果状态。

## §3 Capacity（用户量 + 数据量）

| 指标 | 目标 | 说明 |
|---|---:|---|
| DAU | 1k-10k | TOC 一般业务中档容量。 |
| MAU | 10k-100k | 支撑小程序轻量练习和 website 导流增长验证。 |
| 峰值并发 | 100-1k QPS | 覆盖挑战入口、提交、查询和结果页访问峰值。 |
| 单条 / 单文件大小 | <= 10MB | 录音文件按中档上限预估，实际编码与时长待 Tech Lead 校准。 |
| 年数据增长 | <= 500GB | 含录音、评分结果、学习记录、埋点和必要审计日志的项目级估算。 |

业务量级总览：

```yaml
business_scale:
  level: medium
  dau: 1k-10k
  peak_qps: 100-1k
  growth_yoy: <=500GB
```

## §4 Data Sensitivity & Security

| 数据类型 | 敏感级别 | 安全要求 |
|---|---|---|
| 用户录音 | 中 | TLS in-transit + at-rest 加密；访问需最小权限；下载与播放需审计。 |
| 学习记录 / 作答记录 | 中 | 作为教育数据与个人学习轨迹处理，限制内部查询范围。 |
| 用户标识 / 手机号 | 中 | 字段级加密或脱敏展示；日志中不得明文输出。 |
| 评分结果与 score bucket | 中 | 小程序与 website 深链只透传 score bucket，不透传原始敏感分数。 |

安全基线：

- 所有外部与内部服务调用必须使用 TLS。
- 存储层对录音文件、评分结果和用户标识做 at-rest 加密。
- 生产环境日志不得记录原始录音地址、明文手机号、身份证号或完整敏感分数。
- 录音访问、导出、删除、训练复用应有审计记录。

## §5 Compliance & Regulation

| 合规项 | 目标 | 说明 |
|---|---|---|
| 等保 | 等保二级 | TOC 教育业务中档推荐。 |
| 行业备案 | 教育部备案 | IELTS / 教育测评相关数据处理需按教育合规口径落地。 |
| 数据出境 | 不出境 | 当前 project-wide NFR 默认大陆境内数据驻留。 |
| 用户授权 | 明确授权 | 录音提交前说明用途、保存、评分、可能的训练复用边界。 |

合规提示：评分结果必须展示“练习参考，非官方成绩”性质说明，避免用户误解为官方 IELTS 成绩。

## §6 Data Retention & Archive

| 数据 | 保留目标 | 归档策略 |
|---|---|---|
| 用户录音 | 3 年 | 1 年热 + 2 年冷；训练复用需受授权范围约束。 |
| 评分结果 | 3 年 | 1 年热 + 2 年冷；支持用户历史结果回看和问题排查。 |
| 任务状态 / 审计日志 | 3 年 | 关键状态与访问审计需可追溯。 |
| 临时上传缓存 | <= 7 天 | 无效、失败或未绑定任务的临时对象应定期清理。 |

保留策略需与用户授权、删除请求和训练复用机制一起校准；如后续 Compliance 要求更短或更长，应进入 NFR refinement。

## §7 Geo & Region

| 指标 | 目标 | 说明 |
|---|---|---|
| 服务区域 | 仅大陆 | 当前项目面向大陆 TOC 小程序用户。 |
| 数据驻留 | 大陆境内 | 用户录音、评分结果、学习记录和日志不出境。 |
| 跨境访问 | 不作为首版目标 | 若后续覆盖港澳或全球业务，应触发 NFR refinement，并重新校验 Compliance。 |

## §8 NFR Inter-dependency Check

| Rule | 结果 | 说明 |
|---|---|---|
| Rule 1: Avail=高 => Geo 不能单点 | ✅ 通过 | Availability=中，不触发 99.99% 多 AZ 依赖。 |
| Rule 2: Sens=中/高 => Compl>=中 | ✅ 通过 | Data Sensitivity=中，Compliance=中。 |
| Rule 3: Geo=高（出境）=> Compl=高 | ✅ 通过 | Geo=低，仅大陆，不触发出境依赖。 |
| Rule 4: Capacity peak_qps>1k => Avail>=中 | ✅ 通过 | Capacity=中，峰值上限 1k QPS，Availability=中。 |
| Rule 5: Retention=高 => Capacity growth_yoy 按高档预估 | ✅ 通过 | Retention=中，不触发高保留容量升级。 |
| Rule 6: Perf=高 + Capacity=高 => §9 OQ 必须列性能优化策略 | ✅ 通过 | Performance=custom，中档 API + 自定义异步目标；Capacity=中。 |

Dependency check status: `passed`。无自动调整，无 PM override。

## §9 Open Questions（缺失项 / 待 Tech Lead 校准）

| OQ ID | Question | Status | Owner |
|---|---|---|---|
| NFR-OQ1 | 异步评分平均 <=20s / 最大 <=30s 的统计口径如何定义，是否排除第三方 AI 服务故障窗口？ | open | Tech Lead + PM |
| NFR-OQ2 | 录音单文件 <=10MB 是否满足首期最长作答时长、音频编码和小程序上传限制？ | open | Tech Lead |
| NFR-OQ3 | 录音数据是否允许用于 AI 评分模型训练；如允许，授权文案、撤回机制和数据隔离如何定义？ | open | Compliance + PM + AI |
| NFR-OQ4 | 用户标识、手机号、评分结果和 score bucket 的字段级加密 / 脱敏边界如何落地？ | open | Security + Eng |
| NFR-OQ5 | project-wide NFR 是否足以覆盖后续 E2 留存和 E3 website 转化；若出现跨端账号打通，应是否新增 Epic 级 NFR？ | open | PM + Eng |

## §10 Changelog

- 2026-05-22-1100：创建 project-wide NFR 首版。业务背景为 TOC 一般业务 / 教育合规 / AI 评分异步处理；§1 Performance SLA 按 PM 自定义为 API p95 <= 500ms / 异步评分平均 <=20s / 异步最大 <=30s，其余 NFR 接受推荐中档或低档 Geo；依赖校验全部通过。
