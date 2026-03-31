# Unity 2023.2 — 可选包和系统

**最后验证:** 2026-03-31

本文档索引 Unity 2023.2 中常用的可选包和系统。

---

## 如何使用本指南

**✅ 详细文档可用** — 有完整使用指南
**🟡 简要概述** — 链接到官方文档
**📦 需要安装** — 通过 Package Manager 安装

---

## 推荐包 (详细文档)

### ✅ Addressables
- **用途:** 高级资源管理（异步加载、远程内容、内存控制）
- **何时使用:** 大型项目、DLC、远程内容传递
- **状态:** 推荐所有项目使用
- **包:** `com.unity.addressables`
- **官方文档:** https://docs.unity3d.com/Packages/com.unity.addressables@1.21/manual/index.html

---

### ✅ Input System
- **用途:** 现代输入处理（可重新绑定、跨平台）
- **何时使用:** 所有新项目
- **状态:** 生产可用
- **包:** `com.unity.inputsystem`
- **官方文档:** https://docs.unity3d.com/Packages/com.unity.inputsystem@1.7/manual/index.html

---

### ✅ TextMeshPro
- **用途:** 高质量文本渲染
- **何时使用:** 所有项目的 UI 文本
- **状态:** 推荐替代旧版 Text 组件
- **包:** `com.unity.textmeshpro` (内置)

---

## 其他生产可用的包 (简要概述)

### 🟡 Visual Effect Graph (VFX Graph)
- **用途:** GPU 加速粒子系统
- **何时使用:** 大规模 VFX、火焰、烟雾、魔法、爆炸
- **状态:** 生产可用
- **包:** `com.unity.visualeffectgraph` (需要 URP/HDRP)
- **官方文档:** https://docs.unity3d.com/Packages/com.unity.visualeffectgraph@14.0/manual/index.html

---

### 🟡 Shader Graph
- **用途:** 可视化着色器编辑器
- **何时使用:** 自定义着色器
- **状态:** 生产可用
- **包:** `com.unity.shadergraph` (URP/HDRP)
- **官方文档:** https://docs.unity3d.com/Packages/com.unity.shadergraph@14.0/manual/index.html

---

### 🟡 Timeline
- **用途:** 影视序列（过场动画、脚本事件）
- **何时使用:** 叙事驱动游戏、过场动画
- **状态:** 生产可用
- **包:** `com.unity.timeline` (内置)
- **官方文档:** https://docs.unity3d.com/Packages/com.unity.timeline@1.7/manual/index.html

---

### 🟡 Cinemachine
- **用途:** 虚拟相机系统
- **何时使用:** 第三人称游戏、影视、复杂相机行为
- **状态:** 生产可用
- **包:** `com.unity.cinemachine`
- **官方文档:** https://docs.unity3d.com/Packages/com.unity.cinemachine@2.9/manual/index.html

---

### 🟡 ProBuilder
- **用途:** 编辑器内 3D 建模
- **何时使用:** 快速原型、关卡灰盒
- **状态:** 生产可用
- **包:** `com.unity.probuilder`
- **官方文档:** https://docs.unity3d.com/Packages/com.unity.probuilder@5.0/manual/index.html

---

## 独立游戏推荐选择

**2D 卡牌游戏** 建议使用:
- Addressables (资源管理)
- Input System (输入)
- TextMeshPro (UI 文本)
- UI Toolkit 或 UGUI (UI)
- Visual Effect Graph (卡牌特效)

---

## 快速决策指南

**需要虚拟相机** → **Cinemachine**
**需要异步资源加载 / DLC** → **Addressables**
**需要现代输入** → **Input System**
**需要 GPU 粒子** → **Visual Effect Graph**
**需要可视化着色器** → **Shader Graph**
**需要过场动画** → **Timeline**
**需要关卡原型** → **ProBuilder**

---

**最后更新:** 2026-03-31
**引擎版本:** Unity 2023.2
