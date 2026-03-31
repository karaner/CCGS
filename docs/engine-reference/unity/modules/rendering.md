# Unity 2023.2 — Rendering 模块参考

**最后验证:** 2026-03-31

---

## 概述

Unity 2023.2 渲染系统：
- **URP (Universal Render Pipeline)**: 跨平台、移动端友好（推荐）
- **HDRP (High Definition Render Pipeline)**: 高端 PC/主机、写实画质
- **Built-in Pipeline**: 已废弃，新项目避免使用

---

## URP 快速参考

### 创建 URP 资源

1. `Assets > Create > Rendering > URP Asset (with Universal Renderer)`
2. 分配给 `Project Settings > Graphics > Scriptable Render Pipeline Settings`

### 材质和着色器

#### Shader Graph（可视化着色器编辑器）

```csharp
// 创建: Assets > Create > Shader Graph > URP > Lit Shader Graph
// 无需代码，可视化节点编辑
```

---

## 照明

### 烘焙照明

```csharp
// 标记对象为静态: Inspector > Static > Contribute GI
// 烘焙: Window > Rendering > Lighting > Generate Lighting
```

---

## 后处理

### Volume 系统

```csharp
using UnityEngine.Rendering;
using UnityEngine.Rendering.Universal;

// 添加 Volume 组件到 GameObject
// 添加 Volume Profile 资源
// 配置效果: Bloom, Color Grading, Depth of Field 等

Volume volume = GetComponent<Volume>();
if (volume.profile.TryGet<Bloom>(out var bloom)) {
    bloom.intensity.value = 2.5f;
}
```

---

## 性能

### SRP Batcher（自动批处理）

```csharp
// 启用: URP Asset > Advanced > SRP Batcher = Enabled
// 对相同着色器变体的绘制进行批处理（最小 CPU 开销）
```

### GPU Instancing

```csharp
// 材质: 勾选 "Enable GPU Instancing"
// 对相同网格（相同材质 + 网格）进行批处理

Graphics.RenderMeshInstanced(
    new RenderParams(material),
    mesh,
    0,
    matrices
);
```

### 遮挡剔除

```csharp
// Window > Rendering > Occlusion Culling
// 为静态几何体烘焙遮挡数据
```

---

## 调试

### Frame Debugger

- `Window > Analysis > Frame Debugger` — 逐步查看绘制调用，检查状态

---

## 参考来源

- https://docs.unity3d.com/2023.2/Documentation/Manual/render-pipelines.html
