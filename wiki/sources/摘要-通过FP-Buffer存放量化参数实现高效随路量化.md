---
title: "摘要-通过FP-Buffer存放量化参数实现高效随路量化"
type: source
tags: [CANN, 昇腾, 性能优化, 量化计算]
sources: [raw/01-articles/通过FP Buffer存放量化参数实现高效随路量化-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
对矩阵乘结果进行量化计算时，可将量化参数搬运到Fixpipe Buffer（C2PIPE2GM）上，调用一次Fixpipe接口实现矩阵乘结果的量化计算。相比于传统方式（矩阵乘结果从L0C到GM，再到UB进行量化计算），使用FP Buffer存放量化参数可减少CO1->workspace、workspace->UB的搬运过程和量化vector计算，仅需调用一次Fixpipe接口即可完成量化，提升内存使用效率。本优化手段仅针对Atlas A2训练系列产品和Atlas A2推理系列产品生效。

## 关联连接
- [[Ascend C]] — CANN算子开发API
- [[FP Buffer]] — Fixpipe Buffer
- [[Fixpipe]] — 固定管道接口
- [[随路量化]] — 量化计算优化策略
