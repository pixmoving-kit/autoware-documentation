---
last_updated: 2026-01-21
interface_type: topic
interface_name: /vehicle/status/gear_status
data_type_name: autoware_vehicle_msgs/msg/GearReport
data_type_link: https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_vehicle_msgs/msg/GearReport.msg
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

获取车辆当前的挡位状态。可用状态见下表。
建议将 QoS 设置为 transient_local，并仅在状态发生变化时发布；但目前许多实现会周期性发布状态。
因此，请确保整个系统保持一致。

| 值 | 说明 |
| ------- | ------------------------------------------------------------------------------------------- |
| PARKING | 发动机或电机与车轮断开连接，并启用驻车机构。 |
| NEUTRAL | 发动机或电机与车轮断开连接。 |
| DRIVE | 发动机或电机与车轮连接，驱动车辆向前行驶。 |
| REVERSE | 发动机或电机与车轮连接，驱动车辆向后行驶。 |

<a id="message"></a>

## 消息

`stamp` 字段表示状态接收时间或 VCU 等硬件的时间。周期性发布时，应使用最新时间，而不是上次状态改变的时间。
`report` 字段应使用上述有效值。程序内部可以使用 `NONE`，但不会通过话题发送此值。
如果车辆具有特殊挡位类型，可以使用 `LOW`、`DRIVE_2` 等值，但需要专门的实现进行处理。

<a id="errors"></a>

## 错误

未知状态：如果车辆接口因连接丢失等原因无法获取状态，则停止发布状态并报告诊断错误。

无效状态：如果车辆接口收到未定义的状态，则停止发布状态并通过诊断信息报告。

硬件故障：如果车辆平台报告传感器故障，则报告诊断错误。

<a id="support"></a>

## 支持要求

此接口是必需的。如果车辆没有挡位，应模拟挡位行为。

<a id="limitations"></a>

## 限制

延迟：换挡是需要时间的机械过程，通常耗时 0.5s 到 2.0s。此处报告的状态反映实际接合的挡位，因此发送指令后会有延迟。Autoware 必须处理这一过渡阶段。

<a id="use-cases"></a>

## 使用场景

- 控制车辆进行自动驾驶。
- 向操作员显示当前挡位状态。

<a id="requirement"></a>

## 要求

- 支持获取车辆当前的挡位状态。
- 必要时支持车辆特有的挡位状态。

<a id="design"></a>

## 设计

- 支持四种常见挡位类型：PARKING、NEUTRAL、DRIVE 和 REVERSE。
- 未使用的值可用于特殊挡位类型。
- 必要时模拟挡位，以提高复用性。

<a id="history"></a>

## 历史记录

| 日期 | 说明 |
| ---------- | -------------------------------- |
| 2026-01-21 | 首次以新格式发布。 |
