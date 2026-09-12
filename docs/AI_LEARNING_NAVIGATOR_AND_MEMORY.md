# AI Learning Navigator & Personal Learning Memory

> Context Vocab 的用户不应该被迫理解系统的信息架构。用户可以自然表达“我想学什么、我忘了什么、我哪里不会”，系统负责把自然语言意图映射到语义簇、场景和个人学习记忆。

---

## 1. 这一层解决什么问题

产品已经有两类一级学习入口：

```text
Semantic Cluster
→ 从语言知识出发，理解表达之间的区别

Scenario
→ 从真实使用环境出发，学习什么时候该怎么说
```

但用户真实使用时，很多时候并不会先决定：

```text
“我要进入语义簇”
还是
“我要进入场景”
```

他更可能直接说：

```text
“angry 和 resentful 到底差在哪？”

“我想表达一种长期被不公平对待后的不爽。”

“昨天学过一个表示烦透了的短语，帮我找出来。”

“给我复习最近总搞错的表达。”

“给我练几个职场里生气但不能太失礼的场景。”
```

因此系统需要一个位于用户和底层学习资产之间的统一导航层：

```text
AI Learning Navigator
```

---

# 2. 顶层交互模型

推荐结构：

```text
                 Natural Language
                        ↓
              AI Learning Navigator
                        ↓
        Intent Understanding / Routing
                        ↓
          Semantic Learning Retrieval
                        ↓
        ┌───────────────┼────────────────┐
        ↓               ↓                ↓
 Semantic Core     Scenario System   Learning Memory
        │               │                │
        └───────────────┼────────────────┘
                        ↓
                Learning Orchestration
                        ↓
          Answer / Explore / Train / Review
```

核心原则：

> 用户只需要表达意图，系统负责判断应该拉起什么知识、什么场景、什么历史学习状态，以及下一步应该做什么。

---

# 3. Semantic Cluster 与 Scenario 仍然是一级入口

Natural Language Interface 不是用来取代 Semantic Cluster 和 Scenario 的。

更准确的关系是：

```text
显式入口
├── Semantic Cluster
└── Scenario

统一自然语言入口
└── AI Learning Navigator
      ├── 路由到 Semantic Cluster
      ├── 路由到 Scenario
      ├── 拉起个人 Learning Memory
      └── 直接进入 AI Training
```

因此用户既可以：

```text
主动浏览语义簇
主动浏览场景
```

也可以：

```text
直接说出自己的问题
让 AI 帮他找到正确入口
```

这是一个多入口、统一底层知识的设计。

---

# 4. AI Learning Navigator 的职责

AI Learning Navigator 不等于 AI Trainer。

二者职责必须区分。

## AI Learning Navigator

负责：

```text
理解用户现在想做什么
→ 找到相关知识
→ 找到相关场景
→ 找到过去学习记录
→ 组装当前学习上下文
→ 决定下一步应该回答、展示、训练还是复习
```

回答：

> “用户现在应该去哪里？”

## AI Trainer

负责：

```text
真正执行训练任务
→ 出题
→ 分析回答
→ 诊断弱点
→ 给反馈
→ 更新 Learner Model
→ 决定下一训练任务
```

回答：

> “进入训练后，应该怎么教这个用户？”

---

# 5. Semantic Learning Retrieval

传统搜索通常是：

```text
keyword → document
```

Context Vocab 需要的是：

```text
natural language
→ communicative intent
→ semantic concepts
→ structured learning assets
```

所以检索对象不应该只有文本，而应该覆盖结构化学习知识：

```text
Communicative Intent
Semantic Cluster
Semantic Boundary
Expression Unit
Semantic Dimension
Utterance Pattern
Scenario Family
Scenario
Learning Metadata
Learner State
Learning History
```

例如用户输入：

```text
“我想表达那种长期被不公平对待以后越来越不爽的感觉。”
```

系统应理解为：

```text
Intent:
express lingering negative feeling caused by perceived unfairness
```

然后检索：

