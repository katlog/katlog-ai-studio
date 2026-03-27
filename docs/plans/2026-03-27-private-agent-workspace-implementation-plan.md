# 私人 Agent Workspace Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 将当前 `katlog-ai-studio` 仓库从“个人 skills 集合”重构为“私人 agent 工作空间”，建立 `agents/` 与 `skillhub/` 并列结构，并完成首批目录、文档与模板落地。  
**Architecture:** 以 `agents/` 和 `skillhub/` 为双核心目录。顶层只保留导航，原有顶层 skills 下沉到 `skillhub/local/`，agent 使用宿主无关配置文件进行装配。  
**Tech Stack:** Markdown, YAML, shell scripts, existing local skill directories

---

### Task 1: 建立新目录骨架

**Files:**
- Create: `agents/`
- Create: `skillhub/catalog/`
- Create: `skillhub/local/`
- Create: `skillhub/mirrors/`
- Create: `skillhub/drafts/`
- Create: `shared/templates/agent/`
- Create: `shared/templates/skill/`
- Create: `shared/schemas/`
- Create: `shared/prompts/`
- Create: `shared/scripts/`
- Create: `docs/architecture/`
- Create: `docs/conventions/`

**Step 1: 创建目录**

Run: `mkdir -p agents skillhub/catalog skillhub/local skillhub/mirrors skillhub/drafts shared/templates/agent shared/templates/skill shared/schemas shared/prompts shared/scripts docs/architecture docs/conventions`
Expected: 所有目录成功创建，无报错

**Step 2: 验证目录结构**

Run: `find agents skillhub shared docs -maxdepth 2 -type d | sort`
Expected: 输出包含上述目录

**Step 3: Commit**

```bash
git add agents skillhub shared docs
git commit -m "chore: scaffold private agent workspace structure"
```

### Task 2: 重写顶层 README

**Files:**
- Modify: `README.md`
- Reference: `docs/plans/2026-03-27-private-agent-workspace-design.md`

**Step 1: 编写新的顶层 README 内容**

顶层 `README.md` 应覆盖：

- 仓库定位
- 主入口为 `agents/`
- `skillhub/` 是私人技能中心
- `shared/` 是模板和公共脚本
- 文档在 `docs/`

**Step 2: 移除旧的技能清单主体**

旧 README 中的大量 skill 介绍、外部资源列表和仓库推荐移至 `skillhub/README.md` 与 `skillhub/catalog/`

**Step 3: 验证 README 导航是否清晰**

Run: `sed -n '1,220p' README.md`
Expected: 内容主要是仓库结构导航，而不是技能详情页

**Step 4: Commit**

```bash
git add README.md
git commit -m "docs: refocus top-level readme on workspace navigation"
```

### Task 3: 建立 `skillhub/README.md` 并迁移现有 README 的 skill 语义

**Files:**
- Create: `skillhub/README.md`
- Create: `skillhub/catalog/curated.md`
- Create: `skillhub/catalog/favorites.md`
- Create: `skillhub/catalog/external-repos.md`
- Reference: `README.md`

**Step 1: 新建 `skillhub/README.md`**

内容应覆盖：

- `skillhub` 的职责
- `catalog/`、`local/`、`mirrors/`、`drafts/` 的区别
- 本地 skill 清单入口
- 常用 skill 仓库入口

**Step 2: 将旧 README 中“收藏 / 推荐 / 常用仓库”信息拆分到 `catalog/`**

- `curated.md`：值得收藏的 skill 和仓库
- `favorites.md`：最常用的 skill 与来源
- `external-repos.md`：外部仓库说明

**Step 3: 验证 `skillhub/README.md` 的定位**

Run: `sed -n '1,260p' skillhub/README.md`
Expected: 清楚说明 `skillhub` 是私人 skill 中心

**Step 4: Commit**

```bash
git add skillhub
git commit -m "docs: split skillhub catalog from top-level readme"
```

### Task 4: 迁移现有本地 skills 到 `skillhub/local/`

**Files:**
- Move: `tech-diagram` -> `skillhub/local/tech-diagram`
- Move: `feishu-docx-to-markdown` -> `skillhub/local/feishu-docx-to-markdown`
- Move: `geektime-downloader` -> `skillhub/local/geektime-downloader`
- Move: `import-md-to-wecom-doc` -> `skillhub/local/import-md-to-wecom-doc`
- Modify: internal docs or references if paths change

**Step 1: 移动目录**

Run: `mv tech-diagram skillhub/local/ && mv feishu-docx-to-markdown skillhub/local/ && mv geektime-downloader skillhub/local/ && mv import-md-to-wecom-doc skillhub/local/`
Expected: 顶层不再直接包含这些 skill 目录

**Step 2: 搜索旧路径引用**

Run: `rg -n "tech-diagram|feishu-docx-to-markdown|geektime-downloader|import-md-to-wecom-doc" .`
Expected: 找出需要更新的 README、脚本或引用

**Step 3: 修正路径引用**

更新所有仍然指向旧顶层路径的文档与脚本。

**Step 4: 验证本地 skill 目录完整**

Run: `find skillhub/local -maxdepth 2 -name SKILL.md | sort`
Expected: 输出 4 个本地 skill 的 `SKILL.md`

**Step 5: Commit**

