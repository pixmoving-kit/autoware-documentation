<a id="planning-factors"></a>

# 规划因素

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/planning/velocity_factors') }}
- {{ link_ad_api('/api/planning/steering_factors') }}

<a id="description"></a>

## 说明

此 API 管理车辆规划的行为。
应用可将车辆行为通知周围人员，并为操作员和乘客进行可视化展示。

<a id="velocity-factors"></a>

## 速度因素

速度因素是一个数组，包含车辆停车或减速行为的信息。
每个因素都有行为类型，具体如下。
某些行为类型还具有 sequence 和 details 等附加信息。

| 行为 | 说明 |
| --------------------------- | ----------------------------------------------------------------------------------- |
| surrounding-obstacle | 车辆紧邻的周围区域存在障碍物。 |
| route-obstacle | 前方路线沿途存在障碍物。 |
| intersection | 行驶路径所经的其他车道中存在障碍物。 |
| crosswalk | 人行横道上存在障碍物。 |
| rear-check | 后方驾驶员视野盲区中存在障碍物。 |
| user-defined-attention-area | 预定义的关注区域内存在障碍物。 |
| roundabout | 环岛内存在障碍物。 |
| no-stopping-area | 禁停区域之后没有足够空间。 |
| stop-sign | 因停车标志而停车。 |
| traffic-signal | 因交通信号灯而停车。 |
| v2x-gate-area | 因门控区域而停车。sequence 为 enter 或 leave，details 为 v2x 类型。 |
| merge | 在车道汇入前停车。 |
| sidewalk | 在穿越人行道前停车。 |
| lane-change | 变道。 |
| avoidance | 为避让当前车道中的障碍物而改变路径。 |
| emergency-operation | 因操作员的紧急指令而停车。 |

每个因素还提供状态、base link 坐标系下的位姿，以及距该位姿的距离。
车辆接近停车位置时，该因素以 APPROACHING 状态出现。
车辆到达该位置并停止后，状态变为 STOPPED。
位姿表示停车位置；如果无法计算停车位置，则使用 base link 的位姿。

![速度因素](planning-factors/velocity-factors.drawio.svg)

<a id="steering-factors"></a>

## 转向因素

转向因素是一个数组，包含左转或右转等需要使用转向灯的驾驶操作信息。
每个因素都有下表所述的行为类型，以及转向方向。
某些行为类型还具有 sequence 和 details 等附加信息。

| 行为 | 说明 |
| ------------------- | --------------------------------------------------------------------------- |
| intersection | 在路口左转或右转。 |
| lane-change | 变道。 |
| avoidance | 为避让障碍物而改变路径。具有变更和返回两个阶段。 |
| start-planner | 待定。 |
| goal-planner | 待定。 |
| emergency-operation | 因操作员的紧急指令而改变路径。 |

每个因素还提供状态、base link 坐标系下的多个位姿，以及距这些位姿的距离。
车辆接近开始转向的位置时，该因素以 APPROACHING 状态出现。
车辆到达该位置时，状态变为 TURNING。
这些位姿表示状态为 TURNING 的区段的起点和终点。

![转向因素 1](planning-factors/steering-factors-1.drawio.svg)

在变道和避障等情况下，车辆会根据情况，在该范围内的某个位置开始转向。
对于这些类型，状态为 TURNING 的区段会动态更新，相关位姿也会随之更新。

![转向因素 2](planning-factors/steering-factors-2.drawio.svg)
