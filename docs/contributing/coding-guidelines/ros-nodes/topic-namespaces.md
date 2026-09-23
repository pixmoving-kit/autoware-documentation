<a id="topic-namespaces"></a>

# 话题命名空间

<a id="overview"></a>

## 概述

ROS 支持为话题、参数和节点设置命名空间，具有以下好处：

- 同一节点类型的多个实例不会发生命名冲突。
- 节点发布的话题可以自动采用该节点的命名空间，形成含义明确且直观的关联。
- 避免根命名空间杂乱。
- 有助于保持关注点分离。

本页重点介绍如何在 Autoware 中使用命名空间，并提供实用示例。话题命名空间的基础知识请参阅[此教程](https://design.ros2.org/articles/topic_and_service_names.html)。

<a id="how-topics-should-be-named-in-node"></a>

## 节点中的话题应如何命名

Autoware 按以下功能类别划分节点，并根据类别为节点添加起始命名空间。

- localization
- perception
- planning
- control
- sensing
- vehicle
- map
- system

当节点在某个命名空间中运行时，它发布的所有话题都会使用相同的命名空间。Autoware 软件栈中的所有节点都必须支持命名空间，应避免在全局命名空间中发布话题等做法。

一般来说，应根据产生话题的节点功能为话题设置命名空间，而非根据使用该话题的一个或多个节点来设置。

根据话题是被节点订阅还是发布，将其分为输入话题和输出话题。在节点中，输入话题命名为 `input/topic_name`，输出话题命名为 `output/topic_name`。

在节点的 launch 文件中配置话题。以 `joy_controller` 节点为例，在下方示例的 `joy_controller.launch.xml` 文件中设置输入、输出话题，并进行话题重映射。

```xml
<launch>
  <arg name="input_joy" default="/joy"/>
  <arg name="input_odometry" default="/localization/kinematic_state"/>

  <arg name="output_control_command" default="/external/$(var external_cmd_source)/joy/control_cmd"/>
  <arg name="output_external_control_command" default="/api/external/set/command/$(var external_cmd_source)/control"/>
  <arg name="output_shift" default="/api/external/set/command/$(var external_cmd_source)/shift"/>
  <arg name="output_turn_signal" default="/api/external/set/command/$(var external_cmd_source)/turn_signal"/>
  <arg name="output_heartbeat" default="/api/external/set/command/$(var external_cmd_source)/heartbeat"/>
  <arg name="output_gate_mode" default="/control/gate_mode_cmd"/>
  <arg name="output_vehicle_engage" default="/vehicle/engage"/>

  <node pkg="joy_controller" exec="joy_controller" name="joy_controller" output="screen">
    <remap from="input/joy" to="$(var input_joy)"/>
    <remap from="input/odometry" to="$(var input_odometry)"/>

    <remap from="output/control_command" to="$(var output_control_command)"/>
    <remap from="output/external_control_command" to="$(var output_external_control_command)"/>
    <remap from="output/shift" to="$(var output_shift)"/>
    <remap from="output/turn_signal" to="$(var output_turn_signal)"/>
    <remap from="output/gate_mode" to="$(var output_gate_mode)"/>
    <remap from="output/heartbeat" to="$(var output_heartbeat)"/>
    <remap from="output/vehicle_engage" to="$(var output_vehicle_engage)"/>
  </node>
</launch>
```

<a id="topic-names-in-the-code"></a>

## 代码中的话题名称

1. 包含 `~`，以应用 launch 配置中的命名空间（不应以根 `/` 开头）。

2. 与其他节点通信的话题，应在话题名称前使用 `~/input` 或 `~/output` 命名空间。

   例如，在 `obstacle_avoidance_planner` 节点中，使用 `~/input/topic_name` 形式的话题名称订阅话题。

   ```cpp
   objects_sub_ = create_subscription<PredictedObjects>(
    "~/input/objects", rclcpp::QoS{10},
    std::bind(&ObstacleAvoidancePlanner::onObjects, this, std::placeholders::_1));
   ```

   例如，在 `obstacle_avoidance_planner` 节点中，使用 `~/output/topic_name` 形式的话题名称发布话题。

   ```cpp
   traj_pub_ = create_publisher<Trajectory>("~/output/path", 1);
   ```

3. 用于可视化或调试的话题应使用 `~/debug/` 命名空间。

   例如，在 `obstacle_avoidance_planner` 节点中，为了调试或可视化话题，可使用 `~/debug/topic_name` 形式的话题名称发布信息。

   ```cpp
   debug_markers_pub_ =
    create_publisher<visualization_msgs::msg::MarkerArray>("~/debug/marker", durable_qos);

   debug_msg_pub_ =
    create_publisher<tier4_debug_msgs::msg::StringStamped>("~/debug/calculation_time", 1);
   ```

   launch 配置中的命名空间会添加到话题名称之前，因此话题名称如下：

   `/planning/scenario_planning/lane_driving/motion_planning/obstacle_avoidance_planner/debug/marker /planning/scenario_planning/lane_driving/motion_planning/obstacle_avoidance_planner/debug/calculation_time`

4. 理由：我们希望能够在 launch 文件中对话题名称进行重映射和配置。
