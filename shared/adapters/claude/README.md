# Claude Adapter

这里定义如何把 `agents/<name>/` 的宿主无关配置映射到 Claude 类运行环境。

## 映射建议

- `AGENTS.md` -> 主行为规则
- `SOUL.md` -> 风格和角色设定
- `MEMORY.md` -> 长期偏好补充
- `SKILLS.yaml` -> skill 启用与限制
- `TOOLS.yaml` -> 工具和权限策略

## 当前状态

仅定义适配边界，未实现自动装配脚本。
