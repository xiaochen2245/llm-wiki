---
title: "摘要-避免TPipe在对象内创建和初始化"
type: source
tags: [CANN, 昇腾, 性能优化, 编译器优化]
sources: [raw/01-articles/避免TPipe在对象内创建和初始化-CANN社区版9.0.0-beta.2-昇腾社区.md]
last_updated: 2026-04-17
---

## 核心摘要
TPipe是管理全局内存和同步的框架，用于为TQue/TBuf进行内存分配。当TPipe对象在KernelExample类内部定义并初始化时，其内存空间包含在KernelExample对象内，TPipe初始化会设置全局变量的TPipe指针，导致KernelExample对象内存被外部污染，编译器将采取保守策略不对Scalar变量进行常量折叠和常量传播。建议将TPipe对象创建于KernelExample类外部，使内存空间独立，触发编译器对KernelExample类内Scalar的编译优化。性能对比显示，Scalar优化效果显著：平均时间从281us降至236us下降17%，Scalar时延占比从21%降至17%。

## 关联连接
- [[AscendC算子优化建议总览表]] — 优化建议总览
- [[核函数内删除Workspace相关冗余操作]] — 头开销优化
- [[设置合适的核数和算子Kernel类型]] — 核数设置
