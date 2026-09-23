<a id="categoryothers"></a>

## 类别：其他

---

<a id="vm-07-01-vector-map-creation-range"></a>

### vm-07-01 矢量地图创建范围

<a id="detail-of-requirements"></a>

#### 需求详情 <!-- omit in toc -->

创建车辆传感器范围内的全部 Lanelet，即使位于自车不会行驶的道路上也应创建，包括与自车 Lanelet 相交的 Lanelet。

但若满足以下条件，必须创建 lanelet 的范围至少为 10 米。

- 车辆沿优先车道通过无交通信号灯的交叉路口。
- 车辆直行或左转通过有交通信号灯的交叉路口

有关交叉路口需求的更多信息，请参阅 [vm-03-04](category_intersection.md#vm-03-04-lanelet-creation-in-the-intersection)。

<a id="behavior-of-autoware"></a>

##### Autoware 的行为： <!-- omit in toc -->

Autoware 检测接近的车辆，并规划避免碰撞的路线。

<a id="caution"></a>

##### 注意

请检查车载传感器的范围。

<a id="preferred-vector-map"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-07-01_1.svg)

<a id="incorrect-vector-map"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-07-01_2.svg)

---

<a id="vm-07-02-range-of-detecting-pedestrians-who-enter-the-road"></a>

### vm-07-02 突入道路行人的检测范围

<a id="detail-of-requirements_1"></a>

#### 需求详情 <!-- omit in toc -->

Autoware 的路侧突入检测功能会跟踪道路边界外的行人和骑行者，在其可能进入道路时减速以避免碰撞。

设置以下类型的 linestring，可让 Autoware 忽略线外的目标，认为其没有突然进入道路的风险。

- guard_rail
- wall
- fence

<a id="preferred-vector-map_1"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-05-04_1.svg)

<a id="incorrect-vector-map_1"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module"></a>

#### 相关 Autoware 模块

- [map_based_prediction - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/perception/autoware_map_based_prediction/)

---

<a id="vm-07-03-guardrails-guard-pipes-fences"></a>

### vm-07-03 护栏、护管和围栏

<a id="detail-of-requirements_2"></a>

#### 需求详情 <!-- omit in toc -->

为护栏或护管（_type: guard_rail_）创建 Linestring 时，应放置在朝向道路一侧最突出部分垂直投影到地面的点上。

围栏（_type:fence_）的 Linestring 也采用相同的位置规则。

<a id="preferred-vector-map_2"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![png](../assets/vm-07-03_1.png)

<a id="incorrect-vector-map_2"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![png](../assets/vm-07-03_2.png)

<a id="related-autoware-module_1"></a>

#### 相关 Autoware 模块

- [可行驶区域设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_planner_common/docs/behavior_path_planner_drivable_area_design/)

---

<a id="vm-07-04-ellipsoidal-height"></a>

### vm-07-04 椭球高

<a id="detail-of-requirements_3"></a>

#### 需求详情 <!-- omit in toc -->

Point 的高度应基于椭球高（WGS84），单位为米。

![svg](../assets/vm-07-04_height_en.svg)

<a id="preferred-vector-map_3"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

Point 的高度是椭球面到地面的距离。

<a id="incorrect-vector-map_3"></a>

#### 错误的矢量地图 <!-- omit in toc -->

Point 的高度为正高，即大地水准面到地面的距离。
