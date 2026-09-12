# Business Boundary & Data Ownership

> 本文档定义 Context Vocab 的业务边界、数据边界和模块 Owner 规则。
>
> 核心原则：**业务边界决定“谁负责什么”，数据边界决定“谁拥有哪份数据、谁有权修改”。**

---

## 1. 为什么必须同时定义业务边界和数据边界

只定义业务边界，容易出现：

```text
模块看起来分开了
但数据库仍然互相直连、互相修改
```

只定义数据边界，容易出现：

```text
表分开了
但业务决策仍然散落在多个模块中
```

因此每个模块都必须回答：

```text
1. Who owns this decision?
   这个决策到底归谁负责？

2. Who owns this data?
   这份数据到底归谁拥有？

3. Who is allowed to mutate it?
   谁被允许修改它？
```

---

# 2. 整个平台的大边界

平台先分为四个大的业务区域：

```text
                         Context Vocab
                              │
          ┌───────────────────┼───────────────────┐
          ↓                   ↓                   ↓
   公共学习基座           用户学习域           传统业务域
Shared Foundation      Learner Platform      Business Platform
          │                   │                   │
          └──────────────┬────┘                   │
                         ↓                        │
                    学习运行时  ←─────────────────┘
                  Learning Runtime
```

### 公共学习基座

描述：

> 英语知识是什么，以及有效学习这类知识需要遵守什么规则。

核心问题：

```text
应该学什么？
表达之间有什么区别？
如何设计有效场景？
什么叫真正掌握？
```

### 用户学习域

描述：

> 某一个具体用户目前学到了什么程度。

核心问题：

```text
这个用户学过什么？
哪里会？
哪里不会？
哪些语义边界仍然容易混淆？
```

### 传统业务域

描述：

> 用户、账号、会员、支付、订阅、权益等传统互联网业务。

核心问题：

```text
他是谁？
是不是会员？
有没有某项能力的使用权限？
```

### 学习运行时

描述：

> 将公共知识、用户状态和当前学习目标组合起来，决定此刻怎么教。

核心问题：

```text
这个用户下一步最应该学什么？
应该给什么场景？
应该提高还是降低难度？
应该继续提示还是撤掉脚手架？
```

---

# 3. Learning Science Kernel · 学习科学规则内核

## 中文解释

它是系统里的“学习规则制定者”。

它不负责存单词，也不负责教某一个具体用户，而是定义：

```text
什么叫 Semantic Cluster
什么叫 Semantic Boundary
什么叫有效 Scenario
什么叫 Difficulty
什么叫 Mastery
什么叫 Transfer
什么时候应该升级难度
什么时候应该复习
什么时候可以认为发生了跨场景迁移
```

例如：

```text
用户只在一个选择题场景里选对 resentful
≠
用户真正掌握 resentful
```

真正的掌握可能要求：

```text
识别
→ 边界区分
→ 场景选择
→ 主动回忆
→ 句式构建
→ 跨场景迁移
→ 自发输出
```

## 边界

| 项目 | 定义 |
|---|---|
| 负责 | 学习规则、难度规则、掌握规则、迁移规则 |
| Owns | Policy、Specification、Rubric、Rule |
| 不负责 | 不存用户状态、不生成具体场景、不管理会员、不负责 UI |
| 用户相关 | 否 |

一句话：

> **Learning Science Kernel 定义“什么才算学会”。**

---

# 4. Semantic Domain · 语义知识域

## 中文解释

这是产品最核心的公共知识域。

它描述：

> 英语表达之间到底有什么区别，以及一个表达应该和哪些相邻表达一起学习。

它拥有：

```text
Communicative Intent   表达意图
Expression Unit        表达单元
Semantic Cluster       语义簇
Semantic Boundary      语义边界
Semantic Dimension     语义维度
Utterance Pattern      表达句式模式
Semantic Metadata      语义学习标签
```

例如：

```text
angry ↔ furious
核心区别：强度

angry ↔ outraged
核心区别：是否包含明显的不公 / 道德冒犯

angry ↔ resentful
核心区别：即时愤怒 vs 长期积累的不满
```

这些是公共知识，不因具体用户而改变。

## 边界

| 项目 | 定义 |
|---|---|
| 负责 | 表达、语义簇、语义边界、表达关系 |
| Owns | Expression、Cluster、Boundary、Dimension、Pattern |
| Reads | Learning Science Kernel |
| 不负责 | 不判断用户会不会、不决定下一题、不管理会员 |
| 用户相关 | 否 |

一句话：

> **Semantic Domain 管“语言知识本身”。**

