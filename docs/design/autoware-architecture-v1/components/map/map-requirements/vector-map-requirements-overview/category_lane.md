<a id="categorylane"></a>

## 类别：车道

---

<a id="vm-01-01-lanelet-basics"></a>

### vm-01-01 Lanelet 基础

<a id="detail-of-requirements"></a>

#### 需求详情 <!-- omit in toc -->

道路的 Lanelet 必须满足以下要求。

- _subtype:road_
- 公共道路设置 location:urban
- 使 Lanelet 方向与车辆行驶方向一致。（可在 [Vector Map Builder](https://docs.web.auto/en/user-manuals/vector-map-builder/screen-layout#project-tab) 中以箭头显示 lanelet 方向。）
- 根据 [vm-01-02](#vm-01-02-allowance-for-lane-changes) 设置是否允许变道。
- 分别为 Lanelet 的 left_bound 和 right_bound 设置 Linestring ID。参阅 [vm-01-03](#vm-01-03-linestring-sharing)。
- 标签：_one_way=yes_。Autoware 当前不支持 no。
- 除起点或终点外，将 Lanelet 连接到其他 Lanelet。
- 将 Lanelet 中的点（x、y、z）与 PCD 地图对齐，确保横向和高程均准确。Point 的高度应基于椭球高（WGS84）。参阅 [vm-07-04](category_others.md#vm-07-04-ellipsoidal-height)。

<a id="preferred-vector-map"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![lanelet](../assets/vm-01-01.svg)

---

<a id="vm-01-02-allowance-for-lane-changes"></a>

### vm-01-02 变道许可

<a id="detail-of-requirements_1"></a>

#### 需求详情 <!-- omit in toc -->

为 Lanelet 的 Linestring 添加标签，指示允许或禁止变道。

- 允许：_lane_change=yes_
- 禁止：_lane_change=no_

根据标线类型设置 Linestring 的 _subtype_。

- _solid_
- _dashed_

<a id="referenced-from-japans-road-traffic-law"></a>

##### 参考日本《道路交通法》 <!-- omit in toc -->

- 白色虚线：表示允许变道和超车。
- 白色实线：表示允许变道和超车。
- 黄色实线：表示禁止变道。

![lines](../assets/vm-01-02.svg)

<a id="related-autoware-module"></a>

#### 相关 Autoware 模块

- [变道设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_lane_change_module/)
- [静态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_static_obstacle_avoidance_module/)
- [动态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_dynamic_obstacle_avoidance_module/)
- [驶出车道设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_out_of_lane_module/)

---

<a id="vm-01-03-linestring-sharing"></a>

### vm-01-03 共享 Linestring

<a id="detail-of-requirements_2"></a>

#### 需求详情 <!-- omit in toc -->

创建物理上相邻的 Lanelet 时，应共享 Linestring。

<a id="behavior-of-autoware"></a>

##### Autoware 的行为 <!-- omit in toc -->

如果自车所在 Lanelet 与相邻 Lanelet 共享 Linestring，则可实现以下行为：

- 车辆驶出所在车道避让障碍物。
- 车辆转弯时略微超出车道边界。
- 变道

![lines](../assets/vm-01-03_1.svg)

<a id="preferred-vector-map_1"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![lines](../assets/vm-01-03_2.svg)

<a id="incorrect-vector-map"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![lines](../assets/vm-01-03_3.svg)

<a id="related-autoware-module_1"></a>

#### 相关 Autoware 模块

- [变道设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_lane_change_module/)
- [静态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_static_obstacle_avoidance_module/)
- [动态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_dynamic_obstacle_avoidance_module/)
- [驶出车道设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_out_of_lane_module/)

---

<a id="vm-01-04-sharing-of-the-centerline-of-lanes-for-opposing-traffic"></a>

### vm-01-04 对向车道共享道路中心线

<a id="detail-of-requirements_3"></a>

#### 需求详情 <!-- omit in toc -->

当自车 lanelet 与对向 lanelet 在物理上相接时，这两个 Lanelet 必须共享道路中心线的 Linestring ID。为此，两者长度必须一致。

<a id="behavior-of-autoware_1"></a>

##### Autoware 的行为：<!-- omit in toc -->

可跨入对向车道避让障碍物。

![svg](../assets/vm-01-04_1.svg)

<a id="preferred-vector-map_2"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-04_2.svg)

<a id="incorrect-vector-map_1"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-04_3.svg)

---

<a id="vm-01-05-lane-geometry"></a>

### vm-01-05 车道几何形状

<a id="detail-of-requirements_4"></a>

#### 需求详情 <!-- omit in toc -->

道路 lanelet 的几何形状应满足以下要求：

- 左右 Linestring 必须沿道路边界线绘制。
- Lanelet 与前后 lanelet 连接的边必须是直线。
- 除 L 形急转段外，轮廓应平滑，不应出现锯齿或凹凸。

![svg](../assets/vm-01-05_1.svg)

<a id="preferred-vector-map_3"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-05_2.svg)

<a id="incorrect-vector-map_2"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-05_3.svg)

