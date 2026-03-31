# Unity 2023.2 — 破坏性变更

**最后验证:** 2026-03-31

本文档追踪 Unity 2022 LTS（可能在模型训练中）与 Unity 2023.2 之间的破坏性 API 变更和行为差异。

> **注意:** Unity 2023.2 在 LLM 训练数据范围内，此文档仅供参考。
> 主要破坏性变更多数在 Unity 6.x 版本。

---

## Unity 2023.2 中的已知变更

### Input System 包变更
**版本:** Unity 2023.x

安装 Input System 包后，遗留 Input 类仍然可用，但新项目推荐使用 Input System。

```csharp
// ✅ 推荐: Input System
using UnityEngine.InputSystem;
if (Keyboard.current.spaceKey.wasPressedThisFrame) {
    Jump();
}
```

---

### Addressables 改进
**版本:** Unity 2023.x

异步加载模式是标准做法。

```csharp
// ✅ 推荐: 异步加载
var handle = Addressables.LoadAssetAsync<Sprite>("key");
var sprite = await handle.Task;
```

---

## 平台特定注意事项

### WebGL
- Unity 2023.2 支持 WebGL 2.0
- WebGPU 在 Unity 6.x 中成为默认

### Android
- 最低 API 等级: Android 5.0 (API 21)

### iOS
- 最低部署目标: iOS 12.0

---

## 迁移检查清单

从旧版本迁移到 Unity 2023.2 时:

- [ ] 审核所有第三方依赖兼容性
- [ ] 检查 Addressables 异步加载使用情况
- [ ] 确认物理系统行为未改变
- [ ] 验证平台最低版本要求

---

**参考来源:**
- https://docs.unity3d.com/2023.2/Documentation/Manual/
- https://unity.com/releases/editor/whats-new/2023.2.0
