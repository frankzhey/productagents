

# BCCchina AI Product Agents 企业级 AI 产研协同



**Created:** April 13, 2026
**Tags:** AI Agents

---



# 系统官方使用手册

## 1. 系统架构与 Agent 矩阵 (System Architecture & Agent Matrix)

系统核心理念：

> **“指令即治理（Instruction = Governance）”**

核心目录：

```
.github/agents/
```

### 架构层次

#### 1️⃣ 全局层

```
copilot-instructions.md
```

* 定义全局工程规范
* 产品术语体系
* 所有 Agent 的基础约束

#### 2️⃣ 专项层

```
instructions/*.md
```

* frontend
* engineering
* product

#### 3️⃣ 执行层

```
.github/agents/
```

* 具体 Agent 执行任务

---

## 2. Agent 矩阵职责定义

| Agent                    | 职责                |
| ------------------------ | ----------------- |
| product-planner.agent.md | PRD、Story、范围粗估    |
| story-splitter.agent.md  | Story 拆分          |
| ux-prototyper.agent.md   | UI 原型 + HTML      |
| eng-reviewer.agent.md    | 技术评审 / API / 数据模型 |
| task-planner.agent.md    | Task 精细拆分 + Unit  |
| wiki-publisher.agent.md  | 知识沉淀 / Wiki       |

---

# 三、标准六步产研工作流

> 从“非结构化需求” → “结构化代码任务”

## Step 1：需求分析与规划

* Agent：Product Planner
* 输出：

  * PRD
  * Story List
  * 粗估范围

---

## Step 2：UI 原型生成

* Agent：UX Prototyper
* 输出：

  * UI 结构说明
  * `/UI/*.html` 原型文件

---

## Step 3：技术评审（核心步骤）

* Agent：Eng Reviewer
* 输出：

  * API 契约
  * 数据模型
  * 技术方案

⚠️ **未评审不得进入开发**

---

## Step 4：任务拆分与精估

* Agent：Task Planner
* 输出：

  * Task 列表
  * 执行顺序
  * Unit / Days

---

## Step 5：文档沉淀（Feedback Loop）

* Agent：Wiki Publisher
* 输出：

  * 企业 Wiki
  * 可检索知识资产

---

# 四、估算体系（Estimation Framework）

## 双层估算机制

### 1️⃣ PRD 阶段（粗估）

* 用途：资源规划
* 类型：范围估算（Range）
* 非承诺

---

### 2️⃣ Task 阶段（精估）

* 用途：开发承诺
* 单位：Unit

---

## Unit 规则（核心）

```text
1 Unit = 0.5 人天（Days）
```

🚫 禁止：

* 模糊估算

✅ 目标：

* 100% 可追踪（Azure DevOps）

---

# 五、设计规范与 Instructions 体系

## 全局规范

* 产品哲学
* 工程标准
* ADO术语体系
* 状态流转

---

## 专项规范

### frontend.instructions.md

* UI组件规范
* HTML 原型标准

### engineering.instructions.md

* 技术选型
* 架构模式
* 代码质量

### product.instructions.md

* PRD标准
* AC规范（Given / When / Then）

---

# 六、典型应用场景

## 场景 A：新产品（Greenfield）

流程：

```
Step1 → Step6 全流程
```

重点：

* UI 原型优先

---

## 场景 B：已有功能优化

流程：

```
Wiki → Product Planner → 标准流程
```

价值：

* 避免历史逻辑丢失

---

## 场景 C：技术重构

流程：

```
Eng Reviewer → Task Planner
```

规则：

* 涉及 API 必须评审

---

# 七、最佳实践 & 管理红线

## ✅ 推荐行为

* 单一源头：必须从 PRD 开始
* AC 完整性：必须可验证
* Unit 强制：必须有估算
* Wiki 闭环：必须沉淀

---


