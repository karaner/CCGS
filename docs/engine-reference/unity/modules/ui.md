# Unity 2023.2 — UI 模块参考

**最后验证:** 2026-03-31

---

## 概述

Unity 2023.2 UI 系统：
- **UI Toolkit**（推荐）：现代化、高性能、HTML/CSS 风格
- **UGUI (Canvas)**：遗留系统，仍支持但新项目不推荐
- **IMGUI**：仅编辑器，运行时 UI 已废弃

---

## UI Toolkit（现代 UI）

### 设置 UI Document

1. 创建 UXML（UI 结构）:
   - `Assets > Create > UI Toolkit > UI Document`
2. 创建 USS（样式）:
   - `Assets > Create > UI Toolkit > StyleSheet`
3. 添加到场景:
   - `GameObject > UI Toolkit > UI Document`

### UXML（UI 结构）

```xml
<ui:UXML xmlns:ui="UnityEngine.UIElements">
    <ui:VisualElement class="container">
        <ui:Label text="主菜单" class="title" />
        <ui:Button name="play-button" text="开始游戏" />
        <ui:Button name="settings-button" text="设置" />
        <ui:Button name="quit-button" text="退出" />
    </ui:VisualElement>
</ui:UXML>
```

### USS（样式）

```css
.container {
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    background-color: rgb(30, 30, 30);
}

.title {
    font-size: 48px;
    color: white;
    margin-bottom: 20px;
}

Button {
    width: 200px;
    height: 50px;
    margin: 10px;
    font-size: 24px;
}
```

### C# 脚本

```csharp
using UnityEngine;
using UnityEngine.UIElements;

public class MainMenu : MonoBehaviour {
    void OnEnable() {
        var root = GetComponent<UIDocument>().rootVisualElement;

        var playButton = root.Q<Button>("play-button");
        playButton.clicked += OnPlayClicked;
    }

    void OnPlayClicked() {
        Debug.Log("开始游戏");
    }
}
```

---

## UGUI（遗留 Canvas UI）

### 基础设置

```csharp
// GameObject > UI > Canvas

// UI 元素:
// - Text (推荐使用 TextMeshPro)
```

### TextMeshPro（更好的文本渲染）

```csharp
using TMPro;

public TextMeshProUGUI scoreText;
scoreText.text = "分数: 100";
```

---

## 性能

### UI Toolkit 优势

- ✅ 更快的渲染（保留模式）
- ✅ 复杂 UI 性能更好
- ✅ 样式更简单（CSS 风格）

---

## 调试

### UI Toolkit Debugger

- `Window > UI Toolkit > Debugger` — 检查元素层级、样式、布局

---

## 参考来源

- https://docs.unity3d.com/2023.2/Documentation/Manual/UIElements.html
