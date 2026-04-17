---
title: "AtomicAdd"
type: concept
tags: [矩阵计算, Matmul, 性能优化]
sources: ["raw/01-articles/Matmul使能AtomicAdd选项-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

AtomicAdd是一种Matmul选项，用于将矩阵乘结果矩阵C累加到GM上的矩阵D的指定地址。通过在GetTensorC接口或IterateAll接口的GM通路上设置enAtomic参数为1，实现搬出到GM时直接累加，避免额外的UB分配和数据搬运。

## 背景问题

### 传统方式的问题
Matmul结果需要与GM上的矩阵D进行Add操作时：
1. Matmul结果矩阵C从GM搬到UB
2. 矩阵D从GM搬到UB
3. UB上进行Add操作
4. 结果从UB搬出到GM

至少需要额外分配一块UB内存，三次额外搬运操作。

## 优化方法

### 使能AtomicAdd
```cpp
// IterateAll接口中的enAtomic设为1
mm.IterateAll(gm_d, 1);

// 或GetTensorC接口
mm.GetTensorC(gm_d, 1);
```

### 数据流
搬出到GM时，矩阵C直接累加到矩阵D的GM地址上。

## 性能收益

### 性能对比示例
- 矩阵维度：M=64, N=256, K=256
- 平均cycle数从154181变为135054
- **性能优化12.4%**

## 适用场景

### 适用场景
- Matmul结果需要与GM现有数据累加
- 避免中间UB分配
- 减少额外搬运操作

### 不适用场景
- Matmul结果直接输出（无需累加）
- 已经使用其他融合优化

## 关联连接
- [[Matmul]] — 矩阵乘接口
- [[摘要-Matmul使能AtomicAdd选项]] — 原始文档
- [[L0C_Buffer]] — 矩阵乘结果存储
