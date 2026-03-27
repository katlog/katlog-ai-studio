# 私人 Agent Workspace 重构设计

**日期：** 2026-03-27  
**状态：** 已确认设计  
**范围：** `/Users/ninebot/Documents/myStudy/learnAI/katlog-ai-studio`

## 背景

当前仓库本质上是一个个人 Skills 集合：

- 顶层直接放置多个本地 skill 目录
- 顶层 `README.md` 同时承担仓库入口、技能清单、常用仓库收藏三种职责
- 仓库缺少 agent 维度的组织方式
- 缺少面向不同 agent 的人格、工具策略、记忆、技能装配配置

目标不是继续把它维护成单纯的 skill 仓库，而是升级为一个私人 agent 配置中心，同时保留个人 skill 收藏与自建能力库。

## 设计目标

本次重构需要满足以下目标：

1. `agents/` 和 `skillhub/` 并列存在，默认从 `agents/` 入口使用
2. 不创建顶层 `AGENTS.md`
3. 顶层 `README.md` 只做仓库导航与结构说明
4. 当前顶层 skill 目录整体迁移到 `skillhub/`
5. 当前 `README.md` 的 skill 清单职责迁移到 `skillhub/README.md`
6. agent 配置采用宿主无关格式，不直接绑定 Codex、Claude 或 OpenClaw
7. skill、agent 身份文件、外部 skill 收藏要明确分层，避免继续混放

## 推荐仓库结构

```text
katlog-ai-studio/
├── README.md
├── docs/
│   ├── architecture/
│   ├── conventions/
│   └── plans/
├── agents/
│   ├── architect/
│   ├── coder/
│   ├── reviewer/
│   ├── researcher/
│   ├── tech-research/
│   └── operator/
├── skillhub/
│   ├── README.md
│   ├── catalog/
│   │   ├── curated.md
│   │   ├── favorites.md
│   │   └── external-repos.md
│   ├── local/
│   │   ├── tech-diagram/
│   │   ├── feishu-docx-to-markdown/
│   │   ├── geektime-downloader/
│   │   └── import-md-to-wecom-doc/
│   ├── mirrors/
│   │   ├── superpowers/
│   │   ├── planning-with-files/
│   │   └── vercel-labs-skills/
│   └── drafts/
├── shared/
│   ├── templates/
│   │   ├── agent/
│   │   └── skill/
│   ├── schemas/
│   ├── prompts/
│   └── scripts/
└── legacy/
```

## 顶层目录职责

### `README.md`

只承担以下职责：

- 说明这个仓库是什么
- 说明主入口是 `agents/`
- 说明 `skillhub/` 是私人 skill 中心
- 说明 `shared/` 是公共模板和脚本区
- 说明文档与规划文件在 `docs/`

顶层 `README.md` 不再承担 skill 详情页职责。

### `agents/`

默认入口。这里定义“有哪些 agent”以及每个 agent 的人格、规则、工具策略、技能装配与记忆约束。

### `skillhub/`

私人 skill 中心，保留你原来仓库的三类语义：

- 个人创建的本地 skills
- 值得收藏的 skills 和仓库索引
- 常用外部 skill 仓库的镜像或受控纳管副本

### `shared/`

放公共模板、schema、通用 prompt、辅助脚本，不承载具体 agent 身份。

### `legacy/`

迁移过渡区。可选。如果迁移过程中不需要缓冲，可以最终删除。

## `skillhub/` 的内部设计

### `skillhub/catalog/`

这是收藏层，不是可执行 skill 目录。

- `curated.md`：精挑细选的 skills 和仓库
- `favorites.md`：你常用、偏爱的 skills
- `external-repos.md`：外部 skill 仓库索引、来源说明、适用场景

这样可以保留现有顶层 `README.md` 里“收藏夹”和“常用仓库”那部分语义。

### `skillhub/local/`

放你自己维护、你能直接改的 skills。当前顶层现有 skill 目录迁移到这里：

- `tech-diagram`
- `feishu-docx-to-markdown`
- `geektime-downloader`
- `import-md-to-wecom-doc`

### `skillhub/mirrors/`

放真正纳入私人体系管理的外部 skill 仓库。它们与“只是推荐的 GitHub 仓库链接”不同，意味着你愿意：

- 在本地保留副本
- 以受控方式升级
- 在 agent 装配时引用它们的能力

### `skillhub/drafts/`

实验型或未完成 skill。避免污染正式 skill 列表。

## `agents/` 的内部设计

每个 agent 目录固定为：

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

