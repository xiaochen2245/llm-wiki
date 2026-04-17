---
title: "TQueBind"
type: concept
tags: [数据搬运, 性能优化, 队列绑定]
sources: ["raw/01-articles/纯搬运类算子VECIN和VECOUT建议复用-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

TQueBind是Ascend C提供的VECIN与VECOUT绑定接口，用于纯搬运类算子。通过TQueBind替代分离的QueI和QueO，可以省略VECIN到VECOUT的LocalTensor拷贝步骤，避免vector的无谓消耗。

## 背景问题

### 纯搬运类算子的冗余
纯搬运类算子在执行时并不涉及实际vector计算，但如果存在冗余的vector指令，会导致算子整体执行时间变长。

### 传统方式的问题
为了保证数据搬入和搬出之间的流水同步，存在LocalTensor -> LocalTensor的DataCopy指令：
```cpp
auto iLocal = QueI.DeQue<ComputeT>();
auto oLocal = QueO.AllocTensor<ComputeT>();
DataCopy(oLocal, iLocal, size); // LocalTensor -> LocalTensor
```

## 优化方法

### TQueBind方式
```cpp
// 使用TQueBind替换原来的QueI，QueO
TQueBind<TPosition::VECIN, TPosition::VECOUT, BUFFER_NUM> queBind;

auto bindLocal = queBind.AllocTensor<ComputeT>();
DataCopy(bindLocal, inGm[i * 32], size);
queBind.EnQue(bindLocal);
bindLocal = queBind.DeQue<ComputeT>();
DataCopyPad(outGm[j], bindLocal, ...);
queBind.FreeTensor(bindLocal);
```

### 优化效果
省略了数据从VECIN拷贝到VECOUT的步骤，避免冗余拷贝。

## 性能收益

### aiv_vec_time优化
将反例中DataCopy指令替换为TQueBind之后：
- aiv_vec_time几乎缩减为0
- 显著减少Vector执行时间

## 适用场景

### 适用场景
- 纯搬运类算子
- 无实际vector计算
- 仅有VECIN到VECOUT的数据搬运

### 不适用场景
- 需要实际vector计算的算子
- VECIN和VECOUT数据格式不同

## 关联连接
- [[TQue]] — 队列类
- [[VECIN]] — 输入向量队列
- [[VECOUT]] — 输出向量队列
- [[摘要-纯搬运类算子VECIN和VECOUT建议复用]] — 原始文档
