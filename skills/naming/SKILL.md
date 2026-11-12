---
name: naming
description: Use when introducing or reviewing naming consistency
---

修饰词必须承担区分作用，确实存在不同再加修饰。比如叫 Batch、Relay 就够了，不因为实现特点就叫 DenseBatch、ScopedRelay。

判断修饰词是否必要，要在项目里找出实际存在的同名核心概念，不能用假想的冲突替它辩护。例如审查 `RuntimePhase` 时，不能因为项目还有构建、任务等其他“状态”，就说 `Runtime` 有区分作用；应搜索是否实际存在另一种 `Phase`。如果所有 `Phase` / `phase` 都指向 `RuntimePhase`，而构建、任务用的是 `state`，就应建议简化为 `Phase`。类型名与存储字段分别按各自上下文判断，不必因此机械地把数据库的 `runtime_phase` 也改成 `phase`。

优先一个词，通常不超过两个词。

强烈建议惯用缩写。多词组合如果作为一个整体概念使用，各词不需要独立参与组合，就倾向缩写；需要保留各部分的组合关系时，再展开。

同一概念不换同义词。类型叫 Batch，变量就用 batch、inputBatch、pendingBatch；上下文清楚时也可以省略 batch，用 input、pending。但不要又用 group、chunk、bundle 指代同一个概念。

如果不一致性来源于多次迭代的历史残留，可以按照最新的来。

review 时可以写个临时工具 dump 类型和变量名字。召回项目的所有概念。