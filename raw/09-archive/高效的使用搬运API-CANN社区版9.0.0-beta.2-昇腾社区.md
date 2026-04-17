---
title: "高效的使用搬运API-CANN社区版9.0.0-beta.2-昇腾社区"
source: "https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/atlas_ascendc_best_practices_10_0015.html"
author:
published:
created: 2026-04-17
description: "高效的使用搬运AP"
tags:
  - "原始网页"
---
## 高效的使用搬运API

## 高效的使用搬运API

【优先级】高

【描述】在使用搬运API时，应该尽可能地通过配置搬运控制参数实现连续搬运或者固定间隔搬运，避免使用for循环，二者效率差距极大。如下图示例，图片的每一行为16KB，需要从每一行中搬运前2KB，针对这种场景，使用for循环遍历每行，每次仅能搬运 **2** KB。若直接配置DataCopyParams参数（包含srcStride/dstStride/blockLen/blockCount），则可以达到一次搬完的效果，每次搬运 **32** KB；参考 [尽量一次搬运较大的数据块](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/atlas_ascendc_best_practices_10_0013.html) 章节介绍的搬运数据量和实际带宽的关系，建议一次搬完。

**图1** 待搬运数据排布  
![](https://www.hiascend.com/doc_center/source/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/figure/zh-cn_image_0000002564089985.png)

【反例】

```
// 搬运数据存在间隔，从GM上每行16KB中搬运2KB数据, 共16行
LocalTensor<float> tensorIn;
GlobalTensor<float> tensorGM;
...
constexpr int32_t copyWidth = 2 * 1024 / sizeof(float);
constexpr int32_t imgWidth = 16 * 1024 / sizeof(float);
constexpr int32_t imgHeight = 16;
// 使用for循环，每次只能搬运2K，重复16次
for (int i = 0; i < imgHeight; i++) {
    DataCopy(tensorIn[i * copyWidth], tensorGM[i * imgWidth], copyWidth);
}
```

【正例】

```
LocalTensor<float> tensorIn;
GlobalTensor<float> tensorGM;
...
constexpr int32_t copyWidth = 2 * 1024 / sizeof(float);
constexpr int32_t imgWidth = 16 * 1024 / sizeof(float);
constexpr int32_t imgHeight = 16;
// 通过DataCopy包含DataCopyParams的接口一次搬完
DataCopyParams copyParams;
copyParams.blockCount = imgHeight;
copyParams.blockLen = copyWidth / 8; // 搬运的单位为DataBlock(32Byte)，每个DataBlock内有8个float
copyParams.srcStride = (imgWidth  - copyWidth) / 8; // 表示两次搬运src之间的间隔，单位为DataBlock
copyParams.dstStride = 0; // 连续写，两次搬运之间dst的间隔为0，单位为DataBlock
DataCopy(tensorGM, tensorIn, copyParams);
```

**父主题：** [内存访问](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/900beta2/opdevg/Ascendcopdevg/atlas_ascendc_best_practices_10_0016.html)

上一篇

GM地址尽量512B对齐