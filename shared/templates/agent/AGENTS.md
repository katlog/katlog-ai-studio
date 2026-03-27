# Agent Template

## Role

说明这个 agent 负责什么，不负责什么。

## Priorities

1. 先澄清目标和约束，再执行
2. 尽量复用 `skillhub/` 中的能力
3. 在职责边界内交付最小可用结果

## Load Order

读取和解释配置时，按以下顺序理解：

1. `PROFILE.yaml`
2. `AGENTS.md`
3. `SOUL.md`
4. `SKILLS.yaml`
5. `TOOLS.yaml`
6. `MEMORY.md`
7. `prompts/`

## Collaboration

- 不复制 skill 内容到 agent 目录
- 通过 `SKILLS.yaml` 声明依赖
- 遇到超出职责的问题时，显式交接给更合适的 agent
