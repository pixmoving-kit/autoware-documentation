<a id="categoryarea"></a>

## 类别：区域

---

<a id="vm-06-01-buffer-zone"></a>

### vm-06-01 缓冲区

<a id="detail-of-requirements"></a>

#### 需求详情 <!-- omit in toc -->

当路面绘有缓冲区（也称斑马纹区域）时，创建 Polygon（_type:hatched_road_markings_）。

- 如果缓冲区与 Lanelet 相邻，两者应共享 Point。
- 如果缓冲区位于交叉路口，应使其 Polygon 与交叉路口的 Polygon（intersection_area）重叠。

<a id="behavior-of-autoware"></a>

##### Autoware 的行为： <!-- omit in toc -->

为避让障碍物，Autoware 将缓冲区视为可行驶区域，并从中通过。

<a id="caution"></a>

##### 注意

- 车辆不得穿越安全区域，务必区分缓冲区与安全区域。- 即使路面绘有缓冲区，若该区域存在杆柱等静态物体、车辆无法通过，也不要为其创建 Polygon。仅应在车辆能够通行的区域建立缓冲区。

![svg](../assets/vm-06-01_1.svg)

![svg](../assets/vm-06-01_2.svg)

<a id="preferred-vector-map"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-06-01_3.svg)

<a id="incorrect-vector-map"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module"></a>

#### 相关 Autoware 模块

- [静态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_static_obstacle_avoidance_module/)
- [动态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_dynamic_obstacle_avoidance_module/)

---

<a id="vm-06-02-no-parking-signs"></a>

### vm-06-02 禁止停车标志

<a id="detail-of-requirements_1"></a>

#### 需求详情 <!-- omit in toc -->

创建矢量地图时，可在特定区域禁止停车，但允许临时停车。

从 Lanelet（_subtype:road_）创建指向 Regulatory Element（_subtype:no_parking_area_）的引用，再让该监管元素引用 Polygon（_type:no_parking_area_）。

有关在 Vector Map Builder 中的创建方法，请参阅 [Web.Auto 文档 - 创建禁止停车区域](https://docs.web.auto/en/user-manuals/vector-map-builder/how-to-use/edit-maps#creation-of-no-parking-area)。

<a id="behavior-of-autoware_1"></a>

##### Autoware 的行为： <!-- omit in toc -->

由于不能在 _no_parking_area_ 内设置目标点，因此 Autoware 无法在此停车。

![svg](../assets/vm-06-02_1.svg)

<a id="preferred-vector-map_1"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-06-02_2.svg)

<a id="incorrect-vector-map_1"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_1"></a>

#### 相关 Autoware 模块

- [目标规划器设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_goal_planner_module/)

---

<a id="vm-06-03-no-stopping-signs"></a>

### vm-06-03 禁止停止标志

<a id="detail-of-requirements_2"></a>

#### 需求详情 <!-- omit in toc -->

创建矢量地图时，可在特定区域禁止停止，但允许临时停车。

从 Lanelet（_subtype:road_）创建指向 Regulatory Element（_subtype:no_parking_area_）的引用，再让该监管元素引用 Polygon（_type:no_parking_area_）。

有关在 Vector Map Builder 中的创建方法，请参阅 [Web.Auto 文档 - 创建禁止停车区域](https://docs.web.auto/en/user-manuals/vector-map-builder/how-to-use/edit-maps#creation-of-no-parking-area)。

<a id="behavior-of-autoware_2"></a>

##### Autoware 的行为： <!-- omit in toc -->

由于不能在 _no_parking_area_ 内设置目标点，因此 Autoware 无法在此停车。

![svg](../assets/vm-06-03_1.svg)

<a id="preferred-vector-map_2"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-06-03_2.svg)

<a id="incorrect-vector-map_2"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_2"></a>

#### 相关 Autoware 模块

- [目标规划器设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_goal_planner_module/)

---

<a id="vm-06-04-no-stopping-sections"></a>

### vm-06-04 禁止停止路段

<a id="detail-of-requirements_3"></a>

#### 需求详情 <!-- omit in toc -->

车辆可能因信号灯或交通拥堵在道路上停车，而创建矢量地图时，可在特定区域禁止任何形式的停止（临时停车、停车、怠速停留）。

从 Lanelet（_subtype:road_）创建指向 Regulatory Element（_subtype:no_stopping_area_）的引用，再让该监管元素引用 Polygon（_type:no_stopping_area_）。

有关在 Vector Map Builder 中的创建方法，请参阅 [Web.Auto 文档 - 创建禁止停止区域](https://docs.web.auto/en/user-manuals/vector-map-builder/how-to-use/edit-maps#creation-of-no-stopping-area)。

<a id="behavior-of-autoware_3"></a>

##### Autoware 的行为： <!-- omit in toc -->

车辆不会在 _no_stopping_area_ 内临时停车。由于不能在 _no_stopping_area_ 内设置目标点，因此也无法在此停车。

![svg](../assets/vm-06-04_1.svg)

<a id="preferred-vector-map_3"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-06-04_2.svg)

<a id="incorrect-vector-map_3"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_3"></a>

#### 相关 Autoware 模块

- [禁止停止区域设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_no_stopping_area_module/)

---

<a id="vm-06-05-detection-area"></a>

### vm-06-05 检测区域

<a id="detail-of-requirements_4"></a>

#### 需求详情 <!-- omit in toc -->

Autoware 通过检测区域内的点云识别障碍物，在停止线前停车并保持停止，直到障碍物离开。要启用这一响应，需在矢量地图中加入检测区域元素。

从 Lanelet（_subtype:road_）创建指向 Regulatory Element（_subtype:detection_area_）的引用，再让该监管元素引用 Polygon（_type:detection_area_）和 Linestring（_type:stop_line_）。

有关在 Vector Map Builder 中的创建方法，请参阅 [Web.Auto 文档 - 创建检测区域](https://docs.web.auto/en/user-manuals/vector-map-builder/how-to-use/edit-maps#creation-of-detection-area)。

<a id="preferred-vector-map_4"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-06-05_1.svg)

<a id="incorrect-vector-map_4"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_4"></a>

#### 相关 Autoware 模块

- [检测区域 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_detection_area_module/)
