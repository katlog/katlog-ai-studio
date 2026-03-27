# Mirrors

这里是受控纳管的外部 skill 仓库清单。

与 `catalog/` 的区别是：

- `catalog/` 只负责收藏和索引
- `mirrors/` 表示这些外部 skills 已经进入本地体系的受控范围

第一版先不直接把外部仓库内容 vendor 进来，而是先保留镜像位和元数据，后续再决定：

- 是否真的 clone 到本仓库
- 是否只保留来源和版本信息
- 是否通过脚本做同步

## 当前纳管清单

- [superpowers](./superpowers/manifest.yaml)
- [planning-with-files](./planning-with-files/manifest.yaml)
- [vercel-labs-skills](./vercel-labs-skills/manifest.yaml)
