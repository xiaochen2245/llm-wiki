---
title: "摘要-ascend-c-api列表"
type: source
tags: [CANN, 昇腾, API参考]
sources: [raw/01-articles/Ascend C API列表-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
Ascend C提供类库API供开发者使用标准C++语法进行编程，主要分为六个层次：基础数据结构（LocalTensor、GlobalTensor、Coordinate、Layout、TensorTrait）、语言扩展层C API、基础API（硬件能力抽象，含ISASI类API）、高阶API（数学库、Matmul、Softmax等）、SIMT API（单指令多线程向量化计算）、Utils API（公共辅助函数）。基础API中的Memory数据搬运API包括DataCopy和Copy，矢量计算API涵盖基础算术、逻辑计算、复合计算、比较与选择、精度转换、归约计算、数据转换、数据填充、数据分散收集、掩码操作等类别。

## 关联连接
- [[AscendC算子优化建议总览]] — 优化建议分类框架
- [[核间负载均衡]] — Tiling策略优化
- [[使能DoubleBuffer]] — 流水编排优化
