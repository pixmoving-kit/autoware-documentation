<a id="fixing-dependent-package-versions"></a>

# 固定依赖软件包版本

Autoware 在 `repositories/autoware.repos` 中管理依赖软件包的版本。
例如，假设您在 autoware_universe 中创建了一个分支并添加新功能。
如果在创建 autoware_universe 分支后使用 `vcs pull` 更新其他依赖，正在开发的 autoware_universe 版本可能与其他依赖不一致，导致整个 Autoware 构建失败。
建议在开始开发时执行以下命令，保存依赖软件包的版本。

```bash
vcs export src --exact > repositories/my_autoware.repos
```
