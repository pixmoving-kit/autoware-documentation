---
last_updated: 2026-01-21
interface_type: topic
interface_name: /vehicle/status/velocity_status
data_type_name: autoware_vehicle_msgs/msg/VelocityReport
data_type_link: https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_vehicle_msgs/msg/VelocityReport.msg
rate: 10
qos_reliability: reliable
qos_durability: volatile
qos_depth: 1
---

# {{ interface_name }}

<a id="specifications"></a>

## 规格

{% include 'design/autoware-architecture-v1/interfaces/templates/topic.jinja2' %}

<a id="description"></a>

## 说明

获取车辆当前的速度状态。

<a id="message"></a>

## 消息

详情请参阅[消息定义]({{ data_type_link }})。

<a id="errors"></a>

## 错误

未知状态：如果车辆接口因连接丢失等原因无法获取状态，则停止发布状态并报告诊断错误。

硬件故障：如果车辆平台报告传感器故障，则报告诊断错误。

<a id="support"></a>

## 支持要求

此接口是必需的。

<a id="limitations"></a>

## 限制

待确认。

<a id="use-cases"></a>

## 使用场景

- 控制车辆进行自动驾驶。
- 向操作员显示当前速度状态。

<a id="requirement"></a>

## 要求

- 支持获取车辆当前的速度状态。
- 无法接收状态或收到未知状态时，通过诊断信息报告错误。

<a id="design"></a>

## 设计

坐标系与符号约定：此接口遵循标准车辆坐标系（ISO 8855 / ROS REP-103）。

- 纵向速度：正值（+）表示**向前**运动。负值（-）表示**向后**运动。
- 横向速度：正值（+）表示**向左**运动。负值（-）表示**向右**运动。
- 航向角速度：正值（+）表示**逆时针**旋转（左转）。

<a id="history"></a>

## 历史记录

| 日期 | 说明 |
| ---------- | -------------------------------- |
| 2026-01-21 | 首次以新格式发布。 |
