<a id="map-component-design"></a>

# 地图组件设计

<a id="1-overview"></a>

## 1. 概述

Autoware 依赖驾驶环境的高精度点云地图和矢量地图，执行定位、路线规划、交通信号灯检测以及行人和其他车辆轨迹预测等任务。

本文介绍 Autoware 地图组件的设计，包括需求、架构设计、功能、数据格式，以及向自动驾驶软件栈其他部分分发地图信息的接口。

<a id="2-requirements"></a>

## 2. 需求

地图应向软件栈其他部分提供两类信息：

- 以矢量地图形式提供道路语义信息
- 以点云地图形式提供环境几何信息（可选）

矢量地图包含道路网络、车道几何形状和交通信号灯的高精度信息，路线规划、交通信号灯检测以及其他车辆和行人轨迹预测都需要这些信息。

在 Autoware 中，三维点云地图主要用于基于 LiDAR 的定位以及部分感知功能。为确定车辆当前的位置和朝向，系统将一个或多个 LiDAR 实时采集的扫描数据与预先生成的三维点云地图匹配。因此，准确的点云地图对良好的定位效果至关重要。不过，如果车辆采用其他精度足够的定位方法，例如基于相机的定位，则使用 Autoware 时可能不需要点云地图。

除上述两类地图外，Autoware 还需要一个补充文件，用于指定地图在大地测量系统中的坐标系。

<a id="3-architecture"></a>

## 3. 架构

下图描述 Autoware 地图组件的高层架构。

![map component architecture](image/high-level-map-diagram.drawio.svg){width="800"}

地图组件由以下子组件组成：

- **点云地图加载**：加载并发布点云地图
- **矢量地图加载**：加载并发布矢量地图
- **投影加载**：加载并发布投影信息，用于在局部坐标（x、y、z）与大地坐标（纬度、经度、高度）之间转换

<a id="4-component-interface"></a>

## 4. 组件接口

<a id="input-to-the-map-component"></a>

### 地图组件的输入

- **来自文件系统**
  - 点云地图及其元数据文件
  - 矢量地图
  - 投影信息

<a id="output-from-the-map-component"></a>

### 地图组件的输出

- **发送到感知传感器组件**
  - 投影信息：用于将 GNSS 数据从大地坐标系转换到局部坐标系
- **发送到定位组件**
  - 点云地图：用于基于 LiDAR 的定位
  - 矢量地图：用于基于道路标线等信息的定位方法
- **发送到感知组件**
  - 点云地图：通过比较 LiDAR 数据与点云地图进行障碍物分割
  - 矢量地图：用于车辆轨迹预测
- **发送到规划组件**
  - 矢量地图：用于行为规划
- **发送到 API 层**
  - 投影信息：用于将定位结果从局部坐标系转换到大地坐标系

<a id="5-map-specification"></a>

## 5. 地图规范

<a id="point-cloud-map"></a>

### 点云地图

点云地图必须以文件形式提供，并满足以下要求：

- 点云地图必须投影到 `map_projection_loader` 定义的同一坐标系，以与 lanelet2 地图以及其他在局部坐标和大地坐标之间转换的软件包保持一致。详情请参阅 [`map_projection_loader` 的说明文档](https://github.com/autowarefoundation/autoware_core/tree/main/map/autoware_map_projection_loader/README.md)。
- 必须使用 [PCD（点云数据）文件格式](https://pointclouds.org/documentation/tutorials/pcd_file_format.html)，可以是单个 PCD 文件，也可以拆分为多个 PCD 文件。
- 地图中的每个点必须包含 X、Y、Z 坐标。
- 可选地为每个点包含强度或 RGB 值。
- 必须覆盖车辆的整个运行区域。还建议根据车载传感器的检测范围，额外包含缓冲区域。
- 为获得可靠的定位结果，分辨率应至少达到 0.2 m。
- 可以使用局部或全局坐标，但若使用 GNSS 数据定位，则必须使用全局坐标（地理配准）。

有关分块地图格式的更多信息，请参阅 [Autoware Universe 中 `map_loader` 的说明文档](https://github.com/autowarefoundation/autoware_core/blob/main/map/autoware_map_loader/README.md)。

!!! note

    Autoware 当前支持三种全局坐标系：[军事网格参考系统（MGRS）](https://en.wikipedia.org/wiki/Military_Grid_Reference_System)、[通用横轴墨卡托（UTM）](https://en.wikipedia.org/wiki/Universal_Transverse_Mercator_coordinate_system)和[日本平面直角坐标系](https://ja.wikipedia.org/wiki/%E5%B9%B3%E9%9D%A2%E7%9B%B4%E8%A7%92%E5%BA%A7%E6%A8%99%E7%B3%BB)。
    对于地理配准地图，优先使用 MGRS 坐标系。
    在 MGRS 坐标系地图中，每个点的 X、Y 坐标表示其在 100,000 米方格内的位置，Z 坐标表示该点的高程。

<a id="vector-map"></a>

### 矢量地图

矢量地图必须以文件形式提供，并满足以下要求：

- 必须采用 [Lanelet2](https://github.com/fzi-forschungszentrum-informatik/Lanelet2) 格式，并包含 [Autoware 所需的额外修改](https://github.com/autowarefoundation/autoware_lanelet2_extension/blob/main/autoware_lanelet2_extension/docs/lanelet2_format_extension.md)。
- 必须包含车道、交通信号灯、停止线、人行横道、停车位和停车场的形状与位置信息。
- 除道路起点或终点外，地图中的每个 lanelet 必须正确连接到其前驱、后继、左邻和右邻 lanelet。
- 地图中的每个 lanelet 必须包含交通规则信息，包括限速、路权、通行方向、关联交通信号灯、停止线和交通标志。
- 必须覆盖车辆的整个运行区域。

有关矢量地图创建的详细规范，请参阅[矢量地图创建需求规范文档](map-requirements/vector-map-requirements-overview/index.md)。

<a id="projection-information"></a>

### 投影信息

投影信息必须以文件形式提供，并满足以下要求：

- 必须采用 YAML 格式，在当前 Autoware Universe 实现中提供给 `map_projection_loader`。
- 文件必须包含以下信息：
  - 在局部和全局坐标之间转换所用的投影方法名称
  - 投影方法的参数（取决于投影方法）

详情请参阅 [Autoware Universe 中 `map_projection_loader` 的说明文档](https://github.com/autowarefoundation/autoware_core/tree/main/map/autoware_map_projection_loader/README.md)。
