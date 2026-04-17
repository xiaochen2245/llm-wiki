---
title: "摘要-算子与高阶API共享临时Buffer"
type: source
tags: [CANN, 昇腾, 性能优化, 内存管理]
sources: [raw/01-articles/算子与高阶API共享临时Buffer-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
当算子使用的高阶API（如SoftMax）需要传入临时Buffer时，该临时空间会挤占算子其他计算的空间，导致单次计算搬运的数据量变少、搬运次数变多。通过共享临时Buffer空间，可以提升单次搬运的数据量，减少搬运次数，从而提升内存使用效率。例如将原本分配给SoftMax的32KB临时空间与Add计算的32KB临时空间合并为64KB共享空间，可使数据搬运次数从16次减少到8次。

## 关联连接
- [[Ascend C]] — CANN算子开发API
- [[Unified Buffer]] — 统一缓冲区，用于临时数据存储
- [[SoftMax]] — 高阶API，需要临时Buffer支持
