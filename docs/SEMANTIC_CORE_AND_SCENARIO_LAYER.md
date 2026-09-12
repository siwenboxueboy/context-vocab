# 语义知识核心与场景层设计

> 本文档修订 Context Vocab 的语义与场景架构。核心结论是：**真实语言不是一棵分类树，而是由表达用法、语义关系、语用约束和真实语料证据组成的多对多关系网络。语义簇不是语言本体，而是为了特定学习目标，从底层语言事实网络中切出的教学视图；场景则是对语义边界的表现、训练和验证环境。**

---

## 1. 核心架构原则

系统必须区分三个不同层次：

```text
真实语言事实
↓
学习知识组织
↓
训练场景与学习运行
```

分别对应：

```text
表达用法 / 义项 + 语义关系 + 语料证据
↓
语义簇 / 语义边界 / 表达句式模式
↓
场景 / 训练任务 / 用户学习状态
```

因此，新的核心原则是：

> **现实语言是图结构，语义簇是教学视图，场景是训练投影。**

不能因为前端希望看到清晰的分类，就把底层语言知识也强行建模成树形结构。

---

## 2. 为什么“一个表达只属于一个语义簇”不符合现实语言

真实语言存在大量一词多义、多义项、多语境和多重表达竞争关系。

例如：

```text
cold
```

可能表示：

```text
温度低
食物变凉
态度冷淡
情感疏离
缺乏同情
```

如果系统只建立一个：

```text
ExpressionUnit = cold
```

然后直接把它放入某个语义簇，就会把多个不同用法混在一起。

因此需要明确区分：

```text
表达形式（Expression Form）
        ↓
表达用法 / 义项（Expression Sense / Usage）
```

例如：

```text
cold
├── cold#temperature
├── cold#emotionally_distant
└── cold#unfriendly
```

真正参与语义关系、语义簇、场景匹配和教学判断的，主要应该是：

> **表达在某个具体用法下的意义，而不是裸字符串本身。**

---

## 3. 底层核心对象：表达用法 / 义项

建议把公共语义知识的基本单位提升为：

```text
表达单元（Expression Unit）
        ↓
表达用法 / 义项（Expression Sense）
```

表达单元描述“形式是什么”，例如：

```text
resent
be fed up with
can't stand
get on my nerves
```

表达用法描述“这个形式在当前语义和语用条件下表达什么”。

概念模型：

```text
ExpressionSense
├── expression_unit_id
├── core_meaning
├── communicative_function
├── semantic_features
├── pragmatic_features
├── register
├── usage_constraints
├── collocations
├── syntactic_patterns
└── evidence_refs
```

一个表达单元可以拥有多个表达用法；一个表达用法也可以参与多个学习关系。

---

## 4. 语义簇不是语言本体，而是学习视图

语义簇（Semantic Cluster）仍然是产品非常重要的学习组织方式，但它不再被定义为最底层语言事实。

更准确的定义是：

> **语义簇是为了某个学习目标、表达意图或使用环境，从底层表达用法与语义关系网络中切出的一组高价值竞争表达。**

因此，同一个表达用法可以同时出现在多个语义簇中。

例如：

```text
resentful
```

可以进入：

```text
愤怒表达语义簇
长期不满表达语义簇
不公平感表达语义簇
职场负面情绪学习簇
```

这些簇并不互相冲突，因为它们服务于不同的学习任务。

因此底层关系必须是：

```text
ExpressionSense
      ↕ 多对多
SemanticCluster
```

而不是：

```text
ExpressionSense
      ↓
唯一 SemanticCluster
```

---

## 5. 语义簇具有学习目标上下文

语义簇不是绝对真理，它具有教学上下文。

例如：

```text
angry
mad
pissed
furious
```

如果目标是：

```text
日常口语
```

它们可能构成很有价值的学习簇。

如果目标是：

```text
正式职场英语
```

`pissed` 的学习优先级和成员角色就可能下降。

如果目标是：

```text
学术写作
```

这一整个簇都可能不再是当前核心。

因此：

```text
底层语义关系网络
+
学习目标
+
使用领域
+
语域
+
学习者阶段
↓
Semantic Cluster View
```

语义簇本质上是一种：

> **面向学习目标的动态知识视图。**

---

## 6. 语义边界仍然是核心教学知识

虽然语义簇只是视图，但语义边界（Semantic Boundary）仍然是系统最重要的教学知识之一。

例如：

```text
angry ↔ furious
主要区别：强度

angry ↔ outraged
主要区别：不公平感 / 道德冒犯

angry ↔ resentful
主要区别：即时愤怒 vs 长期积累的不满
```

不过语义边界也必须承认语言的概率性。

不能把所有区别都建成：

```text
A 永远如此
B 永远如此
```

关系类型至少需要区分：

