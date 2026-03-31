# Unity 2023.2 — Cinemachine

**最后验证:** 2026-03-31
**状态:** 生产可用
**包:** `com.unity.cinemachine` (Package Manager)

---

## 概述

**Cinemachine** 是 Unity 的虚拟相机系统，无需手动脚本即可实现专业的动态相机行为。

**使用 Cinemachine 用于：**
- 第三人称跟随相机
- 过场动画和影视
- 相机混合和过渡
- 动态取景
- 屏幕抖动和相机效果

---

## 基础设置

### 添加 Cinemachine Brain 到主相机

```csharp
// 创建第一个虚拟相机时会自动添加
// 或手动：Add Component > Cinemachine Brain
```

### 创建虚拟相机

`GameObject > Cinemachine > Cinemachine Camera`

这会创建一个带有默认设置的 **CinemachineCamera** GameObject。

---

## 虚拟相机组件

### CinemachineCamera

```csharp
using Unity.Cinemachine;

public class CameraController : MonoBehaviour {
    public CinemachineCamera virtualCamera;

    void Start() {
        virtualCamera.Priority = 10;
        virtualCamera.Follow = playerTransform;
        virtualCamera.LookAt = playerTransform;
    }
}
```

---

## 相机跟随模式

### 第三人称跟随

```csharp
// Inspector 配置:
// CinemachineCamera > Body > 3rd Person Follow
// - Shoulder Offset: (0.5, 0, 0)
// - Camera Distance: 5.0
// - Vertical Damping: 0.5
```

---

## 相机混合

### 基于优先级的混合

```csharp
public CinemachineCamera normalCamera; // Priority: 10
public CinemachineCamera aimCamera;    // Priority: 5

void StartAiming() {
    aimCamera.Priority = 15; // 现在活动
}

void StopAiming() {
    aimCamera.Priority = 5;
}
```

---

## 相机抖动

### Impulse Source

```csharp
using Unity.Cinemachine;

public class ExplosionShake : MonoBehaviour {
    public CinemachineImpulseSource impulseSource;

    void Explode() {
        impulseSource.GenerateImpulse();
    }
}
```

---

## 调试

### Cinemachine Debug

```
Window > Analysis > Cinemachine Debug
显示活动相机、混合信息、镜头质量
```

---

## 参考来源

- https://docs.unity3d.com/2023.2/Documentation/Manual cinematics.html
