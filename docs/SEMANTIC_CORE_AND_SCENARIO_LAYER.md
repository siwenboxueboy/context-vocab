# Semantic Core & Scenario Layer

> 本文档补充 Context Vocab 的核心架构定位：**Semantic Cluster / Semantic Boundary 是知识核心，Scenario System 是承载、观测、训练语义知识的学习环境，AI 是知识构建与训练控制引擎。**

---

## 1. 核心定位

Context Vocab 不应把“场景库”理解为产品的核心知识资产。

更准确的关系是：

```text
Semantic Cluster
        ↓
Semantic Boundary
        ↓
Scenario System
        ↓
AI Training
        ↓
Spontaneous Production
```

其中：

```text
Semantic Cluster
= 学什么

Semantic Boundary
= 真正需要掌握什么区别

Scenario System
= 如何把这些区别表现出来、让用户感受到并被测试

AI Trainer
= 如何根据用户状态持续选择训练目标并推进掌握
```

因此，产品核心可以概括为：

> **以语义簇为知识核心，以场景系统为学习环境，以 AI 为知识构建和训练引擎。**

---

## 2. 为什么 Semantic Cluster / Boundary 才是核心

场景会变化，但表达之间的语义关系相对稳定。

例如：

```text
angry ↔ furious
主要边界：intensity

angry ↔ outraged
主要边界：perceived injustice / moral violation

angry ↔ resentful
主要边界：immediate anger vs lingering grievance
```

这些边界本身才是系统真正要沉淀的语言知识。

一个场景只是把其中某一条边界实例化。

例如：

```text
Boundary:
angry ↔ outraged

核心差异：
普通愤怒
vs
因不公 / 道德冒犯产生的愤怒
```

场景可以是：

```text
有人故意撞坏你的车
→ angry / furious

公司明知产品伤害消费者仍然隐瞒
→ outraged
```

场景可以无限变化，但它们都在服务同一个 Semantic Boundary。

因此：

> **Scenario 是 Semantic Knowledge 的实例化，而不是知识本体。**

---

## 3. Scenario System 的真正角色

Scenario System 仍然非常重要，但它的定位应该是“学习脚手架 / 运行环境”，而不是知识核心。

它承担三种核心职责：

### 3.1 知识载体

把抽象 Semantic Boundary 转换为用户可以感知的真实情境。

```text
Semantic Boundary
        ↓
Concrete Scenario
        ↓
用户产生直觉
```

### 3.2 知识观测器

通过用户在场景中的选择或表达，判断用户是否真正掌握目标边界。

```text
Scenario
→ User Response
→ Boundary Diagnosis
```

### 3.3 知识训练器

通过不断改变 Domain、Relationship、Register、Intensity 等变量，反复训练同一语义边界。

```text
同一 Boundary
↓
职场场景
↓
家庭场景
↓
社交场景
↓
正式场景
↓
自由表达
```

最终验证用户是否形成跨场景迁移能力。

---

## 4. Scenario Library 与 Scenario System 的区别

不应把 Scenario System 简化成“存很多题目的场景库”。

Scenario Library 只是 Scenario System 的一个组成部分。

完整 Scenario System 更应该包含：

```text
Scenario Taxonomy
Scenario Domain
Scenario Family
Scenario Template
Scenario Variables
Scenario Difficulty Model
Scenario Generation Rules
Scenario Validation Rules
Boundary ↔ Scenario Mapping
Concrete Scenario Library
```

其中真正关键的是：

> **Boundary ↔ Scenario Mapping**

系统必须知道：

```text
这个场景到底在训练哪一条 Semantic Boundary？
这个场景为什么能够暴露这条 Boundary？
用户答错时，暴露的是哪个语义维度没有掌握？
```

所以，场景数量本身不是核心价值。

核心是：

> **场景是否能够精准承载和训练目标语义边界。**

---

## 5. AI 在这里的三个角色

AI 贯穿整个系统，但不取代系统结构。

### 5.1 AI Semantic Builder

负责搭建语言知识核心：

```text
Expression Discovery
→ Semantic Cluster
→ Semantic Dimensions
→ Semantic Boundaries
→ Judge / Validate
→ Persist
```

