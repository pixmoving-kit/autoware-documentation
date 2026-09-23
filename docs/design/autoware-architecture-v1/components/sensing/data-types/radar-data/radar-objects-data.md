<a id="radar-objects-data-pre-processing-design"></a>

# 雷达目标数据预处理设计

<a id="overview"></a>

## 概述

<a id="pipeline"></a>

### 流水线

此图描述雷达目标的预处理流水线。

![radar-objects-sensing](image/radar-objects-sensing.drawio.svg)

<a id="interface"></a>

### 接口

- 输入
  - 来自设备的雷达数据
  - 自车运动的 Twist 信息
- 输出
  - 合并后的雷达目标信息

<a id="note"></a>

### 注意

- 雷达预处理软件包通过传感器坐标系下的 `ros-perception/radar_msgs/msg/RadarTrack.msg` 消息类型过滤噪声。
- 建议通过消息转换器将各传感器坐标系转换到 base_link。
- 如果存在多个雷达目标，目标合并软件包将连接这些目标。

<a id="input"></a>

## 输入

Autoware 使用的雷达目标数据类型为 [radar_msgs/msg/RadarTracks.msg](https://github.com/ros-perception/radar_msgs/blob/ros2/msg/RadarTracks.msg)。
详情见[雷达数据消息](reference-implementations/data-message.md)。

<a id="reference-implementations"></a>

## 参考实现

<a id="device-driver-for-radars"></a>

### 雷达设备驱动程序

Autoware 雷达驱动程序支持 `ros-perception/radar_msgs/msg/RadarScan.msg` 和 `autoware_auto_perception_msgs/msg/TrackedObjects.msg`。

详情见[雷达设备驱动程序](reference-implementations/device-driver.md)。

<a id="noise-filter"></a>

### 噪声滤波器

- [radar_tracks_noise_filter](https://github.com/autowarefoundation/autoware_universe/tree/main/sensing/autoware_radar_tracks_noise_filter)

雷达可以通过多普勒速度检测 x 轴速度，但无法检测 y 轴速度。某些雷达可以在设备内部估计 y 轴速度，但有时精度不足。此软件包通过 y 轴阈值滤波器将这些目标视为噪声处理。

<a id="message-converter"></a>

### 消息转换器

- [radar_tracks_msgs_converter](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_radar_tracks_msgs_converter)

此软件包将 `radar_msgs/msg/RadarTracks` 转换为 `autoware_auto_perception_msgs/msg/DetectedObject`，并执行自车运动补偿和坐标变换。

<a id="object-merger"></a>

### 目标合并器

- [object_merger](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_object_merger)

此软件包可以合并 2 个 `autoware_auto_perception_msgs/msg/DetectedObject` 话题。

- [simple_object_merger](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_simple_object_merger)

此软件包可以简单地合并多个 `autoware_auto_perception_msgs/msg/DetectedObject` 话题。
与 [object_merger](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_object_merger) 不同，此软件包不使用关联算法，能够以较低计算开销完成合并。

- [topic_tools](https://github.com/ros-tooling/topic_tools)

如果车辆有 1 个雷达，可以使用 `topic_tools` 中的话题转发工具。
示例如下。

```xml
<launch>
  <group>
    <push-ros-namespace namespace="radar"/>
    <group>
      <push-ros-namespace namespace="front_center"/>
      <include file="$(find-pkg-share example_launch)/launch/ars408.launch.xml">
        <arg name="interface" value="can0" />
      </include>
    </group>
    <node pkg="topic_tools" exec="relay" name="radar_relay" output="log" args="front_center/detected_objects detected_objects"/>
  </group>
</launch>
```

<a id="appendix"></a>

## 附录

<a id="discussion"></a>

### 讨论

雷达架构设计讨论如下。

- [讨论 2531](https://github.com/orgs/autowarefoundation/discussions/2531)
- [讨论 2532](https://github.com/orgs/autowarefoundation/discussions/2532)。
