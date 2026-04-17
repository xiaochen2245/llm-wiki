---
title: "摘要-使能DoubleBuffer"
type: source
tags: [CANN, 昇腾, 性能优化, 流水编排]
sources: [raw/01-articles/使能DoubleBuffer-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
AI Core指令队列包括Vector指令队列(V)、Cube指令队列(M)、Scalar指令队列(S)和搬运指令队列(MTE1/MTE2/MTE3)，不同指令队列相互独立可并行执行。未使能DoubleBuffer时，CopyIn、Compute、CopyOut三阶段串行执行，Vector利用率仅1/3。使能DoubleBuffer将数据一分为二（如Tensor1、Tensor2），当Vector单元计算Tensor1时，Tensor2执行CopyIn；当Vector切换计算Tensor2时，Tensor1执行CopyOut，实现数据搬运与Vector计算并行。通过InitBuffer设置内存块个数为2使能DoubleBuffer。需要注意：当数据搬运时间较短或原始数据较小可一次性完成计算时，DoubleBuffer可能无法带来明显性能提升甚至适得其反。

## 关联连接
- [[AscendC算子优化建议总览表]] — 优化建议总览
- [[尽量一次搬运较大的数据块]] — 内存访问优化
- [[高效的使用搬运API]] — 搬运效率优化
