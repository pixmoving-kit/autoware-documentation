<a id="categorystop-line"></a>

## 类别：停止线

---

<a id="vm-02-01-stop-line-alignment"></a>

### vm-02-01 停止线对齐

<a id="detail-of-requirements"></a>

#### 需求详情 <!-- omit in toc -->

将停止线的 Linestring（_type:stop_line_）放置在白线靠近来车方向一侧的边缘。

有关在 Vector Map Builder 中的创建方法，请参阅 [Web.Auto 文档 - 创建和编辑停止点（StopPoint）](https://docs.web.auto/en/user-manuals/vector-map-builder/how-to-use/edit-maps#creation-and-edit-of-a-stop-point-stoppoint)。

![svg](../assets/vm-02-01_1.svg)

<a id="preferred-vector-map"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-02-01_2.svg)

<a id="incorrect-vector-map"></a>

#### 错误的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-02-01_3.svg)

---

<a id="vm-02-02-stop-sign"></a>

### vm-02-02 停车标志

<a id="detail-of-requirements_1"></a>

#### 需求详情 <!-- omit in toc -->

道路上没有停止线但有停车标志时，在标志旁创建 Linestring 作为停止线。

从 Lanelet（_subtype:road_）创建指向 Regulatory Element（_subtype:traffic_sign_）的引用，再让该监管元素引用 Linestring（_type:stop_line_）和 Linestring（_type:traffic_sign, subtype:stop_sign_）。

![svg](../assets/vm-02-02_1.svg)

<a id="preferred-vector-map_1"></a>

#### 推荐的矢量地图 <!-- omit in toc -->

![svg](../assets/vm-02-02_2.svg)

<a id="incorrect-vector-map_1"></a>

#### 错误的矢量地图 <!-- omit in toc -->

无特别说明。

<a id="related-autoware-module"></a>

#### 相关 Autoware 模块

- [停止线设计 - Autoware Universe 文档](https://autowarefoundation.github.io/autoware_core/main/planning/behavior_velocity_planner/autoware_behavior_velocity_stop_line_module/)
