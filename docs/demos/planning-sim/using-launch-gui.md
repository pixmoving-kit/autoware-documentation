<a id="planning-simulator-demo-with-autoware-launch-gui"></a>

# 使用 Autoware Launch GUI 进行规划仿真演示

本节逐步介绍如何使用 Autoware Launch GUI 进行规划仿真，为基础仿真章节中的命令行操作提供另一种方式。

<a id="getting-started-with-autoware-launch-gui"></a>

## Autoware Launch GUI 入门

1. **安装：** 确保已安装 Autoware Launch GUI。参阅[安装说明](https://github.com/autowarefoundation/autoware-launch-gui#installation)。

2. **启动 GUI：** 从应用程序菜单中打开 Autoware Launch GUI。

   ![启动 GUI 的界面截图](images/launch-gui/launch_gui_main.png)

<a id="launching-a-planning-simulation"></a>

## 启动规划仿真

<a id="lane-driving-scenario"></a>

### 车道内行驶场景

1. **设置 Autoware 路径：** 在 GUI 中设置 Autoware 的安装路径。

   ![设置 Autoware 路径的界面截图](images/launch-gui/launch_gui_setup.png)

2. **选择启动文件：** 为车道内行驶场景选择 `planning_simulator.launch.xml`。

   ![选择启动文件的界面截图](images/launch-gui/selecting_launch_file.png)

3. **自定义参数：** 根据需要调整 `map_path`、`vehicle_model` 和 `sensor_model` 等参数。

   ![自定义参数的界面截图](images/launch-gui/customizing-parameters1.png)
   ![自定义参数的界面截图](images/launch-gui/customizing-parameters2.png)

4. **开始仿真：** 点击启动按钮开始仿真。

   ![开始仿真的界面截图](images/launch-gui/starting_simulation.png)

5. **其他场景：** 此后，可以按照[规划场景仿真](index.md#basic-simulations)中的说明操作。

<a id="monitoring-and-managing-the-simulation"></a>

## 监控和管理仿真

- **实时监控：** 使用 GUI 实时监控 CPU／内存使用情况和 Autoware 日志。
- **配置管理：** 保存仿真配置，便于在后续仿真中快速使用。
- **调整参数：** 通过 GUI 便捷地动态修改仿真参数。
