---
title: "AI Core"
type: entity
tags: [昇腾, 计算单元, 硬件架构]
sources: ["raw/01-articles/Ascend C API列表-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

AI Core是昇腾处理器的核心计算单元，采用达芬奇架构，包含多个异构计算单元和存储单元。每个AI Core可以独立执行向量运算、矩阵运算和标量运算，通过指令队列实现并行流水线。

## 关键信息

### 计算单元
- **Vector计算单元（V）**：负责矢量运算，如Add、Exp、Abs等逐元素操作
- **Cube计算单元（M）**：负责矩阵运算，如矩阵乘（Matmul/Mmad）
- **Scalar计算单元（S）**：负责标量运算和控制流
- **搬运单元（MTE1/MTE2/MTE3）**：负责GM与本地存储之间的数据搬运

### 存储单元
| 存储类型 | 容量 | 带宽 | 用途 |
|---------|------|------|------|
| GM (Global Memory) | 大 | 1.6TB/s | 核间共享，存放输入输出数据 |
| L2 Cache | 192MB | 7TB/s | 核间共享，热点数据缓存 |
| L1 Cache | - | - | 核内共享，bias数据等 |
| UB (Unified Buffer) | - | - | 本地计算缓冲 |
| L0C (CO1) | - | - | 矩阵乘结果累加存储 |

### 指令队列
不同指令队列间的独立性和可并行执行特性是性能优化的基础：
- **V队列**：Vector指令队列
- **M队列**：Cube指令队列
- **S队列**：Scalar指令队列
- **MTE队列**：搬运指令队列

### 数据流
```
GM -> MTE2 -> L1/UB -> Vector/Cube计算 -> L1/UB -> MTE3 -> GM
```

## 关联连接
- [[昇腾]] — 硬件载体
- [[CANN]] — 软件架构
- [[Ascend C]] — 编程接口
- [[Vector计算]] — Vector计算单元
- [[Cube计算]] — Cube计算单元
