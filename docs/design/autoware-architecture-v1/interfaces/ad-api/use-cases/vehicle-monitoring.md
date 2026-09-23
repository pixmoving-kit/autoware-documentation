<a id="vehicle-monitoring"></a>

# 车辆监控

AD API 提供当前车辆状态，用于远程监控、面向乘客的可视化等。
根据希望监控的数据，使用以下 API。

<a id="vehicle-status"></a>

## 车辆状态

[车辆状态](../features/vehicle-status.md)提供运动学信息、指示灯和尺寸等基本信息。
远程操作员可据此了解车辆的位置和速度。
对于 FMS 等应用，这有助于发现停滞或突然制动等需要协助的车辆。
还可以根据车辆尺寸确定与物体之间的实际距离。

<a id="planning-factors"></a>

## 规划因素

[规划因素](../features/planning-factors.md)提供车辆的规划状态。
HMI 可利用这些信息提醒车辆即将发生的突然运动，并向乘客说明停车原因，以改善乘坐舒适性。

<a id="detected-objects"></a>

## 检测目标

[感知](../features/perception.md)提供 Autoware 检测到的目标。
HMI 可利用这些信息将车辆周围的目标可视化。