```bash
git add skillhub README.md
git commit -m "refactor: move local skills under skillhub/local"
```

### Task 5: 为 agent 建立模板与规范文件

**Files:**
- Create: `shared/templates/agent/AGENTS.md`
- Create: `shared/templates/agent/SOUL.md`
- Create: `shared/templates/agent/PROFILE.yaml`
- Create: `shared/templates/agent/SKILLS.yaml`
- Create: `shared/templates/agent/TOOLS.yaml`
- Create: `shared/templates/agent/MEMORY.md`
- Create: `docs/conventions/agent-layout.md`

**Step 1: 编写 agent 模板**

模板应体现固定文件职责，并留出占位字段。

**Step 2: 编写目录约定文档**

`docs/conventions/agent-layout.md` 应说明：

- 哪些文件属于 agent 身份
- 哪些配置使用 YAML
- 哪些内容不能放进 `skillhub`

**Step 3: 验证模板可读性**

Run: `find shared/templates/agent -maxdepth 1 -type f | sort`
Expected: 输出 6 个 agent 模板文件

**Step 4: Commit**

```bash
git add shared docs/conventions
git commit -m "docs: add agent templates and layout conventions"
```

### Task 6: 初始化第一批 agent 目录

**Files:**
- Create: `agents/architect/*`
- Create: `agents/coder/*`
- Create: `agents/reviewer/*`
- Create: `agents/researcher/*`
- Create: `agents/tech-research/*`
- Create: `agents/operator/*`

**Step 1: 基于模板创建 agent 目录**

每个 agent 至少包含：

- `AGENTS.md`
- `SOUL.md`
- `PROFILE.yaml`
- `SKILLS.yaml`
- `TOOLS.yaml`
- `MEMORY.md`
- `prompts/`

**Step 2: 填充最小可用内容**

为每个 agent 写入：

- 角色职责
- 默认语言
- 默认 skill 依赖
- 工具约束

**Step 3: 验证 agent 目录完整**

Run: `find agents -maxdepth 2 | sort`
Expected: 六个 agent 目录均包含完整文件

**Step 4: Commit**

```bash
git add agents
git commit -m "feat: initialize first batch of personal agents"
```

### Task 7: 调整安装脚本与维护脚本

**Files:**
- Modify: `install-skills.sh`
- Create: `shared/scripts/README.md`
- Optional Create: `shared/scripts/bootstrap-workspace.sh`

**Step 1: 评估现有脚本定位**

当前 `install-skills.sh` 只负责安装外部 skills，需要重新定义为：

- 安装外部 skills
- 或仅迁移为 `shared/scripts/`

**Step 2: 选择以下两种方案之一**

方案 A：保留顶层 `install-skills.sh`，只用于外部 skill 安装  
方案 B：将其迁入 `shared/scripts/`，顶层只保留轻量入口

推荐方案 B。

**Step 3: 验证脚本说明**

Run: `sed -n '1,220p' install-skills.sh`
Expected: 脚本职责与新结构一致，或者已有替代入口说明

**Step 4: Commit**

```bash
git add install-skills.sh shared/scripts
git commit -m "chore: align workspace scripts with new layout"
```

### Task 8: 添加宿主适配预留说明

**Files:**
- Create: `docs/architecture/host-adapters.md`

**Step 1: 编写适配说明**

说明当前配置是宿主无关的内部标准，并预留未来适配：

- Codex
- Claude
- OpenClaw

**Step 2: 明确适配层职责**

适配层只做映射，不改写原始 agent 配置。

**Step 3: Commit**

```bash
git add docs/architecture/host-adapters.md
git commit -m "docs: define host adapter boundary for agent configs"
```

### Task 9: 验证整体结构

**Files:**
- Verify: `README.md`
- Verify: `skillhub/README.md`
- Verify: `agents/`
- Verify: `skillhub/local/`
- Verify: `shared/templates/agent/`

**Step 1: 运行结构检查**

Run: `find . -maxdepth 3 \( -path './.git' -o -path './.claude' -o -path './.playwright-mcp' \) -prune -o -print | sort`
Expected: 输出符合新结构

**Step 2: 运行路径引用检查**

Run: `rg -n "^\./(tech-diagram|feishu-docx-to-markdown|geektime-downloader|import-md-to-wecom-doc)|\btech-diagram/\b|\bfeishu-docx-to-markdown/\b" README.md skillhub docs shared agents`
Expected: 不再出现错误的旧顶层引用

**Step 3: 人工审查**

检查：

- 顶层 `README.md` 是否足够克制
- `skillhub/README.md` 是否承接了旧 skill 索引职责
- agent 文件是否职责清晰

**Step 4: Commit**

```bash
git add .
git commit -m "chore: verify private agent workspace structure"
```

## 注意事项

- 不要创建顶层 `AGENTS.md`
- 不要把 `SOUL.md` 放进 `skillhub`
- 不要把外部仓库推荐混成“本地 skill 目录”
- 迁移 skill 目录后必须统一修正文档引用
- 第一版先建立宿主无关配置，不急着做运行时适配脚本

## 建议执行顺序

1. 目录骨架
2. 顶层 README 重写
3. `skillhub` README 和 catalog 拆分
4. 本地 skill 迁移
5. agent 模板
6. 首批 agent 初始化
7. 脚本对齐
8. 宿主适配预留
9. 最终验证
