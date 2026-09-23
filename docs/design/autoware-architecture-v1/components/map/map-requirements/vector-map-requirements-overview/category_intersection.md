<a id="categoryintersection"></a>

## 类别：交叉路口

---

<a id="vm-03-01-intersection-criteria"></a>

### vm-03-01 交叉路口标准

<a id="detail-of-requirements"></a>

#### 需求详情 <!-- omit in toc -->

创建交叉路口的基本标准：

- 使用 Polygon（_type:intersection_area_）围住交叉路口的可行驶区域。
- 为交叉路口内的所有 Lanelet 添加 _turn_direction_。
- 确保交叉路口内的所有 lanelet 带有以下标签：
  - _key:intersection_area_
  - _value: Polygon 的 ID_
- 为需要的 Lanelet 添加 _right_of_way_。
- 还需正确创建交通信号灯、人行横道和停止线。

详细信息请参阅本页中的相应需求。

<a id="autoware-modules"></a>

##### Autoware 模块 <!-- omit in toc -->

- _turn_direction_ 和 _right_of_way_ 需求与交叉路口模块相关，该模块考虑交通信号灯指示，规划速度以避免与其他车辆碰撞。
- _intersection_area_ 需求与避障模块相关，该模块规划在交叉路口偏出车道避障的路线。

<a id="preferred-vector-map"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="incorrect-vector-map"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)
- [盲区设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_blind_spot_module/)
- [静态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_static_obstacle_avoidance_module/)
- [动态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_dynamic_obstacle_avoidance_module/)

---

<a id="vm-03-02-lanelets-turn-direction-and-virtual-linestring"></a>

### vm-03-02 Lanelet 转向方向与虚拟 Linestring

<a id="detail-of-requirements_1"></a>

#### 需求详情 <!-- omit in toc -->

为交叉路口内的 Lanelet 添加以下标签：

- turn_direction : straight
- turn_direction : left
- turn_direction : right

此外，如果交叉路口 Lanelet 的左侧或右侧 Linestring 没有对应道路标线，应将其指定为 _type:virtual_。

<a id="behavior-of-autoware"></a>

##### Autoware 的行为： <!-- omit in toc -->

默认情况下，Autoware 会在带有 turn_direction 标签的 Lanelet 前 30 米开始闪烁转向灯。如果需要更改闪烁时机，请添加以下标签：

- key: _turn_signal_distance_
- value：数值（m）

![svg](../assets/vm-03-02_1.svg)

<a id="preferred-vector-map_1"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-02_2.svg)

<a id="incorrect-vector-map_1"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_1"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)
- [盲区设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_blind_spot_module/)
- [behavior_velocity_planner 中的 virtual_traffic_light - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_virtual_traffic_light_module/)

---

<a id="vm-03-03-lanelet-width-in-the-intersection"></a>

### vm-03-03 交叉路口内的 Lanelet 宽度

<a id="detail-of-requirements_2"></a>

#### 需求详情： <!-- omit in toc -->

交叉路口内的 Lanelet 应保持一致的宽度。此外，应使用平滑曲线绘制 Linestring。

此曲线的形状必须由矢量地图制作者确定。

![svg](../assets/vm-03-03_1.svg)

<a id="preferred-vector-map_2"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-03_2.svg)

<a id="incorrect-vector-map_2"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-03_3.svg)

---

<a id="vm-03-04-lanelet-creation-in-the-intersection"></a>

### vm-03-04 在交叉路口中创建 Lanelet

<a id="detail-of-requirements_3"></a>

#### 需求详情 <!-- omit in toc -->

创建交叉路口内的所有 Lanelet，包括车辆不会行驶的 lanelet。此外，应将停止线和交通信号灯正确关联到 Lanelet。

另请参阅创建范围 [vm-07-01](category_others.md#vm-07-01-vector-map-creation-range)

<a id="behavior-of-autoware_1"></a>

##### Autoware 的行为 <!-- omit in toc -->

Autoware 使用 lanelet 预测其他车辆的运动，并据此规划自车速度。因此，需要创建交叉路口内的全部 lanelet。

![svg](../assets/vm-03-04_1.svg)

<a id="preferred-vector-map_3"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-04_2.svg)

<a id="incorrect-vector-map_3"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-04_3.svg)

<a id="related-autoware-module_2"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-05-lanelet-division-in-the-intersection"></a>

### vm-03-05 交叉路口内的 Lanelet 分割

<a id="detail-of-requirements_4"></a>

#### 需求详情 <!-- omit in toc -->

