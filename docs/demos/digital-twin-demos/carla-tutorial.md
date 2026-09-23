<a id="carla-simulator"></a>

# CARLA 仿真器

[CARLA](https://carla.org) 是自动驾驶研究领域知名的开源仿真器。
目前尚无针对 Autoware Universe 的官方支持，但部分社区项目提供了支持。
本文列出这些项目，供希望将 Autoware 与 CARLA 配合使用的用户参考。
如果遇到问题，可以向相应项目反馈。

<a id="project-lists-in-alphabetical-order"></a>

## 项目列表（按字母顺序排列）

## autoware_carla_interface

这是一个 Autoware ROS 功能包，用于实现 Autoware 与 CARLA 仿真器之间的通信，以进行自动驾驶仿真。
它已集成到 autoware_universe 中，并持续维护，以保持与 Autoware 最新更新的兼容性。

- 功能包链接和教程：[autoware_carla_interface](https://github.com/autowarefoundation/autoware_universe/tree/main/simulator/autoware_carla_interface)。

<a id="autoware_tensorrt_vad-end-to-end-planning"></a>

### autoware_tensorrt_vad（端到端规划）

这是经过 TensorRT 优化的向量化自动驾驶（[VAD](https://github.com/hustvl/VAD)）节点，用在 CARLA（[Bench2Drive](https://github.com/Thinklab-SJTU/Bench2Drive)）上训练的单个端到端模型替代传统的感知／定位／规划栈。它提供专用于 CARLA 的启动文件，并通过 `e2e_simulator.launch.xml` 与 `autoware_launch` 集成。

- 项目链接：[autoware_tensorrt_vad](https://github.com/autowarefoundation/autoware_universe/tree/main/e2e/autoware_tensorrt_vad)（README 包含参数、话题和模型详情）
- 使用方法（CARLA E2E 模式）：
  1. 构建功能包及其依赖：`colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --packages-up-to autoware_tensorrt_vad`。
  2. 按照 `autoware_carla_interface` 的说明准备 CARLA（使用 `carla_sensor_kit`，使相机话题与 VAD 训练时的顺序一致：FRONT、BACK、FRONT_LEFT、BACK_LEFT、FRONT_RIGHT、BACK_RIGHT）。
  3. 确保已下载 VAD 模型（Autoware 设置脚本默认将其下载到 `~/autoware_data/ml_models/vad`）。
  4. 启动 CARLA 服务器（无窗口、适用于 GPU 的示例）：`./CarlaUE4.sh -prefernvidia -quality-level=Low -RenderOffScreen`。
  5. 以 E2E 模式启动 Autoware：

     ```bash
     ros2 launch autoware_launch e2e_simulator.launch.xml \
       map_path:=$HOME/autoware_data/maps/Town01 \
       vehicle_model:=sample_vehicle \
       sensor_model:=carla_sensor_kit \
       simulator_type:=carla \
       use_e2e_planning:=true \
       e2e_planning_type:=vad
     ```

### carla_autoware_bridge

这是 `carla_ros_bridge` 的附加功能包，用于将 CARLA 仿真器连接到 Autoware Universe 软件。

- 项目链接：[carla_autoware_bridge](https://github.com/Robotics010/carla_autoware_bridge)
- 教程：[https://github.com/Robotics010/carla_autoware_bridge/blob/master/getting-started.md](https://github.com/Robotics010/carla_autoware_bridge/blob/master/getting-started.md)

### open_planner

集成式开源规划器及相关工具，用于自动驾驶车辆和移动机器人的自主导航。

- 项目链接：[open_planner](https://github.com/ZATiTech/open_planner/tree/humble)
- 教程：[https://github.com/ZATiTech/open_planner/blob/humble/op_carla_bridge/README.md](https://github.com/ZATiTech/open_planner/blob/humble/op_carla_bridge/README.md)

### zenoh_carla_bridge

该项目主要用于控制 CARLA 中的多辆车辆。
它使用 [Zenoh](https://zenoh.io/) 桥接 Autoware 和 CARLA，并能够区分不同车辆的消息。
欢迎向 [autoware_carla_launch](https://github.com/evshary/autoware_carla_launch) 提问或反馈问题。

- 项目链接：
  - [autoware_carla_launch](https://github.com/evshary/autoware_carla_launch)：便于运行桥接程序和 Autoware 的集成环境。
  - [zenoh_carla_bridge](https://github.com/evshary/zenoh_carla_bridge)：桥接实现。
- 教程：
  - [autoware_carla_launch 文档](https://autoware-carla-launch.readthedocs.io/en/latest/)：官方文档，包含安装方法和多种使用场景。
  - [使用 Zenoh 在 CARLA 中运行多辆由 Autoware 驱动的车辆](https://autoware.org/running-multiple-autoware-powered-vehicles-in-carla-using-zenoh)：Autoware 技术博客中的介绍。
