<a id="autoware-core-digital-twin-simulation-with-awsim"></a>

# 使用 AWSIM 进行 Autoware Core 数字孪生仿真

<a id="installing-autoware"></a>

## 安装 Autoware

本页介绍已安装 Autoware Core 的环境中的操作步骤。
如果尚未安装 Autoware，请参阅[安装指南](../../../installation/index.md)。

<a id="download-awsim"></a>

## 下载 AWSIM

1. 从[此页面](https://autowarefoundation.github.io/AWSIM/Downloads/)下载以下文件。
   - AWSIM-Demo.zip
   - Shinjuku-Map.zip

2. 解压下载的文件。本页假定文件放置在以下路径。
   - $HOME/Downloads/AWSIM-Demo
   - $HOME/Downloads/Shinjuku-Map

<a id="start-simulation"></a>

## 开始仿真

1. 根据安装方式，按照下方对应章节启动 Autoware。
   - 启动通过 Docker 安装的 Autoware
   - 启动通过源码安装的 Autoware
   - 启动通过 Debian 软件包安装的 Autoware

   地图会在 RViz 中显示，如下图所示。
   ![RViz](images/rviz.png)

2. 启动 AWSIM。

   ```bash
   cd $HOME/Downloads/AWSIM-Demo
   ./AWSIM-Demo.x86_64
   ```

   AWSIM 界面如下图所示。
   ![AWSIM](images/awsim.png)

3. 初始化位姿。选择“2D Pose Estimate”，并沿箭头方向拖动鼠标。

   ![初始化位姿](images/init-pose.png)

4. 设置目标位姿。选择“2D Goal Pose”，并沿箭头方向拖动鼠标。

   ![目标位姿](images/goal-pose.png)

5. 开始自动驾驶。

   ```bash
   source $HOME/autoware_launch_workspace/install/setup.bash

   ros2 topic pub /system/operation_mode/state autoware_adapi_v1_msgs/msg/OperationModeState \
     "{mode: 2, is_autoware_control_enabled: true, is_autonomous_mode_available: true}" \
     --once --qos-durability transient_local

   ros2 topic pub /control/command/gear_cmd autoware_vehicle_msgs/msg/GearCommand "command: 2" \
     --once --qos-durability transient_local
   ```

<a id="launch-autoware-for-docker-installation"></a>

## 启动通过 Docker 安装的 Autoware

1. 运行以下命令。

   ```bash
   xhost +local:
   docker run --rm -it --net host -e DISPLAY=$DISPLAY -v $HOME/Downloads/Shinjuku-Map/map:/home/aw/autoware_data/maps ghcr.io/autowarefoundation/autoware:core-humble
   ```

2. 在 Docker 容器中运行以下命令。

   ```bash
   ros2 launch autoware_core autoware_core.launch.xml use_sim_time:=true map_path:=/home/aw/autoware_data/maps vehicle_model:=autoware_sample_vehicle sensor_model:=autoware_awsim_sensor_kit
   ```

<a id="launch-autoware-for-source-installation"></a>

## 启动通过源码安装的 Autoware

1. 运行以下命令。

   ```bash
   cd $HOME/autoware_core_workspace
   source install/setup.bash
   ros2 launch autoware_core autoware_core.launch.xml use_sim_time:=true map_path:=$HOME/Downloads/Shinjuku-Map/map vehicle_model:=autoware_sample_vehicle sensor_model:=autoware_awsim_sensor_kit
   ```

<a id="launch-autoware-for-debian-package-installation"></a>

## 启动通过 Debian 软件包安装的 Autoware

1. 运行以下命令。

   ```bash
   source /opt/ros/humble/setup.bash
   ros2 launch autoware_core autoware_core.launch.xml use_sim_time:=true map_path:=$HOME/Downloads/Shinjuku-Map/map vehicle_model:=autoware_sample_vehicle sensor_model:=autoware_awsim_sensor_kit
   ```
