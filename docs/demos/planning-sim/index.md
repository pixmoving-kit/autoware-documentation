<a id="planning-simulation"></a>

# 规划仿真

<a id="preparation"></a>

## 准备工作

<a id="download-the-sample-map"></a>

### 下载示例地图

使用 [`demo_artifacts`](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/demo_artifacts) Ansible 角色，将示例地图下载并解压到 `~/autoware_data/maps/sample-map-planning/`：

```bash
ansible-galaxy collection install -f -r "ansible-galaxy-requirements.yaml"
ansible-playbook autoware.dev_env.install_dev_env --tags demo_artifacts --ask-become-pass
```

!!! info

    示例地图：Copyright 2020 TIER IV, Inc.

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

<a id="launch-autoware-planning-simulator"></a>

## 启动 Autoware 规划仿真器

```bash
source ~/autoware/install/setup.bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/autoware_data/maps/sample-map-planning vehicle_model:=sample_vehicle sensor_model:=sample_sensor_kit
```

!!! warning

    注意，此处不能用 `~` 代替 `$HOME`。

    如果使用 `~`，地图将无法加载。

---

!!! info "[使用 Autoware Launch GUI](using-launch-gui.md)"

    如果你更倾向于使用图形用户界面（GUI）而非命令行来启动和管理仿真，请参阅本文末尾[使用 Autoware Launch GUI](using-launch-gui.md)章节中的分步指南。

---

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-film: 参考视频教程](https://drive.google.com/file/d/1bs_dX1JJ76qHk-SGvS6YF9gmekkN8fz7/view?usp=sharing){ .md-button style="margin: 5px" }

</div>

---

<a id="basic-simulations"></a>

## 基础仿真

<div style="text-align: center;" markdown="1">

[车道内行驶](lane-driving.md){ .md-button style="margin: 5px" }
[泊车](parking.md){ .md-button style="margin: 5px" }
[驶离与靠边停车](pull-over-out.md){ .md-button style="margin: 5px" }
[变道](lane-change.md){ .md-button style="margin: 5px" }

</div>

<a id="advanced-simulations"></a>

## 进阶仿真

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-cube: 放置虚拟物体](placing-objects.md){ .md-button style="margin: 5px" }
[避障](avoidance.md){ .md-button style="margin: 5px" }
[交通信号灯识别仿真](traffic-light.md){ .md-button style="margin: 5px" }
[驶过人行横道](crosswalk.md){ .md-button style="margin: 5px" }

</div>

<a id="increase-the-maximum-velocity"></a>

## 提高最大速度

Autoware 原本支持较宽的速度范围，但出于安全考虑，默认最大速度被限制为 **15 km/h**。
因此，即使在 RViz 面板中将滑块拖到更高速度，系统也不会允许超过该限制。

要以更高速度运行 Autoware，可以修改 `autoware_launch` 仓库中的配置文件 [autoware_launch/config/planning/scenario_planning/common/common.param.yaml](https://github.com/autowarefoundation/autoware_launch/blob/main/autoware_launch/config/planning/scenario_planning/common/common.param.yaml) 中的 `max_vel` 参数。

!!! example

    将 `max_vel` 设为 `20.0`（20 m/s = 72 km/h）。
    然后启动规划仿真器，放置车辆，并使用滑块设置速度上限。

![提高最大速度](images/others/increase-max-velocity.png)

<a id="create-your-own-map"></a>

## 创建自己的地图

上述内容均在规划仿真器中使用示例地图进行。如果希望使用自己环境的地图运行 Autoware，请参阅[如何创建矢量地图](../../tutorials/integrating-autoware/creating-maps/index.md)章节。
