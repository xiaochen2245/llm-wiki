---
title: "CANN"
type: entity
tags: [昇腾, 计算架构, AI框架]
sources: ["raw/01-articles/Ascend C API列表-CANN社区版9.0.0-beta.2-昇腾社区.md", "raw/01-articles/优化建议总览表-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

CANN（Compute Architecture for Neural Networks）是华为为AI场景优化的异构计算架构，作为昇腾AI处理器的基础软件层，提供完整的算子开发工具链和运行时支持。CANN是连接硬件与AI框架的桥梁，支持TensorFlow、PyTorch、MindSpore等主流AI框架。

## 关键信息

### 架构层次
- **应用层**：支持多种AI框架（TensorFlow、PyTorch、MindSpore）
- **运行时层**：提供算子调度、内存管理、设备管理
- **框架层**：CCE编译器、图优化、算子融合
- **芯片层**：适配昇腾AI处理器系列

### 核心组件
- **Ascend C**：C/C++算子开发API，支持在AI Core上编写高性能算子
- **CCE编译器**：自定义算子编译工具
- **Profiling工具**：性能分析优化工具

### 优化分类
根据CAN N昇腾社区的性能优化建议总览表，优化建议分为以下几类：
1. **Tiling策略**：核间负载均衡
2. **头尾开销优化**：核数设置、TilingData精简、TPipe优化、Workspace删除
3. **流水编排**：DoubleBuffer机制
4. **内存访问**：大块数据传输、搬运API优化、同地址规避、L2 CacheMode设置
5. **矢量计算**：UB Buffer融合、Counter模式、归约操作
6. **矩阵计算**：BT Buffer、FP Buffer、L0C Buffer、AtomicAdd

## 关联连接
- [[昇腾]] — 华为AI处理器品牌，CANN的硬件载体
- [[Ascend C]] — CANN下的C/C++算子开发API
- [[AI Core]] — 昇腾处理器的核心计算单元
- [[摘要-ascend-c-api列表]] — Ascend C API完整列表
- [[摘要-ascendc算子优化建议总览表]] — 性能优化建议总览表
