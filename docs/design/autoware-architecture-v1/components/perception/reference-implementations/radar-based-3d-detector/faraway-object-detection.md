<a id="radar-faraway-dynamic-objects-detection-with-radar-objects"></a>

# 使用雷达目标检测远距离动态目标

<a id="overview"></a>

## 概述

下图描述雷达远距离动态目标检测流程。

![faraway object detection](image/faraway-object-detection.drawio.svg)

<a id="reference-implementation"></a>

## 参考实现

<a id="crossing-filter"></a>

### 横穿过滤器

- [radar_crossing_objects_noise_filter](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_radar_crossing_objects_noise_filter)

此软件包可过滤横穿自车方向的噪声目标，这些目标很可能是虚假目标。

<a id="velocity-filter"></a>

### 速度过滤器

- [object_velocity_splitter](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_object_velocity_splitter)

静态目标中包含许多噪声，例如地面反射产生的目标。
在许多情况下，雷达能够稳定检测动态目标。
可使用 `object_velocity_splitter` 过滤静态目标。

<a id="range-filter"></a>

### 范围过滤器

- [object_range_splitter](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_object_range_splitter)

某些雷达有时会为近距离物体产生虚假目标。
可使用 `object_range_splitter` 过滤这些目标。

<a id="vector-map-filter"></a>

### 矢量地图过滤器

- [object-lanelet-filter](https://github.com/autowarefoundation/autoware_universe/blob/main/perception/autoware_detected_object_validation/object-lanelet-filter.md)

在大多数情况下，车辆在可行驶区域内行驶。
可使用 `object-lanelet-filter` 过滤可行驶区域外的目标。
`object-lanelet-filter` 会过滤矢量地图定义的可行驶区域之外的目标。

请注意，将 `object-lanelet-filter` 用于雷达远距离检测时，除自动驾驶车辆行驶的区域外，还需在矢量地图中定义其他可行驶区域。

<a id="radar-object-clustering"></a>

### 雷达目标聚类

- [radar_object_clustering](https://github.com/autowarefoundation/autoware_universe/tree/main/perception/autoware_radar_object_clustering)

此软件包可将同一物体的多个雷达检测结果合并为一个，并调整类别和尺寸。
它可抑制跟踪模块中目标分裂的现象。

![radar_object_clustering](https://raw.githubusercontent.com/autowarefoundation/autoware_universe/main/perception/autoware_radar_object_clustering/docs/radar_clustering.drawio.svg)

## Note

<a id="parameter-tuning"></a>

### 参数调优

仅由雷达执行的检测需要多种较强的噪声处理。
因此存在取舍：加强噪声处理会使原本希望检测的物体消失，而减弱处理又可能让噪声目标始终出现在车前，导致自动驾驶系统无法起步。
调整参数时必须注意这一取舍。

<a id="limitation"></a>

### 限制

- 高架铁路和立交桥上的车辆

如果使用二维雷达（可检测 xy 轴二维坐标，但无法检测 z 轴），且行驶区域有高架铁路或立交桥上的车辆，雷达处理会检测到它们，从而不利于规划结果。
此外，当前雷达处理不具有标签分类功能，因此会将高架铁路检测为车辆，导致非预期行为。
