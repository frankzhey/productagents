# IELTS 口语 Mock 活动 — 小程序录音测评入口 PRD

> **版本**: v1.0  
> **日期**: 2026-04-22  
> **状态**: Draft — 待 PM 审阅  
> **所属 Epic**: IELTS 口语 Mock 活动  
> **输出语言规则**: User Story → 英文 | AC → 中文 | 其余 → 中文

---

## 1. Feature Summary

**背景**  
在微信小程序上搭建一次性的 IELTS 口语 Part 2 模拟测评活动入口。考生通过微信授权后，可随机查看 38 套 Part 2 题目之一（图片形式），录制语音作答并上传，系统通过 AI 评分后返回 CEFR 等级结果。

**目标**
- 收集考生 IELTS 口语 Part 2 真实作答音频（数据采集）
- 提供 AI 驱动的 CEFR 口语等级反馈（用户价值）
- 提升考生对 3Ups / IELTS 品牌认知，推动微信生态传播

**用户价值**
- 免费体验 AI 口语评测，获得可视化 CEFR 等级反馈
- 结果页具备社交分享传播价值

---

## 2. Product / Opportunity Brief

| 维度 | 说明 |
|------|------|
| 当前问题 | 缺乏面向 TOC 考生的轻量级口语测评小程序入口；Speakup 能力尚未在微信生态有效触达 |
| 为什么现在做 | 公司战略从 TOB 向 TOC / Mobile 转移；微信小程序是核心触达渠道；活动形式是低成本品牌露出手段 |
| 目标用户 | 参加雅思考试或备考雅思的考生（微信用户） |
| 业务价值 | 收集口语音频数据支持 AI 模型迭代；Speakup 品牌曝光；验证小程序口语测评 TOC 场景可行性 |

---

## 3. Value Hypothesis

| 假设 | 说明 |
|------|------|
| 用户行为假设 | 考生愿意在小程序完成一次完整 Part 2 口语录音测评，并在获得 CEFR 结果后主动分享 |
| 用户价值 | 免费 AI 评分 + 个性化 CEFR 等级可视化反馈 + 社交传播动机 |
| 业务价值 | 采集真实口语数据；小程序 TOC 用户互动数据；验证音频测评 TOC 场景 |
| 验证信号 | 录音上传完成率 / 结果页查看率 / 分享次数 |

---

## 4. KPI Tree

| 层级 | 指标 | 说明 |
|------|------|------|
| North Star | 完成全流程测评并查看结果的考生数量 | 进入活动 → 上传录音 → 查看成绩 |
| Leading Indicator | 录音页进入人数 | 主页 CTA 点击率 |
| Leading Indicator | 录音上传成功率 | 上传成功 / 进入录音页 |
| Leading Indicator | 结果页查看率 | 成绩回传后查看比例 |
| Leading Indicator | 分享次数 | 朋友圈 + 好友分享合计 |
| Guardrail | 录音上传失败率 | 不超过 5% |
| Guardrail | 重复提交率 | 0（系统强制限制） |
| Guardrail | 成绩回传时长 | 目标 ≤ 7 个自然日 |

---

## 5. Epic

**Epic Name**: IELTS 口语 Mock 活动 — 小程序录音测评入口

**Context**  
在现有微信小程序中新增活动模块，支持考生完成从微信授权 → 查看题目 → 录音 → 上传 → 等待评分 → 查看 CEFR 结果的全流程。后台支持管理员配置活动开关与 Banner 图片；评分结果通过 Callback 方式回传，自动解锁考生结果页。

---

## 6. Feature List

| # | Feature Name | Description | Value |
|---|---|---|---|
| F1 | 微信授权与身份绑定 | 考生通过微信授权登录，系统获取并绑定 unionId 作为唯一身份标识 | 支撑一次性提交限制与成绩关联 |
| F2 | 活动主页状态管理 | 根据考生状态动态展示 CTA 按钮（未提交 / 待结果 / 已出结果）；活动结束时切换结束状态与 Banner | 引导考生进入正确流程 |
| F3 | 后台活动配置管理 | 管理员配置活动开关（活动中 / 已结束）及 Banner 图片 | 支持运营灵活控制活动生命周期 |
| F4 | 录音页题目展示 | 随机显示 38 套 Part 2 题目中的一套（图片形式） | 提供真实考试模拟体验 |
| F5 | 录音与回放功能 | 调起麦克风录音，2 分钟倒计时自动结束，支持停止、回放、重录 | 完整录音体验，用户可控提交质量 |
| F6 | 录音上传与一次性提交控制 | 上传 mp3 录音文件，与 unionId 关联；每个 unionId 仅可提交一次 | 数据采集核心功能；确保活动公平性 |
| F7 | 等待结果页与分享 | 显示评估等待提示；支持分享小程序链接至好友 / 朋友圈 | 维持用户预期；提升社交传播 |
| F8 | CEFR 成绩展示 | 展示 CEFR 等级图片（A1–C2）与对应背景；随机显示对应等级文案；附 CEFR 说明链接 | 交付核心用户价值；驱动分享与品牌认知 |
| F9 | 成绩 Callback 接收与状态更新 | 后端接收评分系统的成绩回传，更新考生状态与 CEFR 结果 | 驱动结果页解锁与展示 |

---

## 7. User Stories and AC

> **FCS 评估说明**  
> F5（录音功能）FCS ≈ 11 → 已调用 Story Splitter，输出 6 个 Stories  
> F6（上传与提交控制）FCS ≈ 11 → 已调用 Story Splitter，输出 7 个 Stories  
> 其余 Feature FCS 5–8 → Product Planner 直接拆分

---

### F1 — 微信授权与身份绑定

---

#### Story F1-1: WeChat Authorization & UnionId Binding

**As a test candidate, I want to authorize with my WeChat account when entering the activity, so that my identity is uniquely bound to my submission and results.**

**AC**:

**场景 1：首次进入小程序，正常微信授权（Happy Path）**
```
Given 用户首次访问本活动小程序
When  页面加载，系统发起微信授权请求
Then  微信弹出「用户信息授权」弹窗
AND   用户点击「允许」后，后端通过 code 换取 unionId，并创建用户记录
AND   用户进入活动主页（Main Page）
```

**场景 2：已授权用户再次进入（静默登录）**
```
Given 用户已授权过本小程序
When  用户重新打开小程序
Then  系统静默完成登录（wx.login），无需用户再次点击授权
AND   直接进入活动主页，展示对应状态
```

**场景 3：用户拒绝微信授权**
```
Given 微信授权弹窗已弹出
When  用户点击「拒绝」
Then  页面展示提示："请授权微信登录以参与本次活动"
AND   提供「重新授权」按钮，用户可再次发起授权
AND   不允许未授权用户进入录音页
```

**场景 4：unionId 获取失败（后端接口异常）**
```
Given 前端已获取 code 并调用后端换取 unionId
When  后端接口返回超时或 5xx
Then  前端展示错误提示："登录失败，请重试"
AND   提供「重试」按钮，重新发起 wx.login 流程
AND   不允许用户在登录状态不确认时进入录音页
```

**场景 5：幂等性——同一用户多次登录**
```
Given 同一 unionId 已存在用户记录
When  用户再次完成微信授权
Then  后端不重复创建用户记录（幂等处理），返回现有用户状态
AND   不影响已有的提交状态与成绩数据
```

