---
title: "摘要-高效的使用搬运API"
type: source
tags: [CANN, 昇腾, 性能优化, 内存访问]
sources: [raw/01-articles/高效的使用搬运API-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
使用搬运API时应尽可能通过配置搬运控制参数DataCopyParams实现连续搬运或固定间隔搬运，避免使用for循环，二者效率差距极大。例如图片每行16KB需搬运前2KB，使用for循环每次只能搬运2KB重复16次，而配置DataCopyParams的blockCount、blockLen、srcStride、dstStride参数可一次搬完32KB。DataCopyParams中blockCount表示块数、blockLen表示每次搬运单位（DataBlock/32Byte）、srcStride表示源间隔、dstStride表示目的间隔。配合单次搬运16KB以上可充分发挥带宽性能。

## 关联连接
- [[AscendC算子优化建议总览表]] — 优化建议总览
- [[尽量一次搬运较大的数据块]] — 带宽利用率
- [[使能DoubleBuffer]] — 流水并行
