# Unity 2023.2 — 动画模块参考

**最后验证:** 2026-03-31

---

## 概述

Unity 2023.2 动画系统：
- **Animator Controller (Mecanim)**: 基于状态机（推荐）
- **Timeline**: 影视序列、过场动画
- **Animation Rigging**: 程序化运行时动画
- **Legacy Animation**: 已废弃，避免使用

---

## Animator Controller (Mecanim)

### 基础设置

```csharp
// 创建: Assets > Create > Animator Controller
// 添加到 GameObject: Add Component > Animator
// 分配 Controller: Animator > Controller = YourAnimatorController
```

### 状态过渡

```csharp
Animator animator = GetComponent<Animator>();

// ✅ 触发过渡
animator.SetTrigger("Jump");

// ✅ Bool 参数
animator.SetBool("IsRunning", true);

// ✅ Float 参数（混合树）
animator.SetFloat("Speed", currentSpeed);

// ✅ Integer 参数
animator.SetInteger("WeaponType", 2);
```

### 动画层

- **Base Layer**: 默认动画（移动）
- **Override Layers**: 覆盖基础层（例如武器切换）
- **Additive Layers**: 在基础上叠加（例如呼吸、瞄准偏移）

---

## 混合树

### 1D 混合树（速度混合）

```csharp
// Idle (Speed = 0) → Walk (Speed = 0.5) → Run (Speed = 1.0)
animator.SetFloat("Speed", moveSpeed);
```

---

## 动画事件

### 从动画剪辑触发事件

```csharp
// 在 Animation 窗口添加：右键时间线 > Add Animation Event
// GameObject 上必须有匹配的方法：

public void OnFootstep() {
    // 播放脚步声
}

public void OnAttackHit() {
    // 造成伤害
}
```

---

## 根运动

### 通过动画控制角色移动

```csharp
Animator animator = GetComponent<Animator>();
animator.applyRootMotion = true; // 基于动画移动角色

void OnAnimatorMove() {
    transform.position += animator.deltaPosition;
    transform.rotation *= animator.deltaRotation;
}
```

---

## Timeline（过场动画）

### 基础 Timeline 设置

```csharp
// 创建: Assets > Create > Timeline
// 添加到 GameObject: Add Component > Playable Director

PlayableDirector director = GetComponent<PlayableDirector>();
director.Play();
```

### Timeline 轨道

- **Activation Track**: 启用/禁用 GameObject
- **Animation Track**: 在 Animator 上播放动画
- **Audio Track**: 同步音频播放
- **Cinemachine Track**: 相机移动
- **Signal Track**: 在特定时间触发事件

---

## 动画回放控制

### 直接播放动画（无状态机）

```csharp
// ✅ CrossFade（平滑过渡）
animator.CrossFade("Attack", 0.2f);

// ✅ Play（立即）
animator.Play("Idle");
```

---

## 性能优化

### 剔除

- `Animator > Culling Mode`:
  - **Always Animate**: 始终更新（昂贵）
  - **Cull Update Transforms**: 屏幕外时停止更新骨骼（推荐）
  - **Cull Completely**: 屏幕外时停止所有动画

---

## 常见模式

### 检查动画是否完成

```csharp
AnimatorStateInfo stateInfo = animator.GetCurrentAnimatorStateInfo(0);
if (stateInfo.IsName("Attack") && stateInfo.normalizedTime >= 1.0f) {
    // 攻击动画完成
}
```

---

## 调试

- `Window > Animation > Animator` — 可视化状态机，查看活动状态
- `Window > Animation > Animation` — 编辑动画剪辑，添加事件

---

## 参考来源

- https://docs.unity3d.com/2023.2/Documentation/Manual/AnimationOverview.html
