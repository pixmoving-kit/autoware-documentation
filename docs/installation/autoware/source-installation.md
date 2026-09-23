<a id="source-installation"></a>

# 源码安装

<a id="prerequisites"></a>

## 前提条件

- 操作系统
  - [Ubuntu 22.04](https://releases.ubuntu.com/22.04/)

- ROS
  - ROS 2 Humble

  ROS 2 的系统依赖请参阅 [REP-2000](https://www.ros.org/reps/rep-2000.html)。

- [Git](https://git-scm.com/)
  - 建议[向 GitHub 注册 SSH 密钥](https://github.com/settings/keys)。

```bash
sudo apt-get -y update
sudo apt-get -y install git
```

<a id="how-to-set-up-a-development-environment"></a>

## 设置开发环境

1. 克隆 `autowarefoundation/autoware` 并进入该目录。

   ```bash
   git clone https://github.com/autowarefoundation/autoware.git
   cd autoware
   ```

   默认检出 `main` 分支，其中包含正在开发的最新变更。如需使用稳定版本，请检出相应的发布标签。例如，使用 `1.9.0` 版本（同时兼容 ROS 2 Humble 和 Jazzy）：

   ```bash
   git checkout 1.9.0
   ```

   可用标签列表见 [Autoware 发布页面](https://github.com/autowarefoundation/autoware/releases)。

2. 如果是首次安装 Autoware，可以使用提供的 Ansible playbook 自动安装依赖。

   ```bash
   bash ansible/scripts/install-ansible.sh
   source ~/.bashrc
   ansible-galaxy collection install -f -r ansible-galaxy-requirements.yaml
   ansible-playbook autoware.dev_env.install_dev_env
   ```

   如需在不支持 **NVIDIA GPU** 的情况下安装：

   ```bash
   ansible-playbook autoware.dev_env.install_dev_env --skip-tags nvidia
   ```

   如果遇到构建问题，请参阅[故障排查](../../community/support/troubleshooting/index.md#build-issues)章节。

!!! info

    安装 NVIDIA 库之前，请确保已阅读并同意相关许可证。

    - [CUDA](https://docs.nvidia.com/cuda/eula/index.html)
    - [cuDNN](https://docs.nvidia.com/deeplearning/cudnn/sla/index.html)
    - [TensorRT](https://docs.nvidia.com/deeplearning/tensorrt/sla/index.html)

!!! note

    以下项目会自动安装。如果 Ansible 脚本无法运行，或你已经安装了不同版本的依赖库，请手动安装以下项目。

    - [安装 Ansible](https://github.com/autowarefoundation/autoware/tree/main/ansible#ansible-installation)
    - [安装构建工具](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/build_tools#manual-installation)
    - [安装开发工具](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/dev_tools#manual-installation)
    - [安装 geographiclib](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/geographiclib#manual-installation)
    - [安装 RMW 实现](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/rmw_implementation#manual-installation)
    - [安装 ROS 2](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/ros2#manual-installation)
    - [安装 ROS 2 开发工具](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/ros2_dev_tools#manual-installation)
    - [安装 Nvidia CUDA](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/cuda#manual-installation)
    - [安装 Nvidia cuDNN 和 TensorRT](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/tensorrt#manual-installation)
    - [安装 Autoware RViz 主题](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/qt5ct_setup#readme)（仅影响 Autoware RViz）
    - [下载制品](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/artifacts#readme)（用于感知推理）

<a id="how-to-set-up-a-workspace"></a>

## 设置工作空间

!!! info "[使用 Autoware Build GUI](#using-autoware-build-gui)"

    如果你更倾向于使用图形用户界面（GUI）而非命令行来启动和管理仿真，请参阅本文末尾“使用 Autoware Build GUI”章节中的分步指南。

1. 创建 `src` 目录并将仓库克隆到其中。

   Autoware 使用 [vcs2l](https://github.com/ros-infrastructure/vcs2l) 创建工作空间。

   ```bash
   cd autoware
   mkdir -p src
   vcs import src < repositories/autoware.repos
   ```

   如果你正在积极参与开发，也可以拉取包含最新更新的 nightly 仓库：

   ```bash
   vcs import src < repositories/autoware-nightly.repos
   ```

   > ⚠️ 注意：nightly 仓库不稳定，可能存在缺陷，请谨慎使用。

   你还可以选择下载包含特定硬件驱动的额外仓库，但构建和运行 Autoware 并不需要这些仓库：

   ```bash
   vcs import src < repositories/extra-packages.repos
   ```

   > ⚠️ 你可能需要手动安装额外功能包的依赖。
   >
   > ➡️ 详情请查看额外功能包的 readme。

2. 将社区功能包添加到工作空间。_（可选）_ <span class="aw-badge-new">新增</span>

   [Autoware Index](autoware-index.md) 是用于扩展 Autoware 的社区功能包注册目录，其中的每个功能包都会基于最新 Autoware 版本进行构建和测试。
   在其[浏览网站](https://autowarefoundation.github.io/autoware-index/)上或通过 `aw-index-cli` 选择功能包，生成 `repositories/autoware-index.repos`，然后以相同方式导入：

   ```bash
   vcs import src < repositories/autoware-index.repos
   ```

   > ➡️ 完整指南请参阅 [Autoware Index](autoware-index.md) 页面。

3. 安装依赖的 ROS 功能包。

   除核心组件外，Autoware 还需要一些 ROS 2 功能包。
   `rosdep` 工具可以自动查找并安装这些依赖。
   运行 `rosdep install` 前，可能需要先运行 `rosdep update`。

   ```bash
   source /opt/ros/humble/setup.bash
   # Make sure all previously installed ros-$ROS_DISTRO-* packages are upgraded to their latest version
   sudo apt update && sudo apt upgrade
   rosdep update
   rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
   ```

4. [安装并设置 ccache，以加快后续构建速度](../../tutorials/others/advanced-usage-of-colcon.md#using-ccache-to-speed-up-recompilation)。_（可选，但强烈推荐）_

5. 构建工作空间。

   Autoware 使用 [colcon](https://github.com/colcon) 构建工作空间。
   更多高级选项请参阅[文档](https://colcon.readthedocs.io/)。

   ```bash
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```

   如果遇到构建问题，请参阅[故障排查](../../community/support/troubleshooting/index.md#build-issues)。

6. 运行 Autoware 前，请完成[网络配置](../../installation/additional-settings-for-developers/network-configuration/index.md)中的步骤。

7. 应用 [ROS 2 控制台设置](../../installation/additional-settings-for-developers/console-settings.md)中的推荐设置，以改善开发体验。_（可选）_

<a id="how-to-update-a-workspace"></a>

## 更新工作空间

1. 更新 `.repos` 文件。

   ```bash
   cd autoware
   git pull <remote> <your branch>
   ```

   `<remote>` 通常为 `git@github.com:autowarefoundation/autoware.git`

2. 更新仓库。

   ```bash
   vcs import src < repositories/autoware.repos
   ```

   > ⚠️ 如果使用 nightly 仓库，也可以一并更新。
   >
   > ```bash
   > vcs import src < repositories/autoware-nightly.repos
   > ```

   ```bash
   vcs pull src
   ```

   对于 Git 用户：
   - `vcs import` 类似于 `git checkout`。
     - 注意，它不会从远程拉取内容。
   - `vcs pull` 类似于 `git pull`。
     - 注意，它不会切换分支。

   更多信息请参阅[官方文档](https://github.com/ros-infrastructure/vcs2l)。

   通过 `vcs import` 导入的依赖可能已被移动或移除。
   Vcs2l 目前无法处理这些情况，因此，如果在 `vcs import` 后构建失败，可能需要清理
   并重新导入所有依赖：

   ```bash
   rm -rf src/*
   vcs import src < repositories/autoware.repos
   ```

   > ⚠️ 如果使用 nightly 仓库，也请一并导入。
   >
   > ```bash
   > vcs import src < repositories/autoware-nightly.repos
   > ```

3. 安装依赖的 ROS 功能包。

   ```bash
   source /opt/ros/humble/setup.bash
   # Make sure all previously installed ros-$ROS_DISTRO-* packages are upgraded to their latest version
   sudo apt update && sudo apt upgrade
   rosdep update
   rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
   ```

4. 构建工作空间。

   ```bash
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```

<a id="using-autoware-build-gui"></a>

## 使用 Autoware Build GUI

除了通过传统命令行方式构建 Autoware 功能包，开发者和用户还可以使用 Autoware Build GUI 获得更简便、友好的操作体验。该 GUI 应用简化了 Autoware 功能包的构建和管理流程。

<a id="integration-with-autoware-source-installation"></a>

### 与 Autoware 源码安装流程结合使用

将 Autoware Build GUI 与传统源码安装流程结合使用时：

- **初始设置**：按照标准 Autoware 源码安装指南设置环境和工作空间。
- **使用 GUI**：完成初始设置后，可以使用 Autoware Build GUI 管理后续构建和功能包更新。

这种结合方式使 Autoware 功能包的构建和管理更加易用，兼顾新用户和有经验的开发者。

<a id="getting-started-with-autoware-build-gui"></a>

### Autoware Build GUI 入门

1. **安装：** 确保已安装 Autoware Build GUI。参阅[安装说明](https://github.com/autowarefoundation/autoware-build-gui#installation)。
2. **启动应用**：安装完成后，启动 Autoware Build GUI。
   ![构建 GUI 主界面](images/build-gui/build_gui_main.png)
3. **设置**：在 GUI 中设置 Autoware 文件夹的路径。
   ![构建 GUI 设置](images/build-gui/build_gui_setup.png)
4. **构建功能包**：选择要构建的 Autoware 功能包，并通过 GUI 管理构建过程。
   ![构建 GUI 构建操作](images/build-gui/build_gui_build.png)

   4.1. **构建配置**：从默认构建配置列表中选择，或手动选择要构建的功能包。
   ![构建 GUI 构建配置](images/build-gui/build_gui_build_configuration.png)

   4.2. **构建选项**：选择构建类型，也可以指定额外的构建选项。
   ![构建 GUI 构建选项](images/build-gui/build_gui_build_options.png)

5. **保存和加载**：保存构建配置供以后使用；如果不想构建所有功能包或使用提供的默认配置，也可以加载此前保存的配置。
   ![构建 GUI 保存配置](images/build-gui/build_gui_save.png)
6. **更新工作空间**：使用 GUI 将 Autoware 工作空间中的功能包更新到最新版本，或将标定工具添加到工作空间。
   ![构建 GUI 更新工作空间](images/build-gui/build_gui_update.png)
