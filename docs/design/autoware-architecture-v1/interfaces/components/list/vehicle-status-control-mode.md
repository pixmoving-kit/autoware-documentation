---
last_updated: 2026-01-21
interface_type: topic
interface_name: /vehicle/status/control_mode
data_type_name: autoware_vehicle_msgs/msg/ControlModeReport
data_type_link: https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_vehicle_msgs/msg/ControlModeReport.msg
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

获取车辆当前的控制模式状态。可用状态见下表。
建议将 QoS 设置为 transient_local，并仅在状态发生变化时发布；但目前许多实现会周期性发布状态。
因此，请确保整个系统保持一致。

请注意，忽略指令并不意味着车辆停止。
车辆仍可通过驾驶席等手动控制接口执行各种驾驶行为。

- 速度组
  - /control/command/control_cmd（longitudinal 字段）
  - /control/command/gear_cmd
- 转向组
  - /control/command/control_cmd（lateral 字段）
  - /control/command/turn_indicators_cmd
- 其他组
  - /control/command/hazard_lights_cmd
  - /vehicle/doors/command

| 控制模式 | 速度组 | 转向组 | 其他组 |
| ------------------------ | -------------- | -------------- | ------------ |
| AUTONOMOUS | 接受 | 接受 | 接受 |
| AUTONOMOUS_STEER_ONLY | 忽略 | 接受 | 接受 |
| AUTONOMOUS_VELOCITY_ONLY | 接受 | 忽略 | 接受 |
| MANUAL | 忽略 | 忽略 | 忽略 |
| DISENGAGED | 待确定 | 待确定 | 待确定 |
| NO_COMMAND | 待确定 | 待确定 | 待确定 |
| NOT_READY | 待确定 | 待确定 | 待确定 |

<a id="message"></a>

## 消息

`stamp` 字段表示状态接收时间或 VCU 等硬件的时间。周期性发布时，应使用最新时间，而不是上次状态改变的时间。
`mode` 字段应使用上述有效值。

<a id="errors"></a>

## 错误

未知状态：如果车辆接口因连接丢失等原因无法获取状态，则停止发布状态并报告诊断错误。

无效状态：如果车辆接口收到未定义的状态，则停止发布状态并通过诊断信息报告。

<a id="support"></a>

## 支持要求

必须支持 `MANUAL` 和 `AUTONOMOUS` 模式。

<a id="limitations"></a>

## 限制

延迟：从发送模式切换请求到此接口更新为新状态之间，必然存在延迟。Autoware 必须处理这一过渡阶段。

人工接管检测：某些车辆平台不会明确报告人工接管是否启用。在这种情况下，车辆接口通过比较指令和反馈来判断该状态，这可能引入检测延迟。

<a id="use-cases"></a>

## 使用场景

- 控制车辆进行自动驾驶。
- 向操作员显示当前控制模式状态。

<a id="requirement"></a>

## 要求

- 支持获取车辆当前的控制模式状态。
- 无法接收状态或收到未知状态时，通过诊断信息报告错误。

<a id="design"></a>

## 设计

无。

<a id="history"></a>

## 历史记录

| 日期 | 说明 |
| ---------- | -------------------------------- |
| 2026-01-21 | 首次以新格式发布。 |
