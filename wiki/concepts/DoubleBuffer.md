---
title: "DoubleBuffer"
type: concept
tags: [流水编排, 性能优化, 并行计算]
sources: ["raw/01-articles/使能DoubleBuffer-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

DoubleBuffer是一种基于MTE指令队列与Vector指令队列的独立性和可并行性的性能优化技术。它通过将数据搬运与Vector计算并行执行，隐藏大部分的数据搬运时间，降低Vector指令的等待时间，最终提高Vector单元的利用效率。

## 核心原理

### 指令队列并行性
AI Core上的指令队列主要包括：
- **V队列**：Vector指令队列
- **M队列**：Cube指令队列
- **S队列**：Scalar指令队列
- **MTE队列**：搬运指令队列（MTE1/MTE2/MTE3）

不同指令队列可并行执行，CopyIn、CopyOut过程和Compute过程可以并行。

### 未使能DoubleBuffer的问题
数据搬运与Vector计算串行执行，Vector利用率仅为1/3：
```
CopyIn -> Compute -> CopyOut -> CopyIn -> Compute -> CopyOut ...
```
假设三阶段耗时相同为t，等待时间过长，Vector利用率严重不足。

### 使能DoubleBuffer的优化
将数据一分为二（Tensor1、Tensor2），实现并行流水线：
```
Tensor1: CopyIn -> Compute -> CopyOut
Tensor2:      CopyIn -> Compute -> CopyOut
             (与Tensor1并行)
```

## 使用方法

```cpp
// InitBuffer中第二个参数设为2表示使能DoubleBuffer
pipe.InitBuffer(inQueueX, 2, 256);
```

### 前提条件
- 循环次数 >= 2
- 内存占用翻倍（2 * size）

## 适用场景

### 适用场景
- 数据搬运时间与计算时间相当
- 数据量较大，需要多次循环处理

### 不适用场景
- 数据搬运时间短，计算时间长（收益小）
- 数据量小，Vector可一次性完成（反而降低利用率）

## 关联连接
- [[摘要-使能doublebuffer]] — DoubleBuffer优化文档
- [[AI Core]] — 硬件执行单元
- [[Vector计算]] — Vector计算单元
- [[Unified_Buffer融合]] — 另一种UB优化技术
