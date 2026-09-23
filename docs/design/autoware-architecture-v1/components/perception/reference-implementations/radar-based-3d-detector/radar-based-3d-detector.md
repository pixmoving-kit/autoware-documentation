<a id="radar-based-3d-detector"></a>

# 基于雷达的 3D 检测器

<a id="overview"></a>

## 概述

<a id="features"></a>

### 功能

基于雷达的 3D 检测器旨在实现：

- 检测超出基于 LiDAR 的 3D 检测范围的目标。

雷达能在比 LiDAR 更远的距离获取数据（> 100m），因此，当 LiDAR 3D 检测距离不足时，可采用基于雷达的 3D 检测器。
基于雷达的 3D 检测距离取决于雷达设备规格。

- 改善动态目标的速度估计

雷达可获取速度信息，通过融合 LiDAR 3D 检测目标与雷达信息，能够估计更精确的速度旋量信息。
这有助于提升目标跟踪/预测以及自适应巡航控制等规划功能的性能。

<a id="whole-pipeline"></a>

### 完整处理流程

使用雷达目标的雷达 3D 检测器包括：

- 使用雷达点云进行 3D 目标检测
- 噪声过滤器
- 远距离动态 3D 目标检测
- 将雷达信息融合到基于 LiDAR 的 3D 目标检测中
- 雷达目标跟踪
- 跟踪目标合并

![Radar based 3D detector](image/radar-based-3d-detector.drawio.svg)

<a id="interface"></a>

### 接口

- 输入
  - 点云消息类型为 `ros-perception/radar_msgs/msg/RadarScan.msg`
  - 雷达目标消息类型为 `autoware_auto_perception_msgs/msg/DetectedObject`。
    - 输入目标需要拼接。
    - 输入目标需要补偿自车运动。
    - 输入目标需要转换到 `base_link`。
- 输出
  - 跟踪目标

<a id="module"></a>

## 模块

<a id="radar-pointcloud-3d-detection"></a>

### 雷达点云 3D 检测

!!! warning

    编写中

<a id="noise-filter-and-radar-faraway-dynamic-3d-object-detection"></a>

### 噪声过滤与雷达远距离动态 3D 目标检测

![faraway object detection](image/faraway-object-detection.drawio.svg)

此功能过滤噪声目标，并检测远距离（> 100m）的动态车辆。
主要思路是：使用 LiDAR 时，可通过 LiDAR 点云准确检测近距离区域，而雷达主要负责检测仅靠 LiDAR 无法检测的远距离目标。
详情请参阅[此文档](faraway-object-detection.md)

<a id="radar-fusion-to-lidar-based-3d-object-detection"></a>

### 将雷达信息融合到基于 LiDAR 的 3D 目标检测中

- [radar_fusion_to_detected_object](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_radar_fusion_to_detected_object)

此软件包包含雷达检测目标与 3D 检测目标的传感器融合模块。融合节点能够：

- 成功匹配雷达数据时，为 3D 检测结果添加速度。跟踪模块利用速度信息改善跟踪结果，规划模块则利用它执行自适应巡航控制等操作。
- 找到对应雷达检测结果时，改善低置信度的 3D 检测结果。

<a id="radar-object-tracking"></a>

### 雷达目标跟踪

!!! warning

    编写中

<a id="merger-of-tracked-object"></a>

### 跟踪目标合并

!!! warning

    编写中

<a id="appendix"></a>

## 附录

<a id="customize-own-radar-interface"></a>

### 自定义雷达接口

Autoware 感知接口定义为 `DetectedObjects`、`TrackedObjects` 和 `PredictedObjects`，其他消息则按具体情况定义。例如，感知模块使用自定义消息 [DetectedObjectWithFeature](https://github.com/tier4/tier4_autoware_msgs/tree/tier4_perception_msgs/msg/object_recognition)。

同样，你也可以调整新的雷达接口。
例如，根据[过去的讨论](https://github.com/ros-perception/radar_msgs/pull/3)，尤其是[这条讨论](https://github.com/ros-perception/radar_msgs/pull/3#issuecomment-661599741)，`RadarTrack` 不包含朝向信息。
如果需要朝向信息，可调整雷达 ROS 驱动，使其直接发布 `TrackedObject`。
