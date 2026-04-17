---
title: "摘要-L2-Cache切分"
type: source
tags: [CANN, 昇腾, 性能优化, 内存访问]
sources: [raw/01-articles/L2 Cache切分-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
AI处理器的L2 Cache读写带宽（约7TB/s）远高于GM带宽（约1.6TB/s），数据命中L2 Cache时读写效率大幅提升。当输入输出数据量超过L2 Cache大小时，Tiling中应使能L2 Cache切分策略，将数据均等切分，使每次计算的数据量小于L2 Cache大小。例如数据量为L2 Cache大小2倍时，分两次计算可确保每次计算的数据都能命中L2 Cache，避免访问GM带来的性能瓶颈。

## 关联连接
- [[Ascend C]] — CANN算子开发API
- [[L2 Cache]] — 二级缓存
- [[Tiling]] — 数据切分策略
- [[内存访问]] — 内存访问优化策略
