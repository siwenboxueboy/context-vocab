# P0 · Semantic Cluster Definition & Evaluation Rubric

> 本文档定义 Context Vocab 第一阶段最核心的领域模型：Semantic Cluster、Semantic Boundary、Expression Unit，以及从表达选择走向整句自然输出的学习层级。

---

## 1. 文档目标

本阶段需要解决：

1. 什么样的一组表达可以构成一个 Semantic Cluster；
2. Cluster 中的成员到底应该建模成 Word 还是更高层对象；
3. 如何评价一个 Cluster 是否值得教学；
4. 如何通过 AI 完成候选生成、边界分析与评分；
5. 如何从“选对表达”继续训练到“整句话能够自然脱口而出”。

第一阶段暂不讨论完整前端、数据库表结构和具体技术栈。

---

## 2. 产品的基本学习链路

传统背词通常是：

```text
Word
→ Translation
→ Example
→ Review
```

Context Vocab 希望训练的是：

```text
真实场景
→ Communicative Intent
→ Semantic Cluster
→ Semantic Boundary
→ Expression Selection
→ Utterance Pattern
→ Example Sentences
→ Active Production
→ Spontaneous Production
```

核心目标不是“知道一个词是什么意思”，而是：

> 在真实语境中知道应该选择哪个表达，并能够用自然的句式把它直接说出来。

---

# 3. Semantic Cluster 定义

## 3.1 核心定义

Semantic Cluster 定义为：

> 一组围绕相同或高度相近的 Communicative Intent，在真实语言表达中存在实际选择竞争关系，并且能够通过有限数量的稳定语义、语用或表达维度进行区分的表达集合。

关键不在于它们是不是词典意义上的“同义词”，而在于：

> 用户在真实表达时，会不会在这些表达之间产生选择。

例如：

```text
annoyed
irritated
angry
furious
outraged
```

它们共同竞争：

```text
express anger / displeasure
```

但可以通过：

```text
intensity
persistence
trigger
injustice
moral judgment
register
```

进行区分。

---

## 3.2 Semantic Cluster 不是 Topic Category

例如：

```text
angry
sad
anxious
ashamed
```

都属于 negative emotion，但它们通常不是同一个表达槽位中的竞争候选，因此不应该因为“主题相关”就放入同一个核心训练簇。

```text
Topic Similarity
≠
Semantic Competition
```

---

## 3.3 Semantic Cluster 不是无限知识图谱

知识图谱可以不断扩张：

```text
angry
→ rage
→ temper
→ hostility
→ aggression
→ violence
```

但学习任务必须收敛。

因此：

> Knowledge Graph 可以无限扩张，但一次 Semantic Cluster 学习任务必须形成有限闭包。

---

# 4. Cluster 成员不是 Word，而是 Expression Unit

如果产品目标是“地道表达”，底层领域对象不能只定义为 Word。

因为真实英语表达中，用户会同时在单词、词组、短语和固定表达之间做选择。

例如：

```text
hate
loathe
can't stand
be sick of
be fed up with
have had enough of
```

这些形式不同，但都可能在“表达厌烦 / 无法忍受”的真实表达任务里形成竞争。

因此 Cluster Member 统一抽象为：

```text
ExpressionUnit
```

而 Word 只是 ExpressionUnit 的一种类型。

---

## 4.1 Expression Unit 类型

首版可包含：

```text
WORD
PHRASAL_VERB
FIXED_PHRASE
IDIOM
COLLOCATION
CONSTRUCTION
```

示例：

```text
WORD
resent

FIXED_PHRASE
can't stand

PHRASAL_VERB
put up with

IDIOM / CONVENTIONAL EXPRESSION
get on my nerves

CONSTRUCTION
I've had enough of ...
```

---

## 4.2 Expression Unit 的核心属性

概念模型：

```text
ExpressionUnit
├── surface_form
├── type
├── communicative_function
├── semantic_features
├── pragmatic_features
├── syntactic_patterns
├── register
├── collocations
└── usage_constraints
```

例如：

```text
surface_form:
  be fed up with

type:
  FIXED_PHRASE

communicative_function:
  express accumulated frustration

syntactic_pattern:
  be fed up with + noun / gerund

register:
  informal-neutral

temporal_feature:
  accumulated / sustained
```

---

## 4.3 不要求 Cluster 成员词性相同

Semantic Cluster 的依据不是 same POS，而是：

```text
same / overlapping communicative intent
+
real expression competition
```

例如：

```text
I hate this practice.
I find this practice abhorrent.
```

