---
title: "摘要-Matmul使能AtomicAdd选项"
type: source
tags: [CANN, 昇腾, 性能优化, 矩阵计算]
sources: [raw/01-articles/Matmul使能AtomicAdd选项-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
对于Matmul得到的结果矩阵C(m,n)，若需要与GM上的矩阵D(m,n)进行Add操作，可在GetTensorC或IterateAll接口的GM通路上将enAtomic参数设为1，开启AtomicAdd累加操作。矩阵C的结果在搬出到GM时将直接累加到矩阵D的GM地址上，实现与矩阵D的Add操作。这种方式避免了额外UB内存分配和多次数据搬运（矩阵C和D分别从GM搬到UB、结果从UB搬出到GM），在M=64、N=256、K=256场景下可获得约12.4%的性能提升。

## 关联连接
- [[Ascend C]] — CANN算子开发API
- [[Matmul]] — 矩阵乘高阶API
- [[AtomicAdd]] — 原子累加选项
- [[矩阵计算]] — 矩阵计算优化策略
