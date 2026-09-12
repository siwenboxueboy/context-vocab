# Core Architecture / Product Learning Model

> Context Vocab 的核心不是“背单词”，而是通过 **语义簇 + 系统化场景 + AI 训练**，最终让用户在真实场景中能够自然、精准、低认知负担地脱口而出地道表达。

---

## 1. 北极星目标

系统的最终目标不是：

```text
记住更多单词
```

而是：

```text
真实场景出现
→ 快速识别表达意图
→ 激活合适的语义区域
→ 选择自然表达
→ 调用熟悉句式
→ 直接输出完整句子
```

最终 Mastery 标准是：

> **Spontaneous Production**
>
> 用户不依赖中文翻译、不临时搜索语法、不依赖候选项，在真实场景中能够快速说出自然、地道、符合语境的完整表达。

---

## 2. 三大核心系统

Context Vocab 的核心产品骨架由三部分组成：

```text
Semantic System
语义簇 + 语义边界

Scenario System
系统化场景模型 + 场景生成

AI Training System
自适应训练 + 诊断 + 收敛
```

三者分别解决：

```text
Semantic Cluster
解决：学什么、和什么一起学、区别在哪

Scenario System
解决：这些区别在什么真实环境里会出现

AI Training
解决：如何根据用户表现动态训练、纠错、迁移和收敛
```

---

# 3. Semantic System：决定“学什么”

## 3.1 Semantic Cluster

Semantic Cluster 是系统组织表达知识的核心结构。

它不是简单的同义词集合，而是：

> 一组围绕同一或高度相近 Communicative Intent、在真实表达中存在选择竞争，并且可以通过有限语义/语用维度进行区分的表达集合。

例如：

```text
annoyed
irritated
angry
furious
outraged
resentful
```

共同服务于：

```text
express anger / displeasure
```

但存在不同边界：

```text
angry ↔ furious
intensity

angry ↔ outraged
injustice / moral violation

angry ↔ resentful
immediate anger vs accumulated grievance
```

---

## 3.2 Expression Unit

Semantic Cluster 的成员不是只允许 Word。

统一抽象为：

```text
Expression Unit
```

可能包括：

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
loathe
can't stand
be fed up with
have had enough of
```

系统关心的不是表达形式是否一致，而是：

> 它们是否在真实语言选择中竞争同一个表达意图。

---

## 3.3 Semantic Boundary

真正需要学习的不是 Cluster 本身，而是 Cluster 内部的 Boundary。

```text
Semantic Cluster
决定：哪些表达值得一起比较

Semantic Boundary
决定：用户到底要学会什么区别
```

Boundary 可以来自：

```text
intensity
persistence
cause
injustice
moral judgment
register
social relationship
speaker attitude
collocation
style
```

并区分：

```text
HARD
TENDENCY
REGISTER
PRAGMATIC
COLLOCATION
STYLE
```

避免把语言中的概率性倾向错误建模为绝对规则。

---

# 4. Scenario System：决定“什么时候这样说”

## 4.1 场景是一等公民

场景不能只是 AI 临时随机生成的一段文字。

Scenario 应该像 Semantic Cluster 一样，是系统中的核心领域对象。

原因是场景承担：

```text
承载语义差异
暴露用户真实混淆
控制训练难度
实现跨场景迁移
避免机械记题
```

---

## 4.2 Scenario 层级

推荐场景模型：

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

例如：

```text
Domain:
Workplace

Family:
Unfair Treatment

Template:
某人获得了本应属于你的利益、认可或成果

Variables:
relationship = boss / coworker / client
duration = once / repeated
severity = mild / serious
register = casual / formal
emotion = immediate / accumulated
medium = conversation / email / complaint

