# Context Vocab

> **不是背出词义，而是练出选词能力。**  
> Learn words by context. Choose words like a native.

## 1. 项目背景

传统背单词产品通常围绕以下链路展开：

```text
单词
→ 中文释义
→ 例句
→ 遗忘曲线复习
```

这套机制对“认识单词”和应试阅读有效，但它很难解决另一个更常见的问题：

> 学习者明明认识很多单词，真正表达时却不知道应该选哪一个。

例如：

```text
hate
loathe
abhor
despise
resent
```

如果只记中文释义，它们很容易被压缩为“讨厌 / 厌恶 / 憎恶”。

但真实英语表达关注的是：

- 情绪强度是否匹配；
- 是否带有道德判断；
- 是否包含轻蔑；
- 是否源于不公平待遇；
- 是即时情绪还是长期积累；
- 当前场景是否正式；
- 母语者在这个场景里更可能选择哪个表达。

因此，本项目不把“中文释义记忆”作为核心学习目标，而是尝试建立：

```text
真实场景
→ 表达意图
→ 语义簇
→ 语义边界
→ 选词判断
→ 主动输出
```

## 2. 项目定位

Context Vocab 是一个面向英语学习者的 **Contextual Vocabulary Trainer / English Word Choice Trainer**。

它不试图成为一个传统“词汇量冲刺工具”，而是解决：

> 如何把已经认识的被动词汇，转化为能够在真实语境中精准调用的主动词汇。

项目更偏向以下用户：

- 已有一定英语基础；
- 认识大量单词，但输出时只会使用基础词；
- 希望提高口语、写作、工作交流中的表达精度；
- 准备 IELTS / TOEFL 等需要输出能力的考试；
- 希望减少“先想中文，再翻译成英文”的表达路径。

项目暂不优先面向：

- 英语零基础用户；
- 纯短期应试词汇突击；
- 仅追求快速扩大阅读识别词汇量的场景。

## 3. 核心产品理念

### 3.1 不以中文释义作为知识本体

中文可以作为低频、可选的辅助脚手架，但默认学习路径应该建立：

```text
Word
→ Concept
→ Situation
→ Emotion
→ Speaker Intent
→ Register
→ Collocation
```

而不是：

```text
Word
→ 中文翻译
```

项目希望训练的是：

> 一个英语母语者在什么情况下会选择这个词，而不是另一个意思接近的词。

---

### 3.2 Never teach a word alone

一个单词不应该被孤立教授。

任何核心单词至少需要回答：

1. 它最接近哪些表达？
2. 它和这些表达之间的边界是什么？
3. 为什么当前场景应该用它，而不是其他候选词？
4. 换一个场景后，哪个词会更自然？

因此，本项目的基础学习单位不是 Word，而是：

```text
Semantic Cluster
+
Semantic Boundary
+
Contrastive Scenario
```

---

### 3.3 没有对比，就没有真正学习

如果学习者只知道：

```text
furious = 非常生气
```

那么他仍然可能不知道什么时候应该使用 `furious`。

真正需要训练的是：

```text
annoyed
irritated
angry
furious
outraged
resentful
```

之间的语义边界。

例如：

```text
angry ↔ furious
主要区别：intensity

angry ↔ outraged
主要区别：perceived injustice / moral violation

angry ↔ resentful
主要区别：immediate anger vs lingering grievance
```

学习的重点不是“记住更多近义词”，而是建立这些词之间的决策边界。

---

### 3.4 地道表达不是“说对”，而是“选得准”

一个表达可能：

- 语法正确；
- 基本语义正确；
- 但强度不自然；
- 语域不合适；
- 情绪色彩不匹配；
- 母语者在当前场景中通常不会这样说。

因此，系统不只判断：

```text
Correct / Wrong
```

而应该逐步评价：

```text
Meaning Match
Intensity Match
Register Match
Pragmatic Match
Collocation Naturalness
Native Preference
```

项目的最终目标是：

> 在多个“都不算错”的候选表达中，选择最符合当前语境的表达。

## 4. 核心概念：Semantic Cluster

### 4.1 定义

语义簇不是简单的“同义词集合”。

本项目暂定义：

> **Semantic Cluster 是一组围绕同一表达意图、在真实表达中存在竞争关系、并能够通过有限几个稳定语义维度进行区分的表达集合。**

一个合格的语义簇至少满足：

```text
1. Same Intent
   竞争同一个表达槽位

2. Confusable
   学习者真实使用时可能产生混淆

3. Distinguishable
   可以通过稳定维度解释为什么使用 A 而不是 B
```

例如：

```text
annoyed
irritated
angry
furious
outraged
```

可以围绕：

```text
intensity
persistence
trigger type
injustice
moral judgment
```

进行区分。

而：

```text
angry
sad
anxious
disappointed
```

虽然同属负面情绪，但不属于同一个核心训练簇，因为它们通常不竞争同一个表达位置。

---

### 4.2 Core Cluster 与 Extended Neighborhood

知识图谱可以非常大，但一次学习任务必须收敛。

因此区分：

```text
Core Cluster
```

