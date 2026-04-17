---
title: "摘要-核函数内删除Workspace相关冗余操作"
type: source
tags: [CANN, 昇腾, 性能优化, 头尾开销]
sources: [raw/01-articles/核函数内删除Workspace相关冗余操作-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
在Ascend C算子工程中，核函数传入的workspace参数已直接赋值为用户Workspace，因此无需通过SetSysWorkspace和GetUserWorkspace来设置和获取Workspace。减少这些冗余判断后，编译器可以在不使用该参数的情况下进一步优化未用到的workspace变量。例如在fast_gelu函数中，workspace参数等价于用户workspace且不为空，但原代码仍进行空指针判断并调用SetSysWorkspace和GetUserWorkspace。删除这些冗余操作可以简化代码并提升编译器优化效果。

## 关联连接
- [[AscendC算子优化建议总览表]] — 优化建议总览
- [[避免TPipe在对象内创建和初始化]] — 编译器优化
- [[设置合适的核数和算子Kernel类型]] — 头开销优化