> **Story Estimation**
> - Size: M | Points: 3 | Units: 2–4 | Effort: 1–2 days  
> - Complexity: Medium | Confidence: Medium  
> - Notes: 涉及 wx.login + code2session + unionId 获取，微信生态鉴权链路；静默登录与显式授权两种路径；用户状态需从后端初始化

---

### F2 — 活动主页状态管理

---

#### Story F2-1: Main Page CTA State Machine (Three States)

**As a test candidate, I want to see the correct call-to-action button based on my current participation status, so that I am guided to the right step without confusion.**

**AC**:

**状态映射表（完整状态机规范）**

| 状态 | 触发条件 | CTA 按钮文案 | 按钮行为 |
|------|----------|-------------|---------|
| 未提交 | unionId 无提交记录 | "立即参与" / "开始口语测评" | 跳转录音页 |
| 已提交待结果 | status = "submitted" | "等待测评结果" | 不可点击（置灰） |
| 成绩已回传 | status = "scored" | "查看结果" | 跳转成绩结果页 |

**场景 1：未提交状态，展示"立即参与"（Happy Path）**
```
Given 用户已完成微信授权，系统查询该 unionId 无提交记录
When  进入活动主页
Then  页面展示"立即参与"按钮（高亮可点击）
AND   点击按钮跳转至录音页（Recording Page）
AND   "开始口语测评"按钮同样显示，行为与"立即参与"一致
```

**场景 2：已提交待结果，展示"等待测评结果"**
```
Given 用户已提交录音，status = "submitted"
When  进入活动主页
Then  页面展示"等待测评结果"按钮，按钮置灰不可点击
AND   不提供任何跳转入口
AND   页面文案区域展示等待提示（具体文案待 PM 提供）
```

**场景 3：成绩已回传，展示"查看结果"**
```
Given 该 unionId 成绩已通过 Callback 回传，status = "scored"
When  用户进入或刷新活动主页
Then  页面展示"查看结果"按钮（高亮可点击）
AND   点击按钮跳转至成绩展示页（Score Page）
```

**场景 4：状态查询接口失败（降级处理）**
```
Given 页面加载时状态查询接口超时或返回 5xx
When  接口请求失败
Then  页面展示 loading 状态
AND   loading 超时（> 10 秒）后展示提示："状态加载失败，请刷新页面"
AND   不展示任何 CTA 按钮（避免误引导）
AND   提供「刷新」按钮
```

**场景 5：幂等性——已提交状态下不可通过任何方式进入录音页**
```
Given 用户 status = "submitted" 或 "scored"
When  用户通过任何路径尝试访问录音页
Then  系统校验状态后拒绝进入，重定向至活动主页
AND   展示已提交状态的 CTA 按钮
```

> **Story Estimation**
> - Size: L | Points: 5 | Units: 4–8 | Effort: 2–4 days  
> - Complexity: High | Confidence: Medium  
> - Notes: 三态状态机 + 页面级权限控制；需与后端用户状态接口联调；状态轮询策略待确认（主页是否轮询 vs 手动刷新）；涉及 "已提交用户返回主页" 的路由拦截逻辑

---

#### Story F2-2: Activity Ended State Display

**As a test candidate who has not yet submitted, I want to see an "activity ended" message when the activity is closed by admin, so that I understand the submission window has passed.**

**AC**:

**场景 1：活动已结束，未提交考生查看主页（Happy Path）**
```
Given 管理员已在后台将活动状态配置为"已结束"
AND   该用户 unionId 无提交记录（未提交）
When  用户进入活动主页
Then  CTA 按钮展示为"谢谢参与，活动结束"，按钮置灰不可点击
AND   Banner 图片切换为"活动结束"版本（由后台配置）
AND   不可通过任何方式进入录音页
```

**场景 2：活动已结束，已提交考生的状态不受影响**
```
Given 管理员已将活动配置为"已结束"
AND   用户已提交录音（status = "submitted" 或 "scored"）
When  用户进入活动主页
Then  页面按照原有状态逻辑展示（等待测评结果 / 查看结果）
AND   活动结束状态仅影响"未提交"用户，不影响已提交用户的流程
```

**场景 3：活动状态从"进行中"切换为"已结束"后，前端感知时机**
```
Given 用户正在查看主页（活动进行中），管理员同时将活动切换为已结束
When  用户刷新页面
Then  前端重新调用后台配置接口，感知最新活动状态
AND   展示"活动结束"状态与对应 Banner
AND   无需实时推送，刷新后感知即可
```

> **Story Estimation**
> - Size: M | Points: 3 | Units: 2–4 | Effort: 1–2 days  
> - Complexity: Medium | Confidence: Medium  
> - Notes: 需后台活动配置接口支持（与 F3 联动）；Banner 图片资源需运营提前准备；"已结束"状态优先级低于"已提交 / 已出分"状态

---

### F3 — 后台活动配置管理

---

#### Story F3-1: Admin Activity Status & Banner Management

**As an admin, I want to configure the activity status (active / ended) and manage banner images for each state via the backend, so that the mini program reflects the correct activity state without code deployment.**

**AC**:

**场景 1：管理员切换活动状态为"已结束"（Happy Path）**
```
Given 管理员已登录后台管理系统，当前活动状态为"进行中"
When  管理员点击「结束活动」并确认
Then  系统更新活动配置：activity_status = "ended"
AND   前端小程序在下次状态查询时感知活动已结束
AND   后台展示当前配置状态为"已结束"，并显示最后修改时间和操作人
```

**场景 2：管理员上传活动结束 Banner**
```
Given 管理员进入 Banner 管理配置页
When  管理员上传"活动结束"状态的 Banner 图片（支持格式：jpg/png，尺寸规范待 PM 确认）
Then  系统保存图片，并立即生效（前端刷新后读取新 Banner）
AND   管理员可预览上传后的 Banner 效果
AND   Banner 替换后旧文件可继续保留（不自动删除）
```

**场景 3：管理员恢复活动状态为"进行中"（活动重启）**
```
Given 活动当前状态为"已结束"
When  管理员点击「恢复活动」并确认
Then  系统更新活动配置：activity_status = "active"
AND   前端感知后恢复正常 CTA 按钮展示逻辑
```

**场景 4：权限控制——只有授权管理员可修改活动配置**
```
Given 非授权用户尝试访问后台活动配置页
When  用户请求活动配置接口
Then  系统返回 403 Forbidden，拒绝访问
AND   操作日志记录该访问行为（含用户 ID + IP + 时间戳）
```

> ⚠️ **Open Question**: 后台活动配置功能归属哪个后台系统？是新建轻量级 Admin 页面，还是接入现有 IOC Admin？请 PM 确认。

> **Story Estimation**
> - Size: M | Points: 3 | Units: 2–4 | Effort: 1–2 days  
> - Complexity: Medium | Confidence: Low  
> - Notes: 后台系统归属未确认（影响工作量）；需提供活动配置 API；建议最小实现为 Toggle 开关 + Banner 图片上传，不需要完整 CMS

---

### F4 — 录音页题目展示

---

#### Story F4-1: Random Part 2 Question Assignment & Display

**As a test candidate, I want to see a randomly assigned IELTS Part 2 question image on the recording page, so that I can read the topic and prepare my speaking response.**

**AC**:

**场景 1：进入录音页，随机展示一道题目（Happy Path）**
```
Given 用户点击主页 CTA 按钮进入录音页
When  录音页加载
Then  系统从 38 套题目中随机抽取一套，展示对应图片
AND   图片完整清晰加载，适配手机屏幕宽度（不裁剪题目内容）
AND   同一用户在此次会话中分配到的题目固定不变（刷新页面不重新随机）
```

