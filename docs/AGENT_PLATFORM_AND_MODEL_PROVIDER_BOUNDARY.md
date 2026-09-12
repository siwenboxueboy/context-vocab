# 英语学习 Agent 平台与模型供应商边界

## 1. 文档目的

本项目的技术定位不是“自研英语学习大模型”，而是：

> **基于外部通用大模型能力构建英语学习 Agent 平台。**

模型负责提供通用语言智能；系统负责提供学习科学、领域知识、用户状态、工具、记忆、检索、策略与工作流。

核心原则：

> **Model is replaceable; learning system is the product.**
>
> 模型是可替换的智能供应商，学习系统才是产品本身。

---

## 2. Agent 与 Model 的边界

### 2.1 Model 是什么

外部模型主要提供：

- 自然语言理解
- 文本生成
- 语义推理
- 场景生成
- 自由表达评估
- 结构化输出
- Embedding 等通用 AI 能力

模型本身不应该拥有：

- Semantic Cluster
- Semantic Boundary
- Scenario System
- Learner State
- Learning Memory
- Adaptive Difficulty
- Training Policy
- 用户长期学习历史
- 产品业务规则

因此：

> **模型负责“智能计算”，系统负责“业务事实与学习状态”。**

### 2.2 Agent 是什么

本项目中的 Agent 不是单纯的一次模型调用，而是：

```text
Agent =
Model
+ Tools
+ Context
+ Memory
+ Domain Knowledge
+ Learning Rules
+ Workflow
+ State
```

一个完整的学习 Agent 执行过程可能是：

```text
用户输入
↓
理解学习意图
↓
读取个人学习状态
↓
检索公共语义知识
↓
选择工具 / 学习策略
↓
调用外部模型
↓
评估模型输出
↓
形成学习反馈
↓
更新 Learner State
↓
决定下一步学习动作
```

因此，大模型只是 Agent Runtime 中的一个能力节点，而不是整个产品。

---

## 3. 平台总体结构

```text
                 English Learning Agent Platform
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
   Domain Knowledge      Learner State      Learning Policy
          │                   │                   │
          └───────────────────┼───────────────────┘
                              ↓
                        Agent Runtime
                              │
          ┌───────────────────┼───────────────────┐
          ↓                   ↓                   ↓
        Tools             Retrieval             Memory
          │                   │                   │
          └───────────────────┼───────────────────┘
                              ↓
                        Model Gateway
                              ↓
         OpenAI / DeepSeek / Anthropic / Other Providers
```

其中：

- **Domain Knowledge**：公共语义知识、场景资产、表达模式等
- **Learner State**：某个用户当前学习到什么程度
- **Learning Policy**：学习科学与训练策略
- **Agent Runtime**：负责执行当前学习任务
- **Tools**：调用内部领域能力或外部工具
- **Retrieval**：查公共知识与用户学习记忆
- **Memory**：保留用户长期学习状态与上下文
- **Model Gateway**：隔离不同 AI 模型供应商

---

## 4. Model Gateway

### 4.1 为什么需要 Gateway

业务模块不应该直接依赖某一家模型厂商的 SDK。

错误方式：

```text
Semantic Builder
↓
直接调用 OpenAI SDK
```

正确方式：

```text
Semantic Builder
↓
Language Model Interface
↓
Model Gateway
↓
具体 Provider
```

这样可以做到：

- 切换模型而不修改领域逻辑
- 不同任务使用不同模型
- 统一限流、重试、超时、成本统计
- 统一日志和可观测性
- 统一结构化输出约束
- 降低供应商锁定

### 4.2 建议抽象能力

概念接口可以包含：

```text
LanguageModel
├── invoke
├── structured_output
├── stream
└── usage

EmbeddingModel
├── embed_query
└── embed_documents
```

具体实现：

```text
OpenAIProvider
DeepSeekProvider
AnthropicProvider
OtherProvider
```

这里的 Provider 只是基础设施适配器，不允许承载学习业务规则。

---

## 5. 不同 Agent 角色

为了避免出现“一个万能 AI Agent”，建议按职责拆分 Agent 角色。

### 5.1 Semantic Builder Agent

负责：

- 发现候选表达
- 构建候选 Semantic Cluster
- 分析 Semantic Boundary
- 提取 Semantic Dimension
- 生成结构化候选知识

它不拥有最终 Semantic 数据。

正式知识仍然属于 Semantic Domain。

### 5.2 Scenario Builder Agent

负责：

- 根据 Boundary 构建场景
- 生成 Teaching / Contrastive / Transfer / Production Scenario
- 控制场景变量与难度

它不能重新定义语义知识。

### 5.3 Learning Navigator Agent

负责：

- 理解用户当前意图
- 判断用户是要查询、探索、训练、复习还是回忆历史
- 路由到 Semantic Retrieval、Scenario Retrieval、Learning Memory 或 Training Runtime

它负责“用户现在想做什么”，不负责“应该怎么教”。

### 5.4 Training Agent

负责：

- 根据 Semantic Knowledge、Scenario Assets、Learner State 和 Learning Policy
- 选择训练目标
- 调整难度
- 控制提示
- 决定何时迁移、复习、进入自由表达

### 5.5 Evaluation Agent

负责：

- 判断用户回答是否表达正确
- 识别语义偏差
- 判断自然度、语域、搭配
- 识别具体薄弱 Semantic Boundary

它负责“诊断”，Training Agent 再决定“下一步怎么教”。

---

## 6. AI 生成内容的治理原则

外部模型的输出不能直接等于系统事实。

标准流程：

```text
Generate
↓
Judge
↓
Validate
↓
Approve
↓
Persist
```

对应中文：

```text
AI 生成候选
↓
AI / 规则评审
↓
学习科学与领域规则校验
↓
审核通过
↓
写入正式领域数据
```

因此：

> **AI generates; system governs.**
>
> AI 负责生成与理解，系统负责约束、验证、持久化与治理。

---

## 7. 我们真正拥有的核心资产

本项目的长期资产不应该依赖某个具体模型。

核心资产包括：

1. Semantic Boundary Network
2. Semantic Cluster / Expression Knowledge
3. Boundary ↔ Scenario Mapping
4. Learning Science Policies
5. Learner State Model
6. Personal Learning Memory
7. 用户真实错误与混淆数据
8. Agent Workflow
9. Evaluation Rubrics
10. Tool Contracts
11. Retrieval / Context Assembly Strategy
12. 内容生成与审核流程

模型升级时，这些资产继续保留，并能直接获得更强的智能能力。

---

## 8. 与业务边界文档的关系

该文档只定义：

> **Agent 平台和外部模型之间的职责边界。**

具体公共学习数据、用户学习数据、传统业务数据的 Owner 与写权限，继续遵循：

`BUSINESS_BOUNDARY_AND_DATA_OWNERSHIP.md`

核心关系：

```text
外部模型
= 通用智能供应商

Learning Platform
= 领域知识 + 学习规则 + 用户状态 + Agent Runtime

Business Platform
= 用户 / 会员 / 支付 / 权益等传统业务
```

---

## 9. 最终技术定位

Context Vocab 的技术定位为：

> **一个由学习科学驱动、以结构化语言知识与用户学习状态为核心、通过外部主流大模型 API 获得通用智能能力的英语学习 Agent 平台。**

不是：

> 自研英语大模型。

也不是：

> 传统背词软件外挂一个聊天机器人。

而是：

> **让外部模型成为可替换的智能引擎，让领域知识、学习状态、工具、记忆、检索与训练策略构成真正的产品核心。**