---

# 5. Scenario Domain · 场景知识域

## 中文解释

场景不是第二套语义知识库。

它的职责是：

> 把抽象的语义边界放进真实语言环境，让用户能够感知、训练和验证这些区别。

它拥有：

```text
Scenario Domain
Scenario Family
Scenario Template
Scenario Variables
Scenario Asset
Boundary ↔ Scenario Mapping
Scenario Metadata
Scenario Difficulty Features
```

例如 Semantic Domain 已经定义：

```text
angry ↔ resentful
区别 = 是否存在长期积累的不公平感
```

Scenario Domain 负责构造：

```text
你的同事连续几个月把你的工作成果
当成自己的成果汇报给领导。
```

用于暴露和训练这个 Boundary。

## 边界

| 项目 | 定义 |
|---|---|
| 负责 | 场景结构、模板、变量、场景资产、Boundary→Scenario 映射 |
| Owns | ScenarioFamily、ScenarioTemplate、ScenarioAsset、Mapping |
| Reads | Semantic Boundary、Learning Science Rules |
| 不负责 | 不重新定义词义、不重新定义 Boundary、不维护用户学习状态 |
| 用户相关 | 基础场景资产通常否；个性化实例可关联用户 |

一句话：

> **Semantic 定义区别，Scenario 承载区别。**

---

# 6. Content Generation · AI 内容生产系统

## 中文解释

这个模块负责“生产候选学习资产”，但不是最终知识的 Owner。

包含：

```text
AI Semantic Builder
AI Scenario Builder
Cluster Judge
Boundary Judge
Scenario Judge
```

它可以生成：

```text
候选语义簇
候选语义边界
候选场景
候选标签
候选句式模式
```

但必须坚持：

```text
LLM Output
≠
System Fact
```

正式进入公共知识资产前，需要：

```text
Generate
↓
Judge
↓
Validate against Kernel
↓
Approve
↓
Persist through Domain API
```

## 边界

| 项目 | 定义 |
|---|---|
| 负责 | AI 生成、Judge、质量评估、候选版本 |
| Owns | GenerationTask、Candidate、JudgeResult、ReviewRecord |
| Writes | 只能通过 Semantic / Scenario Domain API 提交正式资产 |
| 不负责 | 不直接拥有正式 Cluster / Boundary / Scenario |
| 用户相关 | 通常否 |

一句话：

> **它负责生产知识，但知识最终属于对应领域。**

---

# 7. Learner Domain · 用户学习状态域

## 中文解释

这个模块开始真正和 `user_id` 绑定。

它不回答：

```text
resentful 是什么意思？
```

它回答：

```text
这个用户对 resentful 学到了什么程度？
```

它拥有：

```text
Learner Profile
Learner Goal
LearnerBoundaryState
LearnerExpressionState
LearnerPatternState
Learning History
Learning Memory
Weak Boundary
Review State
Proficiency Estimate
```

例如：

```text
user_id = 10001

angry ↔ resentful
recognition = strong
discrimination = medium
active_retrieval = weak
cross_context_transfer = weak
```

## 边界

| 项目 | 定义 |
|---|---|
| 负责 | 某个用户学到什么程度 |
| Owns | 所有 user_id 关联的学习状态 |
| Reads | Semantic ID、Scenario ID、Pattern ID |
| 不负责 | 不修改公共知识，不重新定义语义边界 |
| 用户相关 | 是 |

一句话：

> **Learner Domain 管“这个人现在会什么”。**

---

# 8. Knowledge 与 Mastery 必须分离

这是数据边界中的核心原则。

错误设计：

```text
user_semantic_boundary

user_id
word_a
word_b
difference
mastery
```

这会导致每个用户都复制公共知识。

正确设计：

```text
semantic_boundary

id = 1024
expression_a = angry
expression_b = resentful
difference = ...
```

然后：

```text
learner_boundary_state

user_id = 10001
boundary_id = 1024
mastery = ...
```

统一原则：

```text
公共知识 ID
+
User ID
=
用户学习状态
```

对应关系：

| 公共知识 | 用户状态 |
|---|---|
| ExpressionUnit | LearnerExpressionState |
| SemanticBoundary | LearnerBoundaryState |
| UtterancePattern | LearnerPatternState |
| Scenario | ScenarioAttempt |

---

# 9. Learning Runtime · 学习编排引擎

## 中文解释

Learner Domain 是“成绩册”。

Learning Runtime 是“老师的教学决策”。

它负责：

