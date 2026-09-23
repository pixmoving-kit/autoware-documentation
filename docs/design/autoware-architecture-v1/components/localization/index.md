定位组件设计文档

<a id="abstract"></a>

## 摘要

<a id="1-requirements"></a>

## 1. 需求

定位旨在估计车辆的位姿、速度和加速度。

目标：

- 提出一种能够尽可能长时间估计车辆位姿、速度和加速度的系统。
- 提出一种能够诊断估计稳定性的系统，并在估计结果不可靠时向错误监控系统发送警告消息。
- 设计可适用于多种传感器配置的车辆定位功能。

非目标：

- 本设计文档不旨在开发具有以下特性的定位系统：
  - 在所有环境中均不会出错
  - 在预定义 ODD（运行设计域）之外工作
  - 性能超出自动驾驶所需水平

<a id="2-sensor-configuration-examples"></a>

## 2. 传感器配置示例

本节展示传感器配置示例及其预期性能。
每种传感器都有优缺点，但融合多种传感器可以提升整体性能。

<a id="3d-lidar-pointcloud-map"></a>

### 3D LiDAR + 点云地图

<a id="expected-situation"></a>

#### 预期场景

- 车辆位于城市等结构丰富的环境中

<a id="situations-that-can-make-the-system-unstable"></a>

#### 可能导致系统不稳定的情况

- 车辆处于乡村、高速公路或隧道等缺乏结构特征的环境中
- 地图创建后环境发生变化，例如积雪或建筑物的新建/拆除。
- 周围物体被遮挡
- 车辆周围存在 LiDAR 无法检测的物体，例如玻璃窗、反光物体或吸光物体（深色物体）
- 环境中存在与车辆 LiDAR 传感器频率相同的激光束

<a id="functionality"></a>

#### 功能

- 系统可估计车辆在点云地图中的位置，误差约为 10cm。
- 系统可在夜间运行。

<a id="3d-lidar-or-camera-vector-map"></a>

### 3D LiDAR 或相机 + 矢量地图

<a id="expected-situation_1"></a>

#### 预期场景

- 高速公路或普通道路等白色标线清晰、弯曲程度较小的道路。

<a id="situations-that-can-make-the-system-unstable_1"></a>

#### 可能导致系统不稳定的情况

- 白色标线磨损，或被雨雪覆盖
- 交叉路口等曲率较大的路段
- 雨水或涂漆导致路面反射发生显著变化

<a id="functionalities"></a>

#### 功能

- 沿横向修正车辆位置。
- 纵向位姿修正可能不准确，但可通过与 GNSS 融合解决。

### GNSS

<a id="expected-situation_2"></a>

#### 预期场景

- 车辆处于乡村等开阔环境中，周围物体很少或没有物体。

<a id="situation-that-can-make-the-system-unstable"></a>

#### 可能导致系统不稳定的情况

- GNSS 信号被隧道或建筑物等周围物体遮挡。

<a id="functionality_1"></a>

#### 功能

- 系统可估计车辆在世界坐标系中的位置，误差约为 10m。
- 配备 RKT-GNSS（实时动态全球导航卫星系统）后，精度可提高到约 10cm。
- 此配置的系统无需环境地图（点云地图或矢量地图）也能工作。

<a id="camera-visual-odometry-visual-slam"></a>

### 相机（视觉里程计、视觉 SLAM）

<a id="expected-situation_3"></a>

#### 预期场景

- 车辆处于城市等视觉特征丰富的环境中。

<a id="situations-that-can-make-the-system-unstable_2"></a>

#### 可能导致系统不稳定的情况

- 车辆处于缺乏纹理的环境中。
- 车辆被其他物体包围。
- 相机观测到显著的光照变化，例如阳光、其他车辆的前照灯或接近隧道出口时引起的变化。
- 车辆处于黑暗环境中。

<a id="functionality_2"></a>

#### 功能

- 系统可通过跟踪视觉特征估计里程计信息。

<a id="wheel-speed-sensor"></a>

### 轮速传感器

<a id="expected-situation_4"></a>

#### 预期场景

- 车辆在平坦、光滑的道路上行驶。

<a id="situations-that-can-make-the-system-unstable_3"></a>

