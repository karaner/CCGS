# Unity 2023.2 — Addressables

**最后验证:** 2026-03-31
**状态:** 生产可用
**包:** `com.unity.addressables` (Package Manager)

---

## 概述

**Addressables** 是 Unity 的高级资源管理系统，替代 `Resources.Load()`，支持异步加载、远程内容传递和更好的内存控制。

**使用 Addressables 用于：**
- 异步资源加载（非阻塞）
- DLC 和远程内容
- 内存优化（按需加载/卸载）
- 资源依赖管理

---

## 基础加载

### 异步加载资源

```csharp
using UnityEngine.AddressableAssets;
using UnityEngine.ResourceManagement.AsyncOperations;

async void Start() {
    AsyncOperationHandle<GameObject> handle = Addressables.LoadAssetAsync<GameObject>("Enemies/Goblin");
    await handle.Task;

    if (handle.Status == AsyncOperationStatus.Succeeded) {
        GameObject prefab = handle.Result;
        Instantiate(prefab);
    }

    // ⚠️ 重要：完成后释放
    Addressables.Release(handle);
}
```

### 加载并实例化

```csharp
async void SpawnEnemy() {
    AsyncOperationHandle<GameObject> handle = Addressables.InstantiateAsync("Enemies/Goblin");
    await handle.Task;

    GameObject enemy = handle.Result;

    // 销毁时释放
    Addressables.ReleaseInstance(enemy);
}
```

---

## 内存管理

### 释放资源

```csharp
// 完成后始终释放
Addressables.Release(handle);

// 对于实例化的对象
Addressables.ReleaseInstance(gameObject);
```

---

## 从 Resources 迁移

```csharp
// ❌ 旧版：Resources.Load（同步，阻塞帧）
GameObject prefab = Resources.Load<GameObject>("Enemies/Goblin");

// ✅ 新版：Addressables（异步，非阻塞）
var handle = await Addressables.LoadAssetAsync<GameObject>("Enemies/Goblin").Task;
GameObject prefab = handle.Result;
```

---

## 参考来源

- https://docs.unity3d.com/2023.2/Documentation/Manual/Addressables.html
