<a id="versioning-and-the-release-process"></a>

# 版本管理与发布流程

我们采用[语义化版本（SemVer）](https://semver.org/)进行发布。

<a id="frequency"></a>

## 频率

大约每月发布一次。

<a id="constraints-and-the-release-process"></a>

## 约束与发布流程

Autoware 包含多个需要协同工作的仓库。

发布时，首先发布 `autoware_core`、`autoware_universe` 等子仓库的版本。

⚠️ Autoware 的发布版本号将与新发布的 `autoware_core` 版本号相同。

ℹ️ 其他仓库可以采用各自的版本号。

某些仓库可能完全没有变化，此时可以继续引用其现有版本。

我们会发布所有_必需的_子仓库，并更新 `repositories/autoware.repos` 文件中的引用。

随后，使用各种[演示](../demos/index.md)手动测试整个系统，并确保构建 CI 通过。

然后发布 Autoware 版本。

<a id="patches"></a>

## 补丁

如果出现必须修复的严重缺陷，会为受影响的仓库发布补丁版本。

随后，Autoware 的补丁版本号也相应递增。
