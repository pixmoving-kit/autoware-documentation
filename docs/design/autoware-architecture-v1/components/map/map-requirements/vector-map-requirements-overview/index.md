<a id="vector-map-creation-requirement-specifications"></a>

# 矢量地图创建需求规范

<a id="overview"></a>

## 概述

Autoware 依赖驾驶环境的高精度点云地图和矢量地图，执行定位、路线规划、交通信号灯检测以及行人和其他车辆轨迹预测等任务。

矢量地图包含道路网络、车道几何形状和交通信号灯的高精度信息，路线规划、交通信号灯检测以及其他车辆和行人轨迹预测都需要这些信息。

矢量地图使用 [lanelet2_extension](https://github.com/autowarefoundation/autoware_lanelet2_extension/blob/main/autoware_lanelet2_extension/docs/lanelet2_format_extension.md)，它基于 [lanelet2](https://github.com/fzi-forschungszentrum-informatik/Lanelet2) 格式，并针对 Autoware 进行了扩展。

矢量地图使用的图元（基本组件）在 [Web.Auto 文档 - 什么是 Lanelet2](https://docs.web.auto/en/user-manuals/vector-map-builder/introduction#what-is-lanelet2) 中说明。以下**矢量地图创建需求规范**以掌握这些知识为前提。

本规范是一组矢量地图创建要求，确保 Autoware 按用户预期安全地自动驾驶。有关创建 Lanelet2 格式 .osm 文件的方法，请参阅[创建矢量地图](../../../../../../tutorials/integrating-autoware/creating-maps/creating-vector-map/index.md)。

<a id="handling-of-the-requirement-specification"></a>

## 需求规范的使用

哪些需求适用完全取决于车辆上的 Autoware 系统配置。创建矢量地图之前，必须明确搭载该系统的车辆在各种环境下应如何行动。

其次，必须遵守自动驾驶车辆运营所在国家的法律。你有责任根据法律选择适用的下列要求。

<a id="caution"></a>

### 注意

- 交通标志和路面标线示例采用日本规范。请替换为所在国家使用的标志和标线。
- 给出的范围和距离是最小值。请确定符合所在国家法律的数值。此外，这些最小值可能随自动驾驶车辆的最大速度而变化。

<a id="list-of-requirement-specifications"></a>

## 需求规范列表

| 类别 | ID | 需求 |
| --------------------------------------------------- | -------- | ------------------------------------------------------- |
| [车道类别](category_lane.md) | vm-01-01 | Lanelet 基础 |
| | vm-01-02 | 变道许可 |
| | vm-01-03 | 共享 Linestring |
| | vm-01-04 | 对向车道共享道路中心线 |
| | vm-01-05 | 车道几何形状 |
| | vm-01-06 | 标线位置（1） |
| | vm-01-07 | 标线位置（2） |
| | vm-01-08 | 标线位置（3） |
| | vm-01-09 | 限速 |
| | vm-01-10 | 中心线 |
| | vm-01-11 | 中心线连接（1） |
| | vm-01-12 | 中心线连接（2） |
| | vm-01-13 | 无道路中心线的道路（1） |
| | vm-01-14 | 无道路中心线的道路（2） |
| | vm-01-15 | 路肩 |
| | vm-01-16 | 路肩共享 Linestring |
| | vm-01-17 | 路侧带 |
| | vm-01-18 | 路侧带共享 Linestring |
| | vm-01-19 | 人行道 |
| [停止线类别](category_stop_line.md) | vm-02-01 | 停止线对齐 |
| | vm-02-02 | 停车标志 |
| [交叉路口类别](category_intersection.md) | vm-03-01 | 交叉路口标准 |
| | vm-03-02 | Lanelet 转向方向与虚拟线 |
| | vm-03-03 | 交叉路口内的 Lanelet 宽度 |
| | vm-03-04 | 在交叉路口中创建 Lanelet |
| | vm-03-05 | 交叉路口内的 Lanelet 分割 |
| | vm-03-06 | 交叉路口内的引导线 |
| | vm-03-07 | 交叉路口内的多个 lanelet |
| | vm-03-08 | 交叉路口区域范围 |
| | vm-03-09 | 交叉路口内的 Lanelet 范围 |
| | vm-03-10 | 路权（有信号灯） |
| | vm-03-11 | 路权（无信号灯） |
| | vm-03-12 | 路权补充说明 |
| | vm-03-13 | 从私人区域汇入及人行道 |
| | vm-03-14 | 道路标线 |
| | vm-03-15 | 自行车专用道 |
| [交通信号灯类别](category_traffic_light.md) | vm-04-01 | 交通信号灯基础 |
| | vm-04-02 | 交通信号灯位置和尺寸 |
| | vm-04-03 | 交通信号灯灯泡 |
| [人行横道类别](category_crosswalk.md) | vm-05-01 | 横跨道路的人行横道 |
| | vm-05-02 | 带行人信号灯的人行横道 |
| | vm-05-03 | 人行横道安全减速 |
| | vm-05-04 | 围栏 |
| [区域类别](category_area.md) | vm-06-01 | 缓冲区 |
| | vm-06-02 | 禁止停车标志 |
| | vm-06-03 | 禁止停止标志 |
| | vm-06-04 | 禁止停止路段 |
| | vm-06-05 | 检测区域 |
| [其他类别](category_others.md) | vm-07-01 | 矢量地图创建范围 |
| | vm-07-02 | 突入道路行人的检测范围 |
| | vm-07-03 | 护栏、护管和围栏 |
| | vm-07-04 | 椭球高 |
