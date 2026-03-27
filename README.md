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

当前已初始化的 agents：

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

当前结构包括：

- `skillhub/local/`：本地 skills
- `skillhub/catalog/`：收藏、精选、外部索引
- `skillhub/mirrors/`：受控纳管的外部 skill 仓库清单

详细说明见 [skillhub/README.md](./skillhub/README.md)。

### `shared/`

放公共模板、schema、prompt 片段、宿主适配骨架和维护脚本，不承载具体 agent 身份。

当前已落地的公共层包括：

- `shared/templates/`：agent / skill 模板
- `shared/scripts/`：工作空间级脚本说明
- `shared/adapters/`：Codex / Claude / OpenClaw 的适配层骨架

### `docs/`

放设计文档、约定和实施计划：

- [docs/plans/2026-03-27-private-agent-workspace-design.md](./docs/plans/2026-03-27-private-agent-workspace-design.md)
- [docs/plans/2026-03-27-private-agent-workspace-implementation-plan.md](./docs/plans/2026-03-27-private-agent-workspace-implementation-plan.md)

## 当前状态

当前仓库已经完成第一轮重构，核心状态如下：

- 本地 skills 已迁移到 `skillhub/local/`
- 顶层 `README.md` 只保留仓库导航职责
- 6 个首批 agent 已初始化
- agent 配置采用宿主无关格式
- `mirrors/` 和 `adapters/` 已建立第一版骨架

## 当前本地 Skills

- `tech-diagram`
- `feishu-docx-to-markdown`
- `geektime-downloader`
- `import-md-to-wecom-doc`

## 当前外部 Mirrors

- `superpowers`
- `planning-with-files`
- `vercel-labs-skills`

## 适配层说明

`shared/adapters/` 的职责是把 `agents/<name>/` 下的宿主无关配置映射到具体宿主，而不是把宿主规则直接写回 agent 原始定义。

当前已经预留：

- `shared/adapters/codex/`
- `shared/adapters/claude/`
- `shared/adapters/openclaw/`

这一层目前还是结构和边界说明，真正可执行的导出脚本还没有开始实现。

## 设计原则

- `agents/` 和 `skillhub/` 并列存在
- 默认从 `agents/` 使用
- 不创建顶层 `AGENTS.md`
- `skillhub/` 只放 skill 与 skill 索引，不放 agent 身份文件
- agent 的人格、规则、工具配置与长期记忆收敛到各自目录下
