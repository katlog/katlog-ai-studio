# SkillHub

这里是我的私人 SkillHub。

它不只是“本地自建 skill 目录”，还同时承担三类职责：

- 维护我自己创建和长期维护的本地 skills
- 收藏、筛选、整理值得长期参考的 skill 资源
- 受控纳管常用的外部 skill 仓库
- 统一记录 skill 的状态、宿主目标和替代关系

## 目录结构

### `catalog/`

收藏与索引层，不是可执行 skill 目录。

- [catalog/curated.md](./catalog/curated.md)：精选 skill 与仓库
- [catalog/favorites.md](./catalog/favorites.md)：我最常用的 skills 和来源
- [catalog/external-repos.md](./catalog/external-repos.md)：外部仓库索引与说明

### `registry/`

技能账本层。这里不放 skill 实体，而是记录每个 skill 的：

- 当前状态
- 宿主目标
- 维护方式
- 使用负担
- 替代关系

详细说明见 [registry/README.md](./registry/README.md)。

### `local/`

本地 skills，放我自己维护的能力：

| Skill | 描述 |
|-------|------|
| [tech-diagram](./local/tech-diagram/) | 技术图表绘制工具，支持架构图、流程图、ER 图、时序图、动画演示等，基于 Mermaid 和 React Flow + dagre |
| [geektime-downloader](./local/geektime-downloader/) | 极客时间课程下载与整理，自动识别课程类型（图文/视频），支持视频转录和内容摘要 |
| [import-md-to-wecom-doc](./local/import-md-to-wecom-doc/) | 将 Markdown 导入企业微信文档，优先保证代码块、换行一致性和表格可读性 |
| [feishu-docx-to-markdown](./local/feishu-docx-to-markdown/) | 将飞书 docx 文档转换为本地 Markdown，保留图片、可复制表格和嵌入 Sheet 预览图 |

### `mirrors/`

受控纳管的外部 skill 仓库。这里的仓库意味着我会考虑：

- 在本地保留副本
- 以受控方式升级
- 将其中部分能力接入 agent 配置

当前镜像清单见 [mirrors/README.md](./mirrors/README.md)。

### `drafts/`

实验中、未定型或暂不公开使用的 skills。

## 当前本地 Skills

### 自建技能

| Skill | 描述 | 来源 |
|-------|------|------|
| **tech-diagram** | 技术图表绘制工具，支持架构图、流程图、ER 图、时序图、动画演示等 | 本地 |
| **geektime-downloader** | 极客时间课程下载与整理工具 | 本地 |
| **import-md-to-wecom-doc** | Markdown 导入企业微信文档并进行表格宽度修复的流程化技能 | 本地 |
| **feishu-docx-to-markdown** | 飞书 docx 转本地 Markdown，保留图片、可复制表格和 Sheet 预览图 | 本地 |

## 常用 Skills（外部）

以下是我长期会参考或安装的常用 Skills：

### 核心开发工具

| Skill | 描述 | 仓库 |
|-------|------|------|
| **superpowers** | 完整的 AI 编程工作流框架，包含头脑风暴、计划编写、TDD、代码审查等完整开发流程 | [obra/superpowers](https://github.com/obra/superpowers) |
| **planning-with-files** | 用 Markdown 文件作为 AI 外部记忆，解决上下文丢失问题，适合复杂项目 | [OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files) |
| **vercel-labs-skills** | Vercel 官方 Skills 工具集，包含 find-skills 等实用工具 | [vercel-labs/skills](https://github.com/vercel-labs/skills) |

### 文档处理

| Skill | 描述 | 来源 |
|-------|------|------|
| **pdf** | PDF 处理工具包，支持文本提取、表格提取、PDF 创建、合并/拆分、表单填写等 | Anthropic 官方 |
| **pptx** | PowerPoint 演示文稿创建、编辑和分析工具 | Anthropic 官方 |

### 前端开发

| Skill | 描述 | 来源 |
|-------|------|------|
| **frontend-design** | 创建独特的生产级前端界面，避免千篇一律的 AI 风格，支持 Web 组件、页面、应用等 | Anthropic 官方 |
| **remotion-best-practices** | Remotion 视频动画制作最佳实践 | 社区 |
| **webapp-testing** | Web 应用测试工具 | 社区 |

### 工具类

| Skill | 描述 | 来源 |
|-------|------|------|
| **skill-creator** | Anthropic 官方的 Skill 创建工具，帮助创建自定义 Skills | Anthropic 官方 |
| **changelog-generator** | 自动生成变更日志 | 社区 |
| **prototype-prompt-generator** | 原型提示词生成器 | 社区 |

## 快速安装

```bash
# 一键安装常用外部 Skills（从 GitHub）
./install-skills.sh

# 或者手动安装单个 Skill
cd ~/.claude/skills
git clone https://github.com/obra/superpowers.git
git clone https://github.com/OthmanAdi/planning-with-files.git
git clone https://github.com/vercel-labs/skills.git vercel-labs-skills
```

Anthropic 官方的 skills（如 `pdf`、`pptx`、`frontend-design`）需要通过官方渠道安装。