`hate` 是 verb，`abhorrent` 是 adjective，但它们在某些表达任务下都可能竞争“如何表达强烈负面态度”。

因此，词性不是 Cluster 的硬边界。

---

## 4.4 Expression Unit 加入 Cluster 的收敛原则

形式可以不同，但必须服务于同一个表达决策。

判断标准不是：

```text
它们意思像不像？
```

而是：

> 用户在真实准备说这句话的时候，会不会把它们当作候选表达？

例如：

```text
angry
furious
be pissed off
```

可能存在较强竞争。

但：

```text
lose one's temper
```

更偏向描述“进入发火行为”的事件，而不是单纯描述状态，因此可能更适合作为邻接表达，而非同一 Core Cluster 的核心成员。

---

# 5. Semantic Cluster 成立的三个必要条件

## 5.1 Same Intent

成员必须竞争相同或高度相近的 Communicative Intent。

## 5.2 Confusable

学习者在真实理解或输出中确实可能混淆这些表达。

## 5.3 Distinguishable

成员之间必须存在可解释、可验证、可训练的区别。

例如：

```text
angry ↔ furious
主要维度：intensity

angry ↔ outraged
主要维度：perceived injustice / moral violation

angry ↔ resentful
主要维度：immediate anger vs lingering grievance
```

如果只能解释成：

```text
“差不多，只是语感不一样”
```

则不适合成为核心训练边界。

---

# 6. Core Cluster 与 Extended Neighborhood

知识网络可以很大，但当前学习单元必须控制复杂度。

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

核心原则：

> 一个 Expression Unit 进入 Core Cluster，必须带来足够高的新增辨析价值。

否则进入 Extended Neighborhood。

首版建议：

```text
Core Members: 4 ~ 8
Primary Dimensions: 2 ~ 5
```

这不是语言学硬限制，而是教学收敛规则。

---

# 7. Semantic Cluster 收敛规则

## 7.1 Intent 收敛

新增表达必须继续竞争当前 Communicative Intent。

## 7.2 Dimension 收敛

核心成员应该主要由少量公共维度描述，例如：

```text
intensity
duration
trigger_type
moral_judgment
register
```

如果需要大量互不相关的维度才能解释，说明 Cluster 可能过大。

## 7.3 Member 收敛

新增成员需要满足：

```text
新增辨析价值 > 新增认知负担
```

否则移入 Extended Neighborhood。

## 7.4 Boundary 收敛

每个 Core Member 至少应该与一个其他 Core Member 形成高价值 Semantic Boundary。

## 7.5 Scenario 收敛

核心 Boundary 必须能够通过真实 Contrastive Scenario 被验证。

如果无法构造稳定场景让候选表达产生自然度差异，则该 Boundary 不适合作为当前教学重点。

---

# 8. Semantic Cluster 评定指标

| 指标 | 说明 |
|---|---|
| Intent Cohesion | 是否围绕同一个表达意图 |
| Semantic Proximity | 成员之间是否足够接近，形成真实竞争 |
| Discriminability | 是否存在稳定、清晰的区别维度 |
| Scenario Separability | 是否能通过场景稳定地区分 |
| Confusion Value | 学习者是否真的容易混淆 |
| Pragmatic Coverage | 是否覆盖有价值的语用差异 |
| Usage Utility | 是否高频、常用、值得主动掌握 |
| Compactness | 是否保持合理认知负担 |
| Boundary Density | 是否存在足够多高价值边界 |

---

## 8.1 三个硬门槛

以下任意一项不满足，就不能进入 Core Cluster：

```text
① 必须竞争同一个表达意图
② 必须存在可解释的区别维度
③ 必须能够构造有效的最小对比场景
```

---

# 9. Semantic Boundary

Semantic Boundary 是系统中的一等公民。

节点告诉系统“学什么”，Boundary 告诉系统“到底需要学会什么”。

示例：

```text
SemanticBoundary

expression_a: angry
expression_b: outraged

dimensions:
  - injustice
  - moral_violation

difference:
  outraged often highlights anger caused by perceived injustice
  or moral violation

relation_type:
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

关系类型至少需要区分：

```text
HARD
TENDENCY
REGISTER
PRAGMATIC
COLLOCATION
STYLE
CONSTRUCTION
```

避免把语言中的倾向误建模成绝对规则。

---

# 10. Semantic Boundary 评估指标

单条 Boundary 可评价：

```text
confusability
difference_clarity
scenario_separability
usage_frequency
learning_value
relation_strength
confidence
```

推荐：

> 先评 Boundary，再汇总 Cluster。

因为真正需要训练的是表达之间的选择边界。

---

# 11. AI Semantic Cluster Evaluation Pipeline

AI 负责发现、解释、生成和评估，但最终规则由系统掌控。

推荐流程：

```text
Candidate Expressions
        ↓
