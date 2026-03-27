# Host Adapters

本仓库中的 agent 配置采用宿主无关结构。

## 目标

让同一套 agent 定义可以被不同宿主适配，而不是直接耦合到某一个运行环境。

目标宿主包括：

- Codex
- Claude
- OpenClaw

## 边界

宿主适配层只负责映射，不负责改写原始 agent 定义。

原始定义位于：

- `agents/<name>/AGENTS.md`
- `agents/<name>/SOUL.md`
- `agents/<name>/PROFILE.yaml`
- `agents/<name>/SKILLS.yaml`
- `agents/<name>/TOOLS.yaml`
- `agents/<name>/MEMORY.md`

## 适配层应该做什么

- 把结构化配置映射成宿主可识别的格式
- 处理宿主的工具能力差异
- 处理宿主的 prompt 装配方式差异
- 处理宿主的文件发现或加载顺序差异

## 适配层不应该做什么

- 修改 agent 的长期人格
- 修改 skill 依赖关系
- 在宿主侧偷偷增加行为规则

## 当前状态

目前仓库只定义了内部标准，还没有正式实现宿主适配脚本。

这意味着：

- 现在的目录设计已经为多宿主复用做准备
- 具体接入 Codex、Claude、OpenClaw 时，需要再增加一层 adapter
