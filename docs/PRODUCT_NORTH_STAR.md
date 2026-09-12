# Product North Star · Spontaneous Production

## 1. 终极目标

Context Vocab 的终极目标不是让用户“记住更多单词”，也不是只让用户“理解近义词之间的区别”。

真正的终点是：

> **当真实场景出现时，用户能够不经过中文翻译、不临时搜索语法、不在多个词之间长时间犹豫，直接脱口而出自然、准确、地道的英语表达。**

这被定义为产品的 North Star：

```text
Spontaneous Production
```

也就是：

```text
真实场景
→ 语义识别
→ 表达自动激活
→ 自然句式自动激活
→ 低认知负担输出完整表达
```

---

## 2. 为什么“记住单词”不是终点

传统学习路径通常停留在：

```text
看到单词
→ 知道中文意思
→ 能在选择题中认出来
```

即使进一步做到：

```text
给一个场景
→ 能从候选词中选对
```

也仍然没有完成真实语言输出。

真实交流要求的是：

```text
场景出现
↓
理解自己想表达的语义和态度
↓
直接激活合适的 Expression
↓
直接激活自然的 Utterance Pattern
↓
完整句子自然输出
```

因此，记忆只是自动化调用的前置条件，而不是产品成功的最终标准。

---

## 3. 三层核心学习对象

### 3.1 Semantic Cluster：选什么

Semantic Cluster 解决：

> 在当前表达意图下，有哪些相近表达正在竞争？

例如：

```text
annoyed
irritated
angry
furious
outraged
resentful
```

它帮助用户建立表达空间，而不是孤立记忆单词。

---

### 3.2 Semantic Boundary：为什么选它

Semantic Boundary 解决：

> 为什么这个场景应该用 A，而不是 B？

例如：

```text
angry ↔ furious
主要差异：intensity

angry ↔ outraged
主要差异：injustice / moral violation

angry ↔ resentful
主要差异：immediate anger vs lingering grievance
```

Semantic Boundary 是“精准选词能力”的核心。

---

### 3.3 Utterance Pattern：怎么自然说出来

只会选择正确的 Expression 仍然不够。

用户还需要把表达转化为能够直接调用的自然句式。

例如学习 `resent`，不能只留下：

```text
resent
```

而应该逐渐形成：

```text
I resent + noun
I resent + doing something
I resent the way + clause
I'm starting to resent + ...
```

再通过多个真实句子形成模式：

```text
I resent the way he treats me.
I resent being left out of important decisions.
I really resent the way they ignore my contribution.
I'm starting to resent having to fix the same problem every week.
```

用户最终记住的不应只是某一句例句，而是可复用的表达模式。

---

## 4. Example Sentence Network

例句不是单词释义的附件，而是表达自动化的重要训练数据。

单个例句的价值有限：

```text
I resent the way he treats me.
```

多个结构相近但场景不同的例句，可以让用户逐渐抽象出 Pattern：

```text
I resent the way he treats me.
I resent the way they ignore my work.
I resent the way she speaks to me.
I resent the way my parents compare me with my brother.
```

用户最终形成：

```text
I resent the way + someone + does something
```

因此系统应该逐渐构建：

```text
Expression Unit
        ↓
Utterance Pattern
        ↓
Example Sentence Network
        ↓
Scenario Variations
        ↓
Active Retrieval
```

而不是：

```text
Word
→ 固定例句
```

---

## 5. 完整学习层级

产品中的 Mastery 不应只用“认识 / 不认识”表示。

推荐采用以下层级：

### Level 1 · Recognition

```text
I understand this expression.
```

用户看到表达能够理解。

### Level 2 · Semantic Discrimination

```text
I can distinguish it from neighboring expressions.
```

用户知道它与语义相近表达的边界。

### Level 3 · Context Matching

```text
I can choose it in the right scenario.
```

给出陌生场景，用户能够选出合适表达。

### Level 4 · Guided Production

```text
I can produce it with hints.
```

给出关键词、句式骨架或少量提示后，用户能够完整表达。

### Level 5 · Free Production

```text
I can produce it without candidate answers.
```

只给真实场景，用户能够主动组织表达。

### Level 6 · Cross-context Transfer

```text
I can use the same semantic distinction in different domains.
```

工作、家庭、社交、正式交流等不同场景下，用户仍能正确调用。

### Level 7 · Spontaneous Production

```text
I can say it naturally with low cognitive effort.
```

这是最终 Mastered 状态。

真实场景出现后：

```text
不翻译中文
不搜索语法
不显著犹豫
不依赖候选项
↓
自然输出
```

---

## 6. 训练脚手架必须逐渐撤掉

产品不能一直停留在选择题。

训练应该逐渐撤除辅助：

```text
4 个候选表达
↓
2 个高混淆候选
↓
关键词提示
↓
Utterance Pattern 提示
↓
只给场景
↓
自由表达
↓
实时对话 / 连续场景
```

如果用户永远依赖选项，他训练的是 recognition，而不是 production。

因此系统必须有明确的：

```text
Scaffolding Removal Strategy
```

目标是不断降低外部提示，让语言知识逐渐内化为自动调用能力。

---

## 7. 产品完整能力链路

```text
Semantic Recognition
        ↓
Semantic Cluster
        ↓
Semantic Boundary
        ↓
Expression Selection
        ↓
Utterance Construction
        ↓
Pattern Internalization
        ↓
Repeated Retrieval
        ↓
Cross-context Transfer
        ↓
Spontaneous Production
```

可以简化为三个问题：

> **Semantic Cluster：选什么？**

> **Utterance Pattern：怎么说？**

> **Scenario：什么时候说？**

三者共同决定最终的地道表达能力。

---

## 8. 产品设计最高约束

未来无论设计 Semantic Cluster、AI Judge、场景生成、SRS、用户能力模型还是前端训练方式，都必须回答：

> **这个功能是否让用户更接近“真实场景下脱口而出地道表达”？**

如果一个功能只能增加词汇数量、学习时长或内容复杂度，却不能提升真实表达的自动调用能力，那么它就不应该成为核心功能。

---

## 9. 最终成功标准

产品成功不能只用：

```text
背了多少词
答对了多少题
连续学习多少天
```

来衡量。

更重要的结果指标应该逐步围绕：

```text
Semantic Accuracy
Expression Selection Accuracy
Utterance Naturalness
Prompt Dependency
Retrieval Latency
Cross-context Transfer
Spontaneous Production Rate
```

最终判断学习是否成功的标准是：

> **当真实场景出现时，合适的英语表达能否快速、自然、低认知负担地被调用出来。**

这条原则是 Context Vocab 后续所有产品设计与技术设计的最高约束。
