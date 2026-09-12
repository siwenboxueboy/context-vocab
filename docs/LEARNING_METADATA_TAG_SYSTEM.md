# Learning Metadata / Tag System

> 本文档定义 Context Vocab 中用于描述“某个语义簇、表达、场景适合谁、适合什么学习目标、应该在什么路径中出现”的元数据体系。

---

## 1. 为什么需要 Tag System

Semantic Cluster 与 Scenario 都需要回答两类完全不同的问题：

```text
Quality Metrics
→ 这个语义簇 / 场景本身质量怎么样？

Learning Metadata
→ 它适合谁？适合什么用途？应该在什么学习路径中出现？
```

这两类数据必须分开。

例如一个语义簇语言学质量很高、边界很清晰，但如果它主要属于科研写作场景，那么对于只想练日常口语的用户，优先级仍然应该很低。

因此：

> Metrics 负责判断“好不好”，Tags / Metadata 负责判断“适不适合当前用户”。

---

## 2. 标签不是平铺字符串

不建议把：

```text
daily
research
common
CET6
formal
```

全部放进一个无结构 tags 数组。

因为这些标签属于完全不同的维度：

```text
daily      → Usage Domain
research   → Usage Domain
common     → Frequency
CET6       → Exam / Learning Goal
formal     → Register
```

因此 Learning Metadata 必须是结构化的。

示例：

```json
{
  "domains": ["daily", "workplace"],
  "learning_goals": ["speaking", "writing"],
  "exam_tracks": ["CET6", "IELTS"],
  "registers": ["neutral", "formal"],
  "frequency": "high",
  "levels": ["B1", "B2"]
}
```

---

## 3. 第一版标签维度

### 3.1 Usage Domain

描述表达或场景主要出现在哪类现实环境。

建议首版：

```text
daily
social
workplace
business
academic
research
travel
school
family
relationship
public_life
online_communication
```

其中 `academic` 与 `research` 可以分开：

- academic：课程、论文、学术讨论等较广义学术语境；
- research：科研论文、实验、研究汇报、技术讨论等更专业场景。

---

### 3.2 Learning Goal

描述用户为什么学习这个资产。

```text
speaking
writing
reading
listening
conversation
email
meeting
presentation
interview
academic_writing
research_writing
```

同一个 Semantic Cluster 可以服务多个 Goal，但对应训练场景与 Utterance Pattern 可以不同。

---

### 3.3 Exam Track

描述与考试体系的关联。

第一版可支持：

```text
CET4
CET6
IELTS
TOEFL
GRE
POSTGRADUATE_ENTRANCE
TEM4
TEM8
```

需要强调：

> Exam Tag 不代表表达只能用于考试，而表示该表达 / 场景对该考试具有较高学习价值。

后续可以进一步区分：

```text
CET6_READING
CET6_WRITING
IELTS_SPEAKING
IELTS_WRITING
TOEFL_SPEAKING
```

但首版不必过度细分。

---

### 3.4 Register

描述表达语域。

```text
very_informal
informal
neutral
formal
academic
professional
literary
```

Register 是“地道表达”的关键标签，因为很多表达并非错误，而是场合不自然。

---

### 3.5 Frequency / Usage Utility

描述真实使用价值，而不是仅描述词典词频。

```text
very_high
high
medium
low
rare
```

需要综合：

```text
真实出现频率
主动表达价值
口语价值
写作价值
当前目标人群价值
```

因此 Frequency 可以由外部语料数据 + AI Judge 共同形成。

---

### 3.6 Proficiency Level

建议优先采用 CEFR：

```text
A1
A2
B1
B2
C1
C2
```

但这里的 Level 不仅是“这个单词难不难”，而应该描述：

> 学习者要稳定理解并使用这个表达 / 边界 / 场景，大约需要什么语言能力。

一个词本身可能是 B1，但它与近义表达之间的精细辨析可能属于 B2/C1。

因此后续可以分别存在：

```text
recognition_level
production_level
boundary_mastery_level
```

---

### 3.7 Communicative Intent