### `AGENTS.md`

放运行规则与工作约束：

- 行为优先级
- 任务边界
- 文件加载顺序
- 与其他 agent 的协作原则
- 对 `SOUL.md`、`SKILLS.yaml`、`TOOLS.yaml` 的解释方式

### `SOUL.md`

放人格与偏好：

- 语气
- 风格
- 决策偏好
- 长期角色设定
- 面对不确定性时的默认倾向

### `PROFILE.yaml`

结构化元数据，给脚本与适配器读取：

- `name`
- `role`
- `summary`
- `default_language`
- `tags`
- `host_targets`
- `entrypoints`

使用 YAML 而不是 Markdown，是为了后续可以自动校验、索引、适配宿主。

### `SKILLS.yaml`

定义这个 agent 能用哪些 skills，以及引用策略：

- 默认启用 skills
- 可选 skills
- 禁用 skills
- 优先级
- 触发条件说明

### `TOOLS.yaml`

定义工具策略：

- 允许使用的工具类别
- 网络访问策略
- 文件写权限偏好
- 高风险操作限制

### `MEMORY.md`

放长期偏好与上下文记忆：

- 用户偏好
- 项目长期约束
- 这个 agent 的稳定工作习惯

### `prompts/`

放场景化 prompt 片段，不把全部内容塞进主规则文件里。

## 结构约束

建议采用以下强约束：

1. `agents/` 下禁止出现 `SKILL.md`
2. `skillhub/` 下禁止出现 `SOUL.md`
3. 外部仓库推荐不得伪装成本地 skill，必须进入 `catalog/` 或 `mirrors/`
4. 只有真正参与装配和复用的技能才进入 `local/` 或 `mirrors/`
5. 顶层不放 agent 行为规则文件

## 第一批 Agent 清单

建议第一批落地 6 个 agent：

### `architect`

负责需求澄清、结构设计、模块划分、方案评审、规划类任务。

### `coder`

负责实现、重构、修 bug、补测试、落地脚本。

### `reviewer`

负责代码审查、风险识别、回归分析、测试缺口检查。

### `researcher`

负责外部信息检索、文档核对、工具调研、方案比选。

### `tech-research`

负责更偏技术深度的专项调研，例如：

- 框架选型
- 架构调研
- 第三方库比较
- 技术路线调研报告

这个角色和 `researcher` 的区别在于：`researcher` 更泛化，`tech-research` 更偏工程技术分析。

### `operator`

负责这个私人 agent 系统本身的维护：

- skill 安装与同步
- 模板生成
- 配置整理
- 目录维护
- 规范收口

## Agent 与 Skill 的默认映射建议

### `architect`

- `brainstorming`
- `writing-plans`
- `tech-diagram`

### `coder`

- `test-driven-development`
- `systematic-debugging`
- `verification-before-completion`

### `reviewer`

- `requesting-code-review`
- `receiving-code-review`

### `researcher`

- `find-skills`
- `openai-docs`
- `summarization`

### `tech-research`

- `find-skills`
- `summarization`
- `openai-docs`
- 后续可补充专项技术调研类本地 skill

### `operator`

- `skill-installer`
- `writing-skills`
- `skill-creator`

## 迁移原则

### 原则 1：先建骨架，再迁内容

先创建新目录和模板，再逐步搬迁现有内容，避免一次性移动导致引用关系失效。

### 原则 2：先迁移说明，再迁移目录

先改文档和路径约定，再挪现有 skill，有助于降低认知混乱。

### 原则 3：顶层只保留导航，不做业务承载

顶层要尽量稳定，不随着单个 skill 或单个 agent 的变化频繁调整。

### 原则 4：agent 与 skill 解耦

agent 只声明依赖 skill，不把 skill 内容复制进自己的目录。

## 不建议照搬 OpenClaw 的部分

可以借鉴 OpenClaw 对 agent 身份文件的拆分方式，但不建议直接照搬其宿主绑定方式。当前仓库应该先做成宿主无关的内部标准，再在未来增加不同宿主的适配层。

## 设计结论

本仓库将从“个人 skills 集合”升级为“私人 agent 工作空间”：

- `agents/` 和 `skillhub/` 并列
- 默认从 `agents/` 使用
- 顶层不设 `AGENTS.md`
- skill 收藏、镜像、本地 skill 分层管理
- agent 使用宿主无关配置文件

后续实施应按“目录骨架 -> README 拆分 -> skill 迁移 -> agent 模板 -> 首批 agent 初始化”的顺序进行。
