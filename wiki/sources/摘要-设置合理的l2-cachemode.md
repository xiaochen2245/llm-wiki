---
title: "摘要-设置合理的L2 CacheMode"
type: source
tags: [CANN, 昇腾, 性能优化, 内存访问]
sources: [raw/01-articles/设置合理的L2 CacheMode-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
L2 Cache用于缓存频繁访问的数据，其带宽相比GM有数倍提升，L2 Cache命中率越高算子性能越好。数据访问总量超出L2 Cache容量时会发生替换，旧数据先写回GM再加载新数据占用带宽。通过GlobalTensor的SetL2CacheHint接口可设置CacheMode：CACHE_MODE_NORMAL表示数据需要经过L2 Cache（默认）；CACHE_MODE_DISABLE表示数据不进入L2 Cache。对于只访问一次的数据设置CACHE_MODE_DISABLE避免占用Cache，对于需要重复读取的数据设置CACHE_MODE_NORMAL保证缓存。性能对比可通过msprof工具获取aiv_gm_to_ub_bw和aiv_main_mem_write_bw写带宽速率。

## 关联连接
- [[AscendC算子优化建议总览表]] — 优化建议总览
- [[避免同地址访问]] — 内存访问优化
- [[尽量一次搬运较大的数据块]] — 带宽优化
