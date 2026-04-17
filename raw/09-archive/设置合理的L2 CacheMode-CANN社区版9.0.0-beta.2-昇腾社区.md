---
title: "设置合理的L2 CacheMode-CANN社区版9.0.0-beta.2-昇腾社区"
source: "https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/atlas_ascendc_best_practices_10_00014.html"
author:
published:
created: 2026-04-17
description: "设置合理的L2 CacheMode "
tags:
  - "原始网页"
---
## 设置合理的L2 CacheMode

## 设置合理的L2 CacheMode

【优先级】高

说明

该性能优化指导适用于如下产品型号：

- Atlas A3 训练系列产品 / Atlas A3 推理系列产品
- Atlas A2 训练系列产品 / Atlas A2 推理系列产品

【描述】L2 Cache常用于缓存频繁访问的数据，其物理位置如下图所示：

![](https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002533169954.png)

L2 Cache的带宽相比GM的带宽有数倍的提升，因此当数据命中L2 Cache时，数据的搬运耗时会优化数倍。通常情况下，L2 Cache命中率越高，算子的性能越好，在实际访问中需要通过设置合理的L2 CacheMode来保证重复读取的数据尽量缓存在L2 Cache上。

#### L2 Cache访问的原理及CacheMode介绍

数据通过MTE2搬运单元搬入时，L2 Cache访问的典型流程如下：

![](https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002564089859.png)

数据通过MTE3或者Fixpipe搬运单元搬出时，L2 Cache访问的典型流程如下：

![](https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002533169960.png)

从上面的流程可以看出，当数据访问总量超出L2 Cache容量时，AI Core会对L2 Cache进行数据替换。由于Cache一致性的要求，替换过程中旧数据需要先写回GM（此过程中会占用GM带宽），旧数据写回后，新的数据才能进入L2 Cache。

开发者可以针对访问的数据设置其CacheMode，对于只访问一次的Global Memory数据设置其访问状态为不进入L2 Cache，这样可以更加高效的利用L2 Cache缓存需要重复读取的数据，避免一次性访问的数据替换有效数据。

#### 设置L2 CacheMode的方法

Ascend C基于GlobalTensor提供了 [SetL2CacheHint](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/API/ascendcopapi/atlasascendc_api_07_00033.html) 接口，用户可以根据需要指定CacheMode。

考虑如下场景，构造两个Tensor的计算，x的输入Shape为(5120, 5120)，y的输入Shape为(5120, 15360)，z的输出Shape为(5120, 15360)，由于两个Tensor的Shape不相等，x分别与y的3个数据块依次相加。该方案主要为了演示CacheMode的功能，示例代码中故意使用重复搬运x的实现方式，真实设计中并不需要采用这个方案。下文完整样例请参考 [设置合理L2 CacheMode样例](https://gitcode.com/cann/asc-devkit/tree/9.0.0-beta.2/examples/01_simd_cpp_api/04_best_practices/00_vector_compute_practices/l2_cache_bypass) 。

![](https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002533169952.png)

<table><colgroup><col> <col> <col></colgroup><thead><tr><th colspan="1"><p>实现方案</p></th><th colspan="1"><p>原始实现</p></th><th colspan="1"><p>优化实现</p></th></tr></thead><tbody><tr><td><p>实现方法</p></td><td><p>总数据量700MB，其中：x：100MB；y：300MB；z：300MB。</p><p>使用40个核参与计算，按列方向切分。</p><p>x、y、z 对应GlobalTensor的CacheMode均设置为CACHE_MODE_NORMAL，需要经过L2 Cache，需要进入L2 Cache的总数据量为700MB。</p></td><td><p>总数据量700MB，其中：x：100MB；y：300MB；z：300MB。</p><p>使用40个核参与计算，按列方向切分。</p><p>x对应的GlobalTensor的CacheMode设置为CACHE_MODE_NORMAL；y和z对应的GlobalTensor的CacheMode设置为CACHE_MODE_DISABLE。只有需要频繁访问的x，设置为需要经过L2 Cache。需要进入L2 Cache的总数据量为100MB。</p></td></tr><tr><td><p>示例代码</p></td><td><div><pre><code>xGm.SetGlobalBuffer((__gm__ float *)x + AscendC::GetBlockIdx() * TILE_N);
yGm.SetGlobalBuffer((__gm__ float *)y + AscendC::GetBlockIdx() * TILE_N);
zGm.SetGlobalBuffer((__gm__ float *)z + AscendC::GetBlockIdx() * TILE_N);</code></pre></div></td><td><div><pre><code>xGm.SetGlobalBuffer((__gm__ float *)x + AscendC::GetBlockIdx() * TILE_N);
yGm.SetGlobalBuffer((__gm__ float *)y + AscendC::GetBlockIdx() * TILE_N);
zGm.SetGlobalBuffer((__gm__ float *)z + AscendC::GetBlockIdx() * TILE_N);
// disable the L2 cache mode of y and z
yGm.SetL2CacheHint(AscendC::CacheMode::CACHE_MODE_DISABLE);
zGm.SetL2CacheHint(AscendC::CacheMode::CACHE_MODE_DISABLE);</code></pre></div></td></tr></tbody></table>

说明

你可以通过执行如下命令行，通过 [msprof](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/devaids/optool/atlasopdev_16_0081.html) 工具获取上述示例的性能数据并进行对比。

```
msprof op --launch-count=2 --output=./prof ./execute_add_op
```

重点关注Memory.csv中的aiv\_gm\_to\_ub\_bw(GB/s)和aiv\_main\_mem\_write\_bw(GB/s)写带宽的速率。

**父主题：** [内存访问](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/atlas_ascendc_best_practices_10_0016.html)

上一篇

避免同地址访问