Concrete Scenario:
Your manager repeatedly presents your work as his own during meetings with senior leadership.
```

---

## 4.3 场景核心维度

首版至少考虑：

```text
Domain
Relationship
Communicative Goal
Intensity / Stakes
Duration
Cause
Register
Medium
Expected Response Form
```

例如：

```text
Domain:
workplace / family / friendship / relationship / school / travel / shopping / public events

Relationship:
stranger / friend / partner / coworker / manager / client / parent

Register:
casual / neutral / professional / formal

Medium:
face-to-face / chat / email / meeting / social media
```

这些指标主要供系统内部使用，前台不直接向用户暴露复杂评分。

---

## 4.4 三类核心场景

### Teaching Scenario

目标：第一次建立典型语义感觉。

特点：

```text
线索明显
语义纯度高
干扰较少
```

---

### Boundary Scenario

目标：精确区分两个或多个近义表达。

特点：

```text
尽量控制其他变量
只突出目标 Boundary
```

例如：

```text
furious ↔ outraged
```

重点控制：

```text
是否存在 injustice / moral violation
```

---

### Transfer Scenario

目标：确认用户真正理解了语义，而不是记住固定例句。

例如用户已经在 workplace 场景学会 `resentful`，继续迁移到：

```text
family
friendship
romantic relationship
school
public life
```

只有跨领域仍能正确调用，才认为语义边界趋于收敛。

---

## 4.5 场景难度

难度不能简单等价于句子更长或词汇更难。

真正的难度来自：

```text
语义线索显式 → 隐式
候选词差异大 → 差异小
单一情绪 → 混合情绪
单一变量 → 多变量
给出选项 → 不给选项
有提示 → 无提示
固定领域 → 跨领域迁移
```

例如：

```text
Level 1
明显提示“不公平”

Level 2
只通过行为隐含不公平

Level 3
加入复杂关系和长期积累

Level 4
不给目标词，只要求自由表达

Level 5
实时对话中自然触发表达
```

---

# 5. AI Training System：决定“怎么真正学会”

AI 不是产品本身，而是训练引擎。

AI 的职责包括：

```text
生成场景
控制变量
构造边界测试
判断表达自然度
解释为什么 A 比 B 更合适
识别用户混淆维度
决定下一轮训练重点
逐步撤掉脚手架
验证跨场景迁移
```

---

## 5.1 AI 不只判断对错

系统不应该只输出：

```text
Correct / Wrong
```

而应该判断：

```text
Meaning Match
Intensity Match
Register Match
Pragmatic Match
Collocation Naturalness
Native Preference
```

但这些内部指标不需要完整暴露给用户。

用户主要看到：

```text
场景
→ 推荐表达
→ 一句核心解释
→ 少量视觉提示
```

复杂指标留在后台。

原则：

> **用户学习的是场景，系统管理的是指标。**

---

# 6. 用户前台与系统后台分离

## 6.1 系统后台

后台可以维护复杂模型：

```text
Semantic Cluster Score
Boundary Quality
Scenario Separability
Confusion Value
Register
Intensity
Pragmatic Features
Learner State
Confidence
```

## 6.2 用户前台

用户优先看到：

```text
Scenario
↓
Expression
↓
Contrast
↓
Lightweight Semantic Cues
↓
Optional Deep Explanation
```

例如内部可能维护：

```text
intensity = 0.87
moral_judgment = 0.92
register = 0.71
```

用户侧只需要轻量呈现：

```text
🔥 强度高
⚖️ 带公平 / 道德判断
💬 日常 / 正式倾向
⏱️ 长期积累
```

图标用于降低理解成本，而不是让用户学习一套新的指标术语。

---

# 7. 从 Expression 到完整句子

用户最终交流时输出的不是 Word，而是 Sentence / Utterance。

因此需要继续向上建模：

```text
Communicative Intent
        ↓
Semantic Cluster
        ↓
Expression Unit
        ↓
Utterance Pattern
        ↓
Example Sentence Network
        ↓
Scenario Retrieval
        ↓
