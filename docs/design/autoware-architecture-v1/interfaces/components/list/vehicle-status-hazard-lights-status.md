---
last_updated: 2026-01-21
interface_type: topic
interface_name: /vehicle/status/hazard_lights_status
data_type_name: autoware_vehicle_msgs/msg/HazardLightsReport
data_type_link: https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_vehicle_msgs/msg/HazardLightsReport.msg
rate: 10 or N/A
qos_reliability: reliable
qos_durability: volatile or transient_local
qos_depth: 1
---

# {{ interface_name }}

<a id="specifications"></a>

## 规格

{% include 'design/autoware-architecture-v1/interfaces/templates/topic.jinja2' %}

<a id="description"></a>

## 说明

获取车辆当前的危险警告灯状态。状态为 `ENABLE` 或 `DISABLE`。
建议将 QoS 设置为 transient_local，并仅在状态发生变化时发布；但目前许多实现会周期性发布状态。
因此，请确保整个系统保持一致。

请注意，此状态表示危险警告灯系统在逻辑上是否启用，即功能是否处于激活状态，而不是灯泡的瞬时物理状态，即闪烁周期中灯泡点亮还是熄灭。因此，危险警告灯闪烁期间，状态通常会持续保持 ENABLE。

<a id="message"></a>

## 消息

`stamp` 字段表示状态接收时间或 VCU 等硬件的时间。周期性发布时，应使用最新时间，而不是上次状态改变的时间。
`report` 字段应使用上述有效值。

<a id="errors"></a>

## 错误

未知状态：如果车辆接口因连接丢失等原因无法获取状态，则停止发布状态并报告诊断错误。

无效状态：如果车辆接口收到未定义的状态，则停止发布状态并通过诊断信息报告。

硬件故障：如果车辆平台报告传感器故障，则报告诊断错误。

<a id="support"></a>

## 支持要求

此接口是必需的。如果车辆没有危险警告灯，应始终将其视为 `DISABLE`。

<a id="limitations"></a>

## 限制

逻辑状态：此接口报告逻辑启用状态，例如拨杆位置或系统状态。通常不会随着灯泡的实际闪烁同步切换 ENABLE/DISABLE。

<a id="use-cases"></a>

## 使用场景

- 控制车辆进行自动驾驶。
- 向操作员显示当前危险警告灯状态。

<a id="requirement"></a>

## 要求

- 支持获取车辆当前的危险警告灯状态。
- 无法接收状态或收到未知状态时，通过诊断信息报告错误。

<a id="design"></a>

## 设计

早期实现将转向灯和危险警告灯作为同一接口的不同状态管理，后来由于需要分别管理状态而拆分。

<a id="history"></a>

## 历史记录

| 日期 | 说明 |
| ---------- | -------------------------------- |
| 2026-01-21 | 首次以新格式发布。 |
