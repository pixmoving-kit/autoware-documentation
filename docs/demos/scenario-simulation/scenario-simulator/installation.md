<a id="installation"></a>

# 安装

本文逐步说明如何构建包含 `scenario_simulator_v2` 的 [AWF Autoware Core/Universe](https://github.com/autowarefoundation/autoware)。

<a id="prerequisites"></a>

## 前提条件

1. [已构建并安装 Autoware](../../../installation/index.md)

<a id="how-to-build"></a>

## 构建方法

1. 进入 Autoware 工作空间：

   ```bash
   cd autoware
   ```

2. 导入仿真器依赖：

   ```bash
   vcs import src < repositories/simulator.repos
   ```

3. 安装依赖的 ROS 软件包：

   ```bash
   source /opt/ros/humble/setup.bash
   rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
   ```

4. 构建工作空间：

   ```bash
   colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
   ```
