---
title: "Unified Buffer融合"
type: concept
tags: [UB, 内存优化, 性能优化]
sources: ["raw/01-articles/通过Unified Buffer融合实现连续vector计算-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

Unified Buffer融合（UB Buffer融合）是一种当算子涉及多次vector计算且前一次计算输出是后一次计算输入时，将前一次计算输出暂存在UB上直接作为下一次计算输入的优化技术。避免数据从UB搬回GM再从GM搬到UB的中间过程，减少GM搬入搬出次数。

## 问题场景

### 反例：频繁GM访问
算子需要进行Exp计算后再进行Abs计算：
1. GM -> UB：搬运源操作数
2. UB计算Exp
3. UB -> GM：搬出Exp结果
4. GM -> UB：搬运Exp结果作为Abs输入
5. UB计算Abs
6. UB -> GM：搬出最终结果

当vector计算为n次时，GM搬进搬出共需要2n次。

### 正例：UB Buffer融合
前一次结果可直接作为后一次计算的输入：
1. GM -> UB：搬运源操作数
2. UB计算Exp
3. UB计算Abs（复用同一UB buffer）
4. UB -> GM：搬出最终结果

共2次GM搬进搬出。

## 性能收益

### 搬运次数对比
| 场景 | GM搬运次数 |
|------|-----------|
| 未融合（n次计算） | 2n次 |
| 融合后 | 2次 |

### 适用场景
- 多次连续vector计算
- 前一次输出是后一次输入
- 计算结果无需中间存储

## 关联连接
- [[摘要-通过Unified-Buffer融合实现连续vector计算]] — 原始文档
- [[AI Core]] — 计算单元
- [[DoubleBuffer]] — 另一种并行优化
