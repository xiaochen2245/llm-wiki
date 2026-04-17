---
title: "尽量一次搬运较大的数据块-CANN社区版9.0.0-beta.2-昇腾社区"
source: "https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/atlas_ascendc_best_practices_10_0013.html"
author:
published:
created: 2026-04-17
description: "尽量一次搬运较大的数据块"
tags:
  - "原始网页"
---
## 尽量一次搬运较大的数据块

## 尽量一次搬运较大的数据块

【优先级】高

【描述】搬运不同大小的数据块时，对带宽的利用率（有效带宽/理论带宽）不一样。根据实测经验，单次搬运数据长度16KB以上时，通常能较好地发挥出带宽的最佳性能。因此对于单次搬运，应考虑尽可能的搬运较大的数据块。下图展示了某款AI处理器上实测的不同搬运数据量下带宽的变化图。

说明

测试数据与处理器型号相关，且实际测试时可能会存在略微抖动，具体带宽数值并不一定和下文的测试数据严格一致。

**图1** UB->GM方向不同单次搬运数据量下实际占用带宽的变化  
![](https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002533010092.png)

**图2** GM->UB方向不同单次搬运数据量下实际占用带宽的变化  
![](https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002533010090.png)

**父主题：** [内存访问](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/atlas_ascendc_best_practices_10_0016.html)

上一篇

使能Iterate或IterateAll异步接口避免AIC/AIV同步依赖