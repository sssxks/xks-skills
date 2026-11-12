---
name: naming
description: Use when introducing or reviewing naming consistency
---

修饰词必须承担区分作用，确实存在不同再加修饰。比如叫 Batch、Relay 就够了，不因为实现特点就叫 DenseBatch、ScopedRelay。

例如 `RuntimePhase`：若项目里没有另一种 `Phase`，就叫 `Phase`；其他叫 `state` 的状态不算区分对象。先搜索，不凭想象保留修饰词。

优先一个词，通常不超过两个词。

强烈建议惯用缩写。多词组合如果作为一个整体概念使用，各词不需要独立参与组合，就倾向缩写；需要保留各部分的组合关系时，再展开。

同一概念不换同义词。类型叫 Batch，变量就用 batch、inputBatch、pendingBatch；上下文清楚时也可以省略 batch，用 input、pending。但不要又用 group、chunk、bundle 指代同一个概念。

如果不一致性来源于多次迭代的历史残留，可以按照最新的来。

review 时可以写个临时工具 dump 类型和变量名字。召回项目的所有概念。