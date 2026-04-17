---
title: "Ascend C"
type: entity
tags: [算子开发, API, C/C++]
sources: ["raw/01-articles/Ascend C API列表-CANN社区版9.0.0-beta.2-昇腾社区.md", "raw/01-articles/优化建议总览表-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

Ascend C是CANN架构下的C/C++算子开发API，专门用于在昇腾AI Core上编写高性能自定义算子。它提供了完整的编程模型，包括内存管理、指令调度、数据搬运等接口，使开发者能够直接控制硬件资源实现极致性能。

## 关键信息

### 核心API类
- **TPipe**：管理全局内存和同步的框架，用于TQue/TBuf内存分配
- **TQue**：模板队列类，管理输入输出数据缓存
- **TBuf**：模板缓冲类，管理临时计算数据
- **LocalTensor**：本地张量，表示UB或L1上的数据
- **GlobalTensor**：全局张量，表示GM上的数据

### 关键接口
- **DataCopy**：数据搬运API，用于GM与UB之间数据传递
- **Mmad**：矩阵乘累加指令
- **Fixpipe**：L0C到GM的数据搬出指令
- **SetFixpipeNz2ndFlag**：设置量化参数

### 存储层次对应
| 存储层级 | API类型 | 说明 |
|---------|--------|------|
| GM | GlobalTensor | 全局内存，容量大速度慢 |
| L2 Cache | 自动缓存 | 192MB，7TB/s带宽 |
| L1 Cache | TQue/C2 | 核间共享 |
| UB | LocalTensor | Unified Buffer，本地计算缓冲 |
| L0C | CO1 | 矩阵乘结果存储 |

### 优化技术关联
- [[DoubleBuffer]] — 基于MTE与Vector队列并行性
- [[Unified_Buffer融合]] — 减少GM搬入搬出
- [[Counter模式]] — Vector算子的mask模式优化
- [[TQueBind]] — VECIN与VECOUT绑定

## 关联连接
- [[CANN]] — 上层计算架构
- [[AI Core]] — 运行的硬件目标
- [[TPipe]] — 内存管理框架
- [[TQue]] — 队列管理
- [[LocalTensor]] — 本地张量
- [[GlobalTensor]] — 全局张量
- [[Matmul]] — 矩阵乘接口
- [[Vector计算]] — 矢量计算
- [[摘要-ascend-c-api列表]] — API完整列表
