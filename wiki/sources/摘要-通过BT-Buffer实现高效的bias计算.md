---
title: "摘要-通过BT-Buffer实现高效的bias计算"
type: source
tags: [CANN, 昇腾, 性能优化, 矩阵计算]
sources: [raw/01-articles/通过BT Buffer实现高效的bias计算-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
进行带bias的矩阵乘计算时，可将bias数据搬运至C2（Bias Table Buffer）上，调用一次Mmad接口实现矩阵乘加bias的计算。相比于传统方式（矩阵乘结果从L0C到GM，再到UB进行加bias运算），使用BT Buffer只需调用一次Mmad接口即可完成矩阵乘加bias，显著减少了数据搬运次数和中间计算步骤，提升了内存使用效率。

## 关联连接
- [[Ascend C]] — CANN算子开发API
- [[BT Buffer]] — Bias Table Buffer
- [[Mmad]] — 矩阵乘加接口
- [[矩阵计算]] — 矩阵计算优化策略
