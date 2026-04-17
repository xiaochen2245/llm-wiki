---
title: "避免同地址访问-CANN社区版9.0.0-beta.2-昇腾社区"
source: "https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/atlas_ascendc_best_practices_10_00013.html"
author:
published:
created: 2026-04-17
description: "避免同地址访问"
tags:
  - "原始网页"
---
## 避免同地址访问

## 避免同地址访问

【优先级】高

说明

该性能优化指导适用于如下产品型号：

- Atlas A3 训练系列产品 / Atlas A3 推理系列产品
- Atlas A2 训练系列产品 / Atlas A2 推理系列产品

【描述】MTE2、MTE3、Scalar等单元访问Global Memory数据时，其地址请求会按照512字节粒度对齐后进行处理。当同时访问Global Memory的数据，且地址处于连续的512字节范围内时，由于数据一致性的原因，多个请求会被串行处理，进而影响数据搬运效率。

当前算子执行机制保证用户Kernel入参（包括Workspace/Tiling）的地址512字节对齐，因此开发者只需要根据地址的偏移量即可判断两个地址是否会落入连续的512字节范围内。

如下图所示，AI Core内的各个核对Global Memory的数据同时发出读写请求，尽管addr0~addr5是多个不同的地址，但因为落在连续的512字节范围内，被视为同一个地址请求，此时这几个数据请求会被串行处理，数据访问效率会降低。同地址访问的影响受同时访问的核数影响，同地址访问的核数越多时，串行导致的性能劣化越严重。

![](https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002533170070.png)