和：

```text
Extended Neighborhood
```

例如：

```text
Core:
annoyed
irritated
angry
furious
outraged

Extended:
livid
irate
incensed
enraged
indignant
```

第一阶段只训练最值得学习、最容易混淆、最高频的核心边界。

## 5. Semantic Cluster 评定指标

语义簇需要可量化评估，而不是仅依赖人工直觉。

首版指标：

| 指标 | 含义 |
|---|---|
| Intent Cohesion | 是否围绕同一个表达意图 |
| Semantic Proximity | 词之间是否足够接近，形成真实竞争 |
| Discriminability | 是否存在清晰、稳定的区别维度 |
| Scenario Separability | 是否能通过真实场景把候选词区分开 |
| Confusion Value | 学习者是否真的容易混淆 |
| Pragmatic Coverage | 是否覆盖有价值的语用差异 |
| Usage Utility | 是否高频、常用、值得主动掌握 |
| Compactness | 是否控制在合理的认知负担范围 |
| Boundary Density | 簇内是否存在足够多高价值语义边界 |

### 5.1 硬门槛

以下三项不满足时，不进入 Core Cluster：

```text
① 必须竞争同一个表达意图
② 必须存在可解释的区别维度
③ 必须能够构造有效的最小对比场景
```

### 5.2 推荐规模

首版教学设计建议：

```text
Core Cluster:
4 ~ 8 个核心表达

Primary Dimensions:
2 ~ 5 个主要区分维度
```

这不是语言学上的硬限制，而是学习体验上的收敛规则。

## 6. Semantic Boundary

Semantic Boundary 是项目中的一等公民。

相比“簇里有哪些词”，项目更关注：

> 这些词之间存在哪些值得学习的选词边界。

示例：

```text
SemanticBoundary

word_a: angry
word_b: outraged

dimensions:
  - injustice
  - moral_violation

difference:
  outraged often highlights anger caused by perceived injustice
  or moral violation

relation_strength:
  TENDENCY

confusability_score:
  0.86

scenario_separability:
  0.91

learning_value:
  0.93

confidence:
  0.88
```

关系需要区分：

```text
HARD
TENDENCY
REGISTER
COLLOCATION
PRAGMATIC
```

避免把所有语言规律都错误地建模成绝对规则。

## 7. AI 在项目中的职责

AI 不是产品本身。

AI 是用于生成、评审、训练和诊断的能力层。

### 7.1 AI 参与语义簇生成

候选链路：

```text
Candidate Words
→ AI Semantic Analysis
→ Semantic Dimensions
→ Pairwise Boundaries
→ Cluster Metrics
→ Cluster Score
→ Core / Extended / Reject
```

AI 不只输出一个总分，而需要提供：

- 指标评分；
- 评分理由；
- 主要区分维度；
- 词对边界；
- 关系类型；
- 置信度。

---

### 7.2 AI 参与语义簇评分

推荐采用：

```text
Cluster-level Judge
+
Pairwise Boundary Judge
+
Scenario Validation
```

其中优先：

> 先评边，再评簇。

因为产品真正训练的是语义边界。

后续可扩展 Multi-Judge：

```text
Judge A: linguistic quality
Judge B: learner confusion
Judge C: real-world usage
```

通过多评审差异形成 confidence score。

---

### 7.3 AI 解释单词

AI 不应该主要回答：

```text
这个词中文是什么意思？
```

而应该解释：

```text
When would a native speaker choose this word?
Why this word instead of its neighbors?
What does this word imply?
When would it sound too strong / formal / unnatural?
```

解释内容至少包括：

- core meaning；
- intensity；
- register；
- emotional coloring；
- pragmatic implication；
- common collocations；
- typical targets；
- semantic neighbors；
- contrastive boundaries；
- typical scenarios；
- counterexamples。

---

### 7.4 AI 混淆考察

这是产品的核心学习环节。

AI 根据语义边界动态生成具有区分度的真实场景。

例如目标簇：

```text
annoyed
irritated
angry
furious
outraged
```

场景：

```text
A. 外卖晚了 10 分钟
B. 同事连续两个小时敲笔
C. 有人故意撞坏你的车
D. 公司系统性克扣员工工资
```

考察重点分别可能是：

```text
A → annoyed
B → irritated
C → angry / furious
D → outraged
```

系统不能只告诉用户“答案对错”，而应说明：

> 为什么这个词比其他候选表达更自然。

## 8. 学习闭环

核心学习闭环：

```text
Semantic Cluster
        ↓
Semantic Boundary
        ↓
Scenario
        ↓
User Choice / Expression
        ↓
AI Evaluation
        ↓
Semantic Gap
        ↓
Learner Model
        ↓
Next Scenario
```

系统真正维护的不是：

```text
furious 熟练度 = 82%
```

而是更细的能力状态：

```text
furious

recognition:       0.95
intensity:         0.73
context_match:     0.68
active_retrieval: 0.42

confusion:
  angry ↔ furious
  furious ↔ outraged
```

## 9. 学习层级

一个词真正“掌握”需要经过：