```text
Semantic Cluster:
anger / resentment

Relevant Expressions:
resent
resentful
bitter
angry

Target Boundaries:
angry ↔ resentful
resentful ↔ bitter
```

而不是只做字符串匹配。

---

# 6. Personal Learning Memory

除了公共语义知识，系统还需要检索“这个用户自己的学习状态”。

这类 Memory 应该和普通 Conversation Memory 区分。

## 6.1 Conversation Memory

主要回答：

```text
我们刚才聊了什么？
当前对话的上下文是什么？
```

## 6.2 Learning Memory

主要回答：

```text
这个用户学过什么？
哪些已经掌握？
哪些总是混淆？
哪些只会认但不会主动表达？
最近训练过哪些场景？
哪些内容应该复习？
```

建议包含：

```text
learned_clusters
learned_expressions
mastered_boundaries
weak_boundaries
recent_scenarios
repeated_errors
active_retrieval_state
cross_context_transfer_state
review_history
last_seen_at
mastery_level
```

Learning Memory 是长期个性化学习的核心资产。

---

# 7. 检索必须同时看“全局知识”和“个人知识”

推荐运行链路：

```text
User Query
   ↓
Intent Understanding
   ↓
Global Semantic Retrieval
   +
Personal Learning Retrieval
   ↓
Context Assembly
   ↓
Learning Decision
   ↓
Response / Navigation / Training
```

例如用户问：

```text
“那种因为别人总抢我功劳，久而久之产生的不满怎么说？”
```

Global Semantic Knowledge 返回：

```text
resent / resentful
angry ↔ resentful boundary
unfairness + lingering grievance
```

Personal Learning Memory 返回：

```text
3 天前学过 resent
angry ↔ resentful 曾答错 2 次
angry 已基本掌握
resent active retrieval 较弱
```

系统的回答就不应该是普通词典答案，而可以是：

```text
你之前学过 resent。

这里关键不是“现在很生气”，
而是长期觉得自己受到不公平对待。

你之前正好容易把 angry 和 resentful 混在一起，
可以直接用一个新的职场场景再试一次。
```

这就是“知识检索 + 个人学习记忆”的价值。

---

# 8. Navigator 的典型意图类型

第一阶段可以识别至少以下几类学习意图：

```text
LOOKUP_EXPRESSION
查某个词 / 短语

COMPARE_EXPRESSIONS
比较多个近义表达

FIND_BY_MEANING
用户不知道词，只描述想表达的意思

EXPLORE_CLUSTER
主动查看某个语义簇

FIND_SCENARIO
想学习某类真实场景

TRAIN
直接进入训练

REVIEW
复习过去学过的内容

RECALL_PAST_LEARNING
找回之前学过但忘记的表达

WEAKNESS_PRACTICE
练习最近容易混淆的边界
```

这些不是前台必须展示给用户的菜单，而是系统内部的路由语义。

---

# 9. 学习编排 Learning Orchestration

Navigator 找到知识以后，不一定只返回“答案”。

它还需要决定最合适的下一步动作。

例如：

```text
用户只是想快速查词
→ 返回简洁解释 + 语义簇入口

用户明显在比较两个表达
→ 打开 Semantic Boundary View

用户在找真实说法
→ 拉起 Scenario + Utterance Pattern

用户问的是曾经学过的内容
→ 优先召回 Learning Memory

用户已经多次混淆同一边界
→ 直接建议 Boundary Training

用户主动说“练一下”
→ 进入 AI Trainer
```

所以系统内部更准确的是：

```text
Retrieve
→ Understand User State
→ Decide Learning Action
→ Execute
```

而不是：

```text
Retrieve
→ Show Search Results
```

---

# 10. 与语义簇 / 场景系统的双向关联

Semantic Cluster 与 Scenario 是两种不同视图，但必须双向可达。

```text
Semantic Cluster
    ↓
Semantic Boundary
    ↓
Related Scenarios
```

用户从语义知识进入，可以直接进入对应场景训练。

