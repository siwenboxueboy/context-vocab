# Learning Science Kernel & API-First Architecture

> Context Vocab 的生产端不应该由某个具体用户界面反向定义，而应该由语言学、二语习得、学习科学和可验证的教学实践共同驱动。前端只是这些学习能力的消费者。

---

## 1. 核心架构原则

系统采用以下原则：

> **Science-driven core, API-first platform, experience-driven clients.**

也就是：

```text
Learning Science / Linguistics
            ↓
Domain Model & Rules
            ↓
Learning Science Kernel
            ↓
Semantic / Scenario / Learning Engines
            ↓
Stable Domain APIs
            ↓
Product Experience Layer
```

生产端负责构建“学习能力”，用户端负责把这些能力组织成“学习体验”。

因此，以下对象不应该由某个页面或交互方式决定：

```text
Semantic Cluster 怎么定义
Semantic Boundary 怎么定义
Scenario 怎么建模
Difficulty 怎么评估
Mastery 怎么判断
什么时候应该升难度
什么时候应该复习
什么时候认为发生了迁移
```

这些应该首先来自可解释、可验证的学习理论与语言知识模型。

---

# 2. 三层结构

## 2.1 Learning Science Kernel

这是系统最底层的领域核心。

它定义：

```text
什么是一个有效的 Semantic Cluster
什么是一个有效的 Semantic Boundary
什么样的 Scenario 能真正承载某个语义边界
什么叫 Difficulty
什么叫 Mastery
什么叫 Transfer
什么叫 Spontaneous Production
```

建议核心领域对象：

```text
CommunicativeIntent
ExpressionUnit
SemanticCluster
SemanticBoundary
SemanticDimension
ScenarioFamily
ScenarioTemplate
ScenarioInstance
LearningObjective
TrainingTask
LearnerState
MasteryState
EvaluationResult
LearningEvent
```

这一层即使没有任何 App，也应该可以独立成立。

---

## 2.2 AI-assisted Engines

AI 是能力实现层，而不是理论定义层。

主要引擎：

```text
Semantic Engine
Scenario Engine
Learning Engine
Retrieval / Navigator Engine
Evaluation Engine
```

AI 在这些引擎中承担：

```text
发现
生成
解释
评估
推理
个性化
```

但它必须工作在 Kernel 规定的规则空间内。

核心原则：

> **Theory defines the space; AI operates inside the space.**

例如：

Kernel 规定一个 Core Semantic Cluster 必须满足：

```text
Same Intent
Confusable
Distinguishable
Scenario Separability
```

AI 可以帮助：

```text
寻找候选表达
提取区别维度
分析词对边界
给指标打分
生成验证场景
```

但 AI 不应该自己重新定义“什么叫 Semantic Cluster”。

---

## 2.3 Product Delivery Layer

到这一层才出现具体产品交互：

```text
Ask
Explore
Practice
Voice
Semantic Cluster View
Scenario View
Learning Progress
Learning Memory
```

客户端通过 API 组合生产端能力。

因此：

> **生产端输出学习语义，前端决定交互表达。**

例如生产端返回：

```json
{
  "boundary": {
    "expression_a": "angry",
    "expression_b": "resentful",
    "dimensions": [
      "duration",
      "perceived_injustice"
    ],
    "relation_type": "PRAGMATIC"
  }
}
```

前端可以把它展示成：

```text
⏱️ 长期积累
⚖️ 不公平感
```

也可以：

- 显示成语义图谱；
- 通过 AI 对话解释；
- 转化成语音训练；
- 放进场景选择题。

但生产端不应该知道前端最终使用了什么图标、卡片或页面布局。

---

# 3. API-first，而不是 Page-first

错误方向：

```text
前端需要 angry 页面
        ↓
后端设计 angry_page 接口
        ↓
为页面不断添加字段
```

这种设计会导致生产端被当前 UI 结构污染。

更合理的是暴露领域能力：

```text
getSemanticCluster()
getSemanticBoundary()
searchExpressions()
getRelatedScenarios()
generateTrainingTask()
evaluateExpression()
getLearnerState()
recordLearningEvent()
```

页面只是这些能力的组合。

---

# 4. 建议的领域 API

## 4.1 Semantic API

负责语义知识能力：

```text
GET  /semantic/clusters/{id}
GET  /semantic/clusters/{id}/boundaries
GET  /semantic/expressions/{id}
GET  /semantic/expressions/{id}/neighbors
GET  /semantic/boundaries/{id}
POST /semantic/search
```

自然语言搜索也可以进入：

```text
“我想表达长期被不公平对待后的不爽”
```

返回相关：

```text
Semantic Cluster
Expression Units
Semantic Boundaries
Learning Metadata
```

---

## 4.2 Scenario API

负责把语义边界映射到真实使用环境：

```text
GET  /scenarios/{id}
POST /scenarios/search
POST /scenarios/generate
GET  /semantic/boundaries/{id}/scenarios
GET  /scenario-families/{id}
```

Scenario System 是学习脚手架，而不是知识核心。

因此核心关系应当是：

```text
Semantic Boundary
        ↓
Boundary ↔ Scenario Mapping
        ↓
Scenario Template / Instance
```

场景数量本身不是价值核心，能否精准承载目标语义边界才是。

---

## 4.3 Learning API

负责具体用户的训练状态与任务编排：

```text
POST /training/next
POST /training/evaluate
GET  /learners/{id}/profile
GET  /learners/{id}/weak-boundaries
GET  /learners/{id}/history
GET  /learners/{id}/mastery
POST /learning-events
```