将交叉路口内的 Lanelet 创建为单个对象，不要拆分。

![svg](../assets/vm-03-05_1.svg)

<a id="preferred-vector-map_4"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-05_2.svg)

<a id="incorrect-vector-map_4"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-03_3.svg)

<a id="related-autoware-module_3"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-06-guide-lines-in-the-intersection"></a>

### vm-03-06 交叉路口内的引导线

<a id="detail-of-requirements_5"></a>

#### 需求详情 <!-- omit in toc -->

如果交叉路口内有引导线，应沿引导线绘制 Lanelet。

当 Lanelet 出现分支时，应从引导线的末端开始分支。但 Lanelet 之间无需共享 point 或 linestring。

![svg](../assets/vm-03-06_1.svg)

<a id="preferred-vector-map_5"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-06_2.svg)

<a id="incorrect-vector-map_5"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_4"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-07-multiple-lanelets-in-the-intersection"></a>

### vm-03-07 交叉路口内的多个 lanelet

<a id="detail-of-requirements_6"></a>

#### 需求详情 <!-- omit in toc -->

在交叉路口通过 Lanelet 连接多条车道时，这些 Lanelet 应彼此相邻，不能交叉。

![svg](../assets/vm-03-07_1.svg)

<a id="preferred-vector-map_6"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-07_2.svg)

<a id="incorrect-vector-map_6"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-07_3.svg)

<a id="related-autoware-module_5"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-08-intersection-area-range"></a>

### vm-03-08 交叉路口区域范围

<a id="detail-of-requirements_7"></a>

#### 需求详情 <!-- omit in toc -->

使用 Polygon（_type:intersection_area_）围住交叉路口的可行驶区域。交叉路口 Polygon 的边界应由以下对象确定。

- Linestring（_subtype:road_border_）
- 交叉路口内 lanelet 连接点处的直线。

<a id="preferred-vector-map_7"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-08_1.svg)

<a id="incorrect-vector-map_7"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_6"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)
- [盲区设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_blind_spot_module/)
- [静态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_static_obstacle_avoidance_module/)
- [动态避障 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_dynamic_obstacle_avoidance_module/)

---

<a id="vm-03-09-range-of-lanelet-in-the-intersection"></a>

### vm-03-09 交叉路口内的 Lanelet 范围

<a id="detail-of-requirements_8"></a>

#### 需求详情 <!-- omit in toc -->

根据停止线的位置确定交叉路口内 lanelet 的起止位置（下文称为 lanelet 连接边界）。

- 存在绘制的停止线时：
  - 停止线的 linestring（_type:stop_line_）位置必须与 lanelet 起点对齐。
  - 将 lanelet 末端延伸至对向车道停止线所在的位置。
- 没有绘制的停止线时：
  - 绘制 linestring（_type:stop_line_），按存在停止线的情况确定位置。

<a id="preferred-vector-map_8"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-09_1.svg)

<a id="incorrect-vector-map_8"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_7"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-10-right-of-way-with-signal"></a>

### vm-03-10 路权（有信号灯）

<a id="detail-of-requirements_9"></a>

#### 需求详情 <!-- omit in toc -->

为满足以下全部条件的 Lanelet 设置监管元素 'right_of_way'：

- 交叉路口内 _turn_direction_ 为 _right_ 或 _left_ 的 Lanelet。
- 与自车 lanelet 相交的 Lanelet。
- 交叉路口设有交通信号灯。

将交叉路口内与自车 lanelet 相交的 lanelet 设置为 _yield_，并将信号切换时序与自车不同的 lanelet 设置为 _yield_。此外，如果自车左转，应将对向车辆的右转车道设置为 _yield_。自车直行的 lanelet（_turn_direction:straight_）无需设置 _yield_。

![svg](../assets/vm-03-10_1.svg)

<a id="preferred-vector-map_9"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

<a id="the-vehicle-turns-left"></a>

##### 自车左转 <!-- omit in toc -->

![svg](../assets/vm-03-10_2.svg)

<a id="the-vehicle-turns-right"></a>

##### 自车右转 <!-- omit in toc -->

![svg](../assets/vm-03-10_3.svg)

<a id="incorrect-vector-map_9"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_8"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-11-right-of-way-without-signal"></a>

### vm-03-11 路权（无信号灯）

<a id="detail-of-requirements_10"></a>

#### 需求详情 <!-- omit in toc -->

为满足以下全部条件的 Lanelet 设置监管元素 'right_of_way'：

