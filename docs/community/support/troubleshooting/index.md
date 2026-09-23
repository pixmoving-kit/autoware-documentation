<a id="troubleshooting"></a>

# 故障排查

<a id="setup-issues"></a>

## 环境配置问题

<a id="cuda-related-errors"></a>

### CUDA 相关错误

安装 CUDA 时，可能因版本冲突出错。可尝试以下方法之一来解决：

- 解除所有 CUDA 相关库的版本锁定，并重新运行 playbook。

  ```bash
  sudo apt-mark unhold  \
    "cuda*"             \
    "libcudnn*"         \
    "libnvinfer*"       \
    "libnvonnxparsers*" \
    "libnvparsers*"     \
    "tensorrt*"         \
    "nvidia*"

  ansible-playbook autoware.dev_env.install_dev_env
  ```

- 卸载所有 CUDA 相关库，并重新运行 playbook。

  ```bash
  sudo apt purge        \
    "cuda*"             \
    "libcudnn*"         \
    "libnvinfer*"       \
    "libnvonnxparsers*" \
    "libnvparsers*"     \
    "tensorrt*"         \
    "nvidia*"

  sudo apt autoremove

  ansible-playbook autoware.dev_env.install_dev_env
  ```

!!! warning

    请注意，这可能破坏系统，请谨慎操作。

- 运行 playbook 时跳过 CUDA 相关库的安装。

  ```bash
  ansible-playbook autoware.dev_env.install_dev_env --skip-tags nvidia
  ```

