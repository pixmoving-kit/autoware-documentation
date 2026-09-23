---
last_updated: 2026-01-21
interface_type: topic
interface_name: /control/control_mode_request
data_type_name: autoware_vehicle_msgs/srv/ControlModeCommand
data_type_link: https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_vehicle_msgs/srv/ControlModeCommand.srv
timeout: ---
---

# {{ interface_name }}

<a id="specifications"></a>

## 规格

{% include 'design/autoware-architecture-v1/interfaces/templates/service.jinja2' %}

<a id="description"></a>

## 说明

向车辆发送控制模式切换请求。控制模式用于管理车辆是否接受 Autoware 的指令。

下表列出各模式接受的指令。

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

<a id="service"></a>

## 服务

`stamp` 字段表示请求发送时间。`mode` 字段应使用上述有效值。

<a id="errors"></a>

## 错误

无效模式：如果指定了未知或不支持的模式，则拒绝请求，服务返回 `success=false` 的响应。

不安全的切换：如果无法安全切换模式，则拒绝请求，服务返回 `success=false` 的响应。
例如，存在硬件故障、紧急停车或较大的指令偏差时。

<a id="support"></a>

## 支持要求

此接口是必需的。
如果无法通过此接口切换模式，例如仅支持硬件开关，则应始终返回失败响应。

<a id="limitations"></a>

## 限制

不使用此接口进行模式切换，可能导致车辆行为突然改变。
在这种情况下，应选择指令差异较小的时候进行切换，例如车辆停止时。

<a id="use-cases"></a>

## 使用场景

- 切换到手动驾驶。
- 车辆停止时切换到 Autoware 控制。
- 手动驾驶期间切换到 Autoware 控制。

<a id="requirement"></a>

## 要求

- 如果车辆支持通过 Autoware 切换模式，至少应接受 MANUAL 和 AUTONOMOUS。
- 如果车辆不支持通过 Autoware 切换模式，应始终返回失败响应。
- 如果请求了未知或不支持的指令，返回失败响应。
- 如果无法安全切换模式，返回失败响应。

<a id="design"></a>

## 设计

- 仅按请求切换模式。

<a id="history"></a>

## 历史记录

| 日期 | 说明 |
| ---------- | -------------------------------- |
| 2026-01-21 | 首次以新格式发布。 |
