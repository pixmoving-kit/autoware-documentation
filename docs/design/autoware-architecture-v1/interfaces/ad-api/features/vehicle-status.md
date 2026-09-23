<a id="vehicle-status"></a>

# 车辆状态

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/vehicle/kinematics') }}
- {{ link_ad_api('/api/vehicle/status') }}
- {{ link_ad_api('/api/vehicle/metrics') }}
- {{ link_ad_api('/api/vehicle/dimensions') }}
- {{ link_ad_api('/api/vehicle/specs') }}

<a id="kinematics"></a>

## 运动学信息

这里提供车辆运动学状态的估计值。应用进行车辆调度时需要车辆位置信息。
应用还可以利用速度和加速度，发现停滞或突然制动等需要操作员协助的车辆。

<a id="status"></a>

## 状态

这里提供车辆自身报告的状态，例如指示灯和转向状态。
这些信息主要用于可视化和远程控制。

<a id="metrics"></a>

## 指标

这里提供车辆报告的指标数据，例如剩余能量。
剩余能量可用于车辆调度。

<a id="dimensions"></a>

## 尺寸

运动学信息中的车辆位置是 base link 的坐标，因此需要利用车辆尺寸确定车辆与物体之间的实际距离。这对于远程车辆支持中的可视化是必要的。
