---
last_updated: 2026-01-21
interface_type: topic
interface_name: /control/command/gear_cmd
data_type_name: autoware_vehicle_msgs/msg/GearCommand
data_type_link: https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_vehicle_msgs/msg/GearCommand.msg
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

向车辆发送挡位切换指令。可用指令见下表。
建议将 QoS 设置为 transient_local，并仅在指令发生变化时发布；但目前许多实现会周期性发布指令。
因此，请确保整个系统保持一致。

| 值 | 说明 |
| ------- | ------------------------------------------------------------------------------------------- |
| PARKING | 发动机或电机与车轮断开连接，并启用驻车机构。 |
| NEUTRAL | 发动机或电机与车轮断开连接。 |
| DRIVE | 发动机或电机与车轮连接，驱动车辆向前行驶。 |
| REVERSE | 发动机或电机与车轮连接，驱动车辆向后行驶。 |

<a id="message"></a>

## 消息

`stamp` 字段表示指令发送时间。周期性发布时，应使用最新时间，而不是上次指令改变的时间。

`command` 字段应使用上述有效值。程序内部可以使用 `NONE`，但不会通过话题发送此值。
如果车辆具有特殊挡位类型，可以使用 `LOW`、`DRIVE_2` 等值，但需要专门的实现进行处理。

<a id="errors"></a>

## 错误

安全保护：如果车辆无法安全换挡，例如车辆尚未停止，则忽略该指令。

无效指令：如果车辆接口收到未定义的指令，则忽略该指令并报告诊断错误。

<a id="support"></a>

## 支持要求

此接口是必需的。如果车辆没有挡位，应模拟挡位行为。

<a id="limitations"></a>

## 限制

响应时间：机械换挡需要时间，通常为 0.5s - 2.0s。发送 gear_cmd 后，gear_status 不会立即改变。

模拟挡位：对于没有物理挡位的车辆（例如直驱电动车），NEUTRAL 状态可能通过逻辑模拟，而不代表机械连接断开。

<a id="use-cases"></a>

## 使用场景

- 控制车辆进行自动驾驶。
- 转发操作员的指令。

<a id="requirement"></a>

## 要求

- 支持向车辆发送挡位指令。
- 必要时支持车辆特有的挡位状态。
- 如果发送了不支持或未知的指令，通过诊断信息报告错误。

<a id="design"></a>

## 设计

- 支持四种常见挡位类型：PARKING、NEUTRAL、DRIVE 和 REVERSE。
- 未使用的值可用于特殊挡位类型。
- 必要时模拟挡位，以提高复用性。
- 为防止频繁换挡，在上一次挡位指令成功执行后的数秒内拒绝新的挡位指令。

<a id="history"></a>

## 历史记录

| 日期 | 说明 |
| ---------- | -------------------------------- |
| 2026-01-21 | 首次以新格式发布。 |
