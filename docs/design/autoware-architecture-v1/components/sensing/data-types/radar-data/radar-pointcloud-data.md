<a id="radar-pointcloud-data-pre-processing-design"></a>

# 雷达点云数据预处理设计

<a id="overview"></a>

## 概述

<a id="pipeline"></a>

### 流水线

此图描述雷达点云的预处理流水线。

![radar-pointcloud-sensing](image/radar-pointcloud-sensing.drawio.svg)

<a id="interface"></a>

### 接口

- 输入
  - 来自设备的雷达数据
  - 自车运动的 Twist 信息
- 输出
  - 动态雷达点云（`ros-perception/radar_msgs/msg/RadarScan.msg`）
  - 过滤噪声后的雷达点云（`sensor_msgs/msg/Pointcloud2.msg`）

<a id="note"></a>

### 注意

- 在传感层中，雷达预处理软件包通过传感器坐标系下的 `ros-perception/radar_msgs/msg/RadarScan.msg` 消息类型过滤噪声。
- 为了让激光雷达软件包使用雷达点云数据，我们建议提供转换器，从 `ros-perception/radar_msgs/msg/RadarScan.msg` 创建 `sensor_msgs/msg/Pointcloud2.msg`。

<a id="reference-implementations"></a>

## 参考实现

<a id="data-message-for-radars"></a>

### 雷达数据消息

Autoware 使用的雷达目标数据类型为 [radar_msgs/msg/RadarScan.msg](https://github.com/ros-perception/radar_msgs/blob/ros2/msg/RadarScan.msg)。
详情见[雷达数据消息](reference-implementations/data-message.md)。

<a id="device-driver-for-radars"></a>

### 雷达设备驱动程序

Autoware 雷达驱动程序支持 `ros-perception/radar_msgs/msg/RadarScan.msg` 和 `autoware_auto_perception_msgs/msg/TrackedObjects.msg`。

详情见[雷达设备驱动程序](reference-implementations/device-driver.md)。

<a id="basic-noise-filter"></a>

### 基础噪声滤波器

- [radar_threshold_filter](https://github.com/autowarefoundation/autoware_universe/tree/main/sensing/autoware_radar_threshold_filter)

此软件包通过阈值去除低幅值、边缘角度和距离过近的点云噪声。
噪声取决于雷达设备和安装位置。

<a id="filter-to-staticdynamic-pointcloud"></a>

### 静态/动态点云过滤

- [radar_static_pointcloud_filter](https://github.com/autowarefoundation/autoware_universe/tree/main/sensing/autoware_radar_static_pointcloud_filter)

此软件包使用多普勒速度和自车运动提取静态/动态雷达点云。
静态雷达点云可用于 NDT 扫描匹配等定位方法，动态雷达点云可用于动态目标检测。

<a id="message-converter-from-radarscan-to-pointcloud2"></a>

### 从 RadarScan 到 Pointcloud2 的消息转换器

- [radar_scan_to_pointcloud2](https://github.com/autowarefoundation/autoware_universe/tree/main/sensing/autoware_radar_scan_to_pointcloud2)

为方便在现有激光雷达软件包中使用雷达点云，我们建议提供 `radar_scan_to_pointcloud2_convertor` 软件包，将 `ros-perception/radar_msgs/msg/RadarScan.msg` 转换为 `sensor_msgs/msg/Pointcloud2.msg`。

| | 激光雷达软件包 | 雷达软件包 |
| :--------: | :-------------------------------: | :-------------------------------------------: |
| 消息 | `sensor_msgs/msg/Pointcloud2.msg` | `ros-perception/radar_msgs/msg/RadarScan.msg` |
| 坐标 | (x, y, z) | (r, θ, φ) |
| 数值 | 强度 | 幅值、多普勒速度 |

考虑的使用场景包括：

- 将 [pointcloud_preprocessor](https://github.com/autowarefoundation/autoware_universe/tree/main/sensing/autoware_pointcloud_preprocessor) 用于雷达扫描。
- 在无激光雷达（相机 + 雷达）系统中，对雷达点应用[地面分割](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_ground_segmentation)等障碍物分割方法。

<a id="appendix"></a>

## 附录

<a id="discussion"></a>

### 讨论

雷达架构设计讨论如下。

- [讨论 2531](https://github.com/orgs/autowarefoundation/discussions/2531)
- [讨论 2532](https://github.com/orgs/autowarefoundation/discussions/2532)。
