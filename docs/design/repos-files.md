<a id="repositoriesrepos-files"></a>

# `repositories/*.repos` 文件

Autoware 采用元仓库方式管理**多个 Git 仓库**。

`autoware` 仓库本身**不包含完整源代码**，而是引用多个独立维护的仓库。

这些文件位于 [autoware/repositories](https://github.com/autowarefoundation/autoware/blob/main/repositories) 目录下。

<div class="grid cards" markdown>

- `autoware.repos`

  ***

  保存必需的 Autoware 仓库引用。

- `autoware-nightly.repos`

  ***

  将 `autoware.repos` 中的部分仓库设置为用于开发的 main 分支。

- `tools.repos`

  ***

  主要保存 [autowarefoundation/autoware_tools](https://github.com/autowarefoundation/autoware_tools.git) 仓库的引用。

- `tools-nightly.repos`

  ***

  将工具仓库设置为 main 分支。

- `simulator.repos`

  ***

  保存 [tier4/scenario_simulator_v2](https://github.com/tier4/scenario_simulator_v2.git) 仓库的引用。

- `extra-packages.repos`

  ***

  保存可选仓库。目前包含 [tier4/pacmod_interface](https://github.com/tier4/pacmod_interface.git) 和 [tier4/tamagawa_imu_driver](https://github.com/tier4/tamagawa_imu_driver.git) 的引用。

- `autoware-index.repos`

  ***

  在本地根据 [Autoware Index](../installation/autoware/autoware-index.md) 社区功能包注册目录生成。它因用户而异，不由 git 跟踪。

</div>

<a id="how-to-use-repos-files"></a>

## 使用 `.repos` 文件

<a id="prerequisites"></a>

### 前提条件

Autoware 使用 [vcs2l](https://github.com/ros-infrastructure/vcs2l) 创建工作空间。

可以通过 `sudo apt install python3-vcs2l` 安装。

<a id="configurations"></a>

### 配置

请始终先导入 `autoware.repos`，因为其中包含必需的仓库。

```bash
vcs import src < repositories/autoware.repos
```

对于大多数情况，这已经足够。

如果需要使用仓库的 nightly 版本，还应导入 `autoware-nightly.repos` 文件。（必须先导入 `autoware.repos`。）

```bash
vcs import src < repositories/autoware-nightly.repos
```

如果需要使用场景仿真器或工具等其他仓库，请导入对应的 `.repos` 文件。

<a id="managing-repositories"></a>

### 管理仓库

通常，以下命令足以更新所有仓库。

```bash
vcs pull src
```

但如果某些仓库存在未提交的修改，可能需要手动处理。

!!! tip

    可以运行 `vcs status src` 检查各仓库状态。

!!! note "`vcs help`"

    可用命令如下：
    ```
    branch     Show the branches
    custom     Run a custom command
    delete     Remove the directories indicated by the list of given repositories.
    diff       Show changes in the working tree
    export     Export the list of repositories
    import     Import the list of repositories
    log        Show commit logs
    pull       Bring changes from the repository into the working copy
    push       Push changes from the working copy to the repository
    remotes    Show the URL of the repository
    status     Show the working tree status
    validate   Validate the repository list file
    ```
    可以通过 `vcs <command> src` 调用。
