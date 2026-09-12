# Core Architecture V2 · AI-Driven Semantic + Scenario + Training System

> Context Vocab 的核心不是“AI 背单词”，而是：**用 AI 搭建语义系统、搭建场景系统，并在此基础上完成个性化训练，最终让用户在真实场景中脱口而出自然、精准、地道的表达。**

---

## 1. 终极目标

产品最终目标不是：

```text
记住更多单词
```

而是：

```text
真实场景出现
→ 识别表达意图
→ 直接激活合适表达
→ 使用自然句式
→ 低认知负担地脱口而出
```

最终 Mastery 标准：

> 用户不依赖中文翻译、不依赖候选项、不临时搜索语法，能够在真实场景中快速调用最自然的表达。

---

# 2. 三个核心系统

整个产品由三个核心系统组成：

```text
Semantic System
+
Scenario System
+
AI Training System
```

分别回答：

```text
Semantic System
→ 学什么？这些表达之间到底差在哪？

Scenario System
→ 这些差异在什么真实环境里出现？

AI Training System
→ 如何根据用户当前薄弱点持续训练，直到能够自然输出？
```

但需要特别明确：

> AI 不只存在于 Training System。

AI 实际贯穿了整个产品的知识生产、场景生产与学习运行过程。

因此，系统更准确的结构是：

```text
                    AI FOUNDATION
                          │
          ┌───────────────┼────────────────┐
          ↓               ↓                ↓
 Semantic Builder   Scenario Builder   AI Trainer
          │               │                │
          ↓               ↓                ↓
 Semantic System    Scenario System   Learning Runtime
          │               │                │
          └───────────────┼────────────────┘
                          ↓
                     Learner Model
                          ↓
                Spontaneous Production
```

---

# 3. AI 的三种核心角色

## 3.1 AI Semantic Builder

负责搭建和维护语义知识体系。

输入可能是：

```text
一个 Communicative Intent
一个种子表达
一批候选表达
一个学习目标
```

AI 负责：

```text
发现候选表达
→ 构建 Semantic Cluster
→ 提取 Semantic Boundary
→ 提取区分维度
→ 判断关系类型
→ 进行指标评分
→ 给出 Core / Extended / Reject 建议
```

产出结构化资产：

```text
Communicative Intent
Semantic Cluster
Expression Unit
Semantic Boundary
Semantic Dimension
Cluster Metrics
Boundary Metrics
Confidence
```

AI 不直接决定最终事实。

所有 AI 生成内容需要经过：

```text
Generate
→ Judge
→ Validate
→ Persist
```

才能进入正式知识资产。

---

## 3.2 AI Scenario Builder

负责把抽象语义边界转化为可学习、可验证的真实场景。

输入：

```text
Semantic Cluster
Semantic Boundary
Target Dimension
Difficulty
Domain
Relationship
Register
```

AI 负责生成：

```text
Teaching Scenario
Contrastive Scenario
Boundary Scenario
Transfer Scenario
Free Production Scenario
```

例如：

```text
Boundary:
furious ↔ outraged

核心差异：
普通强烈愤怒
vs
因不公 / 道德冒犯产生的强烈愤怒
```

AI Scenario Builder 不应该随机出题，而应该通过场景变量精确控制：

```text
intensity
cause
injustice
moral violation
duration
relationship
register
domain
medium
```

最终目标：

> 场景必须能够真正暴露目标 Semantic Boundary。

---

## 3.3 AI Trainer

负责用户运行时训练。

输入：

```text
Semantic Assets
Scenario Assets
Learner State
Current Training Objective
User Response
```

AI 负责：

```text
选择训练目标
→ 选择 / 生成场景
→ 让用户选择或自由表达
→ 判断表达自然度
→ 识别错误边界
→ 给最小必要反馈
→ 更新 Learner Model
→ 决定下一训练任务
```

最终逐步撤掉脚手架：

```text
4 个选项
→ 2 个选项
→ 关键词提示
→ 句式提示
→ 只给场景
→ 自由表达
→ 实时对话
```

---

# 4. 两个运行平面

为了避免把“生成教材”和“教用户”混成一次 Prompt，系统划分为两个平面。