**场景 2：题目图片加载失败**
```
Given 用户进入录音页
When  题目图片加载超时（> 5 秒）或资源返回错误
Then  展示错误占位图与提示："题目加载失败，请刷新"
AND   提供「刷新」按钮，重新加载当前已分配的题目
AND   刷新后展示同一题目（不重新随机分配）
```

**场景 3：题目分配记录与 unionId 关联（幂等性）**
```
Given 用户本次会话已分配题目编号
When  用户关闭小程序后重新进入录音页（未提交状态）
Then  系统展示本次活动中该 unionId 已分配的同一题目
AND   不重新随机分配（每个考生本次活动全程使用同一题目）
```

> ⚠️ **Open Question**: 题目分配是前端随机（每次进入随机）还是后端分配（unionId 绑定固定题目）？建议后端分配以保证数据采集质量和考试公平性，请 PM 确认。

> **Story Estimation**
> - Size: S | Points: 2 | Units: 1–2 | Effort: 0.5–1 day  
> - Complexity: Low | Confidence: Medium  
> - Notes: 38 套题目图片资源需提前上传至 CDN；题目分配策略（前端 vs 后端）影响后端工作量；建议后端分配并记录，支持数据分析

---

### F5 — 录音与回放功能（Story Splitter 拆分结果）

> **FCS = 11，已由 Story Splitter 拆分为 6 个 Stories，以下为集成后完整版本（含 Product Planner 补充的 Estimation + Engineering Notes）**

---

#### Story F5-1: Microphone Permission Request & Pre-check

**As a test candidate, I want the app to request microphone permission before I start recording, so that I can record my speaking answer without unexpected interruptions.**

**AC**: 见 Story Splitter 输出 F5-1（已涵盖 5 类操作流程 AC）

> **Story Estimation（Product Planner 补充）**
> - Size: S | Points: 2 | Units: 2–3 | Effort: 1–1.5 days  
> - Complexity: Medium | Confidence: Medium  
> - Notes: 微信小程序麦克风权限申请（wx.authorize + scope.record）；权限被拒后需引导至 wx.openSetting；不同微信版本权限 API 行为差异需研发确认兼容性

---

#### Story F5-2: Start Recording & 2-Minute Countdown Timer

**As a test candidate, I want to start recording with a real-time 2-minute countdown visible on screen, so that I can manage my speaking time effectively.**

**AC**: 见 Story Splitter 输出 F5-2

> **Story Estimation（Product Planner 补充）**
> - Size: S | Points: 2 | Units: 3–4 | Effort: 1.5–2 days  
> - Complexity: Medium | Confidence: Medium  
> - Notes: 使用 wx.getRecorderManager() + format: 'mp3'；倒计时需使用 setInterval 管理，切后台行为需研发确认（App.onHide 是否影响计时器）；⚠️ Open Question: format: 'mp3' 微信兼容性需研发验证（部分 Android 机型可能不支持，备选 aac 转码方案）

---

#### Story F5-3: Manual Stop Recording & Action Panel Display

**As a test candidate, I want to manually stop my recording and see action options, so that I can decide whether to re-record, preview, or upload my answer.**

**AC**: 见 Story Splitter 输出 F5-3

> **Story Estimation（Product Planner 补充）**
> - Size: M | Points: 3 | Units: 3–5 | Effort: 1.5–2.5 days  
> - Complexity: High | Confidence: Medium  
> - Notes: 录音状态机（就绪 → 录音中 → 录音完成）；onStop 回调中 tempFilePath 处理；最小录音时长校验（阈值待 PM 确认，建议 5 秒）；⚠️ Open Question: 最小录音时长阈值是多少？

---

#### Story F5-4: Auto-Stop Recording at 2-Minute Limit

**As a test candidate, I want my recording to stop automatically when the 2-minute limit is reached, so that I don't accidentally exceed the allowed speaking time.**

**AC**: 见 Story Splitter 输出 F5-4

> **Story Estimation（Product Planner 补充）**
> - Size: S | Points: 2 | Units: 2–3 | Effort: 1–1.5 days  
> - Complexity: Medium | Confidence: High  
> - Notes: 计时器与 RecorderManager onStop 事件协调；需处理 onStop 回调超时保护机制；与 F5-3 共享状态机，建议同 Sprint 开发

---

#### Story F5-5: Re-record Confirmation Dialog & Cache Clearing

**As a test candidate, I want to see a confirmation dialog before discarding my existing recording, so that I don't accidentally lose my current audio.**

**AC**: 见 Story Splitter 输出 F5-5（已涵盖 6 个场景，含弹窗四种关闭方式）

> **Story Estimation（Product Planner 补充）**
> - Size: S | Points: 2 | Units: 2–3 | Effort: 1–1.5 days  
> - Complexity: Low | Confidence: High  
> - Notes: 弹窗四种关闭方式（确认 / 取消 / × / 蒙层外点击）均已在 AC 中覆盖；本地缓存清除使用 wx.getFileSystemManager()

---

#### Story F5-6: Recording Playback

**As a test candidate, I want to play back my recorded audio before submitting, so that I can verify the recording quality and decide whether to re-record or proceed with submission.**

**AC**: 见 Story Splitter 输出 F5-6

> **Story Estimation（Product Planner 补充）**
> - Size: S | Points: 2 | Units: 2–3 | Effort: 1–1.5 days  
> - Complexity: Low | Confidence: High  
> - Notes: 使用 InnerAudioContext 播放本地 tempFilePath；播放与上传按钮互斥（播放中点击上传，先停止播放再触发上传）；播放与重录按钮互斥

---

### F6 — 录音上传与一次性提交控制（Story Splitter 拆分结果）

> **FCS = 11，已由 Story Splitter 拆分为 7 个 Stories，以下为集成后完整版本**

---

#### Story F6-1: Obtain Pre-signed Upload URL

**As a test candidate, I want the app to securely obtain an upload URL from the backend before transferring my recording, so that my audio file is uploaded with proper authentication.**

**AC**: 见 Story Splitter 输出 F6-1

> **Story Estimation（Product Planner 补充）**
> - Size: S | Points: 2 | Units: 2–3 | Effort: 1–1.5 days  
> - Complexity: Medium | Confidence: Low  
> - Notes: 涉及后端预签名 URL 生成接口；⚠️ Open Question: 文件存储使用阿里云 OSS 还是腾讯云 COS？直接影响预签名 URL 格式和 SDK 选型；URL 有效期待研发与 PM 确认（建议 15 分钟）

---

#### Story F6-2: Upload Audio File with Loading State

**As a test candidate, I want to see a loading indicator while my recording is being uploaded, so that I know the upload is in progress and I don't accidentally navigate away.**

**AC**: 见 Story Splitter 输出 F6-2

> **Story Estimation（Product Planner 补充）**
> - Size: M | Points: 3 | Units: 4–5 | Effort: 2–2.5 days  
> - Complexity: High | Confidence: Medium  
> - Notes: wx.uploadFile 直传 OSS/COS；上传中所有按钮置灰（包括重录和播放）；上传成功后需额外调用后端通知接口（两步链路），整体耗时受网络影响较大；建议增加上传超时设置（建议 60 秒）

---

#### Story F6-3: Upload Error Handling & Retry

**As a test candidate, I want to be able to retry uploading if my recording upload fails, so that a temporary network issue doesn't prevent me from completing my submission.**

