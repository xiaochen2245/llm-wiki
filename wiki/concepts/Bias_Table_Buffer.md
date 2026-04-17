---
title: "Bias Table Buffer"
type: concept
tags: [矩阵计算, Bias, Buffer]
sources: ["raw/01-articles/通过BT Buffer实现高效的bias计算-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

Bias Table Buffer（简称BT Buffer或C2）是一种用于存放bias数据的专用Buffer。在带bias的矩阵乘计算中，可将bias数据搬运至BT Buffer，调用一次Mmad接口实现矩阵乘加bias的计算，无需中间GM搬运。

## 背景问题

### 传统方式
矩阵乘带bias计算的传统方式：
1. 矩阵乘结果从CO1(L0C)搬运到workspace(GM)
2. 从workspace搬运到UB
3. 在UB上进行加bias运算
4. 最后将结果搬运到GM

当循环n次时，分别增加了n次CO1->workspace、workspace->UB的搬运。

## 优化方法

### BT Buffer方式
将bias数据搬运至BT Buffer：
1. 将bias从GM搬运到L1
2. 将bias从L1搬运到BT（C2）
3. 调用Mmad接口实现矩阵乘加bias

```cpp
// 将bias从L1搬运到BT
DataCopy(bias2Local, bias1Local, { 1, (uint16_t)(n * sizeof(float) / 64), 0, 0 });

// 调用Mmad接口实现矩阵乘加bias
Mmad(c1Local, a2Local, b2Local, bias2Local, mmadParams);
```

## 性能收益

### 数据流对比
| 方式 | 数据搬运路径 |
|------|------------|
| 传统方式 | CO1->workspace->UB->GM |
| BT Buffer | CO1直接加bias->GM |

### 优势
- 减少CO1->workspace搬运
- 减少workspace->UB搬运
- UB上无需额外加bias计算

## 关联连接
- [[Bias]] — Bias相关概念
- [[Mmad]] — 矩阵乘累加指令
- [[L0C_Buffer]] — CO1存储
- [[摘要-通过BT-Buffer实现高效的bias计算]] — 原始文档