这是 Semantic System 与 Learning Metadata 的连接点。

例如：

```text
express_anger
express_dislike
express_uncertainty
make_request
refuse
complain
criticize
agree
disagree
persuade
suggest
express_probability
express_importance
```

Intent 不应只作为普通标签存在，它本身仍然是 Semantic Cluster 的核心组织维度。

但为了检索、推荐和学习路径编排，可以同步作为可索引元数据。

---

### 3.8 Relationship

主要用于 Scenario。

```text
stranger
friend
close_friend
partner
family
parent_child
coworker
manager_subordinate
client_service
teacher_student
public_audience
```

因为同一个语义意图在不同关系中，表达方式可能显著不同。

---

### 3.9 Medium / Channel

主要用于 Scenario 与 Utterance Pattern。

```text
face_to_face
chat
email
meeting
phone
presentation
social_media
academic_paper
report
```

例如同一个 complaint：

```text
朋友聊天
vs
工作邮件
vs
正式投诉
```

自然表达会完全不同。

---

## 4. Metadata 适用对象

Learning Metadata 不应该只挂在 Semantic Cluster 上。

建议至少支持：

```text
Semantic Cluster
Expression Unit
Semantic Boundary
Scenario Family
Scenario Template
Concrete Scenario
Utterance Pattern
Example Sentence
```

但每类对象关注的标签不同。

### Semantic Cluster

重点：

```text
Domain
Learning Goal
Exam Track
Frequency
Level
Communicative Intent
```

### Expression Unit

重点：

```text
Register
Frequency
Domain
Level
Exam Track
```

### Scenario

重点：

```text
Domain
Relationship
Medium
Register
Learning Goal
Difficulty
Exam Track
```

### Utterance Pattern / Example Sentence

重点：

```text
Register
Medium
Domain
Frequency
Production Level
```

---

## 5. Semantic Cluster Metadata 示例

```json
{
  "cluster": "anger_and_displeasure",
  "members": [
    "annoyed",
    "irritated",
    "angry",
    "furious",
    "outraged",
    "resentful"
  ],
  "metadata": {
    "domains": ["daily", "social", "workplace"],
    "learning_goals": ["speaking", "writing", "conversation"],
    "exam_tracks": ["CET6", "IELTS", "TOEFL"],
    "registers": ["informal", "neutral", "formal"],
    "frequency": "high",
    "levels": ["B1", "B2", "C1"],
    "communicative_intents": ["express_anger", "express_displeasure"]
  }
}
```

---

## 6. Scenario Metadata 示例

```json
{
  "scenario": "manager_repeatedly_takes_credit_for_your_work",
  "target_boundary": "angry_vs_resentful",
  "metadata": {
    "domains": ["workplace"],
    "relationships": ["manager_subordinate"],
    "learning_goals": ["speaking", "conversation"],
    "registers": ["neutral"],
    "mediums": ["face_to_face", "chat"],
    "exam_tracks": ["IELTS"],
    "frequency": "common",
    "level": "B2",
    "scenario_type": "boundary_training"
  }
}
```

---

## 7. 同一个 Semantic Cluster 可以拥有不同学习视图

这是 Tag System 最重要的价值之一。

底层语义边界可以保持稳定，但不同用户目标对应不同内容视图。

例如同一个 `resent`：

### Daily / Conversation

```text
I really resent the way he treats me.
```

重点：

```text
日常关系
情绪表达
自然口语
```

### Workplace

```text
I resent the fact that my contribution keeps being overlooked.
```

重点：

```text
职场不公平
中性表达
职业关系
```

### IELTS / Writing

```text
People may become resentful when they feel they are being treated unfairly.
```

重点：

```text
抽象论述
正式程度提升
写作可迁移
```

因此：

> Semantic Knowledge 可以共享，但 Scenario、Utterance Pattern 与训练优先级应该随 Learning Context 改变。

---

## 8. User Goal Profile

仅有 Learning Metadata 还不够。

用户侧需要对应的 User Goal Profile：

