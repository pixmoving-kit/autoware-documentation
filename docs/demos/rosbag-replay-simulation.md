<a id="rosbag-replay-simulation"></a>

# Rosbag 回放仿真

<a id="preparation"></a>

## 准备工作

<a id="download-the-sample-map-and-rosbag"></a>

### 下载示例地图和 rosbag

使用 [`demo_artifacts`](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/demo_artifacts) Ansible 角色，下载并解压示例地图和示例 rosbag：

```bash
ansible-galaxy collection install -f -r "ansible-galaxy-requirements.yaml"
ansible-playbook autoware.dev_env.install_dev_env --tags demo_artifacts --ask-become-pass
```

运行该角色后：

- 示例地图 → `~/autoware_data/maps/sample-map-rosbag/`
- 示例 rosbag → `~/autoware_data/recordings/bags/sample-rosbag/`

<a id="make-sure-the-ml-model-artifacts-are-downloaded"></a>

### 确保已下载机器学习模型制品

检查是否存在 `~/autoware_data/ml_models` 文件夹及其中的文件。

```bash
$ cd ~/autoware_data/ml_models
$ ls -C -w 30
bevfusion
calibration_status_classifier
camera_streampetr
diffusion_planner
image_projection_based_fusion
lidar_apollo_instance_segmentation
lidar_centerpoint
lidar_frnet
lidar_transfusion
ptv3
simpl_prediction
tensorrt_bevdet
tensorrt_yolox
traffic_light_classifier
traffic_light_fine_detector
vad
yabloc_pose_initializer
```

如果没有，请按照[手动下载制品](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/artifacts)的说明操作。

!!! info

    - 示例地图和 rosbag：Copyright 2020 TIER IV, Inc.
    - 出于隐私考虑，该 rosbag 不包含图像数据，这会导致：
      - 无法使用此示例 rosbag 测试交通信号灯识别功能。
      - 目标检测精度下降。

<a id="how-to-run-a-rosbag-replay-simulation"></a>

## 运行 rosbag 回放仿真

!!! tip "[使用 Autoware Launch GUI](#using-autoware-launch-gui)"

    如果你更倾向于使用图形用户界面（GUI）而非命令行来启动和管理仿真，请参阅本文末尾“使用 Autoware Launch GUI”章节中的分步指南。

1. 启动 Autoware。

   ```sh
   source ~/autoware/install/setup.bash
   ros2 launch autoware_launch logging_simulator.launch.xml map_path:=$HOME/autoware_data/maps/sample-map-rosbag vehicle_model:=sample_vehicle sensor_model:=sample_sensor_kit
   ```

   注意，此处不能用 `~` 代替 `$HOME`。

   ![启动 Autoware 后](images/rosbag-replay/after-autoware-launch.png)

   > ⚠️ 播放 `rosbag` 前，终端中可能会出现错误和警告消息，这是正常现象。开始播放 `rosbag` 并正确完成初始化后，这些消息应会消失。

2. 播放示例 rosbag 文件。

   ```sh
   source ~/autoware/install/setup.bash
   ros2 bag play ~/autoware_data/recordings/bags/sample-rosbag/ -r 0.2 -s sqlite3
   ```

   > ⚠️ 由于 `rosbag` 中的时间戳与当前系统时间戳存在差异，Autoware 可能在终端中发出警告，提示时间戳不匹配。这是正常现象。

   ![播放 rosbag 后](images/rosbag-replay/after-rosbag-play.png)

3. 要使视图聚焦于自车，请将 RViz Views 面板中的 `Target Frame` 从 `viewer` 改为 `base_link`。

   ![更改目标坐标系](images/rosbag-replay/change-target-frame.png)

4. 要将视图切换为 `Third Person Follower` 等类型，请修改 RViz Views 面板中的 `Type`。

   ![第三人称跟随视图](images/rosbag-replay/third-person-follower.png)

!!! tip

    [:fa-cl-s fa-film: 参考视频教程](https://drive.google.com/file/d/12D6aSC1Y3Kf7STtEPWG5RYynxKdVcPrc/view?usp=sharing){ .md-button }

<a id="using-autoware-launch-gui"></a>

## 使用 Autoware Launch GUI

本节逐步介绍如何使用 Autoware Launch GUI 启动和管理 rosbag 回放仿真，为上一节中的命令行操作提供另一种方式。

<a id="getting-started-with-autoware-launch-gui"></a>

### Autoware Launch GUI 入门

1. **安装：** 确保已安装 Autoware Launch GUI。参阅[安装说明](https://github.com/autowarefoundation/autoware-launch-gui#installation)。

2. **启动 GUI：** 从应用程序菜单中打开 Autoware Launch GUI。
   ![启动 GUI 的界面截图](images/rosbag-replay/launch-gui/launch_gui_main.png)

<a id="launching-a-logging-simulation"></a>

### 启动日志仿真

1. **设置 Autoware 路径：** 在 GUI 中设置 Autoware 的安装路径。
   ![设置 Autoware 路径的界面截图](images/rosbag-replay/launch-gui/launch_gui_setup.png)
2. **选择启动文件：** 为车道内行驶场景选择 `logging_simulator.launch.xml`。
   ![选择启动文件的界面截图](images/rosbag-replay/launch-gui/selecting_launch_file.png)
3. **自定义参数：** 根据需要调整 `map_path`、`vehicle_model` 和 `sensor_model` 等参数。

   ![自定义参数的界面截图](images/rosbag-replay/launch-gui/customizing-parameters1.png)
   ![自定义参数的界面截图](images/rosbag-replay/launch-gui/customizing-parameters2.png)

4. **开始仿真：** 点击启动按钮开始仿真，并查看所有日志。

   ![开始仿真的界面截图](images/rosbag-replay/launch-gui/starting_simulation.png)

5. **播放 Rosbag：** 切换到 `Rosbag` 选项卡，选择要播放的 rosbag 文件。

   ![选择 rosbag 文件的界面截图](images/rosbag-replay/launch-gui/selecting_rosbag_file.png)

6. **调整播放速度：** 根据需要调整播放速度及其他希望自定义的参数。

   ![调整播放速度的界面截图](images/rosbag-replay/launch-gui/adjusting_flags.png)

7. **开始播放：** 点击播放按钮开始回放 rosbag，即可使用 `pause/play`、`stop` 和 `speed slider` 等设置。

   ![开始播放的界面截图](images/rosbag-replay/launch-gui/starting_playback.png)

8. **查看仿真：** 切换到 `RViz` 窗口查看仿真。

   ![播放 rosbag 后](images/rosbag-replay/after-rosbag-play.png)

9. 要使视图聚焦于自车，请将 RViz Views 面板中的 `Target Frame` 从 `viewer` 改为 `base_link`。

   ![更改目标坐标系](images/rosbag-replay/change-target-frame.png)

10. 要将视图切换为 `Third Person Follower` 等类型，请修改 RViz Views 面板中的 `Type`。

    ![第三人称跟随视图](images/rosbag-replay/third-person-follower.png)
