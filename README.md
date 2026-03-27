# katlog-ai-studio

这是我的私人 Agent Workspace。

这个仓库不再只是一个 skills 集合，而是一个同时维护：

- 私人 agent 配置
- 私人 skillhub
- 共享模板、脚本和约定

的工作空间。

## 目录导航

### `agents/`

默认入口。这里定义不同 agent 的角色、人格、技能装配、工具策略和长期记忆。

计划中的首批 agents：

- `architect`
- `coder`
- `reviewer`
- `researcher`
- `tech-research`
- `operator`

### `skillhub/`

私人技能中心。这里保留三类内容：

- 我自己维护的本地 skills
- 值得收藏和长期参考的 skill 资源
- 纳入本地体系管理的外部 skill 仓库

详细说明见 [skillhub/README.md](./skillhub/README.md)。

### `shared/`

放公共模板、schema、prompt 片段、宿主适配骨架和维护脚本，不承载具体 agent 身份。

### `docs/`

放设计文档、约定和实施计划：

- [docs/plans/2026-03-27-private-agent-workspace-design.md](./docs/plans/2026-03-27-private-agent-workspace-design.md)
- [docs/plans/2026-03-27-private-agent-workspace-implementation-plan.md](./docs/plans/2026-03-27-private-agent-workspace-implementation-plan.md)

## 当前状态

当前仓库正在从“个人 skills 集合”迁移为“私人 agent 工作空间”：

- 顶层 skill 目录将逐步迁移到 `skillhub/local/`
- 顶层 `README.md` 只保留导航职责
- agent 配置采用宿主无关格式，后续再适配具体运行环境

## 设计原则

- `agents/` 和 `skillhub/` 并列存在
- 默认从 `agents/` 使用
- 不创建顶层 `AGENTS.md`
- `skillhub/` 只放 skill 与 skill 索引，不放 agent 身份文件
- agent 的人格、规则、工具配置与长期记忆收敛到各自目录下