**AC**: 见 Story Splitter 输出 F6-3（已涵盖三类错误：系统报错 / 业务规则不满足 / 并发限制）

> **Story Estimation（Product Planner 补充）**
> - Size: M | Points: 3 | Units: 4–5 | Effort: 2–2.5 days  
> - Complexity: High | Confidence: Medium  
> - Notes: 三类错误独立 AC 已覆盖；⚠️ Open Questions: 文件大小上限（建议 50MB）/ 上传频次限制（N 次 / T 分钟 / 冷却时间）/ 最大建议重试次数请 PM 确认

---

#### Story F6-4: Backend Submission Record Write & UnionId Binding

**As a test candidate, I want my submission to be recorded with my WeChat identity after the file is uploaded, so that the system can reliably track my one-time submission status.**

**AC**: 见 Story Splitter 输出 F6-4（含状态机完整映射表：pending → submitted → scored）

> **Story Estimation（Product Planner 补充）**
> - Size: M | Points: 3 | Units: 3–5 | Effort: 1.5–2.5 days  
> - Complexity: High | Confidence: Medium  
> - Notes: 后端数据库写入需事务保证；unionId 字段设唯一约束防并发重复写入；DB 写入失败时前端触发重试不重复上传文件（仅重调通知接口）；日志必须包含 trace_id + unionId + timestamp + error_type

---

#### Story F6-5: Redirect to Waiting Page on Upload Success

**As a test candidate, I want to be automatically redirected to a waiting page after successfully submitting my recording, so that I know my submission is complete and I am waiting for results.**

**AC**: 见 Story Splitter 输出 F6-5

> **Story Estimation（Product Planner 补充）**
> - Size: S | Points: 2 | Units: 2–3 | Effort: 1–1.5 days  
> - Complexity: Low | Confidence: High  
> - Notes: wx.navigateTo 跳转等待页；后端 409（已提交）在前端视同成功跳转；⚠️ Open Question: 从等待页返回时是否允许返回到录音页？建议通过 wx.redirectTo 替代 navigateTo，禁止返回录音页

---

#### Story F6-6: Backend One-time Submission Enforcement

**As a system, I want to enforce that each WeChat unionId can only submit once regardless of how the upload function is accessed, so that the one-time submission rule is consistently enforced even if the frontend is bypassed.**

**AC**: 见 Story Splitter 输出 F6-6（含双重校验机制 + 安全日志 + 保守拒绝策略）

> **Story Estimation（Product Planner 补充）**
> - Size: M | Points: 3 | Units: 3–4 | Effort: 1.5–2 days  
> - Complexity: High | Confidence: High  
> - Notes: 后端 DB 设 unionId 唯一约束；双重校验节点（获取 URL 阶段 + 写入阶段）；409 vs 5xx 区分逻辑需后端明确实现；安全日志含来源 IP + unionId + trace_id

---

#### Story F6-7: Frontend Submitted State Lock (Hide Recording Entry)

**As a test candidate who has already submitted my recording, I want to see a clear submitted status screen instead of the recording interface, so that I cannot accidentally re-record or re-submit.**

**AC**: 见 Story Splitter 输出 F6-7（含三态状态机展示 + loading 保护 + 接口失败降级）

**状态机完整映射**

| 状态 | 展示 | 可操作项 |
|------|------|---------|
| 未提交 | 录音页正常界面 | 录音入口 / 操作面板 |
| submitted | "您已完成本次测评提交，正在评分中" | 「查看等待页」跳转 |
| scored | "评分已完成，点击查看您的结果" | 「查看结果」跳转 |

> **Story Estimation（Product Planner 补充）**
> - Size: S | Points: 2 | Units: 3–4 | Effort: 1.5–2 days  
> - Complexity: Medium | Confidence: High  
> - Notes: 页面加载时调用状态查询接口；loading 期间隐藏所有入口（保守策略）；不依赖本地缓存，每次刷新从后端重新获取

---

### F7 — 等待结果页与分享

---

#### Story F7-1: Waiting for Score Page Display

**As a test candidate who has submitted my recording, I want to see a waiting page with the expected evaluation timeline, so that I know my submission was successful and understand when to expect my results.**

**AC**:

**场景 1：进入等待结果页，正常展示（Happy Path）**
```
Given 用户刚完成录音上传，或再次进入小程序时 status = "submitted"
When  用户进入等待结果页
Then  页面展示：
        - 固定文案："评估时间：评估结果通常在 1 周内返回，请耐心等待"
        - 提交成功确认标识（图标或文案，样式待 UX 设计）
        - 分享按钮（进入 F7-2）
AND   不展示任何录音入口
AND   不展示"查看结果"按钮（成绩未回传）
```

**场景 2：成绩已回传，等待页自动提示用户查看**
```
Given 用户处于等待结果页，后端已收到成绩 Callback，status 更新为 "scored"
When  用户刷新等待页（或页面轮询检测到状态变更）
Then  等待页展示"评分完成，点击查看您的结果"
AND   出现「查看结果」按钮，点击跳转至成绩展示页
```

> ⚠️ **Open Question**: 等待页是否需要前端轮询自动感知成绩状态变化？如需轮询，建议轮询间隔（30 秒）和最大次数（无限直到 scored）请 PM 确认。

> **Story Estimation**
> - Size: S | Points: 2 | Units: 1–2 | Effort: 0.5–1 day  
> - Complexity: Low | Confidence: High  
> - Notes: 固定文案页面；若需轮询则复杂度升级为 Medium；建议 UX 设计一个视觉上有正向情感的等待状态界面

---

#### Story F7-2: Share Mini Program Link

**As a test candidate, I want to share the mini program activity link with friends or to WeChat Moments, so that my friends can discover and participate in the speaking test activity.**

**AC**:

**场景 1：分享至微信好友（Happy Path）**
```
Given 用户处于等待结果页或其他支持分享的页面
When  用户点击「分享」按钮，选择"发送给朋友"
Then  调用 wx.shareAppMessage，生成小程序分享卡片
AND   分享卡片标题、封面图片、跳转路径由后台配置（不硬编码）
AND   好友收到分享后点击可直接进入活动主页
```

**场景 2：分享至朋友圈**
```
Given 用户点击「分享」按钮，选择"分享到朋友圈"
When  触发朋友圈分享
Then  调用 wx.shareTimeline（需小程序开通朋友圈分享权限）
AND   分享标题与封面独立配置（可与好友分享不同）
```

**场景 3：幂等性——已提交或已出分用户均可分享**
```
Given 用户处于任意状态（已提交 / 已出分 / 活动结束）
When  用户点击分享
Then  分享功能正常执行，不受用户自身状态影响
AND   分享的链接始终指向活动主页
```

> **Story Estimation**
> - Size: S | Points: 2 | Units: 1–2 | Effort: 0.5–1 day  
> - Complexity: Low | Confidence: High  
> - Notes: 朋友圈分享需小程序后台开通"分享到朋友圈"能力；分享卡片素材需运营提前准备

---

### F8 — CEFR 成绩展示

---

#### Story F8-1: CEFR Level Result Display (Image, Background & Copy)

**As a test candidate, I want to see my CEFR level result with the corresponding level image, background design, a randomly selected motivational message, and a CEFR reference link, so that I receive personalized feedback on my speaking proficiency.**

**AC**:

