---
title: "摘要-ascendc算子优化建议总览表"
type: source
tags: [CANN, 昇腾, 性能优化]
sources: [raw/01-articles/优化建议总览表-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
AscendC算子性能优化建议分为六大分类。Tiling策略提供切分方案优化建议；头尾开销优化涵盖核数设置、TilingData结构精简、TPipe优化、Workspace冗余删除等；流水编排通过DoubleBuffer和异步接口实现任务并行化；内存访问优化关注数据块大小、GM地址对齐、搬运API效率、同地址访问规避、L2 CacheMode设置；矢量计算优化包括Unified Buffer融合、Counter模式、低延迟指令；矩阵计算优化涉及BT Buffer、FP Buffer、L0C Buffer、Matmul AtomicAdd等机制。

## 关联连接
- [[Ascend C API列表]] — API参考文档
- [[核间负载均衡]] — Tiling策略具体建议
- [[头尾开销优化]] — 核数与Kernel类型设置
