---
title: "Fixpipe Buffer"
type: concept
tags: [矩阵计算, 量化, Buffer]
sources: ["raw/01-articles/通过FP Buffer存放量化参数实现高效随路量化-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

Fixpipe Buffer（简称FP Buffer或C2PIPE2GM）是一种用于存放量化参数的专用Buffer。通过将量化参数搬运到FP Buffer，调用一次Fixpipe接口实现矩阵乘结果的量化计算，实现随路量化，减少数据搬运次数。

## 背景问题

### 传统量化方式
矩阵乘结果量化计算的传统方式：
1. 矩阵乘结果从CO1搬运到workspace
2. 从workspace搬运到UB
3. 量化参数搬运到UB
4. 在UB上进行量化计算
5. 结果从UB搬到GM

多增加了CO1->workspace、workspace->UB的搬运过程和量化vector计算。

## 优化方法

### FP Buffer方式
将量化参数搬运到FP Buffer上：
1. 量化参数从GM搬运到L1
2. 量化参数从L1搬运到FP Buffer（C2PIPE2GM）
3. 调用Fixpipe接口实现矩阵乘结果的量化计算

```cpp
// 将量化参数从L1->FP Buffer
DataCopy(deqLocal, deq1Local, { 1, (uint16_t)(QuantSize * sizeof(uint64_t) / 128), 0, 0 });

// 设置量化参数
SetFixpipeNz2ndFlag(1, 0, 0);
DataCopyCO12DstParams dataCopyParams;
dataCopyParams.quantPre = QuantMode_t::VQF322B8_PRE;
dataCopyParams.nz2ndEn = true;

// 矩阵乘结果量化后直接搬出
DataCopy(cGM, c1Local, DataCopyCO12DstParams);
```

## 性能收益

### 随路量化
矩阵乘结果在搬出过程中直接进行量化，无需中间存储和额外搬运。

### 数据流对比
| 方式 | 量化计算位置 |
|------|------------|
| 传统方式 | UB上，需要额外计算 |
| FP Buffer | 搬出过程中，随路完成 |

## 关联连接
- [[量化]] — 量化相关概念
- [[Fixpipe]] — 搬出指令
- [[L0C_Buffer]] — CO1存储
- [[摘要-通过FP-Buffer存放量化参数实现高效随路量化]] — 原始文档
