<a id="categorytraffic-light"></a>

## 类别：交通信号灯

---

<a id="vm-04-01-traffic-light-basics"></a>

### vm-04-01 交通信号灯基础

<a id="detail-of-requirements"></a>

#### 需求详情 <!-- omit in toc -->

在矢量地图中创建交通信号灯时，应满足以下要求：

- 道路 Lanelet（_subtype:road_）。数量：一个。
- 交通信号灯。可以有多个。
  - 信号灯 Linestring（_type:traffic_light_）。
  - 信号灯灯泡 Linestring（_type:light_bulbs_）。
  - 停止线 Linestring（_type:stop_line_）。
- 交通信号灯监管元素（_subtype:traffic_light_）。由道路 Lanelet 引用，并引用信号灯（_traffic_light_、_light_bulbs_）和停止线（_stop_line_）。数量：一个。

有关在 Vector Map Builder 中的创建方法，请参阅 [Web.Auto 文档 - 创建交通信号灯和停止线](https://docs.web.auto/en/user-manuals/vector-map-builder/how-to-use/edit-maps#creation-of-a-traffic-light-and-a-stop-line)。

有关交通信号灯和灯泡对象的规范，请参阅 vm-04-02 和 vm-04-03。

<a id="preferred-vector-map"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-04-01_1.svg)

如果交叉路口有人行横道，应使道路 Lanelet 与人行横道 Lanelet 相交并重叠。

![svg](../assets/vm-04-01_2.svg)

<a id="related-autoware-module"></a>

#### 相关 Autoware 模块

- [交通信号灯设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_traffic_light_module/)

---

<a id="vm-04-02-traffic-light-position-and-size"></a>

### vm-04-02 交通信号灯位置和尺寸

<a id="detail-of-requirements_1"></a>

#### 需求详情 <!-- omit in toc -->

使用 Linestring 创建交通信号灯。

- _type:traffic_light_
- _subtype:red_yellow_green_（可选）

将 Linestring 的长度（从起点到终点）精确对齐到交通信号灯的底边。确保 Linestring 的三维坐标准确体现信号灯的位置高度。

使用 _tag:height_ 表示交通信号灯自身的高度，例如 50cm 应写为 _tag:height=0.5_。注意，此高度表示信号灯尺寸，而非其位置。

<a id="supplemental-information"></a>

##### 补充信息 <!-- omit in toc -->

Autoware 当前忽略 subtype _red_yellow_green_。

<a id="preferred-vector-map_1"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-04-02_1.svg)

<a id="incorrect-vector-map"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_1"></a>

#### 相关 Autoware 模块

- [交通信号灯设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_traffic_light_module/)

---

<a id="vm-04-03-traffic-light-lamps"></a>

### vm-04-03 交通信号灯灯泡

<a id="detail-of-requirements_2"></a>

#### 需求详情 <!-- omit in toc -->

为使系统能够检测交通信号灯颜色，必须以对象形式准确创建其颜色配置和排列。用 Point 表示灯泡位置。彩色灯使用 _color_ 标签表示颜色，箭头灯使用 _arrow_ 标签表示方向。

- _tag: color = red, yellow, green_
- _tag: arrow = up, right, left, up_light, up_left_

创建 Linestring 时使用灯泡的 Point。

- _type: light_bulbs_

<a id="preferred-vector-map_2"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-04-03_1.svg)

灯泡 Point 的顺序可以是 1→2→3→4，也可以是 4→3→2→1，两者均可。

<a id="incorrect-vector-map_1"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module_2"></a>

#### 相关 Autoware 模块

- [交通信号灯设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_traffic_light_module/)