反方向：

```text
Scenario
    ↓
Best Expression
    ↓
Target Semantic Boundary
    ↓
Semantic Cluster
```

用户在做场景时，也可以随时反查：

```text
“为什么不是另一个词？”
“这几个表达完整区别是什么？”
```

Navigator 负责完成这种跨视图跳转。

---

# 11. Learning Metadata 参与检索

Semantic Retrieval 不能只按语义相似度排序。

还应该结合：

```text
Usage Domain
Learning Goal
Exam Track
Register
Frequency
Difficulty
User Goal Profile
Current Mastery
Current Weakness
Recent Learning History
```

例如用户目标是：

```text
CET-6 + 日常口语
```

即使系统检索到很多相关表达，也应该优先返回：

```text
高频
CET-6 相关
日常可用
用户尚未掌握
```

而不是单纯返回语义最接近但极低频的表达。

因此可以把推荐优先级理解为：

```text
Learning Priority
≈
Semantic Relevance
× User Goal Match
× Usage Utility
× Current Weakness
× Review Urgency
```

具体公式后续再设计，当前先明确原则。

---

# 12. 一个完整例子

用户输入：

```text
“昨天那个表示忍无可忍的短语是什么？”
```

Navigator：

```text
1. 识别为 RECALL_PAST_LEARNING
2. 搜索 Learning Memory
3. 找到昨天训练记录
4. 召回 be fed up with
5. 同时找到所属 Semantic Cluster
6. 找到用户对应 Mastery State
```

系统响应可以是：

```text
be fed up with

你昨天在“长期积累的不耐烦”这一组里练过它。

Pattern:
I'm fed up with + noun / gerund

如果需要，可以直接继续昨天没完全掌握的场景。
```

用户无需记住课程位置、簇名称或菜单路径。

---

# 13. AI-native 产品原则

这一层形成几个重要原则：

### 原则 1

> 用户不需要理解系统的信息架构。

### 原则 2

> Natural Language 是统一入口，但不是唯一入口。

Semantic Cluster 和 Scenario 仍然可以被直接浏览。

### 原则 3

> 搜索不是找文档，而是找“当前最相关的学习知识”。

### 原则 4

> 回答必须同时理解语言知识和用户自己的学习历史。

### 原则 5

> AI Learning Navigator 负责“去哪学”，AI Trainer 负责“怎么学”。

---

# 14. 更新后的整体用户侧架构

```text
                           User
                            │
          ┌─────────────────┼─────────────────┐
          ↓                 ↓                 ↓
 Natural Language     Semantic Cluster     Scenario
          │                 │                 │
          └─────────────────┼─────────────────┘
                            ↓
                  AI Learning Navigator
                            ↓
                Semantic Learning Retrieval
                            ↓
           ┌────────────────┼────────────────┐
           ↓                ↓                ↓
     Semantic Core    Scenario System   Learning Memory
           │                │                │
           └────────────────┼────────────────┘
                            ↓
                 Learning Orchestration
                            ↓
      ┌───────────────┬───────────────┬───────────────┐
      ↓               ↓               ↓               ↓
    Answer          Explore          Review          Train
                                                      ↓
                                                 AI Trainer
                                                      ↓
                                               Learner Model
                                                      ↓
                                         Spontaneous Production
```

---

# 15. 最终定位

Context Vocab 的检索层不应该只是一个搜索框。

它应该成为：

> 一个理解用户自然语言意图、能够检索语义知识、关联真实场景、召回个人学习历史，并决定下一学习动作的 AI Learning Navigator。

最终形成：

```text
自然表达问题
→ 系统理解学习意图
→ 找到语义簇 / 场景 / 历史知识
→ 给当前最相关的学习内容
→ 必要时直接进入训练
→ 持续更新个人 Learner Model
```

这使产品从“内容型英语工具”进一步转变为：

> **能够记住你学过什么、理解你现在想表达什么，并持续连接旧知识与新场景的 AI-native 个性化表达学习系统。**