Learning Engine 根据：

```text
Learner State
+
Semantic Boundary
+
Scenario Difficulty
+
Learning Objective
```

决定下一步训练任务。

---

## 4.4 Navigator API

AI Learning Navigator 是用户自然语言入口背后的统一编排能力：

```text
POST /navigate
```

输入可能是：

```text
“angry 和 resentful 有什么区别？”

“昨天那个表示忍无可忍的短语是什么？”

“给我练几个职场里表达不满的场景。”
```

Navigator 负责组合：

```text
Semantic Retrieval
+
Scenario Retrieval
+
Learning Memory
+
Learner State
```

并判断应该返回：

```text
Lookup
Explore
Review
Practice
Training
```

Navigator 负责“去哪里”，Trainer 负责“怎么教”。

---

# 5. Learning Metadata 与 UI Label 分离

生产端维护的是结构化 Learning Metadata：

```text
usage_domain
learning_goal
exam_track
register
frequency
cefr_level
communicative_intent
```

例如：

```json
{
  "usage_domains": ["daily", "workplace"],
  "learning_goals": ["speaking"],
  "exam_tracks": ["CET6"],
  "registers": ["neutral"],
  "frequency": "high",
  "cefr_levels": ["B1", "B2"]
}
```

这些是领域数据。

前端显示：

```text
[职场] [常用] [CET6] [B2]
```

只是展示策略。

原则：

> **Metadata is domain data; UI labels are presentation choices.**

---

# 6. 生产端不由用户端交互直接控制

用户行为非常有价值，但它应该进入“验证与研究”链路，而不是直接定义底层模型。

正确链路：

```text
Theory
  ↓
Domain Model
  ↓
Learning Experiment
  ↓
User Interaction Data
  ↓
Evaluation / Research
  ↓
Model Revision
```

而不是：

```text
用户更喜欢某个页面
↓
直接改变 Semantic Cluster 定义
```

用户数据可以帮助验证：

```text
某条 Boundary 是否真的容易混淆
某类 Scenario 是否真的具有区分度
某个难度梯度是否合理
某种训练方式是否提高迁移能力
```

但底层定义需要经过科学解释与验证。

---

# 7. 研究与产品之间的反馈闭环

生产端应该允许数据反哺理论，但方式是“证据驱动的修正”。

```text
Learning Science Hypothesis
        ↓
System Rule
        ↓
Training Experiment
        ↓
Learning Event Data
        ↓
Outcome Analysis
        ↓
Confirm / Refine / Reject
```

例如：

假设：

```text
angry ↔ resentful
主要学习边界是：
即时愤怒 vs 长期积累的不公平感
```

系统可以通过真实用户训练数据验证：

```text
用户在哪类 Scenario 中最容易混淆？
更换 Domain 后是否还能保持正确判断？
Free Production 中是否能主动召回 resentful？
经过 Contrastive Training 后错误率是否下降？
```

这才是底层模型真正迭代的依据。

---

# 8. Semantic Core、Scenario Layer、Learning Runtime 的重新定位

系统应明确：

```text
Semantic Cluster
= 知识组织核心

Semantic Boundary
= 真正需要被掌握的知识

Scenario System
= 语义边界的表现、观测、训练和验证环境

Learner Model
= 用户当前能力状态

AI Trainer
= 学习控制器
```

因此：

> 场景是用户体验的重要中心，但不是知识中心。

最重要的生产资产优先级更接近：

```text
1. Semantic Cluster / Boundary Network
2. Boundary ↔ Scenario Mapping
3. Real Learner Confusion Data
4. Learner Mastery / Transfer Data
5. Utterance Pattern Assets
6. Scenario Library Size
```

不是场景越多越好，而是语义边界越可靠、场景越能精准训练这些边界越好。

---

# 9. AI 的正确定位

完整系统中 AI 可以承担：

```text
AI Semantic Builder
AI Scenario Builder
AI Judge
AI Evaluator
AI Learning Navigator
AI Trainer
AI Difficulty Adapter
```

但是需要统一遵循：

```text
AI generates
AI reasons
AI evaluates

System constrains
System validates
System persists
System versions
System governs
```

即：

> **AI generates; system governs.**

进一步：

> **Learning science defines the system; AI operationalizes it.**

---

# 10. 最终架构

```text
                    Learning Science
                          ↓
                  Domain Model / Rules
                          ↓
                 Learning Science Kernel
                          ↓
       ┌──────────────────┼──────────────────┐
       ↓                  ↓                  ↓
 Semantic Engine     Scenario Engine     Learning Engine
       ↑                  ↑                  ↑
       └──────────── AI-assisted ────────────┘
                          ↓
                     Domain APIs
                          ↓
              Product Experience Layer
                          ↓
      Ask / Explore / Practice / Voice / Other Clients
```

同时：

```text
User Interaction
        ↓
Learning Events / Telemetry
        ↓
Research & Evaluation
        ↓
Evidence-based Model Revision
        ↓
Learning Science Kernel
```

形成长期迭代闭环。

---

# 11. 一句话结论

Context Vocab 的生产端不是某个 App 的“内容后台”。

它应该逐步演化成：

> **一套由学习科学驱动、由 AI 实现和增强、通过领域 API 对外提供能力的英语语义表达学习基础设施。**

用户端只是其中一个客户端。

因此整个系统的长期架构原则是：

> **Science-driven core. AI-assisted engines. API-first platform. Experience-driven clients.**
