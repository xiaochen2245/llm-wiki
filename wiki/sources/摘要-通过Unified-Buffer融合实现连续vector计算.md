---
title: "摘要-通过Unified-Buffer融合实现连续vector计算"
type: source
tags: [CANN, 昇腾, 性能优化, 矢量计算]
sources: [raw/01-articles/通过Unified Buffer融合实现连续vector计算-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
当算子涉及多次vector计算且前一次计算输出是后一次计算输入时，可以将前一次计算输出暂存在UB（Unified Buffer）上直接作为下一次计算的输入，无需将结果从UB搬运到GM后再从GM搬运到UB。这种UB Buffer融合方式可减少GM搬入搬出次数。对于n次vector计算，传统方式需要2n次GM搬进搬出，而融合后只需2次（开始时源操作数搬入UB，计算结束后结果搬出GM）。

## 关联连接
- [[Ascend C]] — CANN算子开发API
- [[Unified Buffer]] — 统一缓冲区
- [[矢量计算]] — Vector计算优化策略