```text
HARD
明确限制

TENDENCY
常见倾向，但不是绝对规则

REGISTER
语域差异

PRAGMATIC
语用差异

COLLOCATION
搭配差异

STYLE
风格差异

CONSTRUCTION
句法 / 构式差异
```

原则：

> **语言中的倾向必须建模成倾向，不能为了系统整齐而伪装成硬规则。**

---

## 7. 一个表达可以参与多个语义边界

语义边界天然也是图结构。

例如：

```text
resentful
├── angry ↔ resentful
├── bitter ↔ resentful
├── frustrated ↔ resentful
└── resentful ↔ fed up
```

不同边界讨论的是不同问题。

因此真正稳定的底层结构更接近：

```text
ExpressionSense
      ↕
SemanticRelation / SemanticBoundary
      ↕
ExpressionSense
```

语义簇只是从这些关系里选择一部分，组织成当前学习任务。

---

## 8. 真实语料证据必须成为一等公民

仅靠 AI 判断语言差异是不够的。

系统必须显式引入：

> **用法证据 / 语料证据（Usage Evidence / Corpus Evidence）**

语料证据用于回答：

```text
这种用法在真实英语中是否存在？
是否常见？
常和什么词搭配？
通常出现在哪种语域？
哪些上下文更自然？
这个所谓的“语义区别”到底有多稳定？
```

概念模型：

```text
UsageEvidence
├── source
├── source_type
├── context
├── expression_sense_id
├── register
├── domain
├── collocation
├── frequency_signal
├── evidence_strength
└── provenance
```

语料证据可以来自不同合法来源，例如公开语料、词典与语言资料、经过许可的数据集、人工审核样本以及真实用户学习数据的统计结果。

---

## 9. 语义边界必须受到语料证据约束

例如系统提出：

```text
irritated 比 annoyed 更强调持续或反复刺激
```

系统不能直接把它保存成 HARD RULE。

更合理的结构是：

```text
relation_type = TENDENCY

supporting_evidence:
- 多个真实用例
- 高频搭配差异
- 多来源一致性

confidence = 0.78
```

因此：

```text
AI 判断
+
真实语料证据
+
领域规则
+
人工 / Judge 验证
↓
正式 Semantic Boundary
```

而不是：

```text
AI 说了
↓
系统就认为是真的
```

---

## 10. 场景不是和词直接绑定，而主要与语义边界绑定

同一个表达用法可以出现在大量场景里。

例如 `resentful` 可以出现在：

```text
职场长期被抢功劳
家庭长期偏心
朋友长期占便宜
伴侣长期忽视
团队利益分配不公平
```

因此简单的：

```text
Expression ↔ Scenario
```

不足以表达产品真正的教学逻辑。

更重要的是：

```text
SemanticBoundary
      ↕ 多对多
Scenario
```

例如：

```text
angry ↔ resentful
↓
长期被抢功劳
长期被忽视贡献
长期受到不公平待遇
```

这些场景共同服务于：

```text
即时愤怒
vs
长期积累的不公平感
```

而 `angry ↔ furious` 则需要另一组主要表现“强度差异”的场景。

因此：

> **场景不是为了证明某个词可以出现，而是为了让目标语义边界在真实语境中可感知、可训练、可验证。**

---

## 11. 场景自身也是多维和多对多的

一个场景可能同时关联：

```text
多个表达用法
多个语义边界
多个学习目标
多个标签
多个句式模式
```

例如“同事长期抢功劳”的场景，既可以训练：

```text
angry ↔ resentful
```

也可能用于：

```text
表达不满
职场反馈
委婉指出不公平
```

所以场景域也不能做成单一树形分类。

场景体系更适合：

```text
Scenario Family
+
Scenario Template
+
Scenario Variables
+
Boundary Mapping
+
Learning Metadata
```

共同描述。

---

## 12. 新的公共学习基座分层

公共学习基座建议明确分成四层：

### 第一层：语言事实层

回答：

> 真实英语实际上是怎么使用的？

包括：

```text
表达形式
表达用法 / 义项
真实语料证据
搭配
语域
频率信号
语用限制
```

### 第二层：语义知识层

回答：

> 我们如何把真实语言中的关系抽象成可学习知识？

包括：

```text
表达意图
语义关系
语义边界
语义维度
语义簇学习视图
```

### 第三层：学习资产层

回答：

> 如何把抽象知识变成可以学习和训练的材料？

包括：

```text
表达句式模式
教学示例
场景族
场景模板
标准场景
Boundary ↔ Scenario Mapping
```

### 第四层：学习科学规则层

回答：

> 如何判断学习、难度、掌握和迁移？

包括：

```text
掌握规则
难度模型
提示撤除规则
迁移规则
复习规则
训练策略
```

整体关系：

