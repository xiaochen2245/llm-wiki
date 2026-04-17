---
title: "摘要-通过L0C-Buffer数据暂存实现高效的矩阵乘结果累加"
type: source
tags: [CANN, 昇腾, 性能优化, 矩阵计算]
sources: [raw/01-articles/通过L0C Buffer数据暂存实现高效的矩阵乘结果累加-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
对矩阵乘结果进行累加（如A1*B1 + A2*B2）时，可将前一次矩阵乘的结果暂存在L0C上，通过Mmad接口的cmatrixInitVal参数配置C矩阵的初始值，直接在L0C上完成累加计算。相比于传统方式（矩阵乘结果从L0C到GM，再到UB进行累加），这种优化可减少n次CO1->workspace、workspace->UB的搬运以及n次Add运算，显著提升内存使用效率。

## 关联连接
- [[Ascend C]] — CANN算子开发API
- [[L0C Buffer]] — L0C缓冲区
- [[Mmad]] — 矩阵乘接口
- [[矩阵计算]] — 矩阵计算优化策略