Intent Analysis
        ↓
Semantic / Pragmatic Feature Extraction
        ↓
Pairwise Boundary Analysis
        ↓
Boundary Scoring
        ↓
Contrastive Scenario Generation
        ↓
Scenario Validation
        ↓
Cluster-level Scoring
        ↓
Rule-based Convergence
        ↓
CORE / EXTENDED / REJECT
```

AI 不允许只输出一个总分。

必须提供：

```text
score
reason
evidence
boundary
dimension
relation_type
confidence
```

---

# 12. AI 评分概念输出

```json
{
  "intent": "express anger or displeasure",
  "members": [
    "annoyed",
    "irritated",
    "angry",
    "furious",
    "outraged"
  ],
  "primary_dimensions": [
    "intensity",
    "persistence",
    "trigger_type",
    "injustice_or_moral_violation"
  ],
  "cluster_metrics": {
    "intent_cohesion": {"score": 0.94, "reason": "..."},
    "semantic_proximity": {"score": 0.88, "reason": "..."},
    "discriminability": {"score": 0.93, "reason": "..."},
    "scenario_separability": {"score": 0.91, "reason": "..."},
    "confusion_value": {"score": 0.85, "reason": "..."},
    "pragmatic_coverage": {"score": 0.89, "reason": "..."},
    "usage_utility": {"score": 0.95, "reason": "..."},
    "compactness": {"score": 0.92, "reason": "..."}
  },
  "boundaries": [
    {
      "expression_a": "annoyed",
      "expression_b": "irritated",
      "dimensions": ["persistence"],
      "difference": "irritated often suggests a more sustained or repeated source of annoyance",
      "relation_type": "TENDENCY",
      "confusability": 0.86,
      "difference_clarity": 0.81,
      "scenario_separability": 0.84,
      "learning_value": 0.90,
      "confidence": 0.82
    }
  ],
  "recommendation": {
    "status": "CORE_CLUSTER",
    "remove": [],
    "move_to_extended": []
  },
  "overall_confidence": 0.89
}
```

---

# 13. 从 Expression Selection 到整句表达

Semantic Cluster 解决的是：

> 选什么表达？

但真实语言输出的最终执行单位通常不是孤立的 Word 或 Phrase，而是完整 Utterance。

因此仅训练 Expression Selection 不足以达到“脱口而出”。

完整链路需要进一步建立：

```text
Communicative Intent
        ↓
Semantic Cluster
        ↓
Expression Unit
        ↓
Utterance Pattern
        ↓
Example Sentences
        ↓
Scenario Variation
        ↓
Active Production
        ↓
Spontaneous Production
```

---

# 14. Expression Unit、Utterance Pattern、Example Sentence 的区别

## 14.1 Expression Unit：选什么

例如：

```text
resent
hate
be fed up with
can't stand
```

它解决的是：

> 当前语境应该选哪个表达？

---

## 14.2 Utterance Pattern：怎么说

Expression Unit 进入真实句子时通常存在稳定构造。

例如：

```text
I resent + noun
I resent + doing
I resent the way + clause

I'm fed up with + noun
I'm fed up with + doing