!!! warning

    请注意，Autoware Universe 的部分组件依赖 CUDA，目前仅支持 [cuda 角色默认配置](https://github.com/autowarefoundation/autoware/blob/main/ansible/roles/cuda/defaults/main.yaml)中指定的 CUDA 版本。
    Autoware 可能也能使用其他 CUDA 版本运行，但这些版本不受支持，功能无法保证。

<a id="build-issues"></a>

## 构建问题

<a id="insufficient-memory"></a>

### 内存不足

构建 Autoware 需要大量内存，如果构建期间内存耗尽，机器可能卡死或崩溃。为避免此问题，应配置 16–32GB 的交换空间。

```bash
# Optional: Check the current swapfile
free -h

# Remove the current swapfile
sudo swapoff /swapfile
sudo rm /swapfile

# Create a new swapfile
sudo fallocate -l 32G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Optional: Check if the change is reflected
free -h
```

有关具体配置步骤和交换空间的说明，请参阅 Digital Ocean 的[“如何在 Ubuntu 20.04 上添加交换空间”教程](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04)。

如果机器的 CPU 核心过多（超过 64 个），可能需要更多内存。
一种解决办法是在构建时限制作业数量。

```bash
MAKEFLAGS="-j4" colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

可以根据系统情况，将 `-j4` 调整为其他数量。
详情请参阅 [GNU make 手册](https://www.gnu.org/software/make/manual/make.html#Parallel-Disable)。

减少同时构建的功能包数量，也可以降低内存占用。
在下面的示例中，并行构建的功能包数量设为 1，`make` 使用的作业数量也限制为 1。

```bash
MAKEFLAGS="-j1" colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --parallel-workers 1
```

!!! note

    同时减少并行构建的功能包数量和 `make` 的作业数量，可以降低内存占用。
    但这也意味着构建耗时更长。

<a id="errors-when-using-the-latest-version-of-autoware"></a>

### 使用最新版 Autoware 时出错

使用最新版 Autoware 时，过时的软件或旧构建文件可能导致问题。

要解决此类问题，首先尝试清理构建产物并重新构建：

```bash
rm -rf build/ install/ log/
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

如果仍未解决，请删除 `src/`，并根据安装方式更新工作区（[Docker](../../../installation/autoware/docker-installation.md#update-the-workspace) / [源码](../../../installation/autoware/source-installation.md#how-to-update-a-workspace)）。

!!! Warning

    删除 `src/` 之前，请确认本地没有需要保留的修改！

如果上述步骤后错误仍然存在，请删除整个工作区，重新克隆仓库，并从头开始安装。

```bash
rm -rf autoware/
git clone https://github.com/autowarefoundation/autoware.git
```

<a id="errors-when-using-a-fixed-version-of-autoware"></a>

### 使用固定版本 Autoware 时出错

原则上，使用固定版本时不应发生错误。不过，可能的原因包括：

- ROS 2 更新引入了破坏性变更。
  - 可查看 ROS Discourse 上的[软件打包与发布管理](https://discourse.ros.org/c/release/16)分类进行确认。
- 本地环境损坏。
  - 检查 `.bashrc` 文件、环境变量及库版本。

除上述原因外，使用固定版本还存在两种常见误解。

1. 仅为 `autowarefoundation/autoware` 使用了固定版本。
   要使用完全固定的版本，必须指定 `.repos` 文件中所有仓库的版本。

2. 更改 `autowarefoundation/autoware` 分支后，未更新工作区。
   更改 `autowarefoundation/autoware` 分支不会影响 `src/` 下的文件，必须运行 `vcs import` 才能更新。

<a id="error-when-building-python-package"></a>

### 构建 Python 功能包时出错

构建过程中可能出现以下问题。

```bash
pkg_resources.extern.packaging.version.InvalidVersion: Invalid version: '0.23ubuntu1'
```

原因是 66.0.0 至 67.5.0 版本的 `setuptools` 强制要求 Python 软件包
符合 [PEP-440](https://peps.python.org/pep-0440/)。
从 67.5.1 起，`setuptools` 提供了[回退机制](https://github.com/pypa/setuptools/commit/1640731114734043b8500d211366fc941b741f67)，使旧软件包能够再次正常工作。

解决方法是使用以下命令将 `setuptools` 更新到最新版。

```bash
pip install --upgrade setuptools
```

<a id="dockerrocker-issues"></a>

## Docker/rocker 问题

使用 Docker 或 rocker 运行 Autoware 时如果出错，请先运行以下命令，确认 Docker 安装正常：

```bash
docker run --rm -it hello-world
docker run --rm -it ubuntu:latest
```

然后，确认可以访问存储在 GitHub Packages 网站上的 Autoware 基础镜像。

```bash
docker run --rm -it ghcr.io/autowarefoundation/autoware:universe-jazzy
```

<a id="runtime-issues"></a>

## 运行时问题

<a id="cyclonedds-failed-to-find-a-free-participant-index-ros-2-jazzy"></a>

### CycloneDDS：Failed to find a free participant index（ROS 2 Jazzy）

使用 ROS 2 Jazzy 与 CycloneDDS 时，可能出现 "Failed to find a free participant index for domain 0"，并导致节点启动失败。原因与解决办法请参阅[运行时故障排查：CycloneDDS 无法找到空闲参与者索引](runtime-troubleshooting.md#cyclonedds-failed-to-find-a-free-participant-index)。

<a id="performance-related-issues"></a>

### 性能相关问题

症状：

- Autoware 运行速度低于预期。
- 消息延迟出现在 RViz2 中。
- 点云滞后。
- 相机图像滞后。
- 点云或标记在 RViz2 中闪烁。
- 多个订阅者使用同一发布者时，消息频率下降。

如果出现上述症状，请查看[性能故障排查](performance-troubleshooting.md)页面。

<a id="map-does-not-display-when-running-the-planning-simulator"></a>

### 运行 Planning Simulator 时地图不显示

运行 Planning Simulator 时，RViz 中不显示地图最常见的原因是[启动命令中的地图路径未正确指定](../../../demos/planning-sim/lane-driving.md)。可以在日志中搜索 `Could not find lanelet map under {path-to-map-dir}/lanelet2_map.osm` 错误，确认是否属于此情况。

另一种可能是 DDS 性能较差，导致地图加载时间过长。对此，请参阅[性能故障排查](performance-troubleshooting.md)。

<a id="died-process-issues"></a>

### 进程退出问题

运行时某些模块可能无法正常启动，终端中可能显示 "process has died"。
可以使用 gdb 工具定位问题发生的位置。

在 autoware 工作区中，以调试模式构建需要分析的模块。

```bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug --packages-up-to <the modules you wish to analyze> --catkin-skip-building-tests --symlink-install
```

在这种配置下，再次运行 autoware 时如果进程异常退出，就会生成 core 文件。
请记得移除 core 文件的大小限制。

```bash
ulimit -c unlimited
```

将 core 文件命名为 `core.<PID>`。

```bash
echo core | sudo tee /proc/sys/kernel/core_pattern
echo -n 1 | sudo tee /proc/sys/kernel/core_uses_pid
```

重新启动 autoware。进程异常退出时会生成 core 文件。
可使用 `ll -ht` 检查是否已生成。

启动 gdb 工具。

```bash
gdb <executable file> <core file>
#You can find the `<executable file>` in the error message.
```

`bt` 用于回溯进程退出时的回调调用栈。
`f <frame number>` 显示某个栈帧的详情，`l` 显示代码。