### 5.2 AI Scenario Builder

负责把语义知识投射到场景环境：

```text
Semantic Boundary
+
Scenario Constraints
↓
Teaching Scenario
Boundary Scenario
Transfer Scenario
Production Scenario
```

### 5.3 AI Trainer

负责用户运行时教学：

```text
Learner State
+
Semantic Boundary
+
Scenario System
↓
下一训练任务
↓
用户回答
↓
语义诊断
↓
更新 Learner Model
```

因此可以概括成：

> **AI 负责建知识、建场景、做训练；系统负责约束、验证、沉淀与收敛。**

---

## 6. 核心架构关系

```text
                   AI FOUNDATION
                         │
             ┌───────────┼───────────┐
             ↓           ↓           ↓
      Semantic Builder  Scenario Builder  AI Trainer
             │           │           │
             ↓           │           │
      ┌───────────────────────┐      │
      │     SEMANTIC CORE     │      │
      │                       │      │
      │ Communicative Intent  │      │
      │ Semantic Cluster      │      │
      │ Expression Units      │      │
      │ Semantic Dimensions   │      │
      │ Semantic Boundaries   │      │
      └───────────┬───────────┘      │
                  │                  │
                  ↓                  │
      ┌───────────────────────┐      │
      │    SCENARIO LAYER     │◀─────┘
      │                       │
      │ Taxonomy              │
      │ Templates             │
      │ Variables             │
      │ Difficulty            │
      │ Boundary Mapping      │
      │ Concrete Scenarios    │
      └───────────┬───────────┘
                  │
                  ↓
      ┌───────────────────────┐
      │   LEARNING RUNTIME    │
      │                       │
      │ Learner Model         │
      │ Adaptive Training     │
      │ AI Evaluation         │
      │ Mastery Convergence   │
      └───────────┬───────────┘
                  │
                  ↓
        Spontaneous Production
```

---

## 7. 与 Learning Metadata 的关系

Semantic Core 和 Scenario Layer 都可以拥有 Learning Metadata。

但 Metadata 不属于语言知识本体，而是描述：

> 这份知识 / 场景适合什么学习目标和使用范围。

例如：

```text
Usage Domain
- daily
- workplace
- academic
- research

Exam Track
- CET-4
- CET-6
- IELTS
- TOEFL

Register
- informal
- neutral
- formal
- academic

Frequency
- high
- common
- medium
- low
```

因此：

```text
Semantic Knowledge
        +
Scenario Layer
        +
Learning Metadata
        +
User Goal Profile
        ↓
Personalized Training
```

Metadata 用于筛选和编排，而不是替代 Semantic Boundary。

---

## 8. 产品资产优先级

长期来看，系统真正有价值的资产优先级应当是：

```text
1. 高质量 Semantic Cluster / Boundary Network

2. Boundary ↔ Scenario Mapping

3. 真实用户 Confusion / Mastery 数据

4. AI Semantic / Scenario / Training Pipeline

5. Utterance Pattern 与真实表达资产

6. Concrete Scenario Library
```

这意味着：

> **不是场景越多，系统越有价值。**

而是：

> **语义边界越准确，场景越能精准承载这些边界，系统越有价值。**

---

## 9. 用户视角与系统视角

用户前台仍然应该以场景为中心。

因为用户最容易理解的是：

```text
发生了什么
→ 我现在该怎么说
```

但系统后台的中心是 Semantic Boundary：

```text
用户看到：Scenario

系统看到：Boundary Test
```

所以可以同时成立：

> **用户学习的是场景，系统学习和管理的是语义边界。**

场景是用户体验中心，但不是知识中心。

---

## 10. 最终架构原则

Context Vocab 的核心逻辑最终收敛为：

```text
Semantic Cluster
决定：学什么

Semantic Boundary
决定：真正要掌握什么区别

Scenario System
决定：如何把区别表现、训练和验证出来

Learning Metadata
决定：这份知识适合谁、什么目标、什么场景

AI Trainer
决定：当前这个用户下一步应该怎么练
```

最终目标始终不变：

> 当真实场景出现时，用户无需经过中文翻译或显式规则推理，就能够快速、自然、准确地调用合适的英语表达。