I can't stand + noun
I can't stand + doing
I can't stand it when + clause
```

Utterance Pattern 解决：

> 选到这个表达以后，它通常如何自然进入一句话？

---

## 14.3 Example Sentence：真实的人到底怎么说

例如：

```text
I really resent the way he takes credit for my work.
```

Example Sentence 不只是语法示例，而应该同时绑定：

```text
scenario
communicative_intent
expression_unit
utterance_pattern
register
speaker_relationship
emotion / stance
```

它解决：

> 在一个真实场景里，完整表达最终是什么样子？

---

# 15. 例句学习不是机械背句子

系统不应该让用户孤立背诵：

```text
I resent the way he treats me.
```

而应该让多个相关句子共同暴露一个可复用 Pattern：

```text
I resent the way he treats me.
I resent the way they ignore my contribution.
I resent the way she talks to me.
I resent the way my parents compare me with my brother.
```

最终希望大脑抽象出：

```text
I resent the way + someone + does something
```

并同时形成语用感觉：

> 当某种持续的对待让我产生不公平感或积累的不满时，这种表达很自然。

这不是 Sentence Memorization，而是：

> Utterance Pattern Acquisition。

---

# 16. 为什么需要 Example Sentence Network

未来不应该只维护：

```text
Expression → Example Sentences
```

更应该维护句子之间的关系：

```text
Sentence A
↕ same pattern
Sentence B
↕ same semantic boundary
Sentence C
↕ same intent, different register
Sentence D
↕ same expression, different scenario
Sentence E
```

这样“例句”本身也形成学习网络。

用户不是记住某一个固定句子，而是在多个句子之间形成：

```text
语义共同点
+
句式共同点
+
场景变化规律
```

最终获得迁移能力。

---

# 17. 学习层级升级

一个表达真正掌握，建议分为六层：

## Level 1 — Semantic Recognition

```text
I understand this expression.
```

能够理解表达的核心含义。

## Level 2 — Semantic Discrimination

```text
I can distinguish it from neighboring expressions.
```

知道它与相邻表达的边界。

## Level 3 — Expression Selection

```text
I can choose it in the right scenario.
```

面对陌生场景能够选到合适表达。

## Level 4 — Utterance Construction

```text
I know how this expression naturally enters a sentence.
```

知道常用 Construction / Pattern。

## Level 5 — Pattern Internalization

```text
I can reuse the pattern across different scenarios.
```

在多个不同场景中使用相同表达骨架，而不是背固定例句。

## Level 6 — Spontaneous Production

```text
The situation triggers a natural sentence directly.
```

真实交流中不再经历：

```text
中文
→ 英文单词
→ 查语法
→ 组句子
```

而逐渐形成：

```text
Situation
→ Intent / Semantic Features
→ Expression + Utterance Pattern
→ Natural Sentence
```

---

# 18. 产品最终训练的不是知识查询，而是程序化调用

传统词汇知识更接近 declarative knowledge：

```text
resent:
feel angry because you believe you were treated unfairly
```

而流利表达需要逐渐形成 procedural language knowledge：

```text
I resent being ...
I resent the way ...
I'm starting to resent ...
```

因此：

> 语言输出不是知识查询，最终应该越来越接近程序化调用。

Semantic Cluster 帮助用户选对表达；Utterance Pattern 帮助用户自然组织语言；多场景 Example Sentence 帮助表达逐渐内化。

---

# 19. 系统的三个核心问题

可以把整个产品最终压缩为三个问题：

### Semantic Cluster

```text
选什么？
```

### Utterance Pattern

```text
怎么说？
```

### Scenario

```text
什么时候说？
```

三者共同构成 Context Vocab 所追求的地道表达能力。

---

# 20. 更新后的整体领域模型

```text
Expression Knowledge Graph
        ↓
Communicative Intent
        ↓
Semantic Cluster
        ↓
Expression Units
        ↓
Semantic / Pragmatic / Syntactic Boundaries
        ↓
Utterance Patterns
        ↓
Example Sentence Network
        ↓
Scenario Training
        ↓
Learner Model
        ↓
Adaptive Practice
        ↓
Spontaneous Production
```

这里不再把 Vocabulary Knowledge Graph 作为最高层抽象，更准确的叫法是：

> **Expression Knowledge Graph**

因为产品最终关心的不是用户认识多少 Word，而是能否自然调用合适的 Expression。

---

# 21. AI 在句子层的职责

除了 Cluster / Boundary 评估，AI 还承担：

```text
Expression → Utterance Pattern 提取
Pattern → 多场景例句生成
Sentence → Naturalness 评估
Sentence → Register 评估
Sentence → Semantic Boundary 解释
用户自由表达 → Native Refinement
```

但 AI 仍然不是系统控制器。

以下内容必须由领域模型和规则掌控：

```text
Cluster 收敛
Boundary 状态
Pattern 状态
Learner State
Mastery / Convergence
复习调度
```

---

# 22. P0 当前核心结论

Context Vocab 不应该建模为：

```text
Word → Meaning
```

也不应该只停留在：

```text
Semantic Cluster → Word Choice
```

完整目标应为：

```text
Intent
→ Semantic Cluster
→ Expression Selection
→ Utterance Pattern
→ Scenario-based Sentence Acquisition
→ Spontaneous Production
```

核心原则：

> **Semantic Cluster 是“选什么”。**

> **Utterance Pattern 是“怎么说”。**

> **Scenario 是“什么时候说”。**

最终学习结果不是“我知道这个单词”，而是：

> **当真实场景出现时，我能够直接调出语义准确、语域合适、句式自然的完整表达。**
