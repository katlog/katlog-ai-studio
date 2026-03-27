# Codex Adapter

这里定义如何把 `agents/<name>/` 的宿主无关配置映射到 Codex 使用方式。

## 映射建议

- `AGENTS.md` -> Codex 的行为说明入口
- `SOUL.md` -> 补充人格和风格设定
- `PROFILE.yaml` -> 供脚本读取，选择 agent 元数据
- `SKILLS.yaml` -> 映射到可用 skill 清单和触发规则
- `TOOLS.yaml` -> 映射到工具使用策略

## 当前状态

仅定义适配边界，未实现自动装配脚本。