**场景 1：成绩已出，正常展示 CEFR 等级结果（Happy Path）**
```
Given 用户 status = "scored"，后端已存储 CEFR 等级（A1/A2/B1/B2/C1/C2）
When  用户进入成绩展示页
Then  页面展示：
        1. CEFR 等级图片（对应 A1–C2 的专属等级图片）
        2. 对应等级的专属背景图片（6 个等级各一张背景）
        3. 文案区域：从该等级的 5 句话中随机抽取 1 句展示
        4. 文字链接："IELTS and the CEFR"（链接 URL 待 PM 提供）
AND   等级图片与背景图片完整清晰，适配手机屏幕
```

**CEFR 等级展示规范（状态机）**

| 等级 | 等级名 | 背景图 | 文案数量 |
|------|--------|--------|---------|
| A1 | 待 PM 提供 | 专属背景 A1 | 5 句（待 PM 提供） |
| A2 | 日常畅聊鸭 | 专属背景 A2 | 5 句（示例见 PRD 附录） |
| B1 | 待 PM 提供 | 专属背景 B1 | 5 句（待 PM 提供） |
| B2 | 待 PM 提供 | 专属背景 B2 | 5 句（待 PM 提供） |
| C1 | 待 PM 提供 | 专属背景 C1 | 5 句（待 PM 提供） |
| C2 | 待 PM 提供 | 专属背景 C2 | 5 句（待 PM 提供） |

**场景 2：随机文案每次展示不同（幂等性）**
```
Given 用户多次进入成绩展示页
When  页面加载
Then  每次从该等级 5 句文案中随机选取 1 句展示（允许连续两次相同）
AND   随机选取逻辑在前端执行，不依赖后端接口
```

**场景 3：图片资源加载失败**
```
Given 用户进入成绩展示页，CDN 图片资源加载失败
When  等级图片或背景图片加载错误
Then  显示占位图（样式待 UX 设计）
AND   文案区域仍正常展示
AND   不因图片加载失败影响整体页面可用性
```

**场景 4：CEFR 说明链接**
```
Given 成绩展示页已加载完成
When  用户点击"IELTS and the CEFR"链接
Then  在小程序内置 WebView（web-view 组件）中打开对应 URL
AND   如 URL 域名未在小程序后台配置白名单，则提示无法打开并提示用户复制链接至浏览器
```

> ⚠️ **Open Questions**:
> 1. A1 / B1 / B2 / C1 / C2 的等级名称（昵称）和对应 5 句文案尚未提供，请 PM 补充
> 2. "IELTS and the CEFR" 链接 URL 请 PM 提供
> 3. 6 张等级图片和 6 张背景图片的设计尺寸规范请 UX 提供

> **Story Estimation**
> - Size: M | Points: 3 | Units: 2–4 | Effort: 1–2 days  
> - Complexity: Medium | Confidence: Low（因文案和图片资源待提供）  
> - Notes: 前端随机文案选取逻辑简单；图片资源管理建议通过 CDN + 后台配置（避免写死代码）；6 套背景图 + 6 套等级图 = 12 张图片资源，设计团队需提前产出

---

### F9 — 成绩 Callback 接收与状态更新

---

#### Story F9-1: Score Callback Reception & Status Update

**As the system, I want to receive CEFR score callbacks from the AI evaluation service and update the candidate's status and result, so that the score page becomes accessible to the candidate when results are ready.**

**AC**:

**场景 1：正常接收成绩 Callback，更新状态（Happy Path）**
```
Given AI 评分服务完成评分，向后端发起 Callback 请求
When  后端接收到 Callback（含 unionId + CEFR 等级 + 其他评分数据）
Then  后端校验 Callback 来源合法性（签名验证 / Token 验证，待技术确认）
AND   更新对应 unionId 的记录：status = "scored"，cefr_level = [A1–C2]
AND   后端返回 200 确认接收成功
AND   记录日志（unionId + cefr_level + callback_time + trace_id）
```

**场景 2：Callback 签名验证失败（安全防护）**
```
Given Callback 请求来源未知或签名不匹配
When  后端接收到 Callback
Then  后端返回 401 / 403，拒绝处理
AND   记录安全告警日志（来源 IP + 请求内容摘要 + timestamp）
AND   不更新任何用户状态
```

**场景 3：Callback 中 unionId 不存在（数据异常）**
```
Given Callback 中携带的 unionId 在系统中无对应提交记录
When  后端处理 Callback
Then  后端返回 404，记录错误日志
AND   不创建新用户记录
```

**场景 4：重复 Callback（幂等处理）**
```
Given 同一 unionId 的 Callback 被重复发送（AI 服务重试机制）
When  后端再次接收到已处理过的 Callback
Then  后端检测到 status 已为 "scored"，不重复更新
AND   返回 200（幂等成功），告知 AI 服务无需再次重试
AND   不触发重复通知或重复评分
```

**场景 5：数据库写入失败，后端通知 AI 服务重发**
```
Given Callback 数据合法但后端 DB 写入失败
When  写入操作超时或抛出异常
Then  后端返回 5xx，触发 AI 服务的重试机制
AND   记录错误日志（含 unionId + error_type + trace_id）
AND   建议研发实现内部重试队列（最多 3 次，间隔指数退避）
```

> ⚠️ **Open Questions**:
> 1. AI 评分服务的 Callback 方式（HTTP Webhook / 消息队列 / 轮询）请技术团队确认
> 2. Callback 安全验证方式（HMAC 签名 / Bearer Token / IP 白名单）请技术团队确认
> 3. CEFR 成绩字段格式（单一字母等级 / 数值 / 其他）请 AI 评分服务提供接口文档

> **Story Estimation**
> - Size: L | Points: 5 | Units: 4–8 | Effort: 2–4 days  
> - Complexity: High | Confidence: Low（因 AI 评分服务接口未知）  
> - Notes: 涉及第三方 AI 评分服务（B-5 强制规范）；需要接口文档；幂等处理和重试机制关键；Callback 安全验证不可省略；建议研发实现死信队列处理持续失败 Callback

---

## 8. Estimation（Planning Level）

### Story Estimation Summary

| Feature | Story ID | Story Name | Size | SP | Units (Range) | Effort (Days) | Complexity | Confidence |
|---------|----------|------------|------|----:|--------------|---------------|------------|------------|
| F1 | F1-1 | WeChat Authorization & UnionId | M | 3 | 2–4 | 1–2 | Medium | Medium |
| F2 | F2-1 | Main Page State Machine | L | 5 | 4–8 | 2–4 | High | Medium |
| F2 | F2-2 | Activity Ended State Display | M | 3 | 2–4 | 1–2 | Medium | Medium |
| F3 | F3-1 | Admin Activity & Banner Config | M | 3 | 2–4 | 1–2 | Medium | Low |
| F4 | F4-1 | Random Question Display | S | 2 | 1–2 | 0.5–1 | Low | Medium |
| F5 | F5-1 | Microphone Permission | S | 2 | 2–3 | 1–1.5 | Medium | Medium |
| F5 | F5-2 | Start Recording + Countdown | S | 2 | 3–4 | 1.5–2 | Medium | Medium |
| F5 | F5-3 | Manual Stop + Action Panel | M | 3 | 3–5 | 1.5–2.5 | High | Medium |
| F5 | F5-4 | Auto-Stop at Countdown Zero | S | 2 | 2–3 | 1–1.5 | Medium | High |
| F5 | F5-5 | Re-record Confirmation Dialog | S | 2 | 2–3 | 1–1.5 | Low | High |
| F5 | F5-6 | Recording Playback | S | 2 | 2–3 | 1–1.5 | Low | High |
| F6 | F6-1 | Get Pre-signed Upload URL | S | 2 | 2–3 | 1–1.5 | Medium | Low |
| F6 | F6-2 | Upload File with Loading | M | 3 | 4–5 | 2–2.5 | High | Medium |
| F6 | F6-3 | Upload Error Handling & Retry | M | 3 | 4–5 | 2–2.5 | High | Medium |
| F6 | F6-4 | Backend Status Write | M | 3 | 3–5 | 1.5–2.5 | High | Medium |
| F6 | F6-5 | Redirect to Waiting Page | S | 2 | 2–3 | 1–1.5 | Low | High |
| F6 | F6-6 | Backend Submission Enforcement | M | 3 | 3–4 | 1.5–2 | High | High |
| F6 | F6-7 | Frontend Submission State Lock | S | 2 | 3–4 | 1.5–2 | Medium | High |
| F7 | F7-1 | Waiting Page Display | S | 2 | 1–2 | 0.5–1 | Low | High |
| F7 | F7-2 | Share Mini Program Link | S | 2 | 1–2 | 0.5–1 | Low | High |
| F8 | F8-1 | CEFR Result Display | M | 3 | 2–4 | 1–2 | Medium | Low |
| F9 | F9-1 | Score Callback & Status Update | L | 5 | 4–8 | 2–4 | High | Low |

