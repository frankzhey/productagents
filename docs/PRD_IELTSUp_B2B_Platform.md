# PRD：IELTS Up B2B 管理平台

**版本：** V1.0  
**日期：** 2026-04-14  
**状态：** 待评审  
**设计来源：** design_V2.md（IELTS Up B2B Platform_v3 Zhanxi）

---

## 目录

1. [Feature Summary](#1-feature-summary)
2. [Product / Opportunity Brief](#2-product--opportunity-brief)
3. [Value Hypothesis](#3-value-hypothesis)
4. [KPI Tree](#4-kpi-tree)
5. [Epic 总览](#5-epic-总览)
6. [Feature List](#6-feature-list)
7. [User Stories & Acceptance Criteria](#7-user-stories--acceptance-criteria)
8. [Estimation（Planning Level）](#8-estimationplanning-level)
9. [Notes for Engineering](#9-notes-for-engineering)
10. [Business Process Flow](#10-business-process-flow)
11. [GWT Scenarios](#11-gwt-scenarios)
12. [Roadmap with Phases](#12-roadmap-with-phases)
13. [User Journey / User Flow](#13-user-journey--user-flow)
14. [Non-functional Requirements](#14-non-functional-requirements)
15. [Capacity Summary](#15-capacity-summary)
16. [Estimation Disclaimer](#16-estimation-disclaimer)
17. [Future Extension Ideas](#17-future-extension-ideas)

---

## 1. Feature Summary

### 背景

BCChina 当前正在推进从 TOB 到 TOC / Mobile Operations 的战略转型，但在此过程中 TOB 机构合作伙伴仍是核心收入来源。机构侧缺乏统一的产品管理、发码管理和数据回看工具，BC 侧缺乏标准化的商业化合同和机构开通流程，导致运营成本高、数据散乱、机构体验差。

### 目标

构建一套面向雅思教育机构的 B2B 管理平台，覆盖：

- BC 内部：客户档案、合同签订、订阅权益、额度管理、机构开通
- 机构侧：首页看板、题库管理、试卷组装、邀请码批次、消费者绑定、报告分析、API 接入
- 共用：RBAC 权限管理、用户组织管理、登录认证

### 用户价值

- BC 运营人员：统一商业化流程，减少人工对账与手动开通
- 机构管理员：一站式管理学员发码、内容配置和数据分析
- 机构教师/助教：便捷管理题库、试卷、批次和报告

---

## 2. Product / Opportunity Brief

**当前问题：**
- BC 对机构的合同、权益、额度分配无统一数字化工具，高度依赖线下流程
- 机构侧缺少统一管理视角，无法自助管理发码、题库、报告
- 机构学员（消费者）使用邀请码后，机构侧无法清晰追踪绑定状态与报告结果
- API 对接机构无法自助管理密钥，BC 侧无法有效审计调用日志

**为什么现在做：**
- Score Up / Write Up / Speak Up 三条产品线已进入批量商业化阶段
- 3Ups website 是公司重点投入的产品聚合平台，需要 B2B 管理后台支撑
- 机构合作伙伴体量增大，手动对接运营成本已超出可接受范围

**目标用户：**
- BC 内部：超级管理员、运营/销售人员
- 外部机构：机构管理员、助教、教师

**业务价值：**
- 降低 BC 人工运营成本
- 提升机构侧自助化率
- 为后续数据化运营和 TOC 转化提供基础数据资产

---

## 3. Value Hypothesis

| 维度 | 内容 |
|------|------|
| **假设** | 提供机构侧一站式管理工具，可提升机构续约意愿和使用粘性 |
| **Expected User Value** | 机构管理员/教师可独立完成发码、题库、报告管理，减少对 BC 客服依赖 |
| **Expected Business Value** | BC 侧运营效率提升，合同/订阅管理规范化，减少人工对账错误率 |
| **Validation Signal** | 机构侧自助操作率（发码、报告查看）> 80%；BC 侧人工开通时间减少 > 50% |

---

## 4. KPI Tree

### North Star

> 机构侧月活跃管理员数 × 机构平均发码批次数 / 月

### Leading Indicators

| 指标 | 说明 |
|------|------|
| 机构开通周期（天） | 从合同签订到管理员首次登录的时间 |
| 批次创建成功率 | 成功创建邀请码批次 / 尝试次数 |
| 邀请码使用率 | 已使用邀请码 / 总发放邀请码 |
| 报告查看率 | 查看报告次数 / 报告已生成总数 |
| API 调用成功率 | API 成功调用次数 / 总调用次数 |

### Guardrails

| 指标 | 说明 |
|------|------|
| 额度超发率 | 额度消耗 > 合同授权量的发生次数，目标为 0 |
| 权限越权操作次数 | 低权限角色访问高权限功能的次数，目标为 0 |
| 敏感操作未审计率 | 高风险操作缺少审计日志的比率，目标为 0 |

---

## 5. Epic 总览

| Epic ID | Epic 名称 | 说明 |
|---------|-----------|------|
| EP-01 | 用户认证与 RBAC 基础 | 登录、密码管理、用户/组织/角色管理（共用模块） |
| EP-02 | BC 商业化运营管理 | 客户管理、合同管理、订阅管理、额度明细、BC 成员管理 |
| EP-03 | 机构内容与发码管理 | 机构首页、题库管理、试卷管理、邀请码批次管理 |
| EP-04 | 报告与数据分析 | WriteUp / SpeakUp / ScoreUp 报告查看、报告详情、批次分析 |
| EP-05 | API 集成管理 | 机构侧 API Key 管理、BC 侧 API 调用审计 |

---

## 6. Feature List

### EP-01：用户认证与 RBAC 基础

| Feature ID | Feature 名称 | 说明 | 价值 |
|------------|-------------|------|------|
| F-01-01 | BC 内部用户登录 | 企业邮箱/Microsoft 账号登录，路由至 BC 视角 | BC 内部人员安全登录 |
| F-01-02 | 机构外部用户登录 | 机构账号密码登录，首次激活，忘记密码 | 机构用户安全登录与自助管理 |
| F-01-03 | 用户与组织管理 | 组织架构树、用户管理、角色管理（三 tab 共用组件） | 全平台权限底座 |
| F-01-04 | RBAC 角色权限管理 | 角色创建、复制、权限分组配置、删除校验 | 细粒度权限控制 |
| F-01-05 | 修改密码 | 旧密码校验、密码强度校验、重新登录提示 | 账号安全 |

### EP-02：BC 商业化运营管理

| Feature ID | Feature 名称 | 说明 | 价值 |
|------------|-------------|------|------|
| F-02-01 | 客户管理 | 客户列表、创建客户、客户详情、编辑基本信息 | 标准化客户档案管理 |
| F-02-02 | 合同管理 | 合同列表、新增合同、编辑合同、查看补充协议 | 数字化合同全生命周期 |
| F-02-03 | 订阅管理 | 订阅列表、订阅详情、权益查看、到期预警 | 合规权益发放与监控 |
| F-02-04 | 额度管理 | 额度充值、额度明细（充值/扣减/回收/过期） | 精确额度控制与对账 |
| F-02-05 | 机构管理员账号管理 | 创建初始账号、再次激活、停用管理员 | 机构开通自动化 |
| F-02-06 | 席位管理 | 查看席位、扩充席位、依据管理 | 授权数量管理 |
| F-02-07 | 服务调整 | API 服务与客户端服务开关，影响提示 | 服务灵活控制 |
| F-02-08 | BC 成员管理 | BC 内部用户/组织/角色管理（复用 RBAC 组件） | 内部团队权限管理 |

### EP-03：机构内容与发码管理

| Feature ID | Feature 名称 | 说明 | 价值 |
|------------|-------------|------|------|
| F-03-01 | 机构首页与数据看板 | 欢迎区、额度卡片、快捷入口、基础数据看板 | 机构数据一览 |
| F-03-02 | 额度总览与消耗记录 | 产品维度额度展示、消耗明细列表 | 机构侧额度自助查看 |
| F-03-03 | WriteUp 题库管理 | 按 Task 1/Task 2 筛选、题目详情查看 | 题库内容运营 |
| F-03-04 | SpeakUp 题库管理 | 按 Part 1/Part 2&3 筛选、题目详情查看 | 题库内容运营 |
| F-03-05 | ScoreUp 题库（只读） | 资源包只读查看 | 内容透明度 |
| F-03-06 | 试卷管理 | 试卷列表、创建试卷（手动组卷）、试卷详情、停用 | 自助内容配置 |
| F-03-07 | 邀请码批次管理 | 批次列表、创建批次、批次详情、一键回收 | 批量发码核心能力 |
| F-03-08 | 消费者信息管理 | 手动录入/导入消费者信息（批次内嵌能力） | 学员绑定与追踪 |

### EP-04：报告与数据分析

| Feature ID | Feature 名称 | 说明 | 价值 |
|------------|-------------|------|------|
| F-04-01 | WriteUp 报告分析 | 批次维度报告列表、筛选、报告状态 | 写作成绩数据回看 |
| F-04-02 | SpeakUp 报告分析 | 批次维度报告列表、口语状态 | 口语成绩数据回看 |
| F-04-03 | 报告详情 | 基础信息、作答结果、评分项、释放记录、手动释放操作 | 单条报告深度查阅 |
| F-04-04 | 批次分析视图 | 批次勾选对比、雷达图/柱状图/分数分布、自动结论 | 批次汇总分析 |

### EP-05：API 集成管理

| Feature ID | Feature 名称 | 说明 | 价值 |
|------------|-------------|------|------|
| F-05-01 | 机构侧 API 管理 | API Key/Secret 查看、脱敏展示、重置、文档链接 | 机构 API 自助接入 |
| F-05-02 | BC 侧 API 调用审计 | 调用记录列表、异常过滤、详情展开 | BC 侧合规审计与排障 |

---

## 7. User Stories & Acceptance Criteria

---

### EP-01：用户认证与 RBAC 基础

---

#### F-01-01：BC 内部用户登录

**Story 1.1.1**

> As a BC internal user, I want to log in using my corporate email and password, so that I can access the BC management view securely.

**AC：**

**场景一：正常登录**
- Given 用户在内部登录页面
- When 输入有效的企业邮箱和正确密码并点击登录
- Then 登录成功，跳转至 BC 视角客户管理页

**场景二：密码错误**
- Given 用户在内部登录页面
- When 输入有效邮箱但密码错误
- Then 提示密码错误，账号输入框保留原值，密码框清空

**场景三：Microsoft 账号登录**
- Given 用户点击 Microsoft 登录按钮
- When 完成 Microsoft 认证流程
- Then 系统验证身份绑定关系，登录成功后跳转 BC 视角

**场景四：空输入校验**
- Given 用户在内部登录页面
- When 未填写邮箱或密码直接点击登录
- Then 字段下方即时显示"必填"错误提示，不发起登录请求

---

#### F-01-02：机构外部用户登录

**Story 1.2.1**

> As an institution user, I want to log in with my institution-assigned account, so that I can access the institution management view.

**AC：**

**场景一：正常登录**
- Given 机构用户在外部登录页面
- When 输入有效的机构邮箱账号和密码
- Then 登录成功，跳转至机构视角首页

**场景二：首次激活**
- Given 机构管理员账号由 BC 创建但尚未激活
- When 用户首次使用凭证登录
- Then 系统引导进入激活/修改密码流程，完成后进入机构首页

**场景三：忘记密码**
- Given 用户点击忘记密码链接
- When 输入注册邮箱并提交
- Then 系统发送重置密码邮件，提示"邮件已发送，请查收"

**场景四：账号被禁用**
- Given 机构用户账号已被 BC 停用
- When 用户尝试登录
- Then 登录失败，提示"账号已禁用，请联系管理员"

---

#### F-01-03：组织架构管理

**Story 1.3.1**

> As an admin, I want to create and manage an organizational hierarchy, so that users can be assigned to the correct organizational nodes.

**AC：**

**场景一：创建组织节点**
- Given 管理员在组织架构 tab
- When 点击"新增组织"并填写名称和上级节点，提交
- Then 新节点出现在左侧组织树对应位置，右侧展示节点详情

**场景二：删除包含成员的组织**
- Given 某组织节点下有绑定用户
- When 管理员尝试删除该节点
- Then 系统阻断操作，提示"请先迁移或移除当前组织成员"

**场景三：删除一级默认节点**
- Given 管理员查看一级根节点
- When 尝试删除该节点
- Then 删除按钮不可用，提示"默认一级节点不可删除"

---

#### F-01-04：用户管理

**Story 1.4.1**

> As an admin, I want to create users and assign them organization-role bindings, so that they can access the platform with correct permissions.

**AC：**

**场景一：创建用户**
- Given 管理员在用户管理 tab
- When 填写用户名、邮箱并提交
- Then 新用户创建成功，跳转至分配组织角色绑定步骤

**场景二：用户未绑定组织角色**
- Given 新建用户未配置任何组织角色绑定
- When 系统完成创建
- Then 用户账号创建但无法使用功能，管理界面显示"待绑定"提示

**场景三：禁用用户**
- Given 管理员选中一个"启用"状态用户
- When 点击"禁用"并确认
- Then 用户账号状态变为"禁用"，该用户不可登录，历史操作日志保留

---

#### F-01-05：角色权限管理

**Story 1.5.1**

> As an admin, I want to create and configure roles with grouped permissions, so that I can control what each user can see and do.

**AC：**

**场景一：创建角色**
- Given 管理员在角色管理 tab
- When 输入角色名称、选择权限分组并提交
- Then 新角色创建成功，出现在角色列表

**场景二：角色名称重复**
- Given 已存在同名角色
- When 管理员提交同名角色
- Then 提示"角色名称已存在，请修改后重试"，不允许创建

**场景三：删除有绑定用户的角色**
- Given 角色下存在关联用户
- When 管理员尝试删除该角色
- Then 操作被阻断，提示"请先解除该角色下所有用户绑定"

**场景四：复制角色**
- Given 管理员选中某角色点击"复制"
- When 填写新角色名称并确认
- Then 新角色创建成功，权限配置与原角色一致

---

#### F-01-06：修改密码

**Story 1.6.1**

> As a user, I want to change my password, so that I can keep my account secure.

**AC：**

**场景一：成功修改密码**
- Given 用户在修改密码页面
- When 正确填写旧密码、满足强度要求的新密码和确认密码，提交
- Then 修改成功，系统提示重新登录，自动退出当前会话

**场景二：旧密码错误**
- Given 用户填写了错误的旧密码
- When 提交表单
- Then 提示"旧密码不正确，请重新输入"

**场景三：新密码强度不足**
- Given 用户输入的新密码未满足强度规则
- When 填写内容时即时校验
- Then 密码框下方展示强度提示，保存按钮不可用

**场景四：确认密码不一致**
- Given 新密码与确认密码输入不同
- When 离开确认密码框
- Then 即时提示"两次输入密码不一致"

---

### EP-02：BC 商业化运营管理

---

#### F-02-01：客户管理

**Story 2.1.1**

> As a BC operator, I want to view and search the customer list, so that I can quickly locate a customer account.

**AC：**

**场景一：客户列表加载**
- Given BC 运营人员进入客户管理页
- When 页面加载完成
- Then 按创建时间倒序展示客户列表

**场景二：按条件筛选**
- Given 客户列表已展示
- When 输入集团名称或客户名称、日期区间并点击搜索
- Then 列表刷新展示匹配结果，空结果显示"未找到匹配客户"并提供清空筛选入口

---

**Story 2.1.2**

> As a BC operator, I want to create a new customer record, so that I can start the contract process.

**AC：**

**场景一：创建客户成功**
- Given BC 运营人员点击"创建客户"
- When 填写客户名称（必填）、联系人（可选）、备注（可选）并提交
- Then 客户创建成功，新记录插入列表顶部，状态显示"未签约"

**场景二：客户名称重复**
- Given 已存在同名客户
- When 提交相同名称的新客户
- Then 提示"客户名称已存在"，不允许提交

**场景三：关闭弹窗时有未保存内容**
- Given 用户在创建客户弹窗中填写了部分内容
- When 点击取消
- Then 弹出二次确认"确定放弃当前修改？"

---

**Story 2.1.3**

> As a BC operator, I want to view and edit customer details, so that I can maintain accurate customer information.

**AC：**

**场景一：查看客户详情**
- Given BC 运营人员点击客户记录
- When 进入详情页
- Then 展示客户名称、合同状态、联系人信息及合同/订阅/额度入口

**场景二：编辑联系人信息**
- Given 管理员在详情页点击"编辑"
- When 修改联系人或备注并保存
- Then 详情卡片即时刷新，仅允许编辑联系人/备注等非主键字段

**场景三：字段无变化时保存**
- Given 管理员进入编辑态但未修改任何字段
- When 查看保存按钮
- Then 保存按钮处于置灰不可用状态

---

#### F-02-02：合同管理

**Story 2.2.1**

> As a BC operator, I want to create a contract with multiple products and service periods, so that the subscription takes effect immediately after signing.

**AC：**

**场景一：新增合同成功**
- Given BC 运营人员在客户详情页点击"新增合同"
- When 填写服务截止时间、选择产品、配置权益和额度并提交
- Then 合同创建成功，系统自动生成生效中订阅并写入审计记录

**场景二：未填写服务截止时间**
- Given 用户填写合同信息
- When 服务截止时间为空并提交
- Then 提示"服务截止时间为必填项"，阻断提交

**场景三：周期重叠校验**
- Given 合同中存在同一产品的两个服务周期
- When 两个周期日期发生重叠
- Then 提示"服务周期存在重叠，请调整后提交"

---

**Story 2.2.2**

> As a BC operator, I want to add a supplementary agreement to an existing contract, so that I can extend or amend contract terms without creating a new contract.

**AC：**

**场景一：新增补充协议**
- Given 管理员在合同详情页
- When 点击"新增补充协议"并填写协议内容
- Then 补充协议独立追加到合同记录，合同整体服务范围更新

**场景二：不支持按单产品拆分补充协议**
- Given 管理员查看补充协议表单
- When 尝试针对单一产品创建独立补充协议
- Then 表单设计不提供此能力，补充协议仅按合同整体追加

---

**Story 2.2.3**

> As a BC operator, I want to edit an active contract, so that I can correct or update service terms with proper audit trail.

**AC：**

**场景一：编辑已生效合同**
- Given 管理员在编辑合同页
- When 修改已生效合同的关键字段（如服务截止时间）并保存
- Then 弹出影响提示，确认后保存，同步刷新关联订阅和额度，并落审计日志

**场景二：合同历史版本查看**
- Given 管理员在合同管理页
- When 查看历史协议
- Then 历史协议默认折叠，可点击展开时间线式查看

---

#### F-02-03：订阅管理

**Story 2.3.1**

> As a BC operator, I want to view the subscription list with filtering, so that I can monitor the status of all active and expired subscriptions.

**AC：**

**场景一：筛选订阅**
- Given BC 运营人员在订阅列表页
- When 按客户、产品、状态（生效中/已到期）、服务区间筛选
- Then 列表展示匹配结果

**场景二：到期预警标识**
- Given 某订阅距服务截止时间不超过预设阈值（如 30 天）
- When 列表展示该订阅
- Then 该订阅行显示"即将到期"预警标识

---

**Story 2.3.2**

> As a BC operator, I want to view the subscription details with quota and entitlement summary, so that I can understand what the institution is entitled to use.

**AC：**

**场景一：查看订阅详情**
- Given 管理员点击某订阅进入详情
- When 页面加载成功
- Then 展示订阅基础信息、服务区间、产品权益、可用额度、累计充值、累计消耗、关联合同和历史调整记录

**场景二：额度耗尽提示**
- Given 某产品可用额度为 0
- When 管理员查看订阅详情
- Then 该产品额度区域显示"额度不足"说明，相关操作按钮禁用并展示原因

---

#### F-02-04：额度管理

**Story 2.4.1**

> As a BC operator, I want to top up credits for a subscription, so that the institution can continue using the product services.

**AC：**

**场景一：充值成功**
- Given 管理员在订阅详情页点击充值
- When 选择预设充值档位、填写截止日期和合同/收据编号，确认提交
- Then 充值成功，额度明细记录更新，订阅详情中可用额度即时刷新

**场景二：重复依据单号**
- Given 输入的合同或收据编号已被使用过
- When 提交充值
- Then 提示"该依据单号已被使用，请检查后重新输入"，阻断提交

**场景三：充值预览**
- Given 管理员选择充值档位
- When 档位选定后
- Then 实时展示充值后的额度变化预览（当前额度 + 本次充值 = 充值后）

---

**Story 2.4.2**

> As a BC operator, I want to view the credit transaction history, so that I can audit all credit changes.

**AC：**

**场景一：查看额度明细**
- Given 管理员进入额度明细页
- When 列表加载
- Then 展示每条流水的来源单据、变更类型（充值/扣减/回收/过期）、操作人、变更后余额

**场景二：系统自动触发标记**
- Given 额度因到期自动失效或系统自动回收
- When 额度明细展示该记录
- Then 操作来源标记为"系统"，而非人工操作人

---

#### F-02-05：机构管理员账号管理

**Story 2.5.1**

> As a BC admin, I want to create an initial admin account for an institution, so that the institution can start onboarding and managing their users.

**AC：**

**场景一：创建管理员账号**
- Given BC 管理员在订阅详情页，尚未创建机构管理员
- When 点击"创建管理员"，填写姓名和邮箱并提交
- Then 账号创建成功，系统发送激活邮件，页面展示账号状态为"未激活"

**场景二：重新激活未激活账号**
- Given 账号处于"未激活"状态
- When BC 管理员点击"再次发送激活邮件"
- Then 激活邮件重新发送，系统提示"激活邮件已重新发送"

**场景三：停用管理员**
- Given 机构管理员账号处于启用状态
- When BC 管理员点击"停用"并确认
- Then 账号状态变为"禁用"，页面显示"账号停用后该机构管理员将无法登录"的影响说明

---

#### F-02-06：席位管理

**Story 2.6.1**

> As a BC operator, I want to manage and expand the seat count for an institution, so that more users can be onboarded within the contract scope.

**AC：**

**场景一：扩充席位**
- Given 管理员在订阅详情席位区域
- When 输入扩充数量和依据说明并确认
- Then 席位扩充成功，账号信息区即时刷新

**场景二：额度不足时扩充**
- Given 当前可用额度为零
- When 管理员查看扩充席位按钮
- Then 按钮不可用，显示"当前无可用额度，无法扩充席位"

---

#### F-02-07：服务调整

**Story 2.7.1**

> As a BC admin, I want to adjust the enabled services for an institution, so that I can control API and product access per contract terms.

**AC：**

**场景一：关闭已开通服务**
- Given BC 管理员在服务调整弹窗
- When 选择关闭某个已开通的 API 服务
- Then 弹窗展示目标服务、当前状态、目标动作和原因输入框，确认后执行，并提示对机构侧功能可见性的影响

**场景二：服务调整成功**
- Given 管理员填写调整原因并确认
- When 提交生效
- Then 服务状态即时更新，订阅详情反映最新开通状态

---

### EP-03：机构内容与发码管理

---

#### F-03-01：机构首页与数据看板

**Story 3.1.1**

> As an institution admin, I want to see a dashboard on the home page with quota status and quick access to core features, so that I can efficiently navigate and monitor key metrics.

**AC：**

**场景一：首页加载**
- Given 机构管理员登录成功
- When 进入首页
- Then 展示欢迎区、有效额度卡片、快捷入口和基础数据看板

**场景二：快捷入口差异化展示**
- Given 当前登录用户角色为助教（非管理员）
- When 查看快捷入口
- Then 仅展示助教权限范围内的功能入口，管理员专属入口不可见

**场景三：卡片点击跳转**
- Given 首页额度卡片展示某产品数据
- When 点击该卡片
- Then 跳转至对应产品额度总览页

---

#### F-03-02：额度总览与消耗记录

**Story 3.2.1**

> As an institution admin, I want to view my quota overview by product, so that I can understand remaining capacity before issuing invite codes.

**AC：**

**场景一：查看额度总览**
- Given 机构管理员进入额度总览
- When 页面加载
- Then 按产品展示有效额度、已用、剩余、到期时间和来源订阅

**场景二：未开通产品**
- Given 某产品未在合同中开通
- When 查看额度总览
- Then 该产品卡片显示灰态，并注明"未开通"

**场景三：消耗记录筛选**
- Given 管理员进入消耗记录页
- When 按产品、时间范围、消耗类型筛选
- Then 列表展示匹配记录；无结果时提示"当前筛选条件下无消耗记录"

---

#### F-03-03 / F-03-04：题库管理（WriteUp & SpeakUp）

**Story 3.3.1**

> As an institution teacher, I want to browse the question bank with filters, so that I can find appropriate questions to include in a paper.

**AC：**

**场景一：WriteUp 题库筛选**
- Given 教师进入 WriteUp 题库
- When 选择 Task 1 或 Task 2，配合关键词搜索
- Then 列表展示匹配题目（ID、标题、标签）

**场景二：SpeakUp 结构约束**
- Given 教师在创建试卷的题库选择场景中
- When 超出 SpeakUp Part 结构限制（如已选 3 道 Part 1，再选 Part 1）
- Then 超出的 Part 1 题目显示禁用态，不可选

**场景三：筛选条件保留**
- Given 教师在题库列表设置了筛选条件
- When 进入题目详情后返回题库列表
- Then 筛选条件保留，列表停留在原来位置

---

#### F-03-06：试卷管理

**Story 3.6.1**

> As an institution teacher, I want to create a paper by manually selecting questions, so that I can build a customized exam for my students.

**AC：**

**场景一：创建 WriteUp 试卷成功**
- Given 教师在创建试卷页选择"WriteUp"产品类型
- When 从题库选择 Task 1 和 Task 2 题目，通过结构校验，点击保存
- Then 试卷创建成功，返回试卷列表，新试卷出现在列表

**场景二：SpeakUp 结构校验不通过**
- Given 教师创建 SpeakUp 试卷，未满足"3 道 Part 1 + 1 道 Part 2&3"结构要求
- When 点击保存
- Then 保存按钮不可用，显示"试卷结构不符合要求，请检查 Part 数量"

**场景三：切换产品类型**
- Given 教师在创建试卷页选择了 WriteUp 并已选入部分题目
- When 切换产品类型为 SpeakUp
- Then 系统清空已选题目并重置题库筛选条件，弹出提示"切换产品将清空已选题目"

**场景四：拖拽调整题目顺序**
- Given 教师在已选题目区域
- When 拖拽某题到新位置
- Then 题目顺序实时更新，序号重新排列

---

**Story 3.6.2**

> As an institution admin, I want to disable a paper, so that it cannot be used in new batches going forward.

**AC：**

**场景一：停用试卷**
- Given 管理员在试卷详情页
- When 点击"停用"并二次确认
- Then 试卷状态变为"已停用"，无法被新批次绑定

**场景二：已绑定批次不受影响**
- Given 某试卷已被历史批次引用
- When 试卷被停用
- Then 该历史批次中的试卷绑定不变，已有邀请码用于考试不受影响

---

#### F-03-07：邀请码批次管理

**Story 3.7.1**

> As an institution admin, I want to create an invite code batch linked to a product and paper, so that I can distribute invite codes to consumers for exam access.

**AC：**

**场景一：创建批次成功（含试卷绑定）**
- Given 管理员创建 WriteUp 批次，选择有效产品、绑定试卷、输入 20 个邀请码数量、设置有效期
- When 通过预览确认提交
- Then 批次创建成功，生成 20 个邀请码，列表显示新批次

**场景二：额度超出阻断**
- Given 当前可用额度剩余 10
- When 输入生成数量为 15
- Then 系统实时显示"剩余额度不足"，保存按钮不可用

**场景三：ScoreUp 不需绑定试卷**
- Given 管理员创建 ScoreUp 批次
- When 选择 ScoreUp 产品类型
- Then 页面不展示绑定试卷步骤，直接进入后续批次配置

**场景四：非 ScoreUp 未绑定试卷**
- Given 管理员创建 WriteUp 批次
- When 未选择试卷直接尝试下一步
- Then 提示"非 ScoreUp 产品必须绑定试卷"，阻断操作

---

**Story 3.7.2**

> As an institution admin, I want to recover unused invite codes from a batch, so that I can reclaim quota from expired or unused codes.

**AC：**

**场景一：一键回收**
- Given 批次状态为"进行中"且存在未使用邀请码
- When 管理员点击"一键回收"并二次确认
- Then 未使用邀请码被回收，额度返还，批次状态更新

**场景二：无未使用码时隐藏回收入口**
- Given 批次内所有邀请码均已使用
- When 管理员查看批次详情
- Then "一键回收"按钮不显示或置灰

---

#### F-03-08：消费者信息管理

**Story 3.8.1**

> As an institution admin or teaching assistant, I want to bind consumer information to invite codes in a batch, so that I can track which student used which invite code.

**AC：**

**场景一：手动新增消费者**
- Given 管理员在批次创建的"绑定消费者"步骤
- When 手动填写消费者姓名、联系方式并保存
- Then 该消费者信息绑定到批次中的一个邀请码

**场景二：导入消费者信息**
- Given 管理员下载模板并填写数据后上传
- When 文件格式正确，识别成功
- Then 展示识别成功的人数，可进行预览和确认

**场景三：导入格式错误**
- Given 上传文件含有格式错误的行
- When 文件上传并识别后
- Then 展示逐行错误提示，而非静默失败

**场景四：历史批次复用消费者**
- Given 管理员点击"选择历史批次"
- When 选择历史批次并确认
- Then 历史批次的消费者信息写回当前批次草稿，有效期/额度/邀请码状态不复用

---

### EP-04：报告与数据分析

---

#### F-04-01 / F-04-02：WriteUp & SpeakUp 报告分析

**Story 4.1.1**

> As an institution admin or teacher, I want to view the report list filtered by batch and consumer, so that I can monitor submission and scoring status.

**AC：**

**场景一：按批次筛选报告**
- Given 机构用户进入 WriteUp 报告分析页
- When 选择某批次并点击搜索
- Then 列表展示该批次下的所有提交记录，含作答状态和报告释放状态

**场景二：报告未生成时状态展示**
- Given 某条记录的说话已完成但报告尚未产出
- When 查看报告列表
- Then 报告状态显示"报告生成中"，释放状态暂不展示

---

#### F-04-03：报告详情

**Story 4.3.1**

> As an institution teacher, I want to view the full report detail for a consumer, so that I can review their performance and provide feedback.

**AC：**

**场景一：查看报告详情**
- Given 教师点击某条报告记录
- When 进入报告详情页
- Then 展示基础信息、作答结果、评分项、报告状态和释放记录

**场景二：手动释放报告**
- Given 批次配置为手动释放
- When 教师在报告详情页点击"释放报告"
- Then 弹出二次确认，确认后报告释放，记录释放时间和操作人

**场景三：已释放报告不可重复释放**
- Given 报告已处于"已释放"状态
- When 查看报告详情
- Then "释放报告"操作不展示，仅展示释放时间和操作人

---

#### F-04-04：批次分析视图

**Story 4.4.1**

> As an institution admin, I want to compare multiple batches with visual analytics, so that I can identify performance trends across different teaching groups.

**AC：**

**场景一：多批次选择对比**
- Given 管理员在批次分析页勾选 2 个以上批次
- When 点击"生成对比分析"
- Then 展示批次总览卡、维度雷达图、维度柱状图、分数分布和自动结论

**场景二：单批次时不触发对比**
- Given 管理员仅勾选 1 个批次
- When 点击"生成对比分析"
- Then 提示"请至少选择 2 个批次进行对比"

---

### EP-05：API 集成管理

---

#### F-05-01：机构侧 API 管理

**Story 5.1.1**

> As an institution admin, I want to manage API keys for product integration, so that I can securely connect external systems to the IELTS Up platform.

**AC：**

**场景一：查看 API Key**
- Given 机构管理员进入 API 管理页
- When 页面加载
- Then 按产品展示 API Key、Secret（默认脱敏）、Endpoint 和创建时间

**场景二：显示/复制敏感信息**
- Given 管理员点击某 Key 的"显示"按钮
- When 单独点击显隐切换
- Then 该行 Key 明文展示，其他行保持脱敏；提供"复制"按钮供直接复制

**场景三：重置 API Key**
- Given 管理员点击"重置"
- When 弹出确认框，确认旧密钥立即失效风险后确认
- Then 新 Key/Secret 生成，旧 Key 立即失效，页面展示新凭证

---

#### F-05-02：BC 侧 API 调用审计

**Story 5.2.1**

> As a BC admin, I want to audit API call records by institution, so that I can troubleshoot integration issues and monitor usage.

**AC：**

**场景一：查看调用记录**
- Given BC 管理员进入 API 调用记录页
- When 页面加载
- Then 展示请求时间、接口、调用方、结果、耗时和错误摘要

**场景二：过滤异常记录**
- Given 管理员需要排查异常
- When 筛选 4xx 或 5xx 状态记录
- Then 仅展示对应错误码的调用记录

**场景三：查看调用详情**
- Given 管理员点击某条调用记录
- When 展开或进入详情
- Then 展示完整请求上下文信息；不提供重放调用入口

---

## 8. Estimation（Planning Level）

### EP-01：用户认证与 RBAC 基础

| Story | Size | Points | Units | Effort | Complexity | Confidence | Notes |
|-------|------|--------|-------|--------|-----------|------------|-------|
| BC 内部用户登录 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含 Microsoft OAuth 集成 |
| 机构外部用户登录 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含首次激活流程、忘记密码 |
| 组织架构管理 | L | 5 | 4–8 | 2–4 days | High | Medium | 树形结构操作，含删除前校验 |
| 用户管理 | L | 5 | 4–8 | 2–4 days | Medium | Medium | 含组织角色绑定矩阵 |
| 角色权限管理 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含权限分组配置 |
| 修改密码 | S | 2 | 1–2 | 0.5–1 day | Low | Medium | 含密码强度校验 |

### EP-02：BC 商业化运营管理

| Story | Size | Points | Units | Effort | Complexity | Confidence | Notes |
|-------|------|--------|-------|--------|-----------|------------|-------|
| 客户管理（列表+创建+详情+编辑） | L | 5 | 4–8 | 2–4 days | Medium | Medium | 多页面、含弹窗与编辑态 |
| 合同管理（新增+编辑+补充协议） | XL | 8 | 8–16 | 4–8 days | High | Low | 多产品多周期复杂表单，关联订阅生成 |
| 订阅管理（列表+详情+权益+预警） | L | 5 | 4–8 | 2–4 days | Medium | Medium | 含多种状态展示和跳转 |
| 额度管理（充值+明细） | L | 5 | 4–8 | 2–4 days | High | Medium | 含幂等性单号校验、系统触发标记 |
| 机构管理员账号管理 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含激活邮件触发 |
| 席位管理 | S | 2 | 1–2 | 0.5–1 day | Low | Medium | 简单扩充表单 |
| 服务调整 | S | 2 | 1–2 | 0.5–1 day | Low | Medium | 含影响说明弹窗 |
| BC 成员管理 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 复用 RBAC 组件，数据范围限 BC |

### EP-03：机构内容与发码管理

| Story | Size | Points | Units | Effort | Complexity | Confidence | Notes |
|-------|------|--------|-------|--------|-----------|------------|-------|
| 机构首页与数据看板 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含差异化快捷入口，需角色判断 |
| 额度总览与消耗记录 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含数据口径与 BC 侧对齐 |
| WriteUp 题库管理 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含上下文筛选保留 |
| SpeakUp 题库管理 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含结构约束禁用态 |
| ScoreUp 题库（只读） | XS | 1 | 0.5–1 | 0.25–0.5 day | Low | Medium | 仅只读展示 |
| 试卷管理（创建+列表+详情+停用） | XL | 8 | 8–16 | 4–8 days | High | Low | 手动组卷+结构校验+拖拽顺序 |
| 邀请码批次管理（创建+列表+详情+回收） | XL | 8 | 8–16 | 4–8 days | High | Low | 三段式表单+额度联动+多状态管理 |
| 消费者信息管理（手动+导入+历史复用） | L | 5 | 4–8 | 2–4 days | High | Medium | 含文件上传解析+逐行错误提示 |

### EP-04：报告与数据分析

| Story | Size | Points | Units | Effort | Complexity | Confidence | Notes |
|-------|------|--------|-------|--------|-----------|------------|-------|
| WriteUp 报告分析列表 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含多维度筛选和状态展示 |
| SpeakUp 报告分析列表 | S | 2 | 1–2 | 0.5–1 day | Low | Medium | 与 WriteUp 结构复用 |
| 报告详情（含手动释放） | L | 5 | 4–8 | 2–4 days | Medium | Medium | 含高风险操作二次确认 |
| 批次分析视图（对比分析） | XL | 8 | 8–16 | 4–8 days | High | Low | 图表可视化+多批次联动对比 |

### EP-05：API 集成管理

| Story | Size | Points | Units | Effort | Complexity | Confidence | Notes |
|-------|------|--------|-------|--------|-----------|------------|-------|
| 机构侧 API 管理 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含脱敏展示+重置高风险操作 |
| BC 侧 API 调用审计 | M | 3 | 2–4 | 1–2 days | Medium | Medium | 含异常过滤+详情展开 |

---

## 9. Notes for Engineering

### EP-01：认证与 RBAC

- 系统边界：RBAC 为全平台共用能力，必须在数据库层级支持多租户隔离（BC 与各机构租户数据隔离）
- 集成点：Microsoft OAuth 2.0 / OIDC 集成（内部用户），邮件服务（激活邮件、重置密码）
- 主要复杂点：用户-组织-角色的三维绑定矩阵；菜单权限/操作权限/数据范围权限三层控制
- 潜在风险：RBAC 设计若不先做领域模型评审，权限矩阵落地时易反复；数据范围权限（租户隔离）需严格测试
- Refinement 重点：权限矩阵表（BC 管理员/BC 运营/机构管理员/助教/教师 × 所有模块/操作）

### EP-02：BC 商业化运营管理

- 系统边界：合同管理 Own by 此平台；可能与 MIS2 / IOC admin 存在数据同步需求，需明确是否有集成点
- 集成点：邮件服务（激活通知），审计日志服务
- 主要复杂点：合同多产品多周期存储建模；订阅自动生成逻辑；额度幂等性充值；合同变更对订阅的级联影响
- 潜在风险：客户→机构→合同→订阅→额度的实体关系模型需提前 ERD 评审；充值幂等逻辑必须在数据库层保证唯一性
- Refinement 重点：领域模型 ERD、合同签订即生效的状态机流转图

### EP-03：机构内容与发码管理

- 系统边界：题库内容 Own by 本平台（手动管理）；试卷/批次/邀请码 Own by 本平台；消费者使用邀请码登录产品端属于产品侧（Speakup/Writeup/Scoreup portal）
- 集成点：Speakup / Writeup / Scoreup 产品端（邀请码验证、报告回传）；文件上传服务（消费者导入模板）
- 主要复杂点：邀请码批次额度联动实时计算；消费者文件解析及逐行错误处理；SpeakUp 试卷结构校验规则
- 潜在风险：邀请码与产品端登录的接入协议需与 Speakup/Writeup/Scoreup 对标；消费者绑定模型首期按"邀请码绑定"处理，后期如需独立学员档案需重构
- Refinement 重点：邀请码状态机、批次-邀请码-消费者-报告关系模型

### EP-04：报告与数据分析

- 系统边界：报告数据由各产品端（Speakup/Writeup）生成并回传，本平台 Own 报告查看与释放控制；评分逻辑 Not Own
- 集成点：Speakup AI scoring callback、Writeup AI scoring callback；报告回传 webhook / polling 策略待定
- 主要复杂点：手动释放报告的状态控制；批次对比分析图表数据聚合（雷达图、柱状图、分数分布）
- 潜在风险：报告字段统一结构需与各产品端协商对齐，否则字段语义不一致；图表库选型和数据量级对性能影响
- Refinement 重点：报告回传接口协议、报告状态机、释放操作幂等性

### EP-05：API 集成管理

- 系统边界：API Key 管理 Own by 本平台；调用审计记录 Own by 本平台；具体 API 业务逻辑 Not Own
- 集成点：API Gateway 层（记录调用日志）
- 主要复杂点：API Secret 的安全存储（加密存储 + 脱敏展示）；重置 Key 后旧 Key 立即失效的一致性保证
- 潜在风险：API Key 重置操作对正在使用中的机构影响大，需有清晰的失效通知机制
- Refinement 重点：API Key 加密策略、调用日志存储和查询性能

---

## 10. Business Process Flow

### 10.1 Happy Path：客户签约到机构开通

```
BC 运营 → 创建客户档案
  → 填写合同（多产品+多周期+服务截止时间）
  → 系统自动生成生效中订阅 + 权益配置
  → BC 管理员创建机构初始管理员账号
  → 系统发送激活邮件
  → 机构管理员激活账号 → 登录机构视角
  → 基于 RBAC 创建本机构用户和组织结构
  → 开始使用题库、试卷、发码功能
```

### 10.2 Exception Path 1：合同服务截止时间未填写

```
BC 运营 → 填写合同时未填服务截止时间
  → 点击提交
  → 系统校验失败 → 提示"服务截止时间为必填项"
  → 运营返回补填 → 重新提交 → 合同创建成功
```

### 10.3 Exception Path 2：额度不足时创建邀请码批次

```
机构管理员 → 进入批次创建页
  → 选择产品，输入邀请码数量
  → 系统实时检测 → 可用额度不足
  → 提示"剩余额度不足，当前可用 X 个"
  → 保存按钮不可用
  → 管理员联系 BC 充值额度 → 额度到账后重新操作
```

### 10.4 Exception Path 3：报告回传失败

```
消费者完成考试 → 产品端提交作答
  → AI 评分服务处理 → callback 回传失败
  → 系统记录回传失败状态 → 触发重试机制（指数退避）
  → 超过最大重试次数 → 报告状态标记为"生成失败"
  → 运营侧可查看异常状态并人工触发补偿
```

---

## 11. GWT Scenarios

### Scenario 1（Happy Path）：机构管理员成功创建邀请码批次

- **Given** 机构管理员已登录，当前 WriteUp 可用额度为 50，已有可用试卷
- **When** 管理员创建 WriteUp 批次，绑定试卷，输入 20 个邀请码，设置有效期，通过确认页提交
- **Then** 批次创建成功，生成 20 个邀请码，额度减少 20，批次状态为"待生效"或"进行中"

### Scenario 2（Failure Path）：BC 运营尝试重复使用同一充值单号

- **Given** BC 运营在订阅详情页操作充值，某收据编号已被录入过一次
- **When** 再次使用同一收据编号提交充值
- **Then** 系统检测到重复依据，提示"该依据单号已被使用，请检查后重新输入"，本次充值被阻断

### Scenario 3（Edge Case）：教师创建 SpeakUp 试卷时选题不满足结构要求

- **Given** 教师进入创建试卷页，选择 SpeakUp 产品
- **When** 只选了 2 道 Part 1 题目和 0 道 Part 2&3，点击保存
- **Then** 保存按钮不可用，显示"SpeakUp 试卷需包含 3 道 Part 1 + 1 道 Part 2&3"

### Scenario 4（Edge Case）：机构管理员账号未激活时登录

- **Given** BC 创建了机构管理员账号，但该管理员尚未点击激活链接
- **When** 管理员尝试使用初始凭证登录
- **Then** 系统识别账号未激活，引导进入激活流程后才允许进入机构视角

### Scenario 5（Edge Case）：权限不足的教师角色尝试进入批次创建页

- **Given** 登录用户角色为机构教师
- **When** 尝试直接访问邀请码批次创建页
- **Then** 页面展示无权限态，不展示创建表单，提示"您没有权限执行此操作"

---

## 12. Roadmap with Phases

### MVP（Phase 1）— 核心商业化链路

**目标：** 支持 BC 完成机构开通，机构完成发码和基础报告查看

| 模块 | 范围 |
|------|------|
| 认证 | BC 内部登录 + 机构外部登录 + 修改密码 |
| RBAC | 用户/组织/角色管理（共用组件） |
| 客户管理 | 列表 + 创建 + 详情 + 编辑 |
| 合同管理 | 新增合同（多产品+多周期） + 订阅自动生成 |
| 额度管理 | 充值 + 明细 |
| 机构管理员开通 | 创建账号 + 激活邮件 |
| 机构首页 | 基础看板 + 快捷入口 |
| 额度总览 | 产品维度展示 |
| 题库 | WriteUp + SpeakUp 查看（手动题库） |
| 试卷管理 | 创建（手动组卷）+ 列表 + 详情 |
| 邀请码批次 | 创建 + 列表 + 详情 + 回收 |
| 消费者信息 | 手动录入 + 文件导入 |
| 报告 | WriteUp + SpeakUp 列表 + 报告详情 + 手动释放 |
| API 管理（机构侧） | Key 查看 + 重置 |

### Phase 2 — 完善运营管理能力

| 模块 | 范围 |
|------|------|
| 合同管理 | 补充协议支持 + 合同编辑增强 |
| 订阅管理 | 到期预警 + 历史协议时间线 |
| 服务调整 | API 服务开关管理 |
| 席位管理 | 扩充席位流程 |
| 批次分析视图 | 多批次对比分析 + 图表可视化 |
| BC 侧 API 审计 | 调用记录 + 异常过滤 |
| BC 成员管理 | 内部组织/用户/角色管理 |

### Future Extension — 延后能力

| 模块 | 说明 |
|------|------|
| 规则/模板组卷 | 首期不支持 |
| 试卷审核/发布/版本 | 首期不支持 |
| ScoreUp 手动组卷 | 首期以资源包方式管理 |
| 机构级导出报告 | 首期不支持 |
| 跨批次趋势分析 | 首期不支持 |
| 独立学员档案/班级管理 | 首期以邀请码消费者绑定替代 |
| 合同续费独立状态 | 首期通过新增合同或新周期处理 |
| 一客户多机构 | 首期按一一对应处理 |

---

## 13. User Journey / User Flow

### BC 运营人员：完成机构开通

```
Step 1: 登录 BC 视角 → 企业邮箱/Microsoft 登录 → 进入客户管理页
Step 2: 创建新客户 → 填写客户名称、联系人 → 提交
Step 3: 进入客户详情 → 点击新增合同
Step 4: 填写合同 → 选择产品、配置周期和额度、填写服务截止时间 → 提交
Step 5: 系统自动生成订阅 → 返回客户详情确认订阅状态
Step 6: 进入订阅详情 → 创建机构管理员账号 → 系统发送激活邮件
Step 7: 通知机构管理员激活并登录
```

### 机构管理员：发放邀请码并追踪结果

```
Step 1: 激活账号 → 登录机构视角 → 首页查看数据看板
Step 2: 进入题库 → 选题 → 创建试卷 → 通过结构校验 → 保存
Step 3: 进入邀请码批次管理 → 创建批次 → 绑定产品和试卷 → 输入数量和有效期
Step 4: 绑定消费者信息（手动录入或文件导入）→ 预览确认 → 创建成功
Step 5: 线下发放邀请码给消费者 → 消费者登录产品端完成考试
Step 6: 进入报告分析页 → 按批次筛选 → 查看报告状态 → 进入报告详情
Step 7: 执行手动释放（如适用） → 确认后报告对消费者端可见
```

---

## 14. Non-functional Requirements

### Performance

| 要求 | 指标 |
|------|------|
| 列表页加载时间 | < 2 秒（正常网络条件） |
| 创建批次（含额度计算）响应 | < 3 秒 |
| 报告详情加载 | < 3 秒 |
| API 调用审计列表（百万条级） | 支持分页查询，单页 < 2 秒 |

### Compatibility

| 要求 | 说明 |
|------|------|
| 浏览器支持 | Chrome / Edge 最新两个版本 |
| 屏幕分辨率 | 最低 1280 × 800，桌面端优先 |
| 移动端 | 首期不作为主要支持目标 |

### Data Limits

| 对象 | 限制 |
|------|------|
| 消费者导入文件 | 单次不超过 1000 条 / 文件 < 5MB |
| 单批次邀请码数量 | 最大 500 个（建议值，需研发确认） |
| 合同补充协议数量 | 无硬限制，建议前端显示上限提示 |

### Retry Strategy

| 场景 | 策略 |
|------|------|
| 报告回传失败 | 指数退避，最大重试 3 次，最终失败标记异常状态 |
| 充值接口超时 | 幂等重试，依赖唯一依据单号防重入 |
| 激活邮件发送失败 | 支持手动重新触发，系统自动重试一次 |

### Security

| 要求 | 说明 |
|------|------|
| 租户数据隔离 | 机构侧用户只能访问本机构数据，不可跨租户查询 |
| API Key 存储 | 加密存储，展示时脱敏 |
| 高风险操作 | 删除/停用/回收/重置必须二次确认并落审计日志 |
| 密码强度 | 必须包含大小写字母、数字，最小 8 位 |
| 防重复提交 | 充值操作使用依据单号唯一性校验；表单提交中按钮置为 loading 状态 |

### Logging & Monitoring

| 要求 | 说明 |
|------|------|
| 审计日志 | 高风险操作必须落审计记录（合同变更、额度充值/回收、账号停用/启用、角色变更） |
| 操作追踪 | 所有 Create/Update/Delete 操作需记录操作人 ID 和时间戳 |
| API 调用日志 | 记录 trace ID、接口、调用方、状态码、耗时、错误摘要 |
| 异常告警 | 报告回传持续失败需触发告警通知 |

---

## 15. Capacity Summary

| 维度 | 数值 |
|------|------|
| **Total Epics** | 5 |
| **Total Features** | 23 |
| **Total Stories** | ~35 |
| **Total Estimated Units** | 110 – 210 units |
| **Total Estimated Effort** | 55 – 105 days |
| **Suggested Sprint Count** | 6 – 10 sprints（2 周/sprint） |
| **Suggested Team Size** | FE 2 + BE 2 + QA 1 = 5 人 |

### Main Complexity Drivers

1. 合同-订阅-额度领域模型复杂度（多产品、多周期、补充协议）
2. 邀请码批次三段式表单 + 额度实时联动 + 消费者绑定
3. 试卷手动组卷的结构校验（SpeakUp Part 规则）
4. RBAC 多维权限矩阵（三层：菜单/操作/数据范围）
5. 报告回传集成（AI scoring callback）
6. 批次对比分析图表可视化

### Main Assumptions

- Speakup / Writeup / Scoreup 产品端已有稳定的 API 接口可对接
- 报告回传采用 webhook callback 方式（需研发确认是否有 polling 备选）
- Microsoft OAuth 集成使用公司现有 Azure AD 租户
- 邮件服务由公司现有邮件基础设施提供
- 合同/订阅数据与 IOC Admin / MIS2 不做实时同步（首期独立管理）

---

## 16. Estimation Disclaimer

> **重要说明：**
>
> 以上所有估算属于 **PRD 阶段的 Planning-level Estimation**，仅用于以下目的：
>
> - 范围判断与优先级排序
> - 资源与团队容量规划
> - 版本迭代周期预估
> - 提前识别高复杂度工作项
>
> 本估算**不代表研发最终承诺**，不等同于精确任务拆分和 Sprint 承诺。
>
> 最终单个任务的工时估算和 Sprint 分配，须经 **Engineering Reviewer / Task Planner** 进行详细 refinement 后确定。
>
> 当前估算 Confidence 普遍为 **Medium / Low**，特别是以下部分：
> - 合同管理（依赖领域模型评审）
> - 试卷管理（依赖结构校验规则最终确认）
> - 邀请码批次管理（依赖产品端接口协议确认）
> - 批次对比分析（依赖图表技术选型）

---

## 17. Future Extension Ideas

| 方向 | 说明 |
|------|------|
| 独立学员档案 | 支持学员跨批次追踪和历史成绩汇总，适合学校场景 |
| 班级编排与排课 | 基于学员档案的班级管理，与邀请码批次结合 |
| 合同续费自动化 | 到期前自动提醒并支持一键续签 |
| 机构侧批量导出报告 | 支持按批次或时间段导出 PDF/Excel 报告 |
| 跨批次趋势分析 | 长周期内机构整体成绩趋势与 CEFR 等级分布 |
| 规则/模板组卷 | 按难度/题型规则自动生成试卷 |
| 审核/发布/版本机制 | 题库和试卷引入审核流与版本控制 |
| TOC 打通 | B2B 机构数据与 Mini Program 用户档案关联（CEFR 等级打通） |
| 多机构并行客户 | 一个客户对应多个机构实体（适合连锁教育集团） |
| Open API 文档自助发布 | 机构技术团队直接在平台查看更新的接口文档 |