### Level 1 — Recognition

```text
I understand this word.
```

看见单词能够理解。

### Level 2 — Boundary Discrimination

```text
I can distinguish this word.
```

知道它与相邻表达的差异。

### Level 3 — Context Matching

```text
I can choose this word in a scenario.
```

面对陌生场景能够做正确选择。

### Level 4 — Active Retrieval

```text
I can recall this word without options.
```

不给候选项也能主动调用。

### Level 5 — Transfer

```text
I can use this semantic distinction across different domains.
```

工作、家庭、社交、正式写作等不同表层场景下仍能保持正确判断。

只有完成跨场景迁移，才认为语义边界趋于收敛。

## 10. 场景训练形式

首版建议支持以下训练模式：

### Context Choice

给场景，选择最自然表达。

### Contrastive Choice

同时给多个近义词，判断哪一个最适合。

### Replacement Test

替换句子里的词，判断语气和语义如何改变。

### Boundary Comparison

针对两个词做最小对比。

### Free Expression

只给真实交流场景，让用户自由表达。

### Native Refinement

用户表达语法正确，但 AI 指出：

```text
可以这么说
≠
当前场景最自然的说法
```

## 11. 用户最终获得的能力

本项目希望用户完成学习后，不只是：

```text
我记住了更多单词。
```

而是：

```text
我认识这些语义相近的表达
        ↓
我知道它们各自的使用边界
        ↓
我能够识别真实场景的语义特征
        ↓
我能够主动选择最匹配的表达
        ↓
我的英语输出更加精准、自然、地道
```

最终目标：

> **从 Vocabulary Size 提升到 Vocabulary Resolution。**

也就是从：

> 我认识多少词

转向：

> 我能多精准地选择词。

## 12. 与传统背词产品的关系

本项目不否定 SRS / 间隔重复。

SRS 仍然可以作为底层调度机制，但复习对象不再只是：

```text
单词
```

而可以是：

```text
语义边界
场景判断
主动提取能力
混淆模式
```

传统模式：

```text
今天重新看到 furious
```

本项目模式：

```text
系统发现你仍然混淆 furious ↔ outraged
↓
生成一个新的陌生场景
↓
再次验证这个边界
```

因此：

> 复习的不是“单词出现次数”，而是“尚未掌握的使用边界”。

## 13. 首版产品链路

首版项目优先完成以下主链：

```text
1. 语义簇定义
        ↓
2. 语义簇指标体系
        ↓
3. AI 评分并结构化灌入
        ↓
4. 前端语义簇展示
        ↓
5. 语义簇单词 AI 解释
        ↓
6. 语义簇 AI 混淆考察
        ↓
7. 用户语义边界状态记录
        ↓
8. 收敛判断与针对性再训练
```

## 14. 首版非目标

为了避免过度设计，首版暂不优先：

- 建立完整英语词典；
- 覆盖全部考试词库；
- 零基础英语教学；
- 复杂社交社区；
- AI 自由聊天陪练；
- 用 AI 替代完整课程体系；
- 一次性解决所有语法、听力、发音问题。

第一阶段只验证一件事情：

> **基于语义簇、语义边界和动态场景训练，能否显著提升学习者的选词能力。**

## 15. 推荐的第一组验证簇

第一阶段可以从高频、差异明显、场景丰富的词群开始：

### Anger / Displeasure

```text
annoyed
irritated
angry
furious
outraged
resentful
```

### Dislike / Aversion

```text
dislike
hate
loathe
detest
abhor
despise
resent
```

### Request / Pressure

```text
ask
request
urge
demand
beg
insist
```

### Certainty

```text
think
believe
assume
suppose
suspect
know
```

通过这些语义簇验证：

```text
Cluster Generation
Boundary Extraction
AI Scoring
Scenario Generation
User Evaluation
Mastery Convergence
```

是否成立。

## 16. 核心架构思想

```text
Vocabulary Knowledge Graph
        ↓
Training Objective
        ↓
Semantic Cluster
        ↓
Contrast Dimensions
        ↓
Semantic Boundaries
        ↓
Scenario Probing
        ↓
Learner Boundary Model
        ↓
Adaptive Training
        ↓
Boundary Mastery
```

核心原则：

> **知识网络可以无限扩张，但一次学习任务必须是有限闭包。**

> **语义簇不是最终学习对象，语义边界才是。**

> **AI 负责发现、解释、生成、评估；规则、数据结构和收敛机制由系统掌控。**

---

## 17. 项目当前阶段

当前状态：**Project Initiation / Concept Validation**

下一步优先事项：

```text
P0  Semantic Cluster Definition
P0  Cluster Evaluation Rubric
P0  Semantic Boundary Model
P0  AI Structured Scoring Schema
P1  First Cluster Dataset
P1  Scenario Generation & Evaluation
P1  Learner Boundary State
P2  Frontend Learning Experience
P2  Adaptive Training Loop
```

当前阶段不急于做大量功能，首先验证核心认知：

> 用户是否会因为“近义词边界 + 场景对比训练”，明显获得比传统背词更强的实际选词能力。