## 4.1 Content Generation Plane

负责构建可复用的学习资产。

```text
Communicative Intent
        ↓
Expression Discovery
        ↓
Semantic Cluster Generation
        ↓
Semantic Boundary Extraction
        ↓
AI Judge
        ↓
Scenario Architecture
        ↓
Scenario Generation
        ↓
Scenario Validation
        ↓
Persisted Learning Assets
```

它更像一个 AI Content Pipeline。

主要产物：

```text
Expression Unit
Semantic Cluster
Semantic Boundary
Semantic Dimension
Utterance Pattern
Scenario Family
Scenario Template
Anchor Scenario
Contrast Scenario
Transfer Scenario
```

这些资产经过验证后应持久化，不应该每次用户进入训练时重新生成一遍。

---

## 4.2 Learning Runtime Plane

负责教“当前这个用户”。

```text
Persisted Learning Assets
           +
      Learner Model
           ↓
      AI Trainer
           ↓
  Current Training Task
           ↓
     User Response
           ↓
      AI Evaluation
           ↓
    Learner Diagnosis
           ↓
  Next Training Objective
```

Content Generation Plane 负责构建稳定知识资产。

Learning Runtime Plane 负责个性化调用这些资产。

---

# 5. Semantic System

Semantic System 的核心不是“词库”，而是表达之间的选择空间。

核心结构：

```text
Communicative Intent
        ↓
Semantic Cluster
        ↓
Expression Units
        ↓
Semantic Dimensions
        ↓
Semantic Boundaries
```

## 5.1 Expression Unit

Semantic Cluster 成员统一定义为 Expression Unit，而不是 Word。

允许：

```text
WORD
PHRASAL_VERB
FIXED_PHRASE
COLLOCATION
CONSTRUCTION
```

例如：

```text
hate
resent
can't stand
be fed up with
have had enough of
```

是否进入同一 Cluster，不由词性决定，而由：

```text
是否竞争相同表达意图
+
是否存在真实选词竞争
+
是否可以清晰区分
```

决定。

---

# 6. Scenario System

场景不是 AI 临时编写的一段文本。

Scenario 必须成为一等领域对象。

建议结构：

```text
Scenario Domain
    ↓
Scenario Family
    ↓
Scenario Template
    ↓
Scenario Variables
    ↓
Concrete Scenario
```

核心变量至少包括：

```text
Domain
Relationship
Communicative Goal
Intensity
Duration
Cause
Register
Medium
Semantic Cue Visibility
Expected Response Form
```

例如：

```text
Domain:
workplace

Family:
unfair treatment

Target Boundary:
angry ↔ resentful

Variables:
relationship = manager
repetition = repeated
injustice = high
emotion_duration = accumulated
register = casual conversation
```

由 AI 根据这些约束生成 Concrete Scenario。

---

# 7. 场景的四种核心职责

## Teaching Scenario

第一次建立表达感觉。

特点：

```text
语义特征明显
歧义低
典型
```

## Boundary Scenario

专门区分两个或多个相近表达。

重点：

```text
控制其他变量
只突出目标 Semantic Boundary
```

## Transfer Scenario

换 Domain、Relationship、Register 后再次测试相同边界。

目的：

> 判断用户学到的是语义概念，还是只记住了例题。

## Production Scenario

不给候选表达，让用户自由输出完整句子。

用于训练：

```text
Expression Selection
+
Utterance Construction
+
Spontaneous Production
```

---

# 8. AI 生成不等于系统事实

AI 可以承担知识和场景生产，但不能无条件信任模型输出。

核心原则：

> AI generates; system governs.

对 Semantic Cluster：

```text
Generate
→ Pairwise Judge
→ Cluster Judge
→ Rule Validation
→ Persist
```

对 Scenario：

```text
Generate
→ Boundary Alignment Judge
→ Naturalness Judge
→ Difficulty Judge
→ Persist / Reject
```

系统规则负责：

```text
阈值
状态
版本
审核
收敛
回滚
```

AI 负责：

```text
发现
生成
解释
评估
```

---

# 9. 从表达选择到整句脱口而出

Semantic Cluster 只解决“选什么”。

最终用户真正说出来的是句子，因此还需要：

