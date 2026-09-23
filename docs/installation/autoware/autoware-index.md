---
status: new
---

# Autoware Index

[Autoware Index](https://autowarefoundation.github.io/autoware-index/) 是用于扩展 Autoware 的社区 ROS 2 功能包注册目录。
它记录了有哪些功能包、源代码所在位置，以及各功能包能否在最新 Autoware 版本上成功构建并通过测试。

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-magnifying-glass: 浏览功能包](https://autowarefoundation.github.io/autoware-index/){ .md-button style="margin: 5px" }
[:fa-cl-s fa-circle-plus: 注册你的功能包](https://autowarefoundation.github.io/autoware-index/register.html){ .md-button style="margin: 5px" }

</div>

[![Autoware Index 浏览网站](images/autoware-index/browse-site.png)](https://autowarefoundation.github.io/autoware-index/)

常规的[源码安装](source-installation.md)不需要使用 Autoware Index。
当你需要默认工作空间之外的社区功能包或额外功能包时，可以使用它：
从注册目录中选择功能包，根据所选内容生成 `.repos` 文件，再像导入其他 [repos 文件](../../design/repos-files.md)一样使用 `vcs import` 导入。

有两种方式可以生成该文件：

- [`aw-index-cli`](https://github.com/autowarefoundation/aw-index-cli)：根据注册目录生成文件的命令行工具。
- [浏览网站](https://autowarefoundation.github.io/autoware-index/)：其中的 **Repos builder** 可直接在浏览器中生成并下载同样的文件。

<a id="using-aw-index-cli"></a>

## 使用 `aw-index-cli`

1. 使用 [pipx](https://pipx.pypa.io/) 安装命令行工具。

   ```bash
   pipx install aw-index-cli   # first install
   pipx upgrade aw-index-cli   # update to the latest release
   ```

2. 在 Autoware 工作空间根目录（克隆得到的 `autoware` 目录）中生成 repos 文件。
   默认情况下，所选内容会写入 `repositories/autoware-index.repos`。

   ```bash
   cd autoware
   aw-index-cli compose --rosdistro jazzy --packages <package_name>
   ```

   你也可以通过注册目录中的键选择整个仓库，或按标签筛选：

   ```bash
   # Pull in a whole repository entry by its registry key.
   aw-index-cli compose --rosdistro jazzy --repository <repository_name>

   # Select by tags. A package matches if it carries any of the listed tags.
   aw-index-cli compose --rosdistro jazzy --tags sensing perception
   ```

   同时使用 `--packages`、`--repository` 和 `--tags` 时，各筛选条件之间为逻辑与关系。
   `--rosdistro` 的值应与你使用的 ROS 2 发行版一致。
   可用的发行版列在 [autoware-index](https://github.com/autowarefoundation/autoware-index/tree/main/distributions) 仓库中。

3. 按常规方式导入仓库并构建工作空间。

   ```bash
   vcs import src < repositories/autoware-index.repos
   rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```

   > ⚠️ 导入的仓库也可能包含未在索引中注册的功能包。
   > 如果只想构建所选功能包，请使用 `colcon build --packages-up-to <package_name>` 限定构建范围。

!!! note

    `autoware-index.repos` 是自动生成的文件。
    请重新运行 `aw-index-cli compose` 来更新它，不要手动编辑。
    由于所选内容因用户而异，`autoware` 仓库中的 git 会忽略此文件。

<a id="checking-your-workspace"></a>

### 检查工作空间

`aw-index-cli check` 会将生成的 `autoware-index.repos` 文件与注册目录和最新验证结果进行比较。
在工作空间根目录中运行该命令，它会自动查找此文件。

```bash
aw-index-cli check
```

当所有所选功能包均通过验证且与注册目录一致时，退出码为 `0`；如果某个功能包验证失败、与注册目录不一致或已被移除，退出码则为 `1`。
因此，可以在 `vcs import` 之后将其用作 CI 检查关卡。

<a id="listing-packages"></a>

### 列出功能包

要在终端中查看已注册的功能包及其最新验证状态，请运行：

```bash
aw-index-cli list --rosdistro jazzy
```

<a id="using-the-browse-site"></a>

## 使用浏览网站

如果你不想安装额外工具：

1. 在[浏览网站](https://autowarefoundation.github.io/autoware-index/)上选择功能包。
2. 打开 **Repos builder** 面板，将所选内容下载为 `autoware-index.repos`。
3. 将文件移入工作空间的 `repositories` 目录并导入：

   ```bash
   cd autoware
   mv ~/Downloads/autoware-index.repos repositories/
   vcs import src < repositories/autoware-index.repos
   ```

<a id="registering-your-packages"></a>

## 注册功能包

要通过索引提供你自己的功能包，请在 [autoware-index](https://github.com/autowarefoundation/autoware-index) 仓库中注册：

- 使用[注册引导页面](https://autowarefoundation.github.io/autoware-index/register.html)。
  该页面会为你编写注册条目，执行与拉取请求检查关卡相同的检查，并代你创建拉取请求。
- 也可以手动注册：派生该仓库，为你支持的每个发行版在 `distributions/<distro>.yaml` 的 `repositories:` 下添加一个条目，然后创建拉取请求。

注册内容合并后，CI 会基于最新 Autoware 版本构建并测试每个已注册的功能包，并在浏览网站上发布结果。
使用 `branch` 引用注册的功能包每晚都会重新验证，而使用 `tag` 和 `sha` 引用的功能包则保持固定版本。

注册格式、标签词汇和验证规则请参阅[贡献指南](https://github.com/autowarefoundation/autoware-index/blob/main/CONTRIBUTING.md)。