Spontaneous Production
```

---

## 7.1 Utterance Pattern

解决：

> 选到这个表达之后，英语通常怎么自然地说出来？

例如：

```text
resent

I resent + noun
I resent + doing something
I resent the way + clause
I'm starting to resent + noun / person
```

---

## 7.2 Example Sentence Network

目标不是背一条固定例句，而是通过多个相关例句抽取出表达骨架。

例如：

```text
I resent the way he treats me.
I resent the way they ignore my contribution.
I resent the way she talks to me.
I resent the way my parents compare me with my brother.
```

最终内化：

```text
I resent the way + someone + does something
```

这比死记一条例句更接近真实语言能力。

---

# 8. Learner Model

系统需要维护的不是简单：

```text
furious 熟练度 = 82%
```

而是用户对不同 Boundary 和表达层级的掌握状态。

例如：

```text
angry ↔ furious
boundary discrimination = strong

angry ↔ outraged
boundary discrimination = weak

resentful
recognition = strong
context matching = medium
active retrieval = weak
spontaneous production = weak
```

AI Trainer 基于这些状态决定下一轮训练内容。

---

# 9. 学习阶段

建议能力阶段：

```text
1. Semantic Recognition
   能理解表达

2. Boundary Discrimination
   能区分相邻表达

3. Context Matching
   给场景能选对

4. Guided Production
   有提示能说出来

5. Free Production
   无候选项能主动调用

6. Pattern Internalization
   能自然使用常见句式

7. Cross-context Transfer
   换领域仍然会使用

8. Spontaneous Production
   低认知负担、快速、自然脱口而出
```

只有第 8 阶段才是真正意义上的 Mastered。

---

# 10. 脚手架逐步撤除

训练过程中，系统应该不断减少帮助：

```text
4 个候选项
↓
2 个候选项
↓
给关键词
↓
给句式提示
↓
只给场景
↓
自由回答
↓
实时对话
```

核心原则：

> 用户不能永远依赖选择题。

如果产品目标是 Spontaneous Production，训练后期必须进入主动检索和自由表达。

---

# 11. 三系统闭环

完整闭环：

```text
Semantic System
生成当前值得训练的 Boundary
        ↓
Scenario System
构造能够暴露该 Boundary 的场景
        ↓
AI Training
要求用户选择或自由表达
        ↓
AI Evaluation
判断自然度与错误来源
        ↓
Learner Model
更新用户掌握状态
        ↓
Weak Boundary Detection
定位仍未掌握的区别
        ↓
Scenario System
生成新的针对性 / 迁移场景
        ↓
继续训练
        ↓
Spontaneous Production
```

---

# 12. 一个完整示例

目标：

```text
angry ↔ outraged
```

Semantic Boundary：

```text
共同点：strong anger
主要区别：outraged 更倾向于由 perceived injustice / moral violation 触发
```

Scenario A：

```text
Someone accidentally scratches your new car.
```

更自然：

```text
angry / furious
```

Scenario B：

```text
A company knowingly hides evidence that its product is seriously harming customers.
```

更自然：

```text
outraged
```

随后迁移：

```text
workplace unfairness
social injustice
school discrimination
friend betrayal
public scandal
```

最终不给候选项，只给：

```text
Tell a friend how you feel about what happened.
```

用户能够自然说出：

```text
I'm absolutely outraged that they knew about it and did nothing.
```

此时才接近真正掌握。

---

# 13. 三条核心产品原则

### 原则一

> **Semantic Cluster 决定学什么，Semantic Boundary 决定区别是什么。**

### 原则二

> **Scenario System 决定这些区别如何进入真实语言环境。**

### 原则三

> **AI Training 负责把理解逐步转化为能够脱口而出的程序化语言能力。**

最终可以浓缩为：

```text
语义簇组织表达
场景承载差异
AI 完成训练
```

目标只有一个：

> **让合适的英语表达在合适的场景中，自然地被调用出来。**
