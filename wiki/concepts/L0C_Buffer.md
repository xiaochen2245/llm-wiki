---
title: "L0C Buffer"
type: concept
tags: [矩阵计算, 累加, CO1]
sources: ["raw/01-articles/通过L0C Buffer数据暂存实现高效的矩阵乘结果累加-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

L0C Buffer（也称为CO1）是一种用于矩阵乘结果累加的专用存储。通过Mmad接口的参数cmatrixInitVal和cmatrixSource配置C矩阵的初始值，实现矩阵乘结果的直接累加，无需中间GM搬运。

## 背景问题

### 传统累加方式
矩阵乘结果累加的传统方式（如A1*B1 + A2*B2）：
1. 第一次矩阵乘结果从CO1搬到workspace
2. 从workspace搬到UB
3. 第二次矩阵乘结果从CO1搬到workspace
4. 从workspace搬到UB
5. 在UB上进行累加计算

当需要累加n次时，增加n次CO1->workspace、n次workspace->UB搬运。

## 优化方法

### L0C Buffer方式
将前一次矩阵乘结果暂存在L0C上，直接调用Mmad实现累加：

```cpp
MmadParams mmadParams;
mmadParams.m = m;
mmadParams.n = n;
mmadParams.k = k;

// 第一次矩阵乘
Mmad(c1Local, a2Local, b2Local, mmadParams);

// 第二次矩阵乘累加第一次矩阵乘的结果
mmadParams.cmatrixInitVal = false;
Mmad(c1Local, a2Local, b2Local, c1Local, mmadParams);
```

### 参数说明
- **cmatrixInitVal**：是否使用初始值
- **cmatrixSource**：C矩阵的数据源

## 性能收益

### 数据流对比
| 方式 | 数据搬运路径 |
|------|------------|
| 传统累加 | CO1->workspace->UB（每次累加） |
| L0C Buffer | CO1直接累加，无需中间搬运 |

### 优势
- 减少CO1->workspace搬运
- 减少workspace->UB搬运
- 减少UB上的累加计算

## 关联连接
- [[矩阵计算]] — 矩阵计算相关
- [[Mmad]] — 矩阵乘累加指令
- [[AtomicAdd]] — 另一种累加方式
- [[摘要-通过L0C-Buffer数据暂存实现高效的矩阵乘结果累加]] — 原始文档