```json
{
  "target_domains": ["daily", "workplace"],
  "learning_goals": ["speaking", "conversation"],
  "exam_tracks": [],
  "preferred_registers": ["informal", "neutral"],
  "current_level": "B1",
  "target_level": "B2"
}
```

另一个用户可能是：

```json
{
  "target_domains": ["academic", "research"],
  "learning_goals": ["academic_writing", "presentation"],
  "exam_tracks": ["IELTS"],
  "preferred_registers": ["formal", "academic"],
  "current_level": "B2",
  "target_level": "C1"
}
```

两个用户可以共享同一份 Semantic Knowledge，但得到完全不同的学习路径。

---

## 9. Learning Priority

后续训练推荐不能只依赖词频或 SRS。

可以抽象为：

```text
Learning Priority
=
Semantic Learning Value
× User Goal Match
× Usage Utility
× Current Weakness
× Review Urgency
```

其中：

```text
Semantic Learning Value
→ 这个边界本身是否值得学

User Goal Match
→ 是否符合当前用户目标

Usage Utility
→ 是否真实常用、有表达价值

Current Weakness
→ 用户当前是否在这个边界上薄弱

Review Urgency
→ 是否需要根据记忆 / 掌握状态重新训练
```

首版不要求立刻实现完整数学公式，但领域模型需要提前保留这些概念。

---

## 10. AI 在 Tag System 中的职责

AI 可以参与 Metadata 生成，但仍然遵守：

> AI generates; system governs.

### AI Metadata Builder

负责根据：

```text
Semantic Cluster
Expression Unit
Scenario
Utterance Pattern
```

生成候选标签。

例如：

```text
Domain
Register
Frequency
Exam Relevance
CEFR Level
Learning Goal
```

### AI Metadata Judge

负责检查：

```text
标签是否合理
标签之间是否冲突
是否过度标记
是否漏掉主要适用场景
```

### System Rules

负责：

```text
标签枚举
层级关系
状态
版本
人工覆盖
最终持久化
```

---

## 11. 用户前台不应该看到完整 Metadata

Metadata 主要用于后台组织和推荐。

用户需要看到的是经过简化后的学习入口，例如：

```text
日常口语
职场英语
CET-6
IELTS Speaking
科研表达
```

以及轻量提示：

```text
💬 日常
💼 职场
🎓 学术
🧪 科研
📝 CET-6
🔥 高频
```

不能要求用户理解后台完整分类体系。

原则仍然是：

> 后台复杂，前台简单。

用户主要关注场景和表达，而不是标签本身。

---

## 12. Tag System 与三大核心系统的关系

```text
                    User Goal Profile
                           │
                           ↓
                  Learning Metadata
                           │
          ┌────────────────┼────────────────┐
          ↓                ↓                ↓
 Semantic System     Scenario System    AI Training
          │                │                │
          ↓                ↓                ↓
筛选适合的语义簇    筛选适合的场景      决定训练优先级
          └────────────────┼────────────────┘
                           ↓
                    Personalized Path
```

因此 Learning Metadata 不是第四个独立教学系统。

它更像横跨 Semantic、Scenario、Training 三层的：

> **Targeting / Routing Layer**

它解决：

> 同一套知识资产，应该如何针对不同用户目标进行筛选、组织和训练。

---

## 13. 核心结论

Context Vocab 后续不能只知道：

```text
这个语义簇质量很好
这个场景很适合训练某条边界
```

还必须知道：

```text
它适合日常还是科研？
适合口语还是写作？
常用还是低频？
适合 B1 还是 C1？
是否覆盖 CET-4 / CET-6 / IELTS？
适合朋友聊天还是正式邮件？
```

最终形成：

```text
Semantic Knowledge
        +
Scenario Assets
        +
Learning Metadata
        +
User Goal Profile
        ↓
Personalized AI Training
```

核心原则：

> **Metrics 判断质量，Metadata 判断适用范围，User Goal 决定当前优先级。**

> **同一套语义知识可以服务不同学习目标，但场景、句式和训练路径必须针对目标重新编排。**
