---
title: "摘要-限制TilingData结构大小"
type: source
tags: [CANN, 昇腾, 性能优化, 头尾开销]
sources: [raw/01-articles/限制TilingData结构大小-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
TilingData结构是Tiling切分信息的载体，Host侧计算完Tiling后通过入参传递给Device侧存储于GM。调用GET_TILING_DATA宏时会将Tiling信息从GM拷贝到AI处理器的栈空间，由于GM访问效率较低且栈空间有限，需限制TilingData结构大小。优化策略包括：减少不必要的TilingData结构变量、根据数据范围选择合适变量类型、合理排布结构体字段（AI处理器访存需8字节对齐）、TilingData整体结构要求8字节补齐。例如NumBlocks信息可通过SetBlockDim接口设置后调用GetBlockNum获取，无需通过TilingData传递；formerNum和tailNum使用uint8_t类型即可；合理调整字段排布可减少补充字节数。

## 关联连接
- [[AscendC算子优化建议总览表]] — 优化建议总览
- [[设置合适的核数和算子Kernel类型]] — 头开销优化
- [[核间负载均衡]] — Tiling切分策略
