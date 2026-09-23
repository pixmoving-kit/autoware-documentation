<a id="perception-component-reference-implementation-design"></a>

# 感知组件参考实现设计

<a id="purpose-of-this-document"></a>

## 本文目的

本文概述参考实现的详细设计，使开发者和用户了解感知组件当前提供的功能，以及如何使用、扩展或添加功能。

<a id="whole-architecture"></a>

## 整体架构

下图描述参考实现的架构。

![overall-perception-architecture](image/reference-implementaion-perception-diagram.drawio.svg)

感知组件由以下子组件组成：

- **障碍物分割**：识别来自自车应避让障碍物的点云，包括动态目标以及应避让的静态障碍物。例如，施工交通锥通过此模块识别。
- **占据栅格地图**：检测盲区（无法获取信息、可能有动态目标突然出现的区域）。
- **目标识别**：识别当前帧中自车周围的动态目标，并预测其未来轨迹。
  - **检测**：检测车辆和行人等动态目标的位姿及速度。
    - **检测器**：逐帧触发目标检测处理。
    - **插值器**：维持稳定的目标检测。即使检测器输出突然不可用，插值器也会利用跟踪模块的输出维持检测结果，避免遗漏目标。
  - **跟踪**：关联多个帧中的检测结果。
  - **预测**：预测动态目标轨迹。
- **交通信号灯识别**：识别交通信号灯颜色和箭头信号方向。

<a id="internal-interface-in-the-perception-component"></a>

### 感知组件内部接口

- **障碍物分割到目标识别**
  - 点云：当前帧观测的点云，已移除地面和离群点。
- **障碍物分割到占据栅格地图**
  - 地面过滤点云：当前帧观测的点云，已移除地面。
- **占据栅格地图到障碍物分割**
  - 占据栅格地图：用于过滤离群点。

<a id="architecture-for-object-recognition"></a>

## 目标识别架构

![Overall Pipeline](image/new_autoware_design.drawio.svg)

<a id="base-3d-detection"></a>

### 基础 3D 检测

Autoware 主要使用机器学习方法进行 3D 检测，称为**基础 3D 检测**。
可用方法包括：

- [CenterPoint](https://github.com/autowarefoundation/autoware.universe/tree/main/perception/autoware_lidar_centerpoint)
- [TransFusion-L](https://github.com/autowarefoundation/autoware.universe/tree/main/perception/autoware_lidar_transfusion)
- [BEVFusion-L](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_bevfusion)
- [Apollo 实例分割](https://github.com/autowarefoundation/autoware.universe/tree/main/perception/autoware_lidar_apollo_instance_segmentation) + [形状估计](https://github.com/autowarefoundation/autoware.universe/tree/main/perception/autoware_shape_estimation)

`Base 3D Detection` 的检测范围通常为 90m 到 120m，取决于具体情况。
如果希望使用相机与 LiDAR 融合，可集成 BEVFusion-CL 等模型（相机与 LiDAR 融合模型）。
但由于 `Base 3D Detection` 是新架构的关键组件，稳定的性能至关重要。
因此，不建议在传感器数据频繁丢失的环境中使用相机与 LiDAR 融合方法。

<a id="near-object-3d-detection"></a>

### 近距离目标 3D 检测

为增强近距离目标（特别是行人和骑行者）的检测，我们引入了可选的**近距离目标 3D 检测**。
它可作为 `Base 3D Detection` 的补充检测方法。

近距离目标检测主要使用 CenterPoint 等擅长检测小目标的机器学习方法。
通过在机器学习模型中应用更高分辨率的体素网格，提高小目标检测精度。
检测范围通常为 30m 到 50m。

<a id="tbd-camera-only-3d-detection"></a>

### （待定）纯相机 3D 检测

为改善 LiDAR 方法难以检测的目标的检测效果，我们引入了可选的**纯相机 3D 检测**。
`Camera-Only 3D detection` 旨在处理 LiDAR 方法难以检测的情况。
例如，`Camera-Only 3D detection` 将处理树木遮挡目标的检测和远距离识别。

注意，我们将对 `Camera-Only 3D Detection` 应用较高的置信度阈值，以抑制误检影响。

<a id="radar-only-faraway-object-3d-detection"></a>

### 纯雷达远距离目标 3D 检测

为增强远距离目标检测，使用**纯雷达 3D 检测**。
详情请参阅[雷达远距离目标检测文档](reference-implementations/radar-based-3d-detector/faraway-object-detection.md)。

<a id="tbd-3d-semantic-segmentation"></a>

### （待定）3D 语义分割

为改善传统 3D 检测方法难以检测的目标，尤其是植被和交通锥的检测效果，我们将实现 **3D 语义分割**。
`3D Semantic Segmentation` 提供非地面点云，以及部分目标和植被的带标签点云。

可用方法包括：

- FRNet（待定）

为与 Autoware 接口集成，使用欧氏聚类方法处理 3D 分割输出。

<a id="cluster-based-3d-detection"></a>

### 基于聚类的 3D 检测

为增强 LiDAR 方法可能难以检测的目标的检测效果，我们提供**基于聚类的 3D 检测**。
`Cluster-Based 3D Detection` 包含多个节点，其处理流程如下。

![](image/clustering_based_detection.drawio.svg)

`Cluster-Based 3D Detection` 基于欧氏聚类，包括基于 roi 的点云融合。
其处理过程结合非地面 LiDAR 点云与 2D 检测或语义分割结果。
它可作为 `Base 3D Detection` 的补充检测。

注意，点云和图像数据量增加时，处理时间也会增加。
因此，在对处理时间要求严格的情况下，建议避免使用此流程，或仅将其用于较小的检测范围。

<a id="multi-object-tracking-v2"></a>

### 多目标跟踪 v2

**多目标跟踪 v2** 基于现有的 [multi_object_tracker](https://github.com/autowarefoundation/autoware.universe/tree/main/perception/autoware_multi_object_tracker)，如下图所示：

![](image/multi_object_tracking.drawio.svg)

主要功能如下。

- **优先级目标合并器**

相比现有的 [object_merger](https://github.com/autowarefoundation/autoware.universe/tree/main/perception/autoware_object_merger)，`Priority Object Merger` 引入了新功能。
`Priority Object Merger` 可处理多个输入，减少对多个 `object_merger` 节点的需求。
这使调试更加容易。

现有 `object_merger` 通过消息过滤器进行近似同步，会引入时间延迟。
输入数据量增加时，延迟也会增加，影响自动驾驶可用性。
此外，如果某些检测失败，合并器无法合并结果，导致可用性下降。

![](image/priority_merger_1.drawio.svg)

`Priority Object Merger` 移除了消息过滤器，采用基于优先级的主检测方法。
订阅主检测输出时，它会收集检测流程的全部输出。
它收集所有检测输出，确保即使次要检测失败，仍能合并其他检测结果，从而提升整体可靠性和可用性。

![](image/priority_merger_2.drawio.svg)

- **静止目标检测**

输入数据量增加时，处理时间也会增加。
为优化性能，我们通过引入静止目标检测降低计算成本。
