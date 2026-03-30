# Registry

这里是 SkillHub 的统一技能账本。

目标很简单：无论是本地 skill 还是外部 skill，都用同一套最小字段回答这些问题：

- 这个 skill 现在是什么状态
- 它适合什么任务
- 它给哪些宿主使用
- 它通过什么方式维护和分发
- 它有没有更合适的替代品

## 维护原则

- 一个 skill 对应一个 `yaml`
- 本地 skill 和外部 skill 一视同仁
- `catalog/` 继续做人读的收藏与索引
- `mirrors/` 继续记录受控纳管的外部仓库
- `registry/` 负责技能状态、宿主目标和替代关系

## 最小字段

```yaml
name: superpowers
source: obra/superpowers
status: active
hosts:
  - claude
  - opencode
method: npx-skills
weight: heavy
fit: complex tasks
alternatives:
  - planning-with-files
  - task-planning
notes: "适合复杂任务；简单任务容易过度设计"
```

## 字段约定

- `name`：skill 名称
- `source`：来源，`local` 或 `owner/repo`
- `status`：`candidate | active | deprecated | archived`
- `hosts`：目标宿主，当前使用 `claude | codex | opencode`
- `method`：`npx-skills | manual | adapter-export`
- `weight`：`light | medium | heavy`
- `fit`：最适合的任务类型
- `alternatives`：替代 skill 列表
- `notes`：简短维护判断

## 维护方式

发现新 skill 时：

1. 新建一个 `registry/<skill>.yaml`
2. 默认先标记为 `candidate`
3. 写明 `hosts`、`method`、`weight`、`fit`
4. 评估通过后再改成 `active`

如果已有 skill 太重或更适合被替代：

1. 保留记录
2. 调整 `status`
3. 在 `alternatives` 和 `notes` 里写清楚替代关系