```text
Expression Unit
       ↓
Utterance Pattern
       ↓
Example Sentence Network
       ↓
Active Production
```

例如：

```text
Expression:
resent
```

对应 Pattern：

```text
I resent + noun
I resent + doing
I resent the way + clause
I'm starting to resent + ...
```

通过多个例句形成模式：

```text
I resent the way he talks to me.
I resent the way they ignore my work.
I resent being treated like a child.
I'm starting to resent having to fix everything myself.
```

系统希望用户最终内化的不是固定例句，而是：

```text
semantic trigger
+
expression
+
utterance pattern
```

最终形成程序化调用。

---

# 10. Learning Runtime 的核心闭环

```text
Semantic Boundary
        ↓
Training Objective
        ↓
Scenario System
        ↓
AI Trainer
        ↓
User Choice / Production
        ↓
AI Diagnosis
        ↓
Learner Boundary State
        ↓
Weakness Detection
        ↓
Next Scenario
        ↓
Cross-context Transfer
        ↓
Spontaneous Production
```

系统真正维护的不是：

```text
furious 熟练度 = 85%
```

而是：

```text
angry ↔ furious
intensity discrimination: strong

furious ↔ outraged
moral/injustice distinction: weak

outraged
recognition: strong
active retrieval: weak
cross-context transfer: weak
```

---

# 11. 用户前台与系统后台分离

后台可以非常复杂，前台必须非常简单。

后台维护：

```text
Semantic Metrics
Boundary Metrics
Scenario Variables
AI Confidence
Learner State
```

用户主要看到：

```text
场景
→ 表达
→ 对比
→ 简单图标提示
→ 必要解释
```

原则：

> 用户学习的是场景，系统管理的是指标。

复杂语义指标可以通过轻量图标表达，例如：

```text
🔥 强度
💬 日常
🎩 正式
⏱️ 持续 / 积累
⚖️ 不公 / 道德判断
```

但指标本身不应该成为用户新的学习负担。

---

# 12. Mastery Levels

最终学习状态建议定义为：

```text
Level 1  Recognition
Level 2  Boundary Discrimination
Level 3  Context Matching
Level 4  Guided Production
Level 5  Free Production
Level 6  Cross-context Transfer
Level 7  Spontaneous Production
```

Level 7 才视为真正 Mastered。

即：

> 场景出现后，合适表达和自然句式可以快速、稳定、低认知负担地被调用出来。

---

# 13. 产品核心数据资产

随着系统运行，真正有价值的数据资产将逐渐变成：

```text
高质量 Semantic Clusters
高质量 Semantic Boundaries
高质量 Scenario Families
Boundary ↔ Scenario 映射
真实用户混淆数据
Learner Mastery 数据
高价值 Utterance Patterns
```

其中最重要的不是单词表，而是：

> 哪些表达容易混淆、区别在哪里、哪些场景最能训练这个区别。

---

# 14. P0 / P1 模块划分

## P0 · Knowledge Foundation

```text
Expression Unit
Semantic Cluster
Semantic Boundary
Semantic Dimension
AI Semantic Builder
Cluster Judge
Boundary Judge
```

## P1 · Scenario Foundation

```text
Scenario Domain
Scenario Family
Scenario Template
Scenario Variables
AI Scenario Builder
Scenario Judge
Difficulty Model
```

## P2 · Learning Runtime

```text
Learner Model
AI Trainer
Training Objective
Response Evaluation
Adaptive Scenario Selection
Mastery / Convergence
```

## P3 · Production Layer

```text
Utterance Pattern
Example Sentence Network
Guided Production
Free Production
Conversation Training
Spontaneous Production
```

---

# 15. 核心架构结论

Context Vocab 不应该被理解为：

```text
AI + 单词软件
```

更准确的结构是：

```text
AI Semantic Builder
        ↓
Semantic System
        ↓
AI Scenario Builder
        ↓
Scenario System
        ↓
AI Trainer
        ↓
Learner Model
        ↓
Spontaneous Production
```

一句话总结：

> **AI 负责建知识、建场景、做训练；系统规则负责约束、验证、沉淀和收敛；最终让用户在真实场景中自然调用最地道的完整表达。**
