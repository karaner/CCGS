# 技术偏好

<!-- 由 /setup-engine 填充。根据用户在开发过程中所做的决策更新。 -->
<!-- 所有 agents 参考此文件以获取项目特定的标准和约定。 -->

## 引擎和语言

- **引擎**: Unity 2023.2
- **语言**: C#
- **渲染**: 2D（主要），后续可能 3D
- **物理**: Unity Physics (2D)

## 命名规范

- **类**: PascalCase（例如 `PlayerController`）
- **公开字段/属性**: PascalCase（例如 `MoveSpeed`）
- **私有字段**: _camelCase（例如 `_moveSpeed`）
- **方法**: PascalCase（例如 `TakeDamage()`）
- **文件**: PascalCase 匹配类名（例如 `PlayerController.cs`）
- **常量**: PascalCase 或 UPPER_SNAKE_CASE
- **场景/预制体**: PascalCase（例如 `BattleScene`、`CardPrefab`）
- **信号/事件**: PascalCase（例如 `OnCardPlayed`、`HealthChanged`）

## 性能预算

- **目标帧率**: 60fps
- **帧预算**: 16.6ms
- **绘制调用**: 每场景 < 100（使用对象池）
- **内存上限**: 初始加载 < 512MB

> **注意**: 这些是建议值。如需调整或确认，请告知。

## 测试

- **框架**: NUnit + Unity Test Framework
- **最低覆盖率**: 核心战斗系统和卡牌效果系统需要单元测试
- **必需测试**: 平衡公式、游戏机制

## 禁止的模式

- 不要使用 `GameObject.Find()` 或 `FindObjectOfType()` — 使用依赖注入或事件系统
- 不要在 Update() 中每帧分配内存 — 使用对象池
- 不要硬编码卡牌数值 — 使用 ScriptableObject 数据资产

## 允许的库/插件

- [尚未配置 — 批准依赖后添加]

## 架构决策日志

<!-- 快速参考，链接到 docs/architecture/ 中的完整 ADR -->
- [尚无 ADR — 使用 /architecture-decision 创建]
