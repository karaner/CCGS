# Unity 2023.2 — 已废弃 API

**最后验证:** 2026-03-31

已废弃 API 快速查询表。
格式: **不要使用 X** → **改用 Y**

---

## Input

| 已废弃 | 替代方案 | 备注 |
|--------|----------|------|
| `Input.GetKey()` | `Keyboard.current[Key.X].isPressed` | 需要 Input System 包 |
| `Input.GetKeyDown()` | `Keyboard.current[Key.X].wasPressedThisFrame` | 需要 Input System 包 |
| `Input.GetMouseButton()` | `Mouse.current.leftButton.isPressed` | 需要 Input System 包 |
| `Input.GetAxis()` | `InputAction` 回调 | 需要 Input System 包 |

**迁移:** 安装 `com.unity.inputsystem` 包。

---

## 资源加载

| 已废弃 | 替代方案 | 备注 |
|--------|----------|------|
| `Resources.Load()` | Addressables | 更好的内存控制，异步加载 |
| 同步资源加载 | `Addressables.LoadAssetAsync()` | 非阻塞 |

**迁移:** 使用 Addressables 进行资源管理。

---

## 场景管理

| 已废弃 | 替代方案 | 备注 |
|--------|----------|------|
| `Application.LoadLevel()` | `SceneManager.LoadScene()` | 现代化场景管理 |
| `Application.LoadLevelAsync()` | `AsyncOperation` + SceneManager | 异步加载 |

---

## 网络

| 已废弃 | 替代方案 | 备注 |
|--------|----------|------|
| `WWW` 类 | `UnityWebRequest` | 现代异步网络 |

---

## 快速迁移示例

### Input 迁移示例
```csharp
// ❌ 已废弃
if (Input.GetKeyDown(KeyCode.Space)) {
    Jump();
}

// ✅ Input System
using UnityEngine.InputSystem;
if (Keyboard.current.spaceKey.wasPressedThisFrame) {
    Jump();
}
```

### 资源加载迁移示例
```csharp
// ❌ 已废弃
var prefab = Resources.Load<GameObject>("Enemies/Goblin");

// ✅ Addressables
var handle = Addressables.LoadAssetAsync<GameObject>("Enemies/Goblin");
await handle.Task;
var prefab = handle.Result;
```

---

**参考来源:**
- https://docs.unity3d.com/2023.2/Documentation/Manual/
