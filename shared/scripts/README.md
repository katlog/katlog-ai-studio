# Shared Scripts

这里放工作空间级公共脚本。

## 当前约定

- 顶层 `install-skills.sh` 仅负责安装外部 skills
- 本仓库自己的 skills 位于 `skillhub/local/`
- 后续如果增加 workspace 初始化脚本，应优先放在这里

## 适合放在这里的脚本

- workspace bootstrap
- 配置校验
- agent 模板生成
- skillhub 同步和整理
