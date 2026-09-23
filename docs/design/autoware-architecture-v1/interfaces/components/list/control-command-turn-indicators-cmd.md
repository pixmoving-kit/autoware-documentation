---
last_updated: 2026-01-21
architecture: autoware components
interface_type: topic
interface_name: /control/command/turn_indicators_cmd
data_type_name: autoware_vehicle_msgs/msg/TurnIndicatorsCommand
data_type_link: https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_vehicle_msgs/msg/TurnIndicatorsCommand.msg
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

向车辆发送转向灯切换指令。指令为 `ENABLE_RIGHT`、`ENABLE_LEFT` 或 `DISABLE`。
建议将 QoS 设置为 transient_local，并仅在指令发生变化时发布；但目前许多实现会周期性发布指令。
因此，请确保整个系统保持一致。

<a id="message"></a>

## 消息

`stamp` 字段表示指令发送时间。周期性发布时，应使用最新时间，而不是上次指令改变的时间。
`command` 字段应使用上述有效值。程序内部可以使用 `NO_COMMAND`，但不会通过话题发送此值。

<a id="errors"></a>

## 错误

无效指令：如果车辆接口收到未定义的指令，则忽略该指令并报告诊断错误。

<a id="support"></a>

## 支持要求

此接口是必需的。如果车辆没有转向灯，应始终忽略该指令。

<a id="limitations"></a>

## 限制

- 对于车辆实际的灯光状态，危险警告灯可能具有更高优先级。

<a id="use-cases"></a>

## 使用场景

- 控制车辆进行自动驾驶。
- 转发操作员的指令。

<a id="requirement"></a>

## 要求

- 支持向车辆发送转向灯指令。
- 如果发送了不支持或未知的指令，通过诊断信息报告错误。

<a id="design"></a>

## 设计

与危险警告灯分离：危险警告灯和转向灯使用独立接口，以便分别管理状态。例如，自动驾驶软件栈可能为路线行驶请求“左转”，而安全系统同时因紧急情况请求“启用危险警告灯”。这样可以简化逻辑，使自动驾驶软件栈和安全系统无需担心各自的状态被对方的指令覆盖。

互斥状态：与危险警告灯不同，转向灯具有方向性，左、右两种状态互斥。因此，它们在单个指令接口中定义为枚举，而不是独立的布尔标志。

<a id="history"></a>

## 历史记录

| 日期 | 说明 |
| ---------- | -------------------------------- |
| 2026-01-21 | 首次以新格式发布。 |
