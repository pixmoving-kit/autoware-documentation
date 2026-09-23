<a id="autoware-core-source-installation-guide"></a>

# Autoware Core 源码安装指南

<a id="prerequisites"></a>

## 前提条件

| 项目                        | 要求                                                                                               |
| --------------------------- | --------------------------------------------------------------------------------------------------------- |
| 操作系统                          | [Ubuntu 22.04](https://releases.ubuntu.com/22.04/)                                                        |
| ROS                         | ROS 2 Humble（ROS 2 的系统依赖请参阅 [REP-2000](https://www.ros.org/reps/rep-2000.html)） |
| [Git](https://git-scm.com/) | 建议[向 GitHub 注册 SSH 密钥](https://github.com/settings/keys)。                         |

<a id="how-to-set-up"></a>

## 设置步骤

1. 安装依赖工具。

   ```bash
   sudo apt -y update
   sudo apt -y install git python3-colcon-common-extensions python3-rosdep
   sudo rosdep init
   ```

2. 创建工作空间并将仓库克隆到其中。

   ```bash
   git clone https://github.com/autowarefoundation/autoware.git $HOME/autoware_core_workspace
   cd $HOME/autoware_core_workspace
   mkdir -p src
   vcs import src < repositories/autoware.repos
   ```

3. 此步骤可选。如果需要最新分支，请切换到 nightly。

   ```bash
   cd $HOME/autoware_core_workspace
   vcs import src < repositories/autoware-nightly.repos
   ```

4. 安装依赖的 ROS 功能包。

   ```bash
   cd $HOME/autoware_core_workspace
   sudo apt update && sudo apt -y upgrade
   rosdep update
   rosdep install -y --from-paths src/core --ignore-src --rosdistro humble
   ```

5. 构建工作空间。

   ```bash
   cd $HOME/autoware_core_workspace
   source /opt/ros/humble/setup.bash
   colcon build --symlink-install --base-paths src/core --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```