```text
选择哪个 Semantic Boundary
选择哪个 Scenario
选择什么 Difficulty
是否撤掉提示
是否做 Transfer
是否进入 Free Production
是否需要 Review
```

输入：

```text
Semantic Knowledge
+
Scenario Assets
+
Learner State
+
User Goal
```

输出：

```text
Current Training Task
```

例如：

```text
用户已经能做 angry ↔ resentful 四选一
但自由表达仍然不会

↓

Learning Runtime 决策：
不要再出选择题
直接进入无候选场景 + 完整句输出
```

## 边界

| 项目 | 定义 |
|---|---|
| 负责 | 下一步怎么教 |
| Owns | TrainingSession、TrainingObjective、TrainingTask、TrainingDecision |
| Reads | Semantic、Scenario、Learner、Entitlement |
| 不负责 | 不拥有公共知识、不拥有 Learner Mastery 事实 |
| 用户相关 | 是 |

一句话：

> **Learner Domain 是成绩册，Learning Runtime 是老师。**

---

# 10. AI Evaluator · AI 表达评估器

## 中文解释

它负责判断：

> 用户刚才说得怎么样？

包括：

```text
语义是否匹配
语法是否正确
表达是否自然
强度是否合适
Register 是否合适
Collocation 是否自然
命中了哪个 Semantic Boundary
错误来自哪个维度
```

输出：

```text
EvaluationResult
```

例如：

```text
用户：
I'm angry because my boss keeps taking credit for my work.

Evaluator：
语法没问题
语义基本正确
但未突出长期不公平造成的积累感
相关 Boundary：angry ↔ resentful
```

然后 Learning Runtime 再决定下一步训练。

一句话：

> **Evaluator 负责诊断，Learning Runtime 负责决策。**

---

# 11. AI Learning Navigator · AI 学习导航器

## 中文解释

它负责理解：

> 用户现在到底想干什么？

例如：

```text
“angry 和 resentful 什么区别？”
→ Explore Semantic Cluster

“给我练几个工作场景”
→ Practice

“昨天那个表示烦透了的短语是什么？”
→ Recall Learning Memory

“我想表达一种长期被针对的不爽”
→ Semantic Retrieval
```

它负责：

```text
Natural Language Understanding
↓
Intent Routing
↓
Semantic Retrieval / Scenario Retrieval / Memory Retrieval / Training
```

一句话：

> **Navigator 是学习导航，Trainer / Runtime 是教学决策。**

---

# 12. Retrieval 与 Learning Memory

## Knowledge Retrieval · 公共知识检索

检索：

```text
Semantic Cluster
Boundary
Expression
Scenario
Utterance Pattern
Metadata
```

回答：

> 系统里有哪些相关知识？

## Personal Learning Memory · 个人学习记忆

检索：

```text
以前学过什么
最近搞错什么
昨天看过什么
哪些 Boundary 一直没掌握
哪些内容很久没复习
```

回答：

> 这个用户过去学过什么？

二者必须逻辑分离。

---

# 13. Business Platform · 传统业务平台

包含：

```text
User
Account
Membership
Subscription
Order
Payment
Entitlement
Coupon
Notification
```

它负责传统业务：

```text
用户是谁
是不是会员
购买了什么
是否拥有某项能力
```

Learning Platform 不应该知道：

```text
支付宝流水号
Apple IAP Receipt
优惠券计算
退款流程
```

Learning Platform 只需要：

```text
user_id
+
EntitlementContext
```

例如：

```text
voice_training = true
advanced_training = true
research_track = false
```

一句话：

> **Business Platform 管商业资格，Learning Platform 管学习。**

---

# 14. 数据分类

以后所有数据必须明确属于以下四类之一：

| 数据类型 | 中文含义 | 用户相关 | 示例 |
|---|---|---:|---|
| Shared Knowledge Data | 公共知识数据 | 否 | Cluster、Boundary、Expression |
| Shared Learning Asset | 公共学习资产 | 否 | Scenario、Pattern |
| Personalized Learning Data | 用户学习数据 | 是 | Mastery、Attempt、Memory |
| Business Data | 商业业务数据 | 是 | User、Membership、Payment |

如果新增一张表时无法判断属于哪一类，说明领域模型可能还不清晰。

---

# 15. Data Ownership Matrix

