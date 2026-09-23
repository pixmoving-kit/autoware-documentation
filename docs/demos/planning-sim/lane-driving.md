<a id="lane-driving-scenario"></a>

# 车道内行驶场景

<a id="1-launch-autoware"></a>

## 1. 启动 Autoware

```bash
source ~/autoware/install/setup.bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/autoware_data/maps/sample-map-planning vehicle_model:=sample_vehicle sensor_model:=sample_sensor_kit
```

!!! warning

    注意，此处不能用 `~` 代替 `$HOME`。

    如果使用 `~`，地图将无法加载。

![启动 Autoware 后](images/lane-following/after-autoware-launch.png)

<a id="2-set-an-initial-pose-for-the-ego-vehicle"></a>

## 2. 设置自车的初始位姿

![设置初始位姿](images/lane-following/set-initial-pose.png)

a) 点击工具栏中的 `2D Pose estimate` 按钮，或按 `P` 键。

b) 在 3D 视图窗格中按住鼠标左键，然后拖动以设置初始位姿的方向。此时应显示代表车辆的图像。

!!! warning

    请将车辆初始位姿的方向设置为与车道方向一致。

    可通过地图上显示的箭头确认车道方向。

<a id="3-set-a-goal-pose-for-the-ego-vehicle"></a>

## 3. 设置自车的目标位姿

a) 点击工具栏中的 `2D Goal Pose` 按钮，或按 `G` 键。

b) 在 3D 视图窗格中按住鼠标左键，然后拖动以设置目标位姿的方向。如果设置正确，会显示从初始位姿到目标位姿的规划路径。

![设置目标位姿](images/lane-following/set-goal-pose.png)

<a id="4-start-the-ego-vehicle"></a>

## 4. 启动自车

现在可以点击 `AutowareStatePanel` 中的 `Auto` 按钮，让自车开始行驶。
也可以运行以下命令，手动启动车辆：

```bash
source ~/autoware/install/setup.bash
ros2 service call /api/operation_mode/change_to_autonomous autoware_adapi_v1_msgs/srv/ChangeOperationMode {}
```

随后，`Auto` 按钮会处于选中状态并变灰。

![开始行驶](images/lane-following/start-driving.png)
