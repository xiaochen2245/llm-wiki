---
title: "Workspace"
type: concept
tags: [内存管理, 性能优化, 冗余操作]
sources: ["raw/01-articles/核函数内删除Workspace相关冗余操作-CANN社区版9.0.0-beta.2-昇腾社区.md"]
last_updated: 2026-04-17
---

## 定义

Workspace是算子运行时的工作空间，用于存放算子执行过程中的临时数据。在Ascend C算子工程中，核函数时传入的workspace参数已直接赋值为用户Workspace，无需再通过SetSysWorkspace和GetUserWorkspace来设置和获取。

## 冗余操作问题

### 典型冗余代码
```cpp
__global__ __aicore__ void fast_gelu(GM_ADDR x, GM_ADDR y, GM_ADDR workspace, GM_ADDR tiling)
{
    // 冗余判断
    if (workspace == nullptr) {
        return;
    }
    SetSysWorkspace(workspace);
    GM_ADDR userWS = GetUserWorkspace(workspace);  // 冗余
    if (userWS == nullptr) {
        return;
    }
    ...
}
```

### 冗余原因
- workspace参数已经直接赋值为用户Workspace
- 无需再次设置和获取

## 优化方法

### 删除冗余操作
```cpp
__global__ __aicore__ void fast_gelu(GM_ADDR x, GM_ADDR y, GM_ADDR workspace, GM_ADDR tiling)
{
    // 无需冗余判断
    REGISTER_TILING_DEFAULT(EleBaseTilingDataV2);
    GET_TILING_DATA_WITH_STRUCT(EleBaseTilingDataV2, tilingData, tiling);
    KERNEL_TASK_TYPE_DEFAULT(KERNEL_TYPE_AIV_ONLY);
    TPipe pipe;
    ...
}
```

### 优化效果
- 减少冗余判断开销
- 编译器可以进一步优化未使用的workspace变量

## 相关API

| API | 说明 |
|-----|------|
| SetSysWorkspace | 设置系统Workspace |
| GetUserWorkspace | 获取用户Workspace |

## 关联连接
- [[内存管理]] — 内存管理相关
- [[头尾开销优化]] — 上层优化方向
- [[摘要-核函数内删除workspace相关冗余操作]] — 原始文档