---

<a id="vm-01-06-line-position-1"></a>

### vm-01-06 标线位置（1）

<a id="detail-of-requirements_5"></a>

#### 需求详情 <!-- omit in toc -->

确保道路中心线 Linestring 位于道路标线的正中间。

![svg](../assets/vm-01-06_1.svg)

<a id="preferred-vector-map_4"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-06_2.svg)

<a id="incorrect-vector-map_3"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-06_3.svg)

---

<a id="vm-01-07-line-position-2"></a>

### vm-01-07 标线位置（2）

<a id="detail-of-requirements_6"></a>

#### 需求详情 <!-- omit in toc -->

当道路外侧存在标线时，将 Linestring 放在标线中心。

![svg](../assets/vm-01-07_1.svg)

<a id="preferred-vector-map_5"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-07_2.svg)

<a id="incorrect-vector-map_4"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

---

<a id="vm-01-08-line-position-3"></a>

### vm-01-08 标线位置（3）

<a id="detail-of-requirements_7"></a>

#### 需求详情 <!-- omit in toc -->

如果道路外侧没有标线，将 Linestring 放在距离道路边缘 0.5 m 的位置。

![svg](../assets/vm-01-08_1.svg)

<a id="caution"></a>

##### 注意

宽度取决于所在国家的法律。

<a id="preferred-vector-map_6"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-08_2.svg)

<a id="incorrect-vector-map_5"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

---

<a id="vm-01-09-speed-limits"></a>

### vm-01-09 限速

<a id="detail-of-requirements_8"></a>

#### 需求详情 <!-- omit in toc -->

在以下情况下，为自车行驶的 Lanelet（_subtype:road_）添加限速（_tag:speed_limit_），单位为 km/h。

- 存在限速交通标志。
- 也可在狭窄道路等处添加限速。

请注意，下列功能通过 Autoware 设置和行为实现。

- 车辆最大速度
- 在弯道、下坡等需要减速的地方调整速度。

![svg](../assets/vm-01-09_1.svg)

<a id="preferred-vector-map_7"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-09_2.svg)

<a id="incorrect-vector-map_6"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

---

<a id="vm-01-10-centerline"></a>

### vm-01-10 中心线

<a id="detail-of-requirements_9"></a>

#### 需求详情 <!-- omit in toc -->

Autoware 设计为沿 Lanelet 左右 Linestring 计算得到的中点行驶。

因某些情况需要将行驶位置向左或向右偏移时，为 Lanelet 创建中心线，并确保形状平滑以便行驶。

![svg](../assets/vm-01-10_1.svg)

<a id="caution_1"></a>

##### 注意

此处的“中心线”与分隔对向车道的道路中央分隔线（中心线）是不同概念。

<a id="preferred-vector-map_8"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-10_2.svg)

<a id="incorrect-vector-map_7"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-10_3.svg)

---

<a id="vm-01-11-centerline-connection-1"></a>

### vm-01-11 中心线连接（1）

<a id="detail-of-requirements_10"></a>

#### 需求详情 <!-- omit in toc -->

为多个 Lanelet 添加中心线后，应将这些中心线连接起来。

![svg](../assets/vm-01-11_1.svg)

<a id="preferred-vector-map_9"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-11_2.svg)

<a id="incorrect-vector-map_8"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-11_3.svg)

---

<a id="vm-01-12-centerline-connection-2"></a>

### vm-01-12 中心线连接（2）

<a id="detail-of-requirements_11"></a>

#### 需求详情 <!-- omit in toc -->

如果添加了中心线的 Lanelet 连接到未添加中心线的 Lanelet，应将新增中心线的起点和终点放在 Lanelet 的中央。确保中心线形状平滑，以便行驶。

![svg](../assets/vm-01-12_1.svg)

<a id="preferred-vector-map_10"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-12_2.svg)

<a id="incorrect-vector-map_9"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-12_3.svg)

---

<a id="vm-01-13-roads-with-no-centerline-1"></a>

### vm-01-13 无道路中心线的道路（1）

<a id="detail-of-requirements_12"></a>

#### 需求详情 <!-- omit in toc -->

当道路没有中央分隔线，但足够宽、可供自车与对向车辆会车时，应在道路中央相邻布置 Lanelet。

![svg](../assets/vm-01-13_1.svg)

<a id="preferred-vector-map_11"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-13_2.svg)

<a id="incorrect-vector-map_10"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

---

<a id="vm-01-14-roads-with-no-centerline-2"></a>

### vm-01-14 无道路中心线的道路（2）

<a id="detail-of-requirements_13"></a>

#### 需求详情 <!-- omit in toc -->

当满足以下全部条件时适用：

