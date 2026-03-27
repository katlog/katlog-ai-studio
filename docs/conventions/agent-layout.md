# Agent Layout Convention

本仓库采用宿主无关的 agent 目录结构。

## 标准结构

```text
agents/<agent>/
├── AGENTS.md
├── SOUL.md
├── PROFILE.yaml
├── SKILLS.yaml
├── TOOLS.yaml
├── MEMORY.md
└── prompts/
```

## 文件职责

- `AGENTS.md`：行为规则、执行边界、优先级和协作方式
- `SOUL.md`：人格、风格、语气和判断倾向
- `PROFILE.yaml`：结构化元数据，便于脚本和适配器读取
- `SKILLS.yaml`：skill 依赖、默认启用项和触发规则
- `TOOLS.yaml`：工具使用策略和高风险限制
- `MEMORY.md`：长期偏好与稳定约束
- `prompts/`：场景化 prompt 片段

## 约束

1. `agents/` 下不放 `SKILL.md`
2. `skillhub/` 下不放 `SOUL.md`
3. 只有真正要被复用的能力才进入 `skillhub/local/` 或 `skillhub/mirrors/`
4. 外部仓库索引统一进入 `skillhub/catalog/`
5. agent 通过 `SKILLS.yaml` 引用能力，不复制能力内容

## 为什么 `PROFILE` 用 YAML

`PROFILE.yaml` 用于结构化字段，例如：

- `name`
- `role`
- `summary`
- `default_language`
- `tags`
- `host_targets`

这样后续可以：

- 扫描 agent 清单
- 自动校验配置完整性
- 为 Codex、Claude、OpenClaw 生成适配层

如果用 Markdown，人工阅读友好，但自动装配和校验成本会更高。
