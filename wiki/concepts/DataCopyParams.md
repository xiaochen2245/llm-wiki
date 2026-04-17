---
title: "DataCopyParams"
type: concept
tags: [数据搬运, API参数, 性能优化]
sources: ["raw/01-articles/高效的使用搬运API-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

DataCopyParams是数据搬运API的参数配置结构，用于实现连续搬运或固定间隔搬运。通过配置blockCount、blockLen、srcStride、dstStride等参数，可以一次完成跨距数据搬运，避免使用for循环多次搬运。

## 参数说明

### 核心参数
| 参数 | 说明 | 单位 |
|------|------|------|
| blockCount | 搬运的块数 | DataBlock(32Byte) |
| blockLen | 每块搬运的长度 | DataBlock(32Byte) |
| srcStride | 源地址跨距（两次搬运src之间的间隔） | DataBlock |
| dstStride | 目标地址跨距（两次搬运dst之间的间隔） | DataBlock |

### 搬运模式
| 模式 | 配置 | 说明 |
|------|------|------|
| 连续搬运 | srcStride=0, dstStride=0 | 数据连续存放 |
| 固定间隔搬运 | srcStride>0 或 dstStride>0 | 间隔固定搬运 |

## 性能对比

### 场景示例
图片每一行为16KB，需要从每一行中搬运前2KB，共16行：

**for循环方式**：
```cpp
for (int i = 0; i < imgHeight; i++) {
    DataCopy(tensorIn[i * copyWidth], tensorGM[i * imgWidth], copyWidth);
}
// 每次搬运2KB，重复16次
```

**DataCopyParams方式**：
```cpp
DataCopyParams copyParams;
copyParams.blockCount = imgHeight;      // 16块
copyParams.blockLen = copyWidth / 8;   // 每块2KB
copyParams.srcStride = (imgWidth - copyWidth) / 8;  // 间隔14KB
copyParams.dstStride = 0;              // 连续写
DataCopy(tensorGM, tensorIn, copyParams);
// 一次搬完32KB
```

### 效率差异
- for循环：每次2KB，重复16次
- DataCopyParams：一次32KB

## 优化建议

尽量一次搬运较大的数据块，利用内存访问带宽特性提升搬运效率。

## 关联连接
- [[数据搬运]] — 数据搬运相关
- [[AI Core]] — 硬件单元
- [[摘要-高效的使用搬运api]] — 原始文档