```text
真实语言事实
↓
语义知识抽象
↓
学习资产构建
↓
学习科学编排
↓
用户训练
```

---

## 13. AI 在体系中的正确位置

AI 不定义语言事实，也不拥有系统知识。

AI 负责：

```text
发现候选表达
发现候选义项
发现候选语义关系
总结语料证据
生成候选语义边界
生成候选场景
评估用户自由表达
```

系统负责：

```text
定义领域模型
保存正式知识
验证证据
执行规则
维护版本
控制状态
维护用户学习历史
```

因此仍然坚持：

> **AI 负责发现和生成，系统负责约束、验证、沉淀和治理。**

---

## 14. 面向 AI 内容生产的建议流程

语义知识生产不再从“直接生成 Cluster”开始。

更合理的流程是：

```text
真实语料 / 已有语言资料
        ↓
表达与义项发现
        ↓
候选语义邻居召回
        ↓
语义关系分析
        ↓
Pairwise Boundary 分析
        ↓
语料证据校验
        ↓
关系类型与置信度判断
        ↓
底层语义网络沉淀
        ↓
根据学习目标生成 Semantic Cluster View
```

即：

> **先构建关系，再形成簇；先验证边，再组织教学视图。**

---

## 15. 面向场景生产的建议流程

场景生产必须从目标 Boundary 出发：

```text
目标 Semantic Boundary
+
语料证据
+
使用领域
+
人物关系
+
语域
+
难度要求
↓
Scenario Candidate
↓
Boundary Alignment Judge
↓
Naturalness Judge
↓
Difficulty Judge
↓
正式 Scenario Asset
```

场景生成必须受到真实语言使用分布约束，不能退化成模型的自由创作。

---

## 16. 数据关系原则

逻辑上建议遵循：

```text
ExpressionUnit
    1
    │
    N
ExpressionSense
    │
    ├──── N:N ──── SemanticClusterView
    │
    ├──── N:N ──── SemanticBoundary
    │
    ├──── N:N ──── UtterancePattern
    │
    └──── N:N ──── Scenario

SemanticBoundary
    │
    └──── N:N ──── Scenario

ExpressionSense / Boundary / Scenario
    │
    └──── 关联 UsageEvidence
```

这里描述的是领域关系，不代表现在就必须使用图数据库。

> **领域模型是图结构，不等于物理存储必须使用图数据库。**

技术实现仍可以先使用关系数据库表达多对多关系。

---

## 17. 与向量检索的关系

Embedding / 向量相似度可以帮助：

```text
发现候选近邻表达
召回相似场景
理解自然语言查询
召回相关学习记忆
```

但：

```text
Vector Similarity
≠
Semantic Boundary
≠
Cluster Membership
```

例如两个表达在向量空间中很接近，只能说明它们可能语义相关，不能自动推出它们存在高价值教学竞争关系。

因此：

```text
向量召回
↓
候选集合
↓
语义关系 / 语料证据 / 领域规则 / AI Judge
↓
正式知识
```

向量索引属于派生检索能力，不是语义知识事实源。

---

## 18. 用户体验与底层知识结构必须解耦

底层可以是复杂的多对多关系图，但前台不需要把复杂度全部暴露给用户。

用户可以通过两个一等入口进入学习：

```text
语义入口
→ 我想知道这些表达有什么区别

场景入口
→ 在这种情况下我应该怎么说
```

两者底层使用同一套语义知识。

因此：

> **前端可以简单，底层不能为了简单而失真。**

---

## 19. 长期核心资产重新定义

长期资产优先级调整为：

```text
1. 表达用法 / 义项与真实语料证据

2. 高质量语义关系 / Semantic Boundary Network

3. 面向不同学习目标的 Semantic Cluster Views

4. Boundary ↔ Scenario Mapping

5. 真实用户混淆、掌握和迁移数据

6. 学习科学规则与训练策略

7. AI 内容生产、评估与训练流水线
```

因此产品壁垒不应建立在“拥有多少单词”或者“生成多少题”上。

真正重要的是：

> **我们是否能够用真实语言证据建立可信的语义关系，并把这些关系转化成可训练、可验证、可个性化推进的学习系统。**

---

## 20. 最终架构原则

Context Vocab 的语义与场景体系最终遵循：

```text
表达用法 / 义项
决定：这个表达在当前上下文中到底是什么意思

语料证据
决定：这种判断是否符合真实语言使用

语义关系 / 语义边界
决定：表达之间真正需要掌握的区别

语义簇学习视图
决定：当前学习目标下应该把哪些表达放在一起学

场景系统
决定：如何把目标边界表现、训练和验证出来

学习运行引擎
决定：当前这个用户下一步应该怎么练
```

最终目标不变：

> 当真实场景出现时，用户无需经过中文翻译或显式规则推理，就能够快速、自然、准确地调用合适的英语表达。
