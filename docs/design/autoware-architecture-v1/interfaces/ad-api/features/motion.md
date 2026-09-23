<a id="motion"></a>

# 运动

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/motion/state') }}
- {{ link_ad_api('/api/motion/accept_start') }}

<a id="description"></a>

## 说明

此 API 管理车辆当前的行为。
应用可将车辆行为通知周围人员，并为操作员和乘客进行可视化展示。

<a id="states"></a>

## 状态

运动状态用于管理车辆的停车和起步。
车辆停止后，状态变为 STOPPED。
随后，当车辆准备起步（仍处于停止状态）时，状态变为 STARTING。
在此状态下，调用起步 API 会使状态变为 MOVING，车辆开始行驶。
通过这一机制，可以在车辆起步前加入播报等处理。
根据配置，状态也可能直接从 STOPPED 转为 MOVING。

![运动状态](motion/state.drawio.svg)

| 状态 | 说明 |
| ---------------- | ----------------------------------------------- |
| STOPPED | 车辆已停止。 |
| STARTING | 车辆仍停止，但正在准备起步。 |
| MOVING | 车辆正在行驶。 |
| BRAKING（待定） | 车辆正在强烈减速。 |
