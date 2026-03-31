# Unity 2023.2 — Physics 模块参考

**最后验证:** 2026-03-31

---

## 概述

Unity 2023.2 使用 **PhysX**：
- 更好的求解器稳定性
- 改进的性能
- 增强的碰撞检测

---

## 核心物理组件

### Rigidbody

```csharp
// ✅ 最佳实践：使用 AddForce，不要直接赋值速度
Rigidbody rb = GetComponent<Rigidbody>();
rb.AddForce(Vector3.forward * 10f, ForceMode.Impulse);

// ❌ 避免：直接速度赋值（可能导致不稳定）
rb.velocity = new Vector3(0, 10, 0);
```

### Colliders

```csharp
// 原始碰撞体：Box, Sphere, Capsule（最便宜）
// 网格碰撞体：昂贵，仅对静态几何体使用

// ✅ 复合碰撞体（多个原始体）> 单个网格碰撞体
```

---

## Raycasting

### 高效 Raycasting（避免分配）

```csharp
// ✅ 非分配 raycast
if (Physics.Raycast(origin, direction, out RaycastHit hit, maxDistance)) {
    Debug.Log($"击中: {hit.collider.name}");
}

// ✅ 多个命中（非分配）
RaycastHit[] results = new RaycastHit[10];
int hitCount = Physics.RaycastNonAlloc(origin, direction, results, maxDistance);

// ❌ 避免：RaycastAll（每次调用分配数组！）
RaycastHit[] hits = Physics.RaycastAll(origin, direction);
```

### LayerMask 选择性 Raycasting

```csharp
int layerMask = 1 << LayerMask.NameToLayer("Enemy");
Physics.Raycast(origin, direction, out RaycastHit hit, maxDistance, layerMask);
```

---

## 物理查询

### OverlapSphere（检查附近对象）

```csharp
Collider[] results = new Collider[10];
int count = Physics.OverlapSphereNonAlloc(center, radius, results);
```

---

## 碰撞事件

### OnCollisionEnter / Stay / Exit

```csharp
void OnCollisionEnter(Collision collision) {
    Debug.Log($"碰撞: {collision.gameObject.name}");
    foreach (ContactPoint contact in collision.contacts) {
        Debug.DrawRay(contact.point, contact.normal, Color.red, 2f);
    }
}
```

### OnTriggerEnter / Stay / Exit

```csharp
void OnTriggerEnter(Collider other) {
    if (other.CompareTag("Pickup")) {
        Destroy(other.gameObject);
    }
}
```

---

## 角色控制器

### CharacterController 组件

```csharp
CharacterController controller = GetComponent<CharacterController>();

Vector3 move = transform.forward * speed * Time.deltaTime;
controller.Move(move);

// 手动应用重力
if (!controller.isGrounded) {
    velocity.y += Physics.gravity.y * Time.deltaTime;
}
controller.Move(velocity * Time.deltaTime);
```

---

## 性能优化

### 物理层碰撞矩阵

`Edit > Project Settings > Physics > Layer Collision Matrix`
- 禁用层之间的不必要碰撞检查
- 大幅性能提升

### 固定时间步

`Edit > Project Settings > Time > Fixed Timestep`
- 默认: 0.02（50 FPS 物理）
- 更低 = 更准确，更高 CPU 成本

### 简化碰撞几何体

- 使用原始碰撞体（box, sphere, capsule）而非网格碰撞体
- 在构建时烘焙网格碰撞体，而非运行时

---

## 常见模式

### 地面检测

```csharp
bool IsGrounded() {
    float rayLength = 0.1f;
    return Physics.Raycast(transform.position, Vector3.down, rayLength);
}
```

---

## 调试

- `Window > Analysis > Physics Debugger` — 可视化碰撞体、接触点、查询

---

## 参考来源

- https://docs.unity3d.com/2023.2/Documentation/Manual/PhysicsOverview.html