---

## 9. Notes for Engineering（Engineering Handover）

### 跨 Story 关键技术决策

| 决策点 | 建议方案 | 风险 |
|--------|---------|------|
| 文件存储 | 阿里云 OSS 或腾讯云 COS（预签名直传） | 未确认，影响 SDK 选型和接口格式 |
| 录音格式 | mp3（wx.getRecorderManager format: 'mp3'） | 部分 Android 机型兼容性待验证；备选 aac + 服务端转码 |
| 成绩 Callback | HTTP Webhook（建议 HMAC 签名验证） | AI 服务接口文档未提供 |
| 状态轮询 | 前端 30 秒轮询 vs Push 通知 | 轮询方案简单但有延迟；Push 需额外能力 |
| 一次性提交 | DB 唯一约束 + 双重后端校验 | 并发场景需确保幂等 |
| 题目分配 | 建议后端分配（unionId 绑定） | 前端随机则无法追溯题目数据 |
| 后台系统 | 待确认（新建 vs IOC Admin 集成） | 影响 F3 工作量 |

### 第三方系统集成（B-5 强制规范）

**AI 评分服务（评分 Callback）**

| 项目 | 状态 |
|------|------|
| 系统名称 | 待确认（Speakup 评分服务 or 专用评分接口） |
| 接口类型 | HTTP Webhook（待确认） |
| 返回值映射 | 待 AI 服务提供接口文档（CEFR 字段格式未知） |
| 并发限制 | 未知，待确认 |
| 降级策略 | AI 服务不可用时前端维持 "submitted" 状态，人工触发补传 Callback |

**微信 API 集成**

| API | 用途 | 注意事项 |
|-----|------|---------|
| wx.login + code2session | 登录获取 unionId | 需服务端 AppSecret，不可在前端暴露 |
| wx.authorize (scope.record) | 麦克风权限 | 拒绝后仅可通过 wx.openSetting 引导 |
| wx.getRecorderManager | 录音 | format: 'mp3' 兼容性需验证 |
| wx.uploadFile | 文件上传 | 直传 OSS/COS 需配置 uploadFile 域名白名单 |
| InnerAudioContext | 录音回放 | 本地 tempFilePath 播放 |
| wx.shareAppMessage | 好友分享 | 需在 Page 配置 onShareAppMessage |
| wx.shareTimeline | 朋友圈分享 | 需开通朋友圈分享能力，需在 Page 配置 onShareTimeline |
| wx.navigateTo / wx.redirectTo | 页面跳转 | 建议等待页跳转使用 redirectTo 防止用户返回录音页 |

---

## 10. Business Process Flow

### Happy Path：完整测评流程

```
考生                小程序前端              后端服务              AI 评分服务
  |                    |                      |                      |
  |-- 打开小程序 ------->|                      |                      |
  |                    |-- 微信授权 ----------->|                      |
  |                    |<-- 返回 unionId -------|                      |
  |                    |-- 查询用户状态 -------->|                      |
  |                    |<-- 未提交，展示主页 ----|                      |
  |-- 点击「立即参与」-->|                      |                      |
  |                    |-- 分配题目 ----------->|                      |
  |                    |<-- 返回题目编号 --------|                      |
  |<-- 展示题目图片 ----|                      |                      |
  |-- 申请麦克风权限 -->|（wx.authorize）        |                      |
  |<-- 权限通过 --------|                      |                      |
  |-- 点击「开始录音」-->|（wx.getRecorderManager）                     |
  |<-- 倒计时 2:00 -----|                      |                      |
  |（录音 2 分钟）      |                      |                      |
  |-- 点击「结束录音」-->|（stop()）            |                      |
  |<-- 操作面板展示 ----|                      |                      |
  |-- 点击「上传录音」-->|                      |                      |
  |                    |-- 获取预签名 URL ------>|                      |
  |                    |<-- 返回预签名 URL ------|                      |
  |                    |-- wx.uploadFile ------->（OSS/COS）            |
  |                    |<-- 上传成功 ------------|                      |
  |                    |-- 上传完成通知 -------->|                      |
  |                    |<-- 写入成功，status=submitted                  |
  |<-- 跳转等待页 ------|                      |                      |
  |（1 周内等待）       |                      |                      |
  |                    |                      |<-- score callback ----|
  |                    |                      |-- 更新 status=scored  |
  |-- 刷新等待页 ------->|                      |                      |
  |                    |-- 查询状态 ----------->|                      |
  |                    |<-- scored, cefr_level--|                      |
  |<-- 展示「查看结果」--|                      |                      |
  |-- 点击「查看结果」-->|                      |                      |
  |<-- CEFR 成绩页 -----|                      |                      |
```

### Exception Path 1：录音上传失败，网络中断

```
考生                小程序前端              后端服务
  |-- 点击「上传录音」-->|                      |
  |                    |-- 获取预签名 URL ------>|
  |                    |<-- 返回预签名 URL ------|
  |                    |-- wx.uploadFile ------->（OSS/COS）
  |                    |（网络中断）             |
  |<-- 展示"上传中断，请检查网络后重试"          |
  |-- 检查网络 --------->|                      |
  |-- 点击「重试」 ------>|                      |
  |                    |-- 重新获取预签名 URL --->|（防 URL 过期）
  |                    |<-- 新预签名 URL --------|
  |                    |-- 重新上传 ------------>（OSS/COS）
  |                    |<-- 上传成功 ------------|
  |<-- 跳转等待页 ------|                      |
```

### Exception Path 2：活动已结束，未提交考生访问

```
考生                小程序前端              后台配置服务
  |-- 打开小程序 ------->|                      |
  |                    |-- 查询活动状态 -------->|
  |                    |<-- activity_status: "ended"
  |                    |-- 查询用户状态 -------->|
  |                    |<-- status: "unsubmitted"|
  |<-- 展示"谢谢参与，活动结束"按钮（置灰）      |
  |<-- 展示活动结束 Banner                       |
  |（考生无法进入录音页）|                       |
```

---

## 11. GWT Scenarios（Top 5）

### Scenario 1：Happy Path — 考生完成全流程测评

