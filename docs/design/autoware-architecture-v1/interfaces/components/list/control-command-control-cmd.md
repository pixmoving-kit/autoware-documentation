---
last_updated: 2026-01-21
interface_type: topic
interface_name: /control/command/control_cmd
data_type_name: autoware_control_msgs/msg/Control
data_type_link: https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_control_msgs/msg/Control.msg
rate: 33
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

向车辆发送控制指令。这是一种可通用于基于[阿克曼运动学模型][ackermann-kinematic-model]的车辆的指令格式。
此阶段仅执行平滑接近目标值等简单控制，不应期望其他控制行为。
例如，为达到目标速度而进行加速和减速的时序控制，应在前一个阶段完成。
因此，通常假定目标值的发送时间间隔足够短，可以进行线性插值。
如果您的车辆接口希望直接支持时序数据，请参与 [ControlHorizon](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_control_msgs/msg/ControlHorizon.msg) 消息的讨论。

[ackermann-kinematic-model]: ../../../../../tutorials/integrating-autoware/creating-vehicle-interface-package/ackermann-kinematic-model.md

<a id="message"></a>

## 消息

消息详情请参阅 [autoware_control_msgs 的 README](https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_control_msgs/README.md)。

给定时刻的速度、加速度和加加速度必须预先规划，并且彼此一致；通常由规划或控制组件完成。
如果这些值不一致，哪个值优先取决于车辆的实现。

发送最新指令会使之前的指令失效，因此不同时间戳的指令不会作为时序数据保留。
仅最新收到的指令有效，而不是时间戳最新的指令。

<a id="errors"></a>

## 错误

指令超时：如果指令的发送频率不足以控制车辆当前速度，车辆将执行紧急停车。

超出范围：如果指令超出给定的物理限制，其值将被限制为允许的最大值。

<a id="support"></a>

## 支持要求

此接口是必需的。如果车辆仅提供专用接口，应提供转换器。
对于使用常规油门踏板和制动踏板控制的车辆，可考虑使用 [autoware_raw_vehicle_cmd_converter](https://github.com/autowarefoundation/autoware_universe/tree/main/vehicle/autoware_raw_vehicle_cmd_converter)。

<a id="limitations"></a>

## 限制

指令过滤：可使用变化率限制器过滤指令，以防止物理上无法实现或不安全的急加速、急转向。

<a id="use-cases"></a>

## 使用场景

- 控制车辆进行自动驾驶。
- 转发操作员的指令。

<a id="requirement"></a>

## 要求

- 支持向车辆发送控制指令。
- 根据控制模式忽略控制指令。
- 在以下情况下通过诊断信息报告错误：
  - 话题频率过低或过高。
  - 收到不可接受的目标值。
  - 无法达到目标值。

<a id="design"></a>

## 设计

待补充：话题频率设置依据的说明。

<a id="history"></a>

## 历史记录

| 日期 | 说明 |
| ---------- | -------------------------------- |
| 2026-01-21 | 首次以新格式发布。 |
