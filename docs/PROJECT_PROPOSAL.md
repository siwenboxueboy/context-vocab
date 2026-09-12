# 项目立项说明

## 项目名称

**Context Vocab**（暂定）

## 一句话定义

通过 **语义簇、语义边界和 AI 场景训练**，帮助英语学习者把“认识的单词”转化为“能够在真实语境中精准调用的表达”。

## 核心问题

中国英语学习者普遍存在：

```text
阅读词汇量较大
≠
主动表达词汇量大
≠
选词精准
```

传统背词产品更擅长解决 recognition，而非 word choice。

用户可能认识：

```text
annoyed / irritated / angry / furious / outraged / resentful
```

真正表达时却统一退化成：

```text
angry
very angry
```

本项目希望解决的是：

> 如何建立这些表达之间的精准语义边界，并让学习者在真实场景中自然调用。

## 核心方案

```text
语义知识图谱
→ 动态语义簇
→ 语义边界
→ AI 场景训练
→ 用户表达
→ AI 诊断
→ Learner Model
→ 自适应再训练
→ 收敛
```

## 核心差异

传统背词：

```text
Word → Meaning → Review
```

本项目：

```text
Intent → Cluster → Boundary → Scenario → Choice → Expression
```

核心口号：

> **不是背出词义，而是练出选词能力。**

## 首阶段验证问题

1. AI 能否稳定生成高质量语义簇？
2. AI 能否按照统一 rubric 对语义簇评分？
3. AI 能否发现并解释有价值的 Semantic Boundary？
4. AI 能否生成真正具有区分度的场景？
5. AI 能否判断用户答案不仅“对不对”，还“自然不自然”？
6. 系统能否识别用户具体混淆了哪条语义边界？
7. 经过多场景训练后，用户能否形成跨场景迁移能力？

## 第一阶段范围

```text
语义簇定义
语义簇评定指标
AI 语义簇评分
Semantic Boundary 数据结构
语义簇展示
AI 单词语境解释
AI 混淆场景考察
Learner Boundary State
收敛规则
```

## 产品目标

用户完成学习后应该达到：

```text
认识词
→ 分清词
→ 场景选得准
→ 不给选项也能想起来
→ 真实表达自然调用
```

最终目标：

> **Learn words by context. Choose words like a native.**