#### 可能导致系统不稳定的情况

- 车辆在湿滑或颠簸道路上行驶，可能导致轮速观测不正确。

<a id="functionality_3"></a>

#### 功能

- 系统可获取车速并估计行驶距离。

### IMU

<a id="expected-environments"></a>

#### 预期环境

- 平坦、光滑的道路

<a id="situations-that-can-make-the-system-unstable_4"></a>

#### 可能导致系统不稳定的情况

- IMU 存在随环境温度变化的偏置[^1]，可能造成传感器观测不正确或里程计漂移。

[^1]: 有关偏置的更多信息，请参阅 [VectorNav IMU 规格页面](https://www.vectornav.com/resources/inertial-navigation-primer/specifications--and--error-budgets/specs-imuspecs)。

<a id="functionality_4"></a>

#### 功能

- 系统可观测加速度和角速度。
- 通过对这些观测值积分，系统可估计局部位姿变化并实现航位推算

<a id="geomagnetic-sensor"></a>

### 地磁传感器

<a id="expected-situation_5"></a>

#### 预期场景

- 车辆处于磁噪声较低的环境中

<a id="situations-that-can-make-the-system-unstable_5"></a>

#### 可能导致系统不稳定的情况

- 车辆处于磁噪声较高的环境中，例如包含钢筋或其他产生电磁波材料的建筑物或结构的环境。

<a id="functionality_5"></a>

#### 功能

- 系统可估计车辆在世界坐标系中的方向。

<a id="magnetic-markers"></a>

### 磁性标记

<a id="expected-situation_6"></a>

#### 预期场景

- 车辆处于安装有磁性标记的环境中。

<a id="situations-where-the-system-becomes-unstable"></a>

#### 系统变得不稳定的情况

- 标记未得到维护。

<a id="functionality_6"></a>

#### 功能

- 检测磁性标记可获取车辆在世界坐标系中的位置。
- 即使道路被积雪覆盖，系统仍可工作。

<a id="3-requirements"></a>

## 3. 需求

- 通过实现不同模块，可以使用多种传感器配置和算法。
- 定位系统可从模糊的初始位置开始位姿估计。
- 系统可给出可靠的初始位置估计。
- 系统可管理初始位置估计状态（未初始化、可初始化或不可初始化），并向错误监控器报告。

<a id="4-architecture"></a>

## 4. 架构

<a id="abstract_1"></a>

### 概述

定义了两种架构：“必需架构”和“推荐架构”。“必需架构”仅包含支持各种定位算法所需的输入和输出。为提高各模块的可复用性，“推荐架构”一节定义了所需组件，并提供更详细的说明。

<a id="required-architecture"></a>

### 必需架构

![必需架构](../../image/localization/required-architecture.png)

<a id="input"></a>

#### 输入

- 传感器消息
  - 例如 LiDAR、相机、GNSS、IMU、CAN 总线等。
  - 为便于复用，数据类型应采用 ROS 基本类型
- 地图数据
  - 例如点云地图、lanelet2 地图、特征地图等。
  - 应根据使用场景和传感器配置选择地图格式
  - 请注意，某些特定场景不需要地图数据（例如仅使用 GNSS 定位）
- tf、static_tf
  - map 坐标系
  - base_link 坐标系

<a id="output"></a>

#### 输出

- 带时间戳和协方差的位姿
  - map 坐标系中的车辆位姿、协方差和时间戳
  - 频率约 50Hz 及以上（取决于规划和控制组件的要求）
- 带时间戳和协方差的速度旋量
  - base_link 坐标系中的车速、协方差和时间戳
  - 频率约 50Hz 及以上
- 带时间戳和协方差的加速度
  - base_link 坐标系中的加速度、协方差和时间戳
  - 频率约 50Hz 及以上
- 诊断信息
  - 指示定位模块是否正常工作的诊断信息
- tf
  - map 到 base_link 的 tf

<a id="recommended-architecture"></a>

### 推荐架构

![推荐架构](../../image/localization/recommended-architecture.png)

<a id="pose-estimator"></a>

#### 位姿估计器

- 将外部传感器观测与地图匹配，估计车辆在 map 坐标系中的位姿
- 向 `PoseTwistFusionFilter` 提供得到的位姿及其协方差

<a id="twist-accel-estimator"></a>

#### 速度与加速度估计器

- 生成车速、角速度、加速度、角加速度及其协方差
  - 可以用单个模块同时处理速度旋量和加速度，也可以创建两个独立模块，架构由开发者选择
- 速度旋量估计器根据内部传感器观测生成速度和角速度
- 加速度估计器根据内部传感器观测生成加速度和角加速度

<a id="kinematics-fusion-filter"></a>

#### 运动学融合滤波器

- 融合以下两类信息，计算并生成最可能的位姿、速度、加速度及其协方差：
  - 位姿估计器获取的位姿。
  - 速度与加速度估计器获取的速度和加速度
- 根据位姿估计结果生成 map 到 base_link 的 tf

<a id="localization-diagnostics"></a>

#### 定位诊断

- 融合多个定位模块提供的信息，监控并保证位姿估计的稳定性和可靠性
- 向错误监控器报告错误状态

<a id="tf-tree"></a>

#### TF 树

![TF 树](../../image/localization/tf-tree.png)

| 坐标系 | 含义 |
| :-------: | :--------------------------------------------------------------------------------------------- |
| earth | ECEF（地心地固坐标系） |
| map | 地图坐标原点（例如 MGRS 原点） |
| viewer | 用于 rviz 的用户定义坐标系 |
| base_link | 自车的参考位姿（后轴中心在地面上的投影） |
| sensor | 各传感器的参考位姿 |

只要保持上述 tf 结构，开发者可按需添加 odom 或 base_footprint 等其他坐标系。

<a id="the-localization-modules-ideal-functionality"></a>

### 定位模块的理想功能

- 定位模块应为控制、规划和感知提供位姿、速度和加速度。
- 延迟和时间错位应足够小或可调，使估计值能够用于 ODD（运行设计域）内的控制。
- 定位模块应输出固定坐标系下的位姿。
- 各传感器应彼此独立，以便替换。
- 定位模块应提供状态，表明自动驾驶车辆能否通过自主功能或地图信息运行。
- 工具或手册应说明如何正确设置定位模块参数
- 应提供有效的标定参数，用于对齐不同坐标系或位姿坐标以及传感器时间戳。

### KPI

为保持足以保障安全运行的位姿估计性能，考虑以下指标：

- 安全
  - 在 ODD 内位姿估计达到所需精度的行驶距离，占 ODD 内总行驶距离的百分比。
  - 在 ODD 内定位模块无法估计位姿时的异常检出率
  - 检测车辆驶出 ODD 的准确率，以百分比表示。
- 计算负载
- 延迟

<a id="5-interface-and-data-structure"></a>

## 5. 接口与数据结构

<a id="6-concerns-assumptions-and-limitations"></a>

## 6. 注意事项、假设与限制

<a id="prerequisites-of-sensors-and-inputs"></a>

### 传感器与输入的前提条件

<a id="sensor-prerequisites"></a>

#### 传感器前提条件

- 输入数据没有缺陷。
  - IMU 等内部传感器观测持续保持适当频率。
- 输入数据具有正确且精确的时间戳。
  - 时间戳不精确可能导致位姿估计不准确或不稳定。
- 传感器正确安装于精确位置，且可通过 TF 获取其信息。
  - 传感器位置不准确可能导致估计结果不正确或不稳定。
  - 需要传感器标定框架来正确获取传感器位置。

<a id="map-prerequisites"></a>

#### 地图前提条件

- 地图包含足够的信息。
  - 地图信息不足可能导致位姿估计不稳定。
  - 需要测试框架检查地图是否包含足够的位姿估计信息。
- 地图与实际环境没有显著差异。
  - 如果实际环境中的物体与地图不同，位姿估计可能不稳定。
  - 地图需要随新物体和季节变化进行更新。
- 地图必须对齐到统一坐标系，或配备对齐框架。
  - 如果使用多个不同坐标系的地图，它们之间的错位会影响定位性能。

<a id="computational-resources"></a>

#### 计算资源

- 应提供足够的计算资源，以保持精度和计算速度。
