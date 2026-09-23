---
last_updated: 2026-01-21
interface_type: topic
interface_name: /vehicle/status/steering_status
data_type_name: autoware_vehicle_msgs/msg/SteeringReport
data_type_link: https://github.com/autowarefoundation/autoware_msgs/blob/main/autoware_vehicle_msgs/msg/SteeringReport.msg
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

获取车辆当前的转向状态。

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

转换精度：此值通常根据方向盘转角和转向传动比计算，传动比可能可变或非线性。因此，其精度取决于传动比模型参数是否正确。

机械间隙：转向柱中的机械间隙可能导致方向盘的小幅运动无法引起实际车轮转动，从而使报告的状态与物理车轮转角存在差异。

轮胎变形：此值表示车轮总成的运动学转角，而不是轮胎接地面的实际侧偏角。

<a id="use-cases"></a>

## 使用场景

- 控制车辆进行自动驾驶。
- 向操作员显示当前转向状态。

<a id="requirement"></a>

## 要求

- 支持获取车辆当前的转向状态。
- 无法接收状态或收到未知状态时，通过诊断信息报告错误。

<a id="design"></a>

## 设计

坐标系与符号约定：此接口遵循标准车辆坐标系（ISO 8855 / ROS REP-103）。

- 正值（+）表示**逆时针**旋转（左转）。
- 负值（-）表示**顺时针**旋转（右转）。

数据来源：报告转向轮转角，取前轮平均值或虚拟中心车轮的转角。
通常使用线性或可变传动比模型，根据方向盘转角传感器数据转换得到。

<a id="history"></a>

## 历史记录

| 日期 | 说明 |
| ---------- | -------------------------------- |
| 2026-01-21 | 首次以新格式发布。 |