避免同地址访问的方法主要有以下两种： **调整数据访问顺序** 和 **修改切分策略** 。下文介绍配套的样例请参考 [避免同地址访问样例](https://gitcode.com/cann/asc-devkit/blob/9.0.0-beta.2/examples/01_simd_cpp_api/04_best_practices/04_memory_access_practices/mata_address_conflict) 。

**调整数据访问顺序**

以一个形状为 (8192, 128) 的float类型输入进行Adds计算为例。

为了体现同地址冲突的影响，上述场景设计中每一行的数据大小为512字节（128个float），每个核每一轮计算处理512 \* 8字节的数据，并进行全核同步（实际场景中并不需要），每一轮计算都需要等待所有核完成当前数据块的计算后，再进行下一轮。

<table><colgroup><col> <col> <col></colgroup><thead><tr><th colspan="1"><p>实现方案</p></th><th colspan="1"><p>原始实现</p></th><th colspan="1"><p>优化实现</p></th></tr></thead><tbody><tr><td><p>实现方法</p></td><td><p>使用16个核参与计算，按列方向进行切分，每个核总计算数据量为8192 * 8；单核执行循环16次，每次计算的数据量为512 * 8；每个核的循环顺序如下图所示，列方向0~15表示每个核的数据块执行顺序。</p><p>由于多个核同时访问同一行数据（512字节），导致同地址冲突的发生。</p></td><td><p>使用16个核参与计算，按列方向进行切分，每个核总计算数据量为8192 * 8；单核执行循环16次，每次计算的数据量为512 * 8；每个核的循环顺序如下图所示，列方向0~15表示每个核的数据块执行顺序。</p><p>由于每个核每一轮处理的地址在不同行，不会同时访问连续的512字节，所以不会导致同地址访问冲突。</p></td></tr><tr><td><p>示意图</p></td><td><img src="https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002564089979.png"></td><td><img src="https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002533170076.png"></td></tr><tr><td><p>示例代码</p></td><td><div><pre><code>for (int32_t i = 0; i < tiling->loopOneCore; i++) {
    AscendC::SyncAll();
    CopyIn(i);
    Compute();
    AscendC::SyncAll();
    CopyOut(i);
}</code></pre></div></td><td><div><pre><code>for (int32_t i = 0; i < tiling->loopOneCore; i++) {
    int32_t newProgress = (i + AscendC::GetBlockIdx()) % tiling->loopOneCore;
    AscendC::SyncAll();
    CopyIn(newProgress);
    Compute();
    AscendC::SyncAll();
    CopyOut(newProgress);
}</code></pre></div></td></tr></tbody></table>

**修改切分策略**

仍以一个形状为 (8192, 128) 的float类型输入进行Adds计算为例。

为了体现同地址冲突的影响，上述场景设计中每一行的数据大小为512字节（128个float），每个核每一轮计算处理512 \* 8字节的数据，并进行全核同步（实际场景中并不需要），每一轮计算都需要等待所有核完成当前数据块的计算后，再进行下一轮。

<table><colgroup><col> <col> <col></colgroup><thead><tr><th colspan="1"><p>实现方案</p></th><th colspan="1"><p>原始实现</p></th><th colspan="1"><p>优化实现</p></th></tr></thead><tbody><tr><td><p>实现方法</p></td><td><p>使用16个核参与计算，按列方向进行切分，每个核总计算数据量为8192 * 8；单核执行循环16次，每次计算的数据量为512 * 8；每个核的循环顺序如下图所示，列方向0~15表示每个核的数据块执行顺序。</p><p>由于多个核同时访问同一行数据（512字节），导致同地址冲突的发生。</p></td><td><p>使用16个核参与计算，按行方向进行切分，每个核总计算数据量为512 * 128；单核执行循环16次，每次计算的数据量为512 * 8；每个核的循环顺序如下图所示（行方向），均为从块0~块15。</p><p>由于每个核每一轮处理的地址在不同行，不会同时访问连续的512字节，所以不会导致同地址访问冲突。</p></td></tr><tr><td><p>示意图</p></td><td><img src="https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002564089977.png"></td><td><img src="https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002533010130.png"></td></tr><tr><td><p>示例代码</p></td><td><div><pre><code>__aicore__ inline void Init(GM_ADDR x, GM_ADDR z, AddsCustomTilingData* tilingPtr)
{
    tiling = tilingPtr;
    xGm.SetGlobalBuffer((__gm__ float *)x + AscendC::GetBlockIdx() * tiling->tileN);
    zGm.SetGlobalBuffer((__gm__ float *)z + AscendC::GetBlockIdx() * tiling->tileN);
   // we disable the L2 cache mode to highlight the influence of the gm address conflict    
    xGm.SetL2CacheHint(AscendC::CacheMode::CACHE_MODE_DISABLE);
    zGm.SetL2CacheHint(AscendC::CacheMode::CACHE_MODE_DISABLE);
    pipe.InitBuffer(inQueueX, BUFFER_NUM, tiling->tileM * tiling->tileN * sizeof(float));
    pipe.InitBuffer(outQueueZ, BUFFER_NUM, tiling->tileM * tiling->tileN * sizeof(float));
}</code></pre></div></td><td><div><pre><code>__aicore__ inline void Init(GM_ADDR x, GM_ADDR z, AddsCustomTilingData* tilingPtr)
{
    tiling = tilingPtr;
    // change the tile method from column split to row split
    xGm.SetGlobalBuffer((__gm__ float *)x + AscendC::GetBlockIdx() * tiling->tileM * tiling->n);
    zGm.SetGlobalBuffer((__gm__ float *)z + AscendC::GetBlockIdx() * tiling->tileM * tiling->n);
    // we disable the L2 cache mode to highlight the influence of the gm address conflict
    xGm.SetL2CacheHint(AscendC::CacheMode::CACHE_MODE_DISABLE);
    zGm.SetL2CacheHint(AscendC::CacheMode::CACHE_MODE_DISABLE);
    pipe.InitBuffer(inQueueX, BUFFER_NUM, tiling->tileM * tiling->tileN * sizeof(float));
    pipe.InitBuffer(outQueueZ, BUFFER_NUM, tiling->tileM * tiling->tileN * sizeof(float));
}</code></pre></div></td></tr></tbody></table>

说明

你可以通过执行如下命令行，通过 [msprof](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/devaids/optool/atlasopdev_16_0081.html) 工具获取上述示例的性能数据并进行对比。

```
msprof op --launch-count=3 --output=./prof ./execute_adds_op
```

重点关注PipeUtilization.csv中的aiv\_mte2\_time(us)和aiv\_mte3\_time(us)搬运指令耗时。

**父主题：** [内存访问](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/atlas_ascendc_best_practices_10_0016.html)

上一篇

非连续搬运场景减少搬运次数