- 道路为没有中央分隔线的单车道，宽度不足以供自车与对向车辆会车。
- 环境中除自动驾驶车辆外，其他车辆不会进入该道路。
- 计划由自动驾驶车辆在该道路上往返运行。

矢量地图创建要求：

- 将两个 Lanelet 重叠放置。

<a id="supplementary-information"></a>

##### 补充信息

- 是否采用此情况取决于当地运营策略和车辆规格，应与地图需求方讨论确定。
- 当前 Autoware 不具备在共用车道上与对向车辆会车的能力。

![svg](../assets/vm-01-14_1.svg)

<a id="preferred-vector-map_12"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-14_2.svg)

<a id="incorrect-vector-map_11"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-14_3.svg)

---

<a id="vm-01-15-road-shoulder"></a>

### vm-01-15 路肩

<a id="detail-of-requirements_14"></a>

#### 需求详情 <!-- omit in toc -->

如果道路旁有路肩，创建路肩 lanelet（_subtype:road_shoulder_）。但交叉路口内无需创建。

路肩 Lanelet 与人行道 Lanelet 共享 Linestring（_subtype:road_border_）。

路肩 Lanelet 不得与另一个路肩 Lanelet 相邻。

路肩 Lanelet 必须与道路 Lanelet 相邻。

<a id="behavior-of-autoware_2"></a>

##### Autoware 的行为 <!-- omit in toc -->

- Autoware 可从路肩出发，也可到达路肩。
- 到达时向边缘靠近的余量由 Autoware 参数 _margin_from_boundary_ 决定，创建矢量地图时无需考虑。
- 如果路肩 lanelet 与以下任一对象重叠，Autoware 不会在其上停车：
  - 标记为 _no_parking_area_ 的 Polygon
  - 标记为 _no_stopping_area_ 的 Polygon
  - 交叉路口附近及内部区域
  - 人行横道

标示路肩边界的 Linestring 无需设置 _tag:lane_change=yes_。

![svg](../assets/vm-01-15_1.svg)

<a id="preferred-vector-map_13"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-15_2.svg)

<a id="incorrect-vector-map_12"></a>

#### 错误的矢量地图 <!-- omit in toc -->

不要为没有路肩的道路创建路肩 Lanelet。

![svg](../assets/vm-01-15_3.svg)

<a id="related-autoware-module_2"></a>

#### 相关 Autoware 模块

- [静态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_static_obstacle_avoidance_module/)
- [动态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_dynamic_obstacle_avoidance_module/)
- [目标规划器设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_goal_planner_module/)

---

<a id="vm-01-16-road-shoulder-linestring-sharing"></a>

### vm-01-16 路肩共享 Linestring

<a id="detail-of-requirements_15"></a>

#### 需求详情 <!-- omit in toc -->

路肩 Lanelet 与相邻道路 Lanelet 应共享 Linestring。

![svg](../assets/vm-01-15_1.svg)

<a id="preferred-vector-map_14"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-16_2.svg)

<a id="incorrect-vector-map_13"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_3"></a>

#### 相关 Autoware 模块

- [静态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_static_obstacle_avoidance_module/)
- [动态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_dynamic_obstacle_avoidance_module/)
- [目标规划器设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_goal_planner_module/)

---

<a id="vm-01-17-side-strip"></a>

### vm-01-17 路侧带

<a id="detail-of-requirements_16"></a>

#### 需求详情 <!-- omit in toc -->

在路侧带创建 Lanelet（_subtype:pedestrian_lane_）。但交叉路口内无需创建。

路侧带 Lanelet 的外侧必须具有 Linestring（_subtype:road_border_）。

![svg](../assets/vm-01-17_1.svg)

<a id="preferred-vector-map_15"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-17_2.svg)

<a id="incorrect-vector-map_14"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

---

<a id="vm-01-18-side-strip-linestring-sharing"></a>

### vm-01-18 路侧带共享 Linestring

<a id="detail-of-requirements_17"></a>

#### 需求详情 <!-- omit in toc -->

路侧带 Lanelet 与相邻道路 Lanelet 应共享 Linestring。

![svg](../assets/vm-01-17_1.svg)

<a id="preferred-vector-map_16"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-18_2.svg)

<a id="incorrect-vector-map_15"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

---

<a id="vm-01-19-sidewalk"></a>

### vm-01-19 人行道

<a id="detail-of-requirements_18"></a>

#### 需求详情 <!-- omit in toc -->

在需要的地方创建人行道 Lanelet（_subtype:walkway_）。仅当人行横道与自车车道相交时创建，没有交叉时不创建。

lanelet（_subtype:walkway_）的长度应覆盖与自车车道相交的区域，并向前后各延伸 3 米。

![svg](../assets/vm-01-19_1.svg)

<a id="preferred-vector-map_17"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-01-19_2.svg)

<a id="incorrect-vector-map_16"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_4"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)
- [人行道设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_walkway_module/)