```
Given 用户首次通过微信授权进入活动小程序，活动进行中，无历史提交记录
When  用户完成"查看题目 → 录音 2 分钟 → 上传成功"全流程
Then  用户跳转至等待结果页，系统展示"评估结果通常在 1 周内返回"
AND   系统数据库记录：unionId + 题目编号 + 文件 key + status=submitted
```

### Scenario 2：重复提交被拦截

```
Given 用户已成功提交录音（status = "submitted"）
When  用户通过任何方式再次尝试进入录音页或调用上传接口
Then  系统拦截操作（前端不展示录音入口，后端返回 409）
AND   展示已提交状态，引导用户至等待结果页
AND   原提交记录和文件不受影响
```

### Scenario 3：活动已结束，未提交用户无法参与

```
Given 管理员已将活动配置为"已结束"，用户无提交记录
When  用户进入活动主页
Then  页面展示"谢谢参与，活动结束"（置灰不可点击）
AND   Banner 展示活动结束版本
AND   用户无法进入录音页
```

### Scenario 4：成绩 Callback 回传后，用户查看 CEFR 结果

```
Given AI 评分服务发送合法 Callback（unionId + cefr_level: "A2"）
When  后端处理 Callback，status 更新为 "scored"
AND   用户进入活动主页或等待页
Then  展示"查看结果"按钮，点击后进入成绩展示页
AND   页面展示 A2 等级图片 + A2 背景图 + 随机一句"日常畅聊鸭"文案 + CEFR 链接
```

### Scenario 5（Edge Case）：录音文件上传成功，但后端状态写入失败

```
Given 文件已上传至 OSS/COS，但后端写入提交记录时 DB 超时
When  后端返回 5xx
Then  前端展示上传失败提示，允许用户重试
AND   重试时不重新上传文件（文件已存在于 OSS）
AND   仅重新调用后端通知接口，传入原文件 key
AND   重试成功后状态写入完成，跳转等待页
```

---

## 12. Roadmap with Phases

### MVP（v1.0）— 活动核心链路

| 优先级 | Feature | Stories | 说明 |
|--------|---------|---------|------|
| P0 | F1 微信授权 | F1-1 | 基础身份能力 |
| P0 | F2 主页状态管理 | F2-1, F2-2 | 活动入口 |
| P0 | F4 题目展示 | F4-1 | 录音前置 |
| P0 | F5 录音功能 | F5-1~F5-6 | 核心录音链路 |
| P0 | F6 上传与提交控制 | F6-1~F6-7 | 数据采集核心 |
| P0 | F7 等待页 | F7-1 | 提交后状态 |
| P0 | F9 成绩 Callback | F9-1 | 成绩回传 |
| P0 | F8 成绩展示 | F8-1 | 结果交付 |

### Phase 2 — 运营与管理能力

| Feature | Stories | 说明 |
|---------|---------|------|
| F3 后台配置管理 | F3-1 | 活动开关 + Banner 管理（MVP 若无后台可临时用配置文件替代） |
| F7 分享功能 | F7-2 | 社交传播 |

### Future Extension

- 多次活动支持（当前为一次性设计，未来支持多期活动）
- 成绩分享海报生成（CEFR 等级个性化海报）
- 评分结果详情页（不仅仅是 CEFR 等级，包含维度得分）
- 触达 Touch points 埋点数据分析（用户漏斗分析）
- 与 3Ups / Speakup 产品打通（引导购买完整测评）

---

## 13. User Journey / User Flow

### 考生用户旅程

| 阶段 | 步骤 | 用户操作 | 系统响应 | 情绪 |
|------|------|----------|----------|------|
| 发现 | 1 | 通过好友分享或自然流量进入小程序 | 微信授权弹窗 | 好奇 |
| 进入 | 2 | 授权微信登录 | 进入活动主页，展示"立即参与"按钮 | 期待 |
| 查看题目 | 3 | 点击"立即参与" | 随机展示一道 Part 2 题目图片 | 专注 |
| 录音准备 | 4 | 阅读题目，点击"开始录音"申请麦克风权限 | 权限授权弹窗 → 倒计时开始 | 紧张 |
| 作答 | 5 | 口头作答，倒计时显示 2:00→0:00 | 倒计时实时更新，时间到自动结束 | 投入 |
| 审听回放 | 6 | 点击"播放"回听录音 | 播放本地录音 | 自我评估 |
| 提交 | 7 | 点击"上传录音" | loading → 跳转等待页 | 期待 |
| 等待 | 8 | 查看等待页文案，可分享给好友 | 展示等待文案 + 分享功能 | 平静期待 |
| 查看结果 | 9 | 约 1 周后收到通知，点击"查看结果" | CEFR 等级页面 + 随机文案 | 惊喜 / 满足 |
| 传播 | 10 | 分享结果或活动链接至好友 / 朋友圈 | 分享卡片生成 | 骄傲 / 分享欲 |

---

## 14. Non-functional Requirements

| 类别 | 要求 | 说明 |
|------|------|------|
| Performance | 页面加载时间 ≤ 2 秒 | 在 4G 网络条件下 |
| Performance | 录音文件上传超时阈值 60 秒 | 超出视为失败，触发重试 |
| Performance | Callback 处理响应时间 ≤ 500ms | 后端 Webhook 接收与入库 |
| Compatibility | 支持微信 7.0+ / iOS 14+ / Android 8+ | 主流微信用户覆盖 |
| Compatibility | mp3 格式兼容性 | 需研发验证主流机型，备选方案 aac |
| Data | 每个 unionId 最多 1 条提交记录 | DB 唯一约束强制保证 |
| Data | 音频文件存储保留期限 | 至少保留 6 个月（待 PM 确认） |
| Retry | 录音上传失败可重试 | 不限次数，每次重新获取预签名 URL |
| Retry | 成绩 Callback 失败重试 | AI 服务侧重试 + 后端内部队列（最多 3 次，指数退避） |
| Security | unionId 不可在小程序前端代码中暴露 | 仅通过后端接口获取和传递 |
| Security | Callback 接口需签名验证 | 防止伪造成绩 Callback |
| Security | 预签名 URL 有效期限制 | 建议 15 分钟，防止 URL 泄露被滥用 |
| Logging | 关键操作必须记录日志 | 包含 trace_id + unionId + service_name + error_level + timestamp |
| Monitoring | 上传失败率告警阈值 > 5% | 触发运维告警 |
| Monitoring | Callback 处理失败告警 | 任何 5xx 或验证失败需告警 |

---

## 15. Capacity Summary（Epic 级）

| 指标 | 数值 |
|------|------|
| Total Features | 9 |
| Total Stories | 22 |
| Total Estimated Units | **56–93 units** |
| Total Estimated Effort | **28–46.5 days** |
| Suggested Sprint Count | 3–5 sprints（2周/Sprint） |
| Suggested Team | 3 人（1 前端 + 1 后端 + 0.5 测试）|
| Main Complexity Drivers | F5 录音 WeChat API、F6 上传链路与幂等控制、F9 Callback 集成 |
| Key Assumptions | AI 评分服务接口文档已提供；文件存储类型已确认；38 套题目图片已准备 |

**工作量分布**

| Feature | Units Range | 占比（中值估算） |
|---------|------------|----------------|
| F5 录音功能（6 stories） | 14–21 | ~25% |
| F6 上传与提交控制（7 stories） | 19–29 | ~33% |
| F9 成绩 Callback（1 story） | 4–8 | ~8% |
| F1+F2+F3 登录与主页（4 stories） | 10–20 | ~20% |
| F4+F7+F8 题目/等待/结果（4 stories） | 7–14 | ~14% |

