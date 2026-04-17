---
title: "摘要-纯搬运类算子VECIN和VECOUT建议复用"
type: source
tags: [CANN, 昇腾, 性能优化, 内存访问]
sources: [raw/01-articles/纯搬运类算子VECIN和VECOUT建议复用-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
纯搬运类算子在执行时不涉及实际vector计算，若存在冗余的vector指令会导致算子整体执行时间变长。Ascend C提供了TQueBind接口，可以将VECIN与VECOUT绑定，省略将数据从VECIN拷贝到VECOUT的步骤，避免vector的无谓消耗。使用TQueBind替换原来的QueI和QueO后，aiv_vec_time几乎缩减为0，显著优化了纯搬运类算子的性能。

## 关联连接
- [[Ascend C]] — CANN算子开发API
- [[TQueBind]] — VECIN与VECOUT绑定接口
- [[Unified Buffer]] — 统一缓冲区
