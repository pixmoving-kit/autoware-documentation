<a id="traffic-light-recognition-simulation"></a>

# 交通信号灯识别仿真

默认情况下，地图上的所有交通信号灯均被视为绿灯。因此，当规划路径经过有信号灯的路口时，自车会直接通过路口而不停车。

以下步骤介绍如何设置和重置信号灯，以测试规划组件的响应。

<a id="set-traffic-light"></a>

## 设置交通信号灯

默认情况下，RViz 不显示地图上的交通信号灯 ID。
按照以下步骤启用交通信号灯 ID 显示：

1. 在 `Displays` 面板中，依次展开 `Map > Lanelet2VectorMap > Namespaces` 旁的三角图标，找到 `traffic_light_id` 话题。
2. 勾选 `traffic_light_id` 复选框。
3. 点击两次 `Map` 复选框以重新加载话题。
4. 放大相关区域或更改视图类型，以便仔细查看 ID。

![查看交通信号灯 ID](images/traffic-light/see-traffic-light-ID.png)

1. 打开 `Panels -> Add new panel`，选择 `tier4_traffic_light_rviz_plugin/TrafficLightPublishPanel`，然后点击 `OK`。

2. 在 `TrafficLightPublishPanel` 中设置交通信号灯的 `ID` 和颜色。

3. 点击 `SET` 按钮。
   ![设置交通信号灯](images/traffic-light/set-traffic-light.png)

4. 最后，点击 `PUBLISH` 按钮，将交通信号灯状态发送给仿真器。所有经过所选信号灯的规划路径都会相应变化。

![发送交通信号灯颜色](images/traffic-light/send-traffic-light-color.png)

<a id="updatereset-traffic-light"></a>

## 更新／重置交通信号灯

选择新的颜色（图中为 `GREEN`）并点击 `SET` 按钮，即可更新交通信号灯颜色。图中自车前方的信号灯由 `RED` 变为 `GREEN` 后，车辆重新起步。

![更新交通信号灯颜色后](images/traffic-light/after-traffic-light-color-update.png)

要从 `TrafficLightPublishPanel` 中移除某个交通信号灯，请点击 `RESET` 按钮。