---

## 16. Estimation Disclaimer

> ⚠️ **以上估算属于 PRD 阶段的 Planning-level Estimation，仅用于范围判断、资源预估和优先级决策，不代表研发最终承诺。**  
> 最终单 Story 估算和任务拆分以 Eng Reviewer / Task Planner 在 Refinement 会议中的输出为准。  
> 以下 Open Questions 被解答后，估算 Confidence 将显著提升，部分 Story 规模可能调整：
> 1. 文件存储类型（OSS / COS）
> 2. AI 评分服务 Callback 接口规格
> 3. 后台管理系统归属
> 4. mp3 格式兼容性验证结果
> 5. A1/B1/B2/C1/C2 文案内容

---

## 17. Future Extension Ideas

- **成绩分享海报**：根据 CEFR 等级生成个性化海报，增强社交传播
- **音频质量前置校验**：上传前本地检测音量 / 静音比例，提升数据质量
- **多期活动支持**：当前为一次性活动设计；未来支持每期活动独立配置与数据隔离
- **与 Speakup 正式产品打通**：在结果页增加引导，促转化至完整 Speakup 产品
- **Touch points 埋点**：补充用户漏斗分析（进入主页 / 进入录音页 / 完成录音 / 上传 / 查看结果 / 分享）
- **部分成绩维度展示**：不仅显示 CEFR 等级，未来可展示流利度 / 发音 / 词汇等维度

---

## 附录 A：已知 CEFR 文案（A2 示例）

**A2 — 日常畅聊鸭（5 选 1 随机展示）**

1. 恭喜解锁日常畅聊鸭！超市点单、餐厅问路都能搞定，日常沟通越来越溜，太能干啦！
2. 日常畅聊鸭你超牛！能简单描述"昨天去哪玩"，口语表达有画面感，你的进步大家都看到啦！
3. 看到你成为日常畅聊鸭好惊喜！听懂对话还能及时回应，每一次交流都是成长，继续加油！
4. 日常畅聊鸭太优秀！基础句型用得超熟练，接下来挑战更复杂表达，你一定没问题！
5. 为日常畅聊鸭喝彩！从"不敢说"到"能畅聊"，你的努力超值得，未来会更棒！

**其余等级文案（A1 / B1 / B2 / C1 / C2）**  
⚠️ 待 PM 提供各等级昵称和 5 句文案后补充。

---

## 附录 B：Open Questions 汇总

| # | 问题 | 影响范围 | 优先级 |
|---|------|---------|--------|
| OQ-1 | 文件存储类型：OSS（阿里云）还是 COS（腾讯云）？ | F6-1, F6-2 | 🔴 High |
| OQ-2 | AI 评分服务 Callback 规格（方式 / 签名 / 字段格式）？ | F9-1 | 🔴 High |
| OQ-3 | A1/B1/B2/C1/C2 等级昵称和各 5 句文案？ | F8-1 | 🔴 High |
| OQ-4 | "IELTS and the CEFR" 链接 URL？ | F8-1 | 🔴 High |
| OQ-5 | 后台配置管理系统归属（新建 vs IOC Admin）？ | F3-1 | 🟠 Medium |
| OQ-6 | 题目分配方式：前端随机 vs 后端按 unionId 分配？ | F4-1 | 🟠 Medium |
| OQ-7 | 最小录音时长阈值（建议 5 秒）？ | F5-3 | 🟠 Medium |
| OQ-8 | 文件大小上限（建议 50MB）？ | F6-3 | 🟠 Medium |
| OQ-9 | 上传频次限制（N 次 / T 分钟 / 冷却 X 秒）？ | F6-3 | 🟡 Low |
| OQ-10 | 等待页是否需要前端轮询状态？轮询间隔？ | F7-1 | 🟡 Low |
| OQ-11 | 从等待页返回的页面栈策略（建议 redirectTo 禁止返回录音页）？ | F6-5 | 🟡 Low |
| OQ-12 | 音频数据保留期限（建议 6 个月）？ | NFR | 🟡 Low |
| OQ-13 | Banner 图片尺寸规范？ | F3-1, F2-2 | 🟡 Low |
| OQ-14 | mp3 格式在目标机型的兼容性验证（研发确认）？ | F5-2 | 🟠 Medium |

---

## Step 11: 规则沉淀检查（Rule Sedimentation）

⚠️ **当前项目尚无 `Rules/ielts-speaking-mock-activity-rules.md` 文件，建议创建。**

以下规则建议在 PM 确认后写入 `Rules/ielts-speaking-mock-activity-rules.md`：

---

```
📥 建议写入 Rules/ielts-speaking-mock-activity-rules.md

Layer 1（业务规则）：
  - 每个 WeChat unionId 在本次活动中仅可提交一次录音，
    此限制由后端强制执行（不依赖前端控制）
    — 归属节点：1.1 提交控制规则

  - 活动结束状态（activity_status = "ended"）仅影响"未提交"用户的 CTA 展示，
    已提交用户的状态流程（待结果 / 已出分）不受活动结束影响
    — 归属节点：1.2 活动状态规则

  - 每个 unionId 在本次活动中分配固定题目（建议后端分配），
    不因页面刷新或重新进入而重新随机
    — 归属节点：1.3 题目分配规则

  - 前端在用户状态接口未返回时，默认隐藏所有操作入口（保守保护策略）
    — 归属节点：1.4 状态展示规则

Layer 2（工程模式）：
  - 云存储上传采用预签名 URL 直传模式（避免后端中转大文件），
    URL 过期时前端自动无感刷新一次，超出后提示用户手动重试
    — 归属节点：2.1 文件上传架构

  - 后端幂等接口在 DB 写入冲突（unionId 重复）时返回 409 而非 5xx，
    前端对 409 做业务判断而非错误处理（视同成功跳转等待页）
    — 归属节点：2.2 幂等设计约定

  - 成绩 Callback 接收必须实现签名验证（HMAC 或 Bearer Token），
    验证失败返回 401/403 并记录安全日志，禁止无鉴权的 Callback 接口
    — 归属节点：2.3 Callback 安全规范

  - 录音状态机为单向流转（不可逆）：
    就绪 → 录音中 → 录音完成 → 已提交，
    状态间前端展示互斥
    — 归属节点：2.4 录音状态机设计

  - 用户状态单向流转（不可逆）：
    unsubmitted → submitted → scored
    DB 状态更新不允许回退
    — 归属节点：2.5 用户状态机设计

Layer 3（AC 约定）：
  - 所有包含上传操作的 Story，AC 必须明确三类错误：
    ① 系统报错（超时/5xx）② 业务规则不满足（格式/大小）③ 并发限制（429），各自独立 AC 场景
    — 归属节点：3.1 上传错误 AC 分类约定

  - 所有弹窗类 Story 必须覆盖四种关闭方式：
    「确认」「取消」「右上角 ×」「蒙层外点击」，并说明各方式副作用
    — 归属节点：3.2 弹窗关闭规范

  - 上传进行中，所有操作按钮（包括「重新录音」「播放」）必须同步置灰，
    防止并发操作引发状态混乱
    — 归属节点：3.3 按钮置灰约定

  - 涉及微信 API 的 Story，AC 必须覆盖 API 调用失败 / 权限拒绝两类场景
    — 归属节点：3.4 微信 API 场景覆盖规范
```
