---
title: "L2 Cache切分"
type: concept
tags: [L2 Cache, 性能优化, 内存优化]
sources: ["raw/01-articles/L2 Cache切分-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

L2 Cache切分是一种当输入输出数据的数据量超过L2 Cache大小时，使能切分策略以提升L2 Cache命中率的性能优化技术。L2 Cache读写混合带宽约7TB/s，而GM带宽约1.6TB/s，命中L2 Cache可显著提升数据访问效率。

## 背景信息

### 存储层次带宽对比
| 存储类型 | 带宽 |
|---------|------|
| L2 Cache | ~7TB/s |
| GM (Global Memory) | ~1.6TB/s |

### 问题场景
- 输入数据大小 = L2CacheSize * 2
- 总核数20，每个核至少需要2次读取输入数据
- 未切分时，每次计算都无法命中L2 Cache

## 切分策略

### 核心原则
1. **数据量控制**：每次计算量不超过L2 Cache大小
2. **分次计算**：将大数据分为多次小计算
3. **缓存复用**：每次计算前读取的数据能够命中L2 Cache

### 示例
假设：
- 输入数据大小 = L2CacheSize * 2
- 总核数 = 20

**未使能L2 Cache切分**：
- 整体一次完成计算
- 每个核需要多次读取GM数据

**使能L2 Cache切分**：
- 输入数据均等切分成2份
- 每次计算量为L2CacheSize
- 每次计算前读取的数据能够命中L2 Cache

## 关联连接
- [[Tiling策略]] — 上层Tiling策略
- [[L2_Cache]] — L2 Cache相关
- [[摘要-L2-Cache切分]] — 原始文档
