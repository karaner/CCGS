# Unity 2023.2 — DOTS / Entities (ECS)

**最后验证:** 2026-03-31
**状态:** 生产可用
**包:** `com.unity.entities` (Package Manager)

---

## 概述

**DOTS (Data-Oriented Technology Stack)** 是 Unity 的高性能 ECS（实体组件系统）框架。专为大规模游戏设计（1000s-10000s 实体）。

**使用 DOTS 用于：**
- RTS 游戏（大量单位）
- 模拟（人群、交通、物理）
- 程序化内容生成
- 性能关键系统

**不要使用 DOTS 用于：**
- 小型游戏（开销不值得）
- 频繁结构更改的游戏玩法
- 大量使用 UnityEngine API（MonoBehaviour 更简单）

---

## 核心概念

### 1. **Entity**
- 轻量级 ID（int）
- 无行为，只是标识符

### 2. **Component**
- 仅数据（无方法）
- 实现 `IComponentData` 的结构体

### 3. **System**
- 对组件进行操作的逻辑
- 实现 `ISystem` 的结构体

---

## 基础 ECS 模式

### 定义组件

```csharp
using Unity.Entities;
using Unity.Mathematics;

// ✅ 组件：仅数据，无方法
public struct Position : IComponentData {
    public float3 Value;
}

public struct Velocity : IComponentData {
    public float3 Value;
}
```

### 定义系统

```csharp
using Unity.Entities;
using Unity.Burst;

[BurstCompile]
public partial struct MovementSystem : ISystem {
    [BurstCompile]
    public void OnUpdate(ref SystemState state) {
        float deltaTime = SystemAPI.Time.DeltaTime;

        foreach (var (transform, velocity) in
            SystemAPI.Query<RefRW<Position>, RefRO<Velocity>>()) {

            transform.ValueRW.Value += velocity.ValueRO.Value * deltaTime;
        }
    }
}
```

---

## Jobs（并行执行）

### IJobEntity（并行 Foreach）

```csharp
[BurstCompile]
public partial struct MovementJob : IJobEntity {
    public float DeltaTime;

    void Execute(ref Position position, in Velocity velocity) {
        position.Value += velocity.Value * DeltaTime;
    }
}

[BurstCompile]
public partial struct MovementSystem : ISystem {
    public void OnUpdate(ref SystemState state) {
        var job = new MovementJob {
            DeltaTime = SystemAPI.Time.DeltaTime
        };
        job.ScheduleParallel();
    }
}
```

---

## 从 MonoBehaviour 迁移

```csharp
// ❌ 旧版：MonoBehaviour
public class Enemy : MonoBehaviour {
    public float speed;
    void Update() {
        transform.position += Vector3.forward * speed * Time.deltaTime;
    }
}

// ✅ 新版：DOTS (ECS)
public struct EnemyData : IComponentData {
    public float Speed;
}

[BurstCompile]
public partial struct EnemyMovementSystem : ISystem {
    public void OnUpdate(ref SystemState state) {
        float dt = SystemAPI.Time.DeltaTime;
        foreach (var (transform, enemy) in
            SystemAPI.Query<RefRW<LocalTransform>, RefRO<EnemyData>>()) {
            transform.ValueRW.Position += new float3(0, 0, enemy.ValueRO.Speed * dt);
        }
    }
}
```

---

## 调试

### Entities Hierarchy Window

```
Window > Entities > Hierarchy
显示所有实体及其组件
```

---

## 参考来源

- https://docs.unity3d.com/2023.2/Documentation/Manual entities about.html
