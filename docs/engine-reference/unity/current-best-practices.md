# Unity 2023.2 — 当前最佳实践

**最后验证:** 2026-03-31

Unity 2023.2 的现代开发模式推荐。

---

## 项目设置

### 选择合适的渲染管线

- **URP (Universal):** 移动端、跨平台、高性能 ✅ 推荐大多数游戏使用
- **HDRP (High Definition):** 高端 PC/主机、写实画质
- **Built-in:** 已废弃，新项目避免使用

---

## 脚本编写

### 使用 C# 8/9 特性 (Unity 2023.x 支持)

```csharp
// ✅ Record 类型用于数据
public record PlayerData(string Name, int Level, float Health);

// ✅ 模式匹配
var result = enemy switch {
    Boss boss => boss.Enrage(),
    Minion minion => minion.Flee(),
    _ => null
};
```

### 使用 Async/Await 进行资源加载

```csharp
// ✅ 现代异步模式
public async Task<GameObject> LoadEnemyAsync(string key) {
    var handle = Addressables.LoadAssetAsync<GameObject>(key);
    return await handle.Task;
}
```

---

## Input

### 使用 Input System 包 (推荐)

```csharp
// ✅ Input Actions (可重新绑定，跨平台)
using UnityEngine.InputSystem;

public class PlayerInput : MonoBehaviour {
    private PlayerControls controls;

    void Awake() {
        controls = new PlayerControls();
        controls.Gameplay.Jump.performed += ctx => Jump();
    }

    void OnEnable() => controls.Enable();
    void OnDisable() => controls.Disable();
}
```

---

## UI

### 使用 UI Toolkit (现代化 UI)

```csharp
// ✅ UI Toolkit (推荐新项目使用)
using UnityEngine.UIElements;

public class MainMenu : MonoBehaviour {
    void OnEnable() {
        var root = GetComponent<UIDocument>().rootVisualElement;

        var playButton = root.Q<Button>("play-button");
        playButton.clicked += StartGame;
    }
}
```

**UXML** (UI结构) + **USS** (样式) = HTML/CSS 类工作流。

### 或使用 TextMeshPro (经典 UGUI 升级)

```csharp
// ✅ TextMeshPro (更好的渲染效果)
GetComponent<TextMeshProUGUI>().text = "Score: 100";
```

---

## 资源管理

### 使用 Addressables (不是 Resources)

```csharp
// ✅ Addressables (异步，内存高效)
using UnityEngine.AddressableAssets;

public async Task SpawnEnemyAsync(string enemyKey) {
    var handle = Addressables.InstantiateAsync(enemyKey);
    var enemy = await handle.Task;
    Addressables.ReleaseInstance(enemy);
}
```

**优势:** 异步加载、远程内容传递、更好的内存控制。

---

## 性能

### 使用对象池 (避免每帧分配)

```csharp
// ✅ 对象池模式
public class ObjectPool {
    private Queue<GameObject> pool = new Queue<GameObject>();

    public GameObject Get() {
        return pool.Count > 0 ? pool.Dequeue() : CreateNew();
    }

    public void Return(GameObject obj) {
        obj.SetActive(false);
        pool.Enqueue(obj);
    }
}
```

### 使用 Jobs System (多线程)

```csharp
// ✅ Burst 编译的 Job (大幅性能提升)
[BurstCompile]
public struct DamageJob : IJobForEach<Health, DamageOverTime> {
    public float DeltaTime;

    public void Execute(ref Health health, ref DamageOverTime dot) {
        health.Value -= dot.DamagePerSecond * DeltaTime;
    }
}
```

---

## 内存管理

### 在 Play Mode 测试中使用 NativeArray

```csharp
// ✅ NativeArray (无 GC，Burst 兼容)
NativeArray<int> data = new NativeArray<int>(1000, Allocator.TempJob);
// ... 在 Job 中使用
data.Dispose(); // 手动清理
```

---

## 测试

### 使用 Unity Test Framework (基于 NUnit)

```csharp
// ✅ Play Mode 测试
[UnityTest]
public IEnumerator Player_TakesDamage_HealthDecreases() {
    var player = new GameObject().AddComponent<Player>();
    player.Health = 100;

    player.TakeDamage(25);
    yield return null; // 等待一帧

    Assert.AreEqual(75, player.Health);
}
```

---

## 调试

### 使用结构化日志

```csharp
// ✅ 结构化日志
Debug.Log($"Player {playerName} scored {score} points");

// ✅ 条件编译调试代码
#if UNITY_EDITOR || DEVELOPMENT_BUILD
    Debug.DrawRay(transform.position, direction, Color.red);
#endif
```

---

## Unity 2023.2 技术栈总结

| 功能 | 使用这个 (2023.2) | 避免使用 (遗留) |
|------|-------------------|-----------------|
| **Input** | Input System 包 | `Input` 类 |
| **UI** | UI Toolkit 或 TextMeshPro | 旧版 Text 组件 |
| **资源** | Addressables | Resources |
| **Jobs** | Burst + IJobForEach | 协程处理重型工作 |
| **场景** | SceneManager | Application.LoadLevel |

---

**参考来源:**
- https://docs.unity3d.com/2023.2/Documentation/Manual/BestPracticeGuides.html
- https://learn.unity.com/