- 交叉路口内 _turn_direction_ 为 _right_ 或 _left_ 的 Lanelet。
- 与自车 lanelet 相交的 Lanelet。
- 交叉路口**没有**交通信号灯。

![svg](../assets/vm-03-11_1.svg)

<a id="preferred-vector-map_10"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

<a id="1-the-vehicle-on-the-priority-lane"></a>

##### ① 自车位于优先车道 <!-- omit in toc -->

![svg](../assets/vm-03-11_2.svg)

<a id="2-the-vehicle-on-the-non-priority-lane"></a>

##### ② 自车位于非优先车道 <!-- omit in toc -->

无需监管元素。但当自车直行时，相对于从对向非优先道路右转的其他车辆，自车具有优先权。因此，此时需要设置 _right_of_way_ 和 _yield_。

![svg](../assets/vm-03-11_3.svg)

<a id="incorrect-vector-map_10"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_9"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-12-right-of-way-supplements"></a>

### vm-03-12 路权补充说明

<a id="detail-of-requirements_11"></a>

#### 需求详情 <!-- omit in toc -->

<a id="why-its-necessary-to-configure-right_of_way"></a>

##### 为什么必须配置 'right_of_way' <!-- omit in toc -->

未设置 'right_of_way' 时，Autoware 会认为与其路径相交的其他车道具有优先权。因此，只要交叉车道上存在其他车辆，无论信号灯如何指示，Autoware 都无法进入交叉路口。

问题示例：即使自车信号灯允许通行，如果其他车辆在对向车道与右转车道的交汇处等待红灯，自车仍会提前等待。

<a id="preferred-vector-map_11"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="incorrect-vector-map_11"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_10"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-13-merging-from-private-area-sidewalk"></a>

### vm-03-13 从私人区域汇入及人行道

<a id="detail-of-requirements_12"></a>

#### 需求详情 <!-- omit in toc -->

为私人区域内的 Lanelet 设置 _location=private_。

当出入私人区域的道路与人行道相交时，为该人行道创建 Lanelet（_subtype:walkway_）。

<a id="behavior-of-autoware_2"></a>

##### Autoware 的行为： <!-- omit in toc -->

- 车辆在进入人行道前临时停车。
- 车辆在汇入公共道路前停车。

![svg](../assets/vm-03-13_1.svg)

<a id="preferred-vector-map_12"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-13_2.svg)

<a id="incorrect-vector-map_12"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_11"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-14-road-marking"></a>

### vm-03-14 道路标线

<a id="detail-of-requirements_13"></a>

#### 需求详情 <!-- omit in toc -->

如果交叉路口引导线前方有停止线，请确保以下设置：

- 为引导线创建 Lanelet。
- 引导线的 Lanelet 引用 Regulatory Element（_subtype:road_marking_）。
- 该监管元素引用 _stop_line_ 的 Linestring。

有关在 Vector Map Builder 中的创建方法，请参阅 [Web.Auto 文档 - 创建监管元素](https://docs.web.auto/en/user-manuals/vector-map-builder/how-to-use/edit-maps#creation-of-regulatory-element)。

![svg](../assets/vm-03-14_1.svg)

<a id="preferred-vector-map_13"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-14_2.svg)

<a id="incorrect-vector-map_13"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_12"></a>

#### 相关 Autoware 模块

- [交叉路口 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)

---

<a id="vm-03-15-exclusive-bicycle-lane"></a>

### vm-03-15 自行车专用道

<a id="detail-of-requirements_14"></a>

#### 需求详情 <!-- omit in toc -->

如果存在自行车专用道，创建 Lanelet（_subtype:road_）。与道路相邻的部分应共享 Linestring。对于交叉路口内与自车左转车道相交的自行车道，应在 _right_of_way_ 下指定 yield_lane。（有关 right_of_way，请参阅 [vm-03-10](./category_intersection.md#vm-03-10-right-of-way-with-signal) 和 [vm-03-11](./category_intersection.md#vm-03-11-right-of-way-without-signal)。）

此外，将 _lane_change = no_ 设置为 OptionalTags。

<a id="behavior-of-autoware_3"></a>

##### Autoware 的行为： <!-- omit in toc -->

盲区检测（卷入检查）功能检查 lanelet(subtype:road)，并判断车辆能否继续前进。

![png](../assets/vm-03-15_1.png)

![svg](../assets/vm-03-15_2.svg)

<a id="preferred-vector-map_14"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-03-15_3.svg)

<a id="incorrect-vector-map_14"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_13"></a>

#### 相关 Autoware 模块

- [盲区设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_blind_spot_module/)
