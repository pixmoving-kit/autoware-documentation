<a id="categorycrosswalk"></a>

## 类别：人行横道

人行横道有两类需求，同一条人行横道可能同时适用两类需求。

- [vm-05-01](./category_crosswalk.md#vm-05-01-crosswalks-across-the-road)：横跨道路的人行横道
- [vm-05-02](./category_crosswalk.md#vm-05-02-crosswalks-with-pedestrian-signals)：带行人信号灯的人行横道

交叉路口处的人行横道还必须满足交叉路口的需求。

---

<a id="vm-05-01-crosswalks-across-the-road"></a>

### vm-05-01 横跨道路的人行横道

<a id="detail-of-requirements"></a>

#### 需求详情 <!-- omit in toc -->

创建所需的要求：

1. 为人行横道创建 Lanelet（_subtype:crosswalk_）。
2. 如果人行横道前有停止线，创建 Linestring（_type:stop_line_）。对向车道的停止线也按相同方式创建。
3. 创建覆盖人行横道的 Polygon（_type:crosswalk_polygon_）。
4. 道路的 Lanelet 引用监管元素（_subtype:crosswalk_），监管元素再引用已创建的 Lanelet、Linestring 和 Polygon。

<a id="supplemental-information"></a>

##### 补充信息

- 将监管元素关联到与人行横道相交的道路 lanelet。
- 与监管元素关联的停止线，不一定必须位于与该监管元素关联的道路 Lanelet 上。

<a id="behavior-of-autoware"></a>

##### Autoware 的行为： <!-- omit in toc -->

当人行横道上有行人或骑行者时，Autoware 会在停止线前停车并等待其通过。待其离开后，Autoware 才会继续前进。

<a id="preferred-vector-map"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-05-01_1.svg)

<a id="incorrect-vector-map"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module"></a>

#### 相关 Autoware 模块

- [人行横道 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_crosswalk_module/)

---

<a id="vm-05-02-crosswalks-with-pedestrian-signals"></a>

### vm-05-02 带行人信号灯的人行横道

<a id="detail-of-requirements_1"></a>

#### 需求详情 <!-- omit in toc -->

创建所需的要求：

- 创建 Lanelet（_subtype:crosswalk_、_participant:pedestrian_）。
- 创建交通信号灯 Linestring。如果存在多个信号灯，则创建多个 Linestring。
  - Linestring
    - _type:traffic_light_
    - _subtype:red_green_
    - _height_：数值
- 确保人行横道的 Lanelet 引用 Regulatory Element（_subtype:traffic_light_），并确保该监管元素引用 Linestring（_type:traffic_light_）。

有关交通信号灯对象的更多信息，请参阅 [vm-04-02](category_traffic_light.md#vm-04-02-traffic-light-position-and-size)。

<a id="preferred-vector-map_1"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-05-02_1.svg)

<a id="incorrect-vector-map_1"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_1"></a>

#### 相关 Autoware 模块

- [人行横道 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_crosswalk_module/)

---

<a id="vm-05-03-deceleration-for-safety-at-crosswalks"></a>

### vm-05-03 人行横道安全减速

<a id="detail-of-requirements_2"></a>

#### 需求详情 <!-- omit in toc -->

为确保通过人行横道时始终减速至安全速度，请为人行横道的 Lanelet（_subtype:crosswalk_）添加以下标签：

- _safety_slow_down_speed_ [m/s]：通过时的最大速度。
- _safety_slow_down_distance_ [m]：最大速度适用区域的起点，以车辆前保险杠到人行横道的距离衡量。

<a id="preferred-vector-map_2"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-05-03_2.svg)

<a id="incorrect-vector-map_2"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_2"></a>

#### 相关 Autoware 模块

- [人行横道 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_crosswalk_module/)

---

<a id="vm-05-04-fences"></a>

### vm-05-04 围栏

<a id="detail-of-requirements_3"></a>

#### 需求详情 <!-- omit in toc -->

Autoware 会检测正在穿越或可能穿越人行横道的行人和自行车。但人行横道附近的围栏幼儿园、游乐场或公园等人群活动频繁的区域，其行人和自行车的预测路径可能影响人行横道检测。

使用 Linestring（_type:fence_）围住不与人行横道连通的区域，无需将其关联到任何对象。

如果道路与人行道之间已有护栏、墙壁或围栏，其后还有另一道围栏，则可以省略第二道围栏。但人行横道周围的区域不适用此省略规则，必须完整创建。

<a id="preferred-vector-map_3"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-05-04_1.svg)

<a id="incorrect-vector-map_3"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_3"></a>

#### 相关 Autoware 模块

- [map_based_prediction - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/perception/autoware_map_based_prediction/)