| 数据 | Owner | 可以修改 | 主要读取方 |
|---|---|---|---|
| ExpressionUnit | Semantic Domain | Semantic Domain | Scenario / Retrieval / Learning Runtime |
| SemanticCluster | Semantic Domain | Semantic Domain | Scenario / Navigator / Runtime |
| SemanticBoundary | Semantic Domain | Semantic Domain | Scenario / Learner / Runtime / Evaluator |
| SemanticDimension | Semantic Domain | Semantic Domain | Scenario / Evaluator |
| UtterancePattern | Semantic Domain | Semantic Domain | Runtime / Evaluator |
| ScenarioFamily | Scenario Domain | Scenario Domain | Runtime / Navigator |
| ScenarioTemplate | Scenario Domain | Scenario Domain | Runtime / AI Scenario Builder |
| ScenarioAsset | Scenario Domain | Scenario Domain | Runtime |
| BoundaryScenarioMapping | Scenario Domain | Scenario Domain | Runtime / Navigator |
| LearnerProfile | Learner Domain | Learner Domain | Runtime / Navigator |
| LearnerBoundaryState | Learner Domain | Learner Domain | Runtime / Analytics |
| LearnerExpressionState | Learner Domain | Learner Domain | Runtime |
| LearnerPatternState | Learner Domain | Learner Domain | Runtime |
| LearningMemory | Learner Domain | Learner Domain | Navigator / Runtime |
| ScenarioAttempt | Learner Domain | Learner Domain | Runtime / Analytics |
| TrainingSession | Learning Runtime | Learning Runtime | Learner / Analytics |
| EvaluationResult | Evaluator / Learning Runtime | 对应 Owner | Learner / Runtime |
| Membership | Business Platform | Business Platform | Application Layer |
| Entitlement | Business Platform | Business Platform | Application Layer / Runtime |

---

# 16. 数据写入规则

必须坚持：

> **只有数据 Owner 可以修改数据语义。**

例如 Scenario Domain 可以读取：

```text
SemanticBoundary
```

但不能直接：

```text
UPDATE semantic_boundary
```

如果 Scenario Domain 发现某个 Boundary 有问题，应该：

```text
BoundaryReviewRequested
```

或者调用 Semantic Domain 的 Review API。

而不是跨域直接修改表。

---

# 17. 用户数据不能直接污染公共知识

正常学习数据流：

```text
Shared Knowledge
      ↓
Learning Runtime
      ↓
User Interaction
      ↓
Learner State
```

不能变成：

```text
User Answer
      ↓
直接修改 Shared Knowledge
```

用户数据如果需要反哺公共基座，必须走研究与验证管线：

```text
User Attempts
    ↓
Analytics
    ↓
异常 / 混淆统计
    ↓
Research / Evaluation
    ↓
Content Review
    ↓
Semantic / Scenario Domain 更新
```

原则：

> **用户数据可以验证公共基座，但不能直接污染公共基座。**

---

# 18. 核心 Owner 规则

```text
“这个表达是什么意思？”
→ Semantic Domain

“这些表达之间为什么不同？”
→ Semantic Domain

“怎么构造场景训练这个区别？”
→ Scenario Domain

“这个用户会不会？”
→ Learner Domain

“下一步怎么教？”
→ Learning Runtime

“用户刚才说得怎么样？”
→ AI Evaluator

“用户现在想干什么？”
→ AI Learning Navigator

“这些知识怎么批量生产？”
→ Content Generation

“他是不是会员？”
→ Business Platform
```

如果未来一个问题出现两个模块都认为“这是我的职责”，需要重新检查领域边界。

---

# 19. 模块化单体优先，不急于微服务

这些模块首先是：

> **Bounded Context / 业务边界**

而不是：

> **必须独立部署的微服务**

前期完全可以采用：

```text
一个后端
+
多个清晰模块 / package
+
明确的数据 Owner
+
禁止跨域直接改表
```

等出现：

```text
独立扩容需求
独立团队
独立发布节奏
不同 SLA
明显的数据隔离需求
```

再决定是否物理拆服务。

原则：

> **先把业务边界设计正确，再决定部署边界。**

---

# 20. 总结

Context Vocab 的核心边界可以概括成：

```text
Learning Science Kernel
定义什么叫正确学习

Semantic Domain
定义英语表达知识

Scenario Domain
把语义区别放进真实环境

Learner Domain
记录这个用户学到了什么程度

Learning Runtime
决定下一步怎么教

AI Evaluator
判断用户刚才表现怎么样

AI Learning Navigator
理解用户现在想做什么

Content Generation
生产候选知识和场景

Business Platform
管理用户、会员、支付和权益
```

最终需要长期坚持两条原则：

> **业务边界：一个核心决策只能有一个明确 Owner。**

> **数据边界：一个核心数据只能有一个明确 Owner，其他模块通过 API / Event 使用。**
