# Unity 2023.2 — Input 模块参考

**最后验证:** 2026-03-31

---

## 概述

Unity 2023.2 输入系统：
- **Input System 包**（推荐）：跨平台、可重新绑定、现代化
- **Legacy Input Manager**：已废弃，新项目避免使用

---

## Input System 包设置

### 安装

1. `Window > Package Manager`
2. 搜索 "Input System"
3. 安装包
4. 提示时重启 Unity

### 启用新的 Input System

`Edit > Project Settings > Player > Active Input Handling`:
- **Input System Package (New)** ✅ 推荐
- **Both**（迁移期间）

---

## Input Actions（推荐模式）

### 创建 Input Actions 资源

1. `Assets > Create > Input Actions`
2. 命名（例如 "PlayerControls"）
3. 打开资源，定义动作：

```
Action Maps:
  Gameplay
    Actions:
      - Move (Value, Vector2)
      - Jump (Button)
      - Fire (Button)
      - Look (Value, Vector2)
```

4. **生成 C# 类**: 在 Inspector 中勾选 "Generate C# Class"
5. 点击 "Apply"

### 使用生成的 Input 类

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerController : MonoBehaviour {
    private PlayerControls controls;

    void Awake() {
        controls = new PlayerControls();
        controls.Gameplay.Jump.performed += ctx => Jump();
    }

    void OnEnable() => controls.Enable();
    void OnDisable() => controls.Disable();

    void Update() {
        Vector2 move = controls.Gameplay.Move.ReadValue<Vector2>();
        transform.Translate(new Vector3(move.x, 0, move.y) * Time.deltaTime);
    }

    void Jump() {
        Debug.Log("Jump!");
    }
}
```

---

## 直接设备访问

### 键盘

```csharp
using UnityEngine.InputSystem;

void Update() {
    if (Keyboard.current.spaceKey.wasPressedThisFrame) { }
    if (Keyboard.current.spaceKey.wasReleasedThisFrame) { }
}
```

### 鼠标

```csharp
using UnityEngine.InputSystem;

void Update() {
    Vector2 mousePos = Mouse.current.position.ReadValue();
    Vector2 mouseDelta = Mouse.current.delta.ReadValue();
    if (Mouse.current.leftButton.wasPressedThisFrame) { }
}
```

### 游戏手柄

```csharp
using UnityEngine.InputSystem;

void Update() {
    Gamepad gamepad = Gamepad.current;
    if (gamepad == null) return;

    if (gamepad.buttonSouth.wasPressedThisFrame) { } // A/Cross
    Vector2 leftStick = gamepad.leftStick.ReadValue();
}
```

---

## 重新绑定（运行时键映射）

### 交互式重新绑定

```csharp
using UnityEngine.InputSystem;

public void RebindJumpKey() {
    var rebindOperation = controls.Gameplay.Jump.PerformInteractiveRebinding()
        .WithControlsExcluding("Mouse")
        .OnComplete(operation => {
            Debug.Log("Rebind complete");
            operation.Dispose();
        })
        .Start();
}
```

### 保存/加载绑定

```csharp
// 保存
string rebinds = controls.SaveBindingOverridesAsJson();
PlayerPrefs.SetString("InputBindings", rebinds);

// 加载
string rebinds = PlayerPrefs.GetString("InputBindings");
controls.LoadBindingOverridesFromJson(rebinds);
```

---

## 动作类型

### Button（按下/释放）

- 单次按下/释放
- 示例：Jump, Fire

### Value（连续）

- 连续值（float, Vector2）
- 示例：Move, Look, Aim

---

## 调试

- `Window > Analysis > Input Debugger` — 查看活动设备、输入值、动作状态

---

## 参考来源

- https://docs.unity3d.com/Packages/com.unity.inputsystem@1.7/manual/index.html
