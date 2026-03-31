# Claude Code 游戏工作室 — 游戏开发 Agent 架构

通过 48 个协调的 Claude Code 子 Agent 管理独立游戏开发。
每个 Agent 拥有特定领域，强调关注点分离和质量。

请始终用简体中文和我对话。如果要编写文档也需要是中文文档。

## 技术栈

- **引擎**: Unity 2023.2
- **语言**: C#
- **版本控制**: Git（基于主干的开发模式）
- **构建系统**: Unity Build Pipeline
- **资源管线**: Unity Asset Import Pipeline + Addressables

> **注意**: 引擎专家 Agent 适用于 Godot、Unity 和 Unreal，并有专门的子专家。使用与你的引擎匹配的组合。

## 项目结构

@.claude/docs/directory-structure.md

## 引擎版本参考

@docs/engine-reference/unity/VERSION.md

## 技术偏好

@.claude/docs/technical-preferences.md

## 协调规则

@.claude/docs/coordination-rules.md

## 协作协议

**用户驱动的协作，而非自主执行。**
每个任务遵循：**问题 -> 选项 -> 决策 -> 草稿 -> 批准**

- Agent 在使用 Write/Edit 工具之前必须问"我可以写入 [filepath] 吗？"
- Agent 必须先显示草稿或摘要，再请求批准
- 多文件更改需要明确批准整个变更集
- 未经用户指示不得提交

参见 `docs/COLLABORATIVE-DESIGN-PRINCIPLE.md` 获取完整协议和示例。

> **首次会话？** 如果项目没有配置引擎且没有游戏概念，
> 运行 `/start` 开始引导式入职流程。

## 编码标准

@.claude/docs/coding-standards.md

## 上下文管理

@.claude/docs/context-management.md
