# Unity 2023.2 — Navigation 模块参考

**最后验证:** 2026-03-31

---

## 概述

Unity 2023.2 导航系统：
- **NavMesh**: AI 代理的内置寻路
- **NavMeshComponents**: 用于运行时 NavMesh 构建的包

---

## NavMesh 基础

### 烘焙 Navigation Mesh

1. 标记可行走表面：
   - 选择 GameObject（地面/地形）
   - Inspector > Navigation > Object tab
   - 勾选 "Navigation Static"

2. 烘焙 NavMesh：
   - `Window > AI > Navigation`
   - Bake tab
   - 点击 "Bake"

3. 配置设置：
   - **Agent Radius**: 代理宽度（默认 0.5m）
   - **Agent Height**: 代理高度（默认 2m）
   - **Max Slope**: 最大可行走坡度（默认 45°）
   - **Step Height**: 最大可攀爬台阶（默认 0.4m）

---

## NavMeshAgent（AI 移动）

### 基础代理设置

```csharp
using UnityEngine;
using UnityEngine.AI;

public class Enemy : MonoBehaviour {
    private NavMeshAgent agent;
    public Transform target;

    void Start() {
        agent = GetComponent<NavMeshAgent>();
    }

    void Update() {
        agent.SetDestination(target.position);
    }
}
```

### NavMeshAgent 属性

```csharp
NavMeshAgent agent = GetComponent<NavMeshAgent>();

agent.speed = 3.5f;
agent.acceleration = 8f;
agent.stoppingDistance = 2f;
agent.autoBraking = true;
agent.angularSpeed = 120f;
agent.obstacleAvoidanceType = ObstacleAvoidanceType.HighQualityObstacleAvoidance;
```

### 检查路径状态

```csharp
void Update() {
    agent.SetDestination(target.position);

    if (agent.hasPath) {
        if (agent.pathStatus == NavMeshPathStatus.PathComplete) {
            Debug.Log("有效路径");
        } else if (agent.pathStatus == NavMeshPathStatus.PathPartial) {
            Debug.Log("部分路径（目的地不可达）");
        }
    }

    if (!agent.pathPending && agent.remainingDistance <= agent.stoppingDistance) {
        Debug.Log("到达目的地");
    }
}
```

---

## NavMesh 区域（可行走成本）

### 定义区域

`Window > AI > Navigation > Areas tab`
- **Walkable**: 成本 1（默认）
- **Not Walkable**: 不可行走
- **Jump**: 成本 2（优先其他路线）

---

## 常见模式

### 巡逻路径点

```csharp
public Transform[] waypoints;
private int currentWaypoint = 0;

void Update() {
    if (!agent.pathPending && agent.remainingDistance < 0.5f) {
        currentWaypoint = (currentWaypoint + 1) % waypoints.Length;
        agent.SetDestination(waypoints[currentWaypoint].position);
    }
}
```

### 追逐玩家

```csharp
public Transform player;
public float chaseRange = 10f;

void Update() {
    float distance = Vector3.Distance(transform.position, player.position);
    if (distance <= chaseRange) {
        agent.SetDestination(player.position);
    } else {
        agent.ResetPath();
    }
}
```

---

## 调试

- `Window > AI > Navigation > Bake tab` — 勾选 "Show NavMesh" 可视化可行走区域

---

## 性能提示

- **限制障碍物回避质量**: 对远处代理使用 `LowQualityObstacleAvoidance`
- **更新频率**: 如果目标没有移动，不要每帧调用 `SetDestination()`

---

## 参考来源

- https://docs.unity3d.com/2023.2/Documentation/Manual/Navigation.html
