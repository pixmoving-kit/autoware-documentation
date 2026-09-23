<a id="docker-installation"></a>

# Docker 安装

Autoware 在 GHCR 上发布预构建的多架构（amd64、arm64）Docker 镜像，包括用于试运行的运行时镜像、用于本地构建的开发镜像，以及用于 GPU 工作负载的 CUDA 变体。

完整的镜像目录包含十二个按构建依赖图组织的镜像，其说明位于 `autoware` 仓库中 Dockerfile 旁的权威 Docker 参考文档。这些章节会随实现更新，建议加入书签：

- [`docker/README.md` → 镜像依赖图](https://github.com/autowarefoundation/autoware/blob/main/docker/README.md#image-graph)——以图示展示镜像之间的依赖关系。
- [`docker/README.md` → 镜像](https://github.com/autowarefoundation/autoware/blob/main/docker/README.md#images)——以表格说明每个镜像及其适用场景。
- [`docker/README.md` → 从 GHCR 拉取](https://github.com/autowarefoundation/autoware/blob/main/docker/README.md#pull-from-ghcr)——介绍标签格式（`<stage>-<ros_distro>[-<date>|-<version>]`）及各变体的 `docker pull` 示例。

大多数用户主要会使用以下两个镜像：

- `ghcr.io/autowarefoundation/autoware:universe-cuda-jazzy`——完整的 Autoware 运行时，内置 NVIDIA CUDA、cuDNN 和 TensorRT。
- `ghcr.io/autowarefoundation/autoware:universe-jazzy`——完整的 Autoware 运行时，不含 GPU 支持。

使用 ROS 2 Humble 时，将 `jazzy` 替换为 `humble`。

这些标签指向最新构建。如需稳定版本，请在标签后附加版本号。例如，ROS 2 Jazzy 上的 `1.9.0` 版本对应 `ghcr.io/autowarefoundation/autoware:universe-jazzy-1.9.0`。

如需了解更全面的容器化部署内容（部署模式、集成、边缘端使用场景），请参阅 Open AD Kit：

- <https://github.com/autowarefoundation/openadkit>
- <https://autowarefoundation.github.io/openadkit/>

!!! info

    继续操作前，请确认并同意 [NVIDIA 深度学习容器许可证](https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license)。拉取和使用 Autoware 的 CUDA 镜像即表示你接受该许可证的条款和条件。

<a id="prerequisites"></a>

## 前提条件

- Docker
- NVIDIA Container Toolkit（推荐）
- 与 NVIDIA CUDA 12 兼容的 GPU 驱动（推荐）

1. 克隆 `autowarefoundation/autoware` 并进入该目录。

   ```bash
   git clone https://github.com/autowarefoundation/autoware.git
   cd autoware
   ```

   默认检出 `main` 分支，其中包含正在开发的最新变更。如需使用稳定版本，请检出相应的发布标签（同时兼容 ROS 2 Humble 和 Jazzy）：

   ```bash
   git checkout 1.9.0
   ```

   可用标签列表见 [Autoware 发布页面](https://github.com/autowarefoundation/autoware/releases)。

2. 安装 Ansible 并运行 Docker 设置 playbook：

   ```bash
   bash ansible/scripts/install-ansible.sh
   ansible-galaxy collection install -f -r ansible-galaxy-requirements.yaml
   ansible-playbook autoware.dev_env.install_docker --ask-become-pass
   ```

   如需在不支持 **NVIDIA GPU** 的情况下安装：

   ```bash
   ansible-playbook autoware.dev_env.install_docker --skip-tags nvidia --ask-become-pass
   ```

   如需仅下载制品：

   ```bash
   ansible-playbook autoware.dev_env.install_dev_env --tags artifacts --ask-become-pass
   ```

!!! info

    目标检测和交通信号灯检测／分类等功能需要 GPU 加速。有关如何在没有 GPU 的情况下启用这些功能，请参阅[不使用 CUDA 运行 Autoware](../../tutorials/others/running-autoware-without-cuda.md)。

<a id="quick-start"></a>

## 快速入门

<a id="launching-the-runtime-container"></a>

### 启动运行时容器

运行时镜像在容器启动时执行 `ros2 launch autoware_launch autoware.launch.xml`。标准的 `docker run` 调用方式见 [`docker/README.md` → 使用方法](https://github.com/autowarefoundation/autoware/blob/main/docker/README.md#usage)，其中包含地图和数据卷、X11 转发、CUDA 透传，以及逐项说明各标志用途的表格。该章节还介绍了无 GPU 变体（移除 NVIDIA 相关标志），以及当 ROS 2 节点需要跨主机通信时如何覆盖默认 CycloneDDS 配置。

<a id="pre-configured-demo-scenarios"></a>

### 预配置的演示场景

[`docker/examples/demos/`](https://github.com/autowarefoundation/autoware/tree/main/docker/examples/demos) 文件夹提供了可直接运行的 Compose 服务组合，无需自行编写 `docker run` 命令。每个演示都有独立的 README，说明前提条件和运行命令：

- [**planning-simulator**](https://github.com/autowarefoundation/autoware/tree/main/docker/examples/demos/planning-simulator)——使用示例地图、车辆和传感器套件的规划仿真器。通过 Compose 叠加配置提供三种渲染方式：默认使用软件渲染，Intel/AMD/Nouveau 主机使用 `docker-compose.dri.yaml`，NVIDIA 专有驱动使用 `docker-compose.nvidia.yaml`。
- [**awsim**](https://github.com/autowarefoundation/autoware/tree/main/docker/examples/demos/awsim)——通过 `network_mode: host` 将 Autoware 与基于 Unity 的 [AWSIM](https://autowarefoundation.github.io/AWSIM/) 仿真器桥接，并启动 `e2e_simulator.launch.xml`。需要 NVIDIA GPU 和 Container Toolkit。
- [**scenario-simulator**](https://github.com/autowarefoundation/autoware/tree/main/docker/examples/demos/scenario-simulator)——以两个共享生成的 CycloneDDS 配置的服务运行 `scenario_simulator_v2` 场景和实时 Autoware 规划栈。

<a id="running-autoware-tutorials"></a>

### 运行 Autoware 教程

在容器内，按照以下链接运行 Autoware 教程：

[规划仿真](../../demos/planning-sim/index.md)

[Rosbag 回放仿真](../../demos/rosbag-replay-simulation.md)。

<a id="deployment"></a>

## 部署

Open AD Kit 为 Autoware 提供多种部署选项，便于在不同平台和场景中部署 Autoware。详情请参阅 [Open AD Kit 文档](https://autowarefoundation.github.io/openadkit/)。

<a id="development"></a>

## 开发

如需基于 Autoware 进行开发，[`docker/examples/basic/`](https://github.com/autowarefoundation/autoware/tree/main/docker/examples/basic) 文件夹提供了三个 Compose 文件。它们均基于 `universe-devel-*` 镜像，启动后即可进入容器 shell，并挂载 `~/autoware_data`（包含 `maps/` 和 `ml_models/`）以及 Autoware 源码树。请选择与你的主机匹配的配置：

| 主机 GPU / 驱动            | Compose 文件                                |
| ---------------------------- | ------------------------------------------- |
| NVIDIA + 专有驱动  | `dev-nvidia.compose.yaml`（推荐）     |
| NVIDIA + Nouveau 开源驱动 | `dev-dri.compose.yaml`                      |
| Intel / AMD                  | `dev-dri.compose.yaml`                      |
| 无 GPU / 无显示设备            | `dev-cpu.compose.yaml`（软件渲染） |

在 Autoware 仓库根目录中运行：

```bash
xhost +local:docker
HOST_UID=$(id -u) HOST_GID=$(id -g) \
  docker compose -f docker/examples/basic/dev-nvidia.compose.yaml run --rm autoware
```

[`docker/examples/basic/README.md`](https://github.com/autowarefoundation/autoware/blob/main/docker/examples/basic/README.md) 进一步介绍了如何验证是否实际启用了硬件加速（`glxinfo -B`）、为什么 `dev-dri` 在使用 NVIDIA 专有驱动时会静默回退到软件渲染，以及如何将第二个终端连接到正在运行的开发容器。

<a id="how-to-set-up-a-workspace"></a>

### 设置工作空间

1. 创建 `src` 目录并将仓库克隆到其中。

   ```bash
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

3. 更新依赖的 ROS 功能包。

   Docker 镜像创建后，Autoware 的依赖可能发生了变化。
   这种情况下，需要运行以下命令更新依赖。

   ```bash
   # Make sure all ros-$ROS_DISTRO-* packages are upgraded to their latest version
   sudo apt update && sudo apt upgrade
   rosdep update
   rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
   ```

4. 构建工作空间。

   ```bash
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```

   如果遇到构建问题，请参阅[故障排查](../../community/support/troubleshooting/index.md#build-issues)。

<a id="update-the-workspace"></a>

### 更新工作空间

```bash
cd autoware
git pull
vcs import src < repositories/autoware.repos

# If you are using nightly repositories, also run the following command:
vcs import src < repositories/autoware-nightly.repos

vcs pull src
# Make sure all ros-$ROS_DISTRO-* packages are upgraded to their latest version
sudo apt update && sudo apt upgrade
rosdep update
rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
```

通过 `vcs import` 导入的依赖可能已被移动或移除。
Vcs2l 目前无法处理这些情况，因此，如果在 `vcs import` 后构建失败，可能需要清理
并重新导入所有依赖：

```bash
rm -rf src/*
vcs import src < repositories/autoware.repos
# If you are using nightly repositories, import them as well.
vcs import src < repositories/autoware-nightly.repos
```

<a id="using-vs-code-remote-containers-for-development"></a>

### 使用 VS Code 远程容器进行开发

使用 [Visual Studio Code](https://code.visualstudio.com/) 和 [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) 扩展，可以方便地在容器化环境中开发 Autoware。

安装 Visual Studio Code 的 [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) 扩展。
然后在命令面板（`F1`）中选择 `Remote-Containers: Reopen in Container`，在容器中重新打开工作空间。

可以选择 `universe-devel-jazzy` 镜像进行不含 CUDA 支持的开发，或选择 `universe-devel-cuda-jazzy` 镜像进行包含 CUDA 支持的开发。

<a id="building-docker-images-from-scratch"></a>

### 从头构建 Docker 镜像

构建流水线使用由 [`docker/docker-bake.hcl`](https://github.com/autowarefoundation/autoware/blob/main/docker/docker-bake.hcl) 驱动的 [`docker buildx bake`](https://docs.docker.com/build/bake/)。构建 `base` 以外的任何目标，都需要先将 Autoware 源码仓库检出到 `src/` 下：

```bash
cd autoware/
vcs import src < repositories/autoware.repos
docker buildx bake -f docker/docker-bake.hcl
```

该命令会构建默认目标（`universe` 和 `universe-cuda`）；[镜像依赖图](https://github.com/autowarefoundation/autoware/blob/main/docker/README.md#image-graph)中的依赖会自动解析。如需构建特定阶段（例如 `core-devel`、`base-cuda-runtime`），将其作为参数传入；如需为 ROS 2 Humble 构建，在命令前加上 `ROS_DISTRO=humble`。完整的目标列表和多架构构建流程请参阅 [`docker/README.md` → 本地构建](https://github.com/autowarefoundation/autoware/blob/main/docker/README.md#build-locally)。

每个镜像都在 [GHCR](https://github.com/autowarefoundation/autoware/pkgs/container/autoware) 上发布带固定日期和发布标签的版本；需要固定镜像版本时，请使用这些标签。
