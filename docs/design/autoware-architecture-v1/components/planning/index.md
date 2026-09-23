<a id="planning-component-design"></a>

# 规划组件设计

<a id="purpose"></a>

## 用途

自动驾驶系统中的规划组件负责为自动驾驶车辆生成目标轨迹（路径和速度），在完成特定任务的同时保障安全并遵守交通规则。

本文概述 Autoware 中的规划需求和设计，帮助开发者理解规划组件的设计与可扩展性。

本文分为两部分：第一部分讨论高层需求与设计，后一部分侧重实际实现及所提供的功能。

<a id="goals-and-non-goals"></a>

## 目标与非目标

我们的目标不仅是开发自动驾驶系统，还要提供“自动驾驶平台”，使用户能够根据自身需求增强自动驾驶功能。

Autoware 采用[微自动驾驶架构](../../../autoware-concepts/index.md)概念，强调高度可扩展性、功能模块化和明确定义的接口。

因此，规划组件的设计策略着眼于**提供可定制、易扩展的规划开发平台**，而非解决每一种复杂自动驾驶场景（这是极具挑战的问题）。我们相信这种方式能够满足广泛需求，并最终解决许多复杂用例。

为明确这一策略，目标和非目标定义如下：

**目标：**

- **提供基本功能，使得能够定义简单的 ODD**
  - 在扩展功能之前，规划组件必须提供自动驾驶所需的基本功能，包括行驶、停车和转向等基本操作，以及在相对安全简单的场景中处理变道和避障。
- **通过功能模块化支持用户扩展**
  - 系统设计为通过扩展功能适应各种运行设计域（ODD）。类似插件的模块化方式，可用于创建满足不同需求的系统，例如不同自动驾驶等级及车辆或环境应用（如 Lv4/Lv2 自动驾驶、公共/私人道路驾驶、大型车辆、小型机器人）。
  - 针对没有障碍物的私人道路等特定 ODD 精简功能也很重要。模块化方式允许降低功耗或传感器要求，以适应具体用户需求。
- **通过人工操作员的决策扩展能力**
  - 引入操作员协助是功能扩展的关键，使系统在人类支持下适应复杂、困难的场景。这里不限定操作员类型：可以是原型开发阶段的随车人员，也可以是在自动驾驶服务紧急情况下接入的远程操作员。

**非目标：**

规划组件支持通过第三方模块扩展。因此，以下并非 Autoware 规划组件的目标：

- 默认提供用户所需的全部功能。
- 提供完整自动驾驶系统的全部功能和性能。
- 提供始终超越人类能力或保证绝对安全的性能。

这些方面基于我们对自动驾驶“平台”的愿景，可能不适用于通常的自动驾驶规划组件。

<a id="high-level-design"></a>

## 高层设计

下图展示规划组件的高层架构。这是理想化设计，当前实现可能有所不同。本文后续章节提供实现细节。

![overall-planning-architecture](image/high-level-planning-diagram.drawio.svg)

遵循微自动驾驶架构原则，我们采用模块化系统框架。规划领域的功能以模块形式实现，可根据具体用例动态或静态调整，包括变道、交叉路口处理和人行横道等模块。

规划组件包含多个子组件：

- **任务规划**：利用地图数据计算当前位置到目的地的路线，功能类似车队管理系统（FMS）或车载导航的路线规划。
- **规划模块**：针对分配的任务规划车辆行为，包括目标轨迹、转向灯信号等，分为行为和运动两类：
  - **行为**：重点计算安全且符合规则的路线，处理变道、进入交叉路口和停止线停车等决策。
  - **运动**：与行为模块协作，考虑车辆运动特性和乘坐舒适性来确定轨迹，包括用于路径整形和速度计算的横向与纵向规划。
- **验证**：保证规划轨迹的安全性和适当性，并具备紧急响应能力。轨迹不合适时，触发紧急处置或生成替代路径。

<a id="highlights"></a>

### 重点

此高层设计的关键方面包括：

<a id="modularization-of-each-function"></a>

#### 各项功能模块化

路线生成、变道和交叉路口管理等基本规划功能均被模块化。这些模块具有标准接口，便于添加或修改。后文将讨论接口细节。如何启用/禁用各模块，请参阅[规划实现文档](https://autowarefoundation.github.io/autoware_universe/main/planning/#how-to-enable-or-disable-planning-module)。

<a id="separation-of-mission-planning-sub-component"></a>

#### 独立的任务规划子组件

任务规划替代现有 FMS（车队管理系统）等服务中的常见功能。遵循高层设计中定义的接口，便于与第三方服务集成。

<a id="separation-of-validation-sub-component"></a>

#### 独立的验证子组件

由于规划组件可扩展，很难确保所有功能具有一致的安全水平。因此，验证功能独立于核心规划模块进行管理，即使任意修改规划模块，也能保持基本安全水平。

<a id="interface-for-hmi-human-machine-interface"></a>

#### HMI（人机界面）接口

HMI 旨在与人工操作员顺畅协作。这些接口支持规划组件与车内或远程操作员协调。

<a id="trade-offs-for-the-separation-of-planning-and-other-components"></a>

#### 规划与其他组件分离的取舍

在 Autoware 总体设计中，将规划、感知、定位和控制等组件分离，便于与第三方模块协作。但这种分离涉及性能与可扩展性之间的取舍。例如，感知组件与规划分离，可能处理不必要的目标；规划与控制分离，则可能使规划难以考虑车辆动力学。为缓解这些问题，可能需要增强接口信息或增加计算工作。

<a id="customize-features"></a>

## 自定义功能

规划组件设计的重要特点是能够集成外部模块。下图展示引入外部功能的多种方式。

![how-to-add-new-modules](image/how-to-add-new-modules.drawio.svg)

<a id="1-adding-new-modules-to-the-planning-component"></a>

### 1. 向规划组件添加新模块

用户可使用新模块增强或替换现有规划功能。这是扩展功能的常用方法，可添加目标 ODD 中缺少的能力，也可精简现有功能。

但添加这些功能需要组织良好的模块接口。截至 2023 年 11 月，理想的模块化系统尚未完全建立，仍有一些限制。详情请参阅参考实现部分的[在当前实现中自定义功能](#customize-features-in-the-current-implementation)和[规划实现文档](https://autowarefoundation.github.io/autoware_universe/main/planning/#how-to-enable-or-disable-planning-module)。

<a id="2-replacing-sub-components-of-planning"></a>

### 2. 替换规划子组件

某些用户可能希望在子组件层面协作和扩展，例如用现有 FMS 服务替换任务规划，或在保留现有验证功能的同时引入第三方轨迹生成模块。

遵循[规划组件内部接口](#internal-interface-in-the-planning-component)，即可在该层面协作和扩展。虽然与现有规划功能的复杂协作可能受限，但仍能集成某些规划组件功能与外部模块。

<a id="3-replacing-the-entire-planning-component"></a>

### 3. 替换整个规划组件

开发自动驾驶规划系统的组织或研究机构，可能希望将专有规划方案与 Autoware 感知或控制模块集成。这可通过替换整个规划系统，并遵循组件间定义的稳健、稳定接口来实现。但需注意，可能无法与现有规划模块直接协作。

<a id="component-interface"></a>

## 组件接口

本节介绍规划组件及其内部模块的输入和输出。当前实现请参阅[规划组件接口](../../interfaces/components/planning.md)页面。

<a id="input-to-the-planning-component"></a>

### 规划组件的输入

- **来自地图组件**
  - 矢量地图：包含环境的全部静态信息，包括用于路线规划的车道连接信息、用于生成参考路径的车道几何信息，以及交通规则信息。
- **来自感知组件**
  - 检测目标信息：提供行人和其他车辆等无法预先获知的目标的实时信息。规划组件规划动作，避免与这些目标碰撞。
  - 检测障碍物信息：提供障碍物位置的实时信息，比检测目标更基础，用于紧急停车等安全措施。
  - 占据地图信息：提供行人和其他车辆存在情况，以及遮挡区域的实时信息。
  - 交通信号灯识别结果：实时提供各交通信号灯的当前状态。规划组件提取与规划路径相关的信息，决定是否在交叉路口停车。
- **来自定位组件**
  - 车辆运动信息：包括自车位置、速度、加速度及其他运动相关数据。
- **来自系统组件**
  - 运行模式：指示车辆是否处于自动驾驶模式。
- **来自人机界面（HMI）**
  - 功能执行：允许人工操作员执行/授权变道或进入交叉路口等自动驾驶操作。
- **来自 API 层**
  - 目的地（目标）：规划组件最终要到达的位置。
  - 检查点：通往目的地路线上的中间点，用于路线计算。
  - 速度限制：设置车辆最高限速。

<a id="output-from-the-planning-component"></a>

### 规划组件的输出

- **发送到控制组件**
  - 轨迹：提供控制组件必须跟踪的平滑位姿、速度旋量和加速度序列。轨迹通常长 10 秒，分辨率为 0.1 秒。
  - 转向灯：根据规划动作控制车辆右转、左转、危险警告等灯光。
- **发送到系统组件**
  - 诊断：报告规划组件状态，指示处理是否正常运行以及是否生成安全规划。
- **发送到人机界面（HMI）**
  - 功能执行可用性：指示可执行或需要执行的操作状态，例如变道或进入交叉路口。
  - 候选轨迹：显示用户执行操作后将实施的潜在轨迹。
- **发送到 API 层**
  - 规划因素：提供当前规划行为的原因信息，可能包括需要避让的目标位置、导致停车决策的障碍物及其他相关信息。

<a id="internal-interface-in-the-planning-component"></a>

### 规划组件内部接口

- **任务规划到场景规划**
  - 路线：提供从起点到目的地所需遵循的路径指导，基于地图中定义的车道 ID 等信息确定。在路线层面不明确指定具体采用哪条车道，路线可包含多条车道。
- **行为规划到运动规划**
  - 路径：提供车辆应跟踪的大致位置和速度。路径点通常间隔约 1 米，也可使用其他间距，但可能影响规划组件精度或性能。
  - 可行驶区域：定义车辆可行驶的区域，例如车道内或物理上可通行的区域。假设运动规划器在该区域内计算最终轨迹。
- **场景规划到验证**
  - 轨迹：定义控制组件尝试跟踪的期望位置、速度和加速度。根据轨迹速度，以约 0.1 秒的间隔定义轨迹点。
- **验证到控制组件**
  - 轨迹：与上述相同，但增加了一些安全考虑。

<a id="detailed-design"></a>

## 详细设计

<a id="supported-features"></a>

### 支持的功能

| 功能 | 描述 | 要求 | 图示 | 演示 |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 路线规划 | 规划从自车位置到目的地的路线。 <br> <br> 参考实现见 [任务规划器](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_mission_planner_universe/), 通过启动以下节点启用： `mission_planner` 节点。 | - Lanelet 地图（行驶 lanelet） | ![route-planning](image/features-route-planning.drawio.svg) |  |
| 根据路线规划路径 | 根据给定路线规划要跟踪的路径。 <br> <br> 参考实现见 [行为路径规划器](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_planner/). | - Lanelet 地图（行驶 lanelet） | ![lane-follow](image/features-lane-follow.drawio.svg) |  |
| 避障 | 规划通过转向避让障碍物的路径。 <br> <br> 参考实现见 [静态避障模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_static_obstacle_avoidance_module/), [路径优化器](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_path_optimizer/). 参数中的启用标志： `launch path_optimizer true` | - 目标信息 | ![obstacle-avoidance](image/features-avoidance.drawio.svg) | [演示视频](https://youtu.be/A_V9yvfKZ4E) <br> [![演示视频](https://img.youtube.com/vi/A_V9yvfKZ4E/0.jpg)](https://www.youtube.com/watch?v=A_V9yvfKZ4E) |
| 路径平滑 | 规划路径以实现平滑转向。 <br> <br> 参考实现见 [路径优化器](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_path_optimizer/). | - Lanelet 地图（行驶 lanelet） | ![path-smoothing](image/features-path-smoothing.drawio.svg) | [演示视频](https://youtu.be/RhyAF26Ppzs) <br> [![演示视频](https://img.youtube.com/vi/RhyAF26Ppzs/0.jpg)](https://www.youtube.com/watch?v=RhyAF26Ppzs) |
| 狭窄空间行驶 | 规划在可行驶区域内行驶的路径。如果无法在可行驶区域内行驶，则停车以避免驶出该区域。 <br> <br> 参考实现见 [路径优化器](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_path_optimizer/). | - Lanelet 地图（高精度车道边界） | ![narrow-space-driving](image/features-narrow-space-driving.drawio.svg) | [演示视频](https://youtu.be/URzcLO2E1vY) <br> [![演示视频](https://img.youtube.com/vi/URzcLO2E1vY/0.jpg)](https://www.youtube.com/watch?v=URzcLO2E1vY) |
| 变道 | 规划变道路径以到达目的地。 <br> <br> 参考实现见 [变道](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_lane_change_module/). | - Lanelet 地图（行驶 lanelet） | ![lane-change](image/features-lane-change.drawio.svg) | [演示视频](https://youtu.be/0jRDGQ84cD4) <br> [![演示视频](https://img.youtube.com/vi/0jRDGQ84cD4/0.jpg)](https://www.youtube.com/watch?v=0jRDGQ84cD4) |
| 靠边停车 | 规划驶向路肩停车的路径。 <br> <br> 参考实现见 [目标规划器](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_goal_planner_module/). | - Lanelet 地图（路肩车道） | ![pull-over](image/features-pull-over.drawio.svg) | 演示视频： <br> [简单靠边停车](https://youtu.be/r3-kAmTb4hc) <br> [![演示视频](https://img.youtube.com/vi/r3-kAmTb4hc/0.jpg)](https://www.youtube.com/watch?v=r3-kAmTb4hc) <br> [前进圆弧靠边停车](https://youtu.be/ornbzkWxRWU) <br> [![演示视频](https://img.youtube.com/vi/ornbzkWxRWU/0.jpg)](https://www.youtube.com/watch?v=ornbzkWxRWU) <br> [倒车圆弧靠边停车](https://youtu.be/if-0tG3AkLo) <br> [![演示视频](https://img.youtube.com/vi/if-0tG3AkLo/0.jpg)](https://www.youtube.com/watch?v=if-0tG3AkLo) |
| 驶离路肩 | 规划从路肩起步的路径。 <br> <br> 参考实现见 [起步规划器](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_start_planner_module/). | - Lanelet 地图（路肩车道） | ![pull-out](image/features-pull-out.drawio.svg) | 演示视频： <br> [简单驶出](https://youtu.be/xOjnPqoHup4) <br> [![演示视频](https://img.youtube.com/vi/xOjnPqoHup4/0.jpg)](https://www.youtube.com/watch?v=xOjnPqoHup4) <br> [倒车驶出](https://youtu.be/iGieijPcPcQ) <br> [![演示视频](https://img.youtube.com/vi/iGieijPcPcQ/0.jpg)](https://www.youtube.com/watch?v=iGieijPcPcQ) |
| 路径横移 | 根据外部指令规划横向偏移路径。 <br> <br> 参考实现见 [横移模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_side_shift_module/). | - 无 | ![side-shift](image/features-side-shift.drawio.svg) |  |
| 障碍物停车 | 规划速度，在路径上的障碍物前停车。 <br> <br> 参考实现见 [障碍物停车规划器](https://autowarefoundation.github.io/autoware_core/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_stop_module/), [障碍物巡航规划器](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_cruise_module/). `launch obstacle_stop_planner`，启用标志： `TODO`, `launch obstacle_cruise_planner`，启用标志： `TODO` | - 目标信息 | ![obstacle-stop](image/features-obstacle-stop.drawio.svg) | [演示视频](https://youtu.be/d8IRW_xArcE) <br> [![演示视频](https://img.youtube.com/vi/d8IRW_xArcE/0.jpg)](https://www.youtube.com/watch?v=d8IRW_xArcE) |
| 障碍物减速 | 规划速度，针对路径周围的障碍物减速。 <br> <br> 参考实现见 [障碍物停车规划器](https://autowarefoundation.github.io/autoware_core/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_stop_module/), [障碍物巡航规划器](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_cruise_module/). | - 目标信息 | ![obstacle-decel](image/features-obstacle-decel.drawio.svg) | [演示视频](https://youtu.be/gvN1otgeaaw) <br> [![演示视频](https://img.youtube.com/vi/gvN1otgeaaw/0.jpg)](https://www.youtube.com/watch?v=gvN1otgeaaw) |
| 自适应巡航控制 | 规划速度以跟随自车前方行驶的车辆。 <br> <br> 参考实现见 [障碍物停车规划器](https://autowarefoundation.github.io/autoware_core/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_stop_module/), [障碍物巡航规划器](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_cruise_module/). | - 目标信息 | ![adaptive-cruise](image/features-adaptive-cruise.drawio.svg) |  |
| 针对切入车辆减速 | 规划速度，避免车辆切入自车车道带来的风险。 <br> <br> 参考实现见 [障碍物巡航规划器](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_cruise_module/). | - 目标信息 | ![cut-in](image/features-cut-in.drawio.svg) |  |
| 起步周围检查 | 规划速度，在车辆周围有障碍物时阻止起步。 <br> <br> 参考实现见 [周围障碍物检查器](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_surround_obstacle_checker/). 参数中的启用标志： `use_surround_obstacle_check true`，位于 [tier4_planning_component.launch.xml](https://github.com/autowarefoundation/autoware_launch/blob/2850d7f4e20b173fde2183d5323debbe0067a990/autoware_launch/launch/components/tier4_planning_component.launch.xml#L8) < | - 目标信息 | ![surround-check](image/features-surround-check.drawio.svg) | [演示视频](https://youtu.be/bbGgtXN3lC4) <br> [![演示视频](https://img.youtube.com/vi/bbGgtXN3lC4/0.jpg)](https://www.youtube.com/watch?v=bbGgtXN3lC4) |
| 弯道减速 | 规划速度，以在弯道上减速。 <br> <br> 参考实现见 [运动速度平滑器](https://autowarefoundation.github.io/autoware_core/main/planning/autoware_velocity_smoother/). | - 无 | ![decel-on-curve](image/features-decel-on-curve.drawio.svg) |  |
| 弯道障碍物减速 | 规划速度，在弯道上因路径周围的障碍物碰撞风险而减速。 <br> <br> 参考实现见 [障碍物速度限制器](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_velocity_limiter_module/). | - 目标信息 <br> - Lanelet 地图（静态障碍物） | ![decel-on-curve-obstacles](image/features-decel-on-curve-obstacles.drawio.svg) | [演示视频](https://youtu.be/I-oFgG6kIAs) <br> [![演示视频](https://img.youtube.com/vi/I-oFgG6kIAs/0.jpg)](https://www.youtube.com/watch?v=I-oFgG6kIAs) |
| 人行横道 | 规划速度，为接近或正在穿越人行横道的行人停车或减速。 <br> <br> 参考实现见 [人行横道模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_crosswalk_module/). | - 目标信息 <br> - Lanelet 地图（人行横道） | ![crosswalk](image/features-crosswalk.drawio.svg) | [演示视频](https://youtu.be/tUvthyIL2W8) <br> [![演示视频](https://img.youtube.com/vi/tUvthyIL2W8/0.jpg)](https://www.youtube.com/watch?v=tUvthyIL2W8) |
| 交叉路口对向车辆检查 | 规划交叉路口左转/右转速度，避免与对向车辆发生危险。 <br> <br> 参考实现见 [交叉路口模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/). | - 目标信息 <br> - Lanelet 地图（交叉路口车道和让行车道） | ![intersection](image/features-intersection.drawio.svg) | [演示视频](https://youtu.be/SGD07Hqg4Hk) <br> [![演示视频](https://img.youtube.com/vi/SGD07Hqg4Hk/0.jpg)](https://www.youtube.com/watch?v=SGD07Hqg4Hk) |
| 交叉路口盲区检查 | 规划交叉路口左转/右转速度，避免与从后方盲区接近的其他车辆或摩托车发生危险。 <br> <br> 参考实现见 [盲区模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_blind_spot_module/). | - 目标信息 <br> - Lanelet 地图（交叉路口车道） | ![blind-spot](image/features-blind-spot.drawio.svg) | [演示视频](https://youtu.be/oaTCJRafDGA) <br> [![演示视频](https://img.youtube.com/vi/oaTCJRafDGA/0.jpg)](https://www.youtube.com/watch?v=oaTCJRafDGA) |
| 交叉路口遮挡检查 | 规划交叉路口左转/右转速度，避免遮挡区域可能驶来的车辆造成风险。 <br> <br> 参考实现见 [交叉路口模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/). | - 目标信息 <br> - Lanelet 地图（交叉路口车道） | ![intersection-occlusion](image/features-intersection-occlusion.drawio.svg) | [演示视频](https://youtu.be/bAHXMB7kbFc) <br> [![演示视频](https://img.youtube.com/vi/bAHXMB7kbFc/0.jpg)](https://www.youtube.com/watch?v=bAHXMB7kbFc) |
| 交叉路口拥堵检测 | 规划交叉路口速度，在前方车辆因拥堵停车时避免进入交叉路口。 <br> <br> 参考实现见 [交叉路口模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/). | - 目标信息 <br> - Lanelet 地图（交叉路口车道） | ![intersection-traffic-jam](image/features-intersection-traffic-jam.drawio.svg) | [演示视频](https://youtu.be/negK4VbrC5o) <br> [![演示视频](https://img.youtube.com/vi/negK4VbrC5o/0.jpg)](https://www.youtube.com/watch?v=negK4VbrC5o) |
| 交通信号灯 | 根据交通信号灯指示规划交叉路口速度。 <br> <br> 参考实现见 [交通信号灯模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_traffic_light_module/). | - 交通信号灯颜色信息 | ![traffic-light](image/features-traffic-light.drawio.svg) | [演示视频](https://youtu.be/lGA53KljQrM) <br> [![演示视频](https://img.youtube.com/vi/lGA53KljQrM/0.jpg)](https://www.youtube.com/watch?v=lGA53KljQrM) |
| 突入检查 | 规划速度，针对附近目标可能突然进入路径的情况减速。 <br> <br> 参考实现见 [突入模块](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_run_out_module/). | - 目标信息 | ![run-out](image/features-run-out.drawio.svg) | [演示视频](https://youtu.be/9IDggldT2t0) <br> [![演示视频](https://img.youtube.com/vi/9IDggldT2t0/0.jpg)](https://www.youtube.com/watch?v=9IDggldT2t0) |
| 停止线 | 规划速度，以在停止线前停车。 <br> <br> 参考实现见 [停止线模块](https://autowarefoundation.github.io/autoware_core/main/planning/behavior_velocity_planner/autoware_behavior_velocity_stop_line_module/). | - Lanelet 地图（停止线） | ![stop-line](image/features-stop-line.drawio.svg) | [演示视频](https://youtu.be/eej9jYt-GSE) <br> [![演示视频](https://img.youtube.com/vi/eej9jYt-GSE/0.jpg)](https://www.youtube.com/watch?v=eej9jYt-GSE) |
| 遮挡点检查 | 规划速度，为从遮挡区域（如大型车辆后方）突然出现的目标减速。 <br> <br> 参考实现见 [遮挡点模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_occlusion_spot_module/). | - 目标信息 <br> - Lanelet 地图（私人/公共车道） | ![occlusion-spot](image/features-occlusion-spot.drawio.svg) | [演示视频](https://youtu.be/3qs8Ivjh1fs) <br> [![演示视频](https://img.youtube.com/vi/3qs8Ivjh1fs/0.jpg)](https://www.youtube.com/watch?v=3qs8Ivjh1fs) |
| 禁止停止区域 | 规划速度，避免在消防站入口前等禁止停止区域停车。 <br> <br> 参考实现见 [禁止停止区域模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_no_stopping_area_module/). | - Lanelet 地图（禁止停止区域） | ![no-stopping-area](image/features-no-stopping-area.drawio.svg) |  |
| 从私人区域汇入公共道路 | 规划从私人车道驶入公共道路的速度，避免与行人或其他车辆碰撞。 <br> <br> 参考实现见 [私人区域汇入模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/). | - 目标信息 <br> - Lanelet 地图（私人/公共车道） | WIP |  |
| 减速带 | 规划速度，为减速带减速。 <br> <br> 参考实现见 [减速带模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_speed_bump_module/). | - Lanelet 地图（减速带） | ![speed-bump](image/features-speed-bump.drawio.svg) | [演示视频](https://youtu.be/FpX3q3YaaCw) <br> [![演示视频](https://img.youtube.com/vi/FpX3q3YaaCw/0.jpg)](https://www.youtube.com/watch?v=FpX3q3YaaCw) |
| 检测区域 | 规划速度，当指定检测区域中存在目标时，在对应停止位置停车。 <br> <br> 参考实现见 [检测区域模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_detection_area_module/). | - Lanelet 地图（检测区域） | ![detection-area](image/features-detection-area.drawio.svg) | [演示视频](https://youtu.be/YzXF4U69lJs) <br> [![演示视频](https://img.youtube.com/vi/YzXF4U69lJs/0.jpg)](https://www.youtube.com/watch?v=YzXF4U69lJs) |
| 不可行驶车道 | 规划速度，在驶出 ODD（运行设计域）指定区域前停车；如果在 ODD 外的车道启动自动模式，也将车辆停止。 <br> <br> 参考实现见 [不可行驶车道模块](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_no_drivable_lane_module/). | - Lanelet 地图（不可行驶车道） | ![no-drivable-lane](image/features-no-drivable-lane.drawio.svg) |  |
| 偏离车道时的碰撞检测 | 规划速度，在自车偏离自身车道时避免与其他车道行驶的车辆发生冲突。 <br> <br> 参考实现见 [驶出车道模块](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_out_of_lane_module/). | - 目标信息 <br> - Lanelet 地图（行驶车道） | WIP |  |
| 泊车 | 针对停车区域内的给定目标规划路径和速度。 <br> <br> 参考实现见 [自由空间规划器](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_freespace_planner/). | - 目标信息 <br> - Lanelet 地图（停车区域） | ![parking](image/features-parking.drawio.svg) | [演示视频](https://youtu.be/rAIYmwpNWfA) <br> [![演示视频](https://img.youtube.com/vi/rAIYmwpNWfA/0.jpg)](https://www.youtube.com/watch?v=rAIYmwpNWfA) |
| 自动紧急制动（AEB） | 预期会与前方目标碰撞时执行紧急停车。注意，此功能作为最后一道安全防线，即使定位或感知系统故障也应有效。 <br> <br> 参考实现见 [驶出车道模块](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_out_of_lane_module/). | - 基础目标 | ![aeb](image/features-aeb.drawio.svg) |  |
| 最小风险操作（MRM） | 发生危险事件时提供适当的 MRM（最小风险操作）指令。例如，发现传感器故障时，根据严重程度发送紧急制动、平缓停车或靠边停车指令。 <br> <br> 参考实现见 TODO | - TODO | WIP |  |
| 轨迹验证 | 检查规划轨迹是否安全。如果不安全，采取修改轨迹、停止发送轨迹或向自动驾驶系统报告等适当措施。 <br> <br> 参考实现见 [规划验证器](https://autowarefoundation.github.io/autoware_universe/main/planning/planning_validator/autoware_planning_validator/). | - 无 | ![trajectory-validation](image/features-trajectory-validation.drawio.svg) |  |
| 行驶车道地图生成 | 根据人工驾驶时记录的定位数据生成车道地图。 <br> <br> 参考实现见 WIP | - 无 | WIP |  |
| 行驶车道优化 | 考虑车辆运动学，优化地图中心线（参考路径）使其平滑。 <br> <br> 参考实现见 [静态中心线优化器](https://autowarefoundation.github.io/autoware_tools/main/planning/autoware_static_centerline_generator/). | - Lanelet 地图（行驶车道） | WIP |  |

<!-- ![supported-functions](image/planning-functions.drawio.svg) -->

<a id="reference-implementation"></a>

### 参考实现

下图描述规划组件的参考实现。通过添加新模块或扩展功能，可支持多种 ODD。

_注意，由于实现困难，某些实现不符合高层架构设计，需要更新。_

![reference-implementation](image/planning-diagram-tmp.drawio.svg)

详情请参阅各软件包中的设计文档。

- [_mission_planner_](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_mission_planner_universe/)：根据地图信息计算起点到目标的路线。
- [_behavior_path_planner_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_planner/)：根据交通规则计算路径和可行驶区域。
  - [_lane_following_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_planner#lane-following)
  - [_lane_change_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_lane_change_module/)
  - [_static_obstacle_avoidance_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_static_obstacle_avoidance_module/)
  - [_pull_over_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_goal_planner_module/)
  - [_pull_out_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_start_planner_module/)
  - [_side_shift_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_side_shift_module/)
- [_behavior_velocity_planner_](https://autowarefoundation.github.io/autoware_core/main/planning/behavior_velocity_planner/autoware_behavior_velocity_planner/)：根据交通规则计算最大速度。
  - [_detection_area_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_detection_area_module/)
  - [_blind_spot_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_blind_spot_module/)
  - [_cross_walk_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_crosswalk_module/)
  - [_stop_line_](https://autowarefoundation.github.io/autoware_core/main/planning/behavior_velocity_planner/autoware_behavior_velocity_stop_line_module/)
  - [_traffic_light_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_traffic_light_module/)
  - [_intersection_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_intersection_module/)
  - [_no_stopping_area_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_no_stopping_area_module/)
  - [_virtual_traffic_light_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_virtual_traffic_light_module/)
  - [_occlusion_spot_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_velocity_planner/autoware_behavior_velocity_occlusion_spot_module/)
  - [_run_out_](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_run_out_module/)
- [_obstacle_avoidance_planner_](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_dynamic_obstacle_avoidance_module/)：在障碍物和可行驶区域约束下计算路径形状
- [_surround_obstacle_checker_](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_surround_obstacle_checker/)：自车周围存在障碍物时，保持车辆停止。仅在车辆停止时生效。
- [_obstacle_stop_planner_](https://autowarefoundation.github.io/autoware_core/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_stop_module/)：轨迹上或附近有障碍物时，根据停车、减速或自适应巡航（跟车）等情况计算轨迹点的最大速度。
  - [_stop_](https://autowarefoundation.github.io/autoware_core/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_stop_module/)
  - [_slow_down_](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_slow_down_module/)
  - [_adaptive_cruise_](https://autowarefoundation.github.io/autoware_universe/main/planning/motion_velocity_planner/autoware_motion_velocity_obstacle_cruise_module/)
- [_costmap_generator_](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_costmap_generator)：根据动态目标和车道信息生成用于路径生成的代价地图。
- [_freespace_planner_](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_freespace_planner/)：在自由空间场景中，考虑曲率等可行性因素计算轨迹。算法见[此处](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_freespace_planning_algorithms/)。
- _scenario_selector_：根据当前场景选择轨迹。
- [_external_velocity_limit_selector_](https://autowarefoundation.github.io/autoware_universe/main/planning/autoware_external_velocity_limit_selector/)：从多个候选中选择适当的速度限制。
- [_motion_velocity_smoother_](https://autowarefoundation.github.io/autoware_core/main/planning/autoware_velocity_smoother/)：考虑速度、加速度和加加速度约束，计算最终速度。

<a id="important-information-in-the-current-implementation"></a>

### 当前实现中的重要信息

相比高层设计，重要差异是“引入场景层”和“明确分离行为与运动”。这些设计用于应对当前性能和实现方面的挑战。应将它们定义为高层设计的一部分，还是作为实现细节加以改进，仍有待讨论。

<a id="introducing-the-scenario-planning-layer"></a>

#### 引入场景规划层

在结构清晰的车道上行驶与在停车场等自由空间中行驶，对接口的要求不同。例如，车道行驶可以用地图 ID 表示路线，但这不适用于自由空间规划。在场景层面（车道行驶、泊车等）切换规划子组件的机制支持灵活的接口设计，但不利于不同场景间的模块复用。

<a id="separation-of-behavior-and-motion"></a>

#### 行为与运动分离

一种经典规划方法是将其分为决定动作的“行为”和确定最终运动的“运动”。但这种分离需要在性能上取舍，功能分离程度越高，性能往往越低。例如，行为模块必须在不了解运动模块最终计算结果的情况下决策，通常会导致保守决策。另一方面，如果融合行为和运动，运动性能与决策会相互依赖，影响扩展性，例如仅扩展决策功能以遵循当地交通规则时就会遇到困难。

如需了解这一背景，可参阅[此前讨论的文档](https://github.com/tier4/AutowareArchitectureProposal.proj/blob/main/docs/design/software_architecture/Planning/DesignRationale.md)。

<a id="customize-features-in-the-current-implementation"></a>

### 在当前实现中自定义功能

当前实现允许添加模块级功能，但未为全部功能提供统一接口。下面简要介绍当前实现中在模块层面扩展的方法。

![reference-implementation-add-new-modules](image/reference-implementation-add-new-modules.drawio.svg)

<a id="add-new-modules-in-behavior_velocity_planner-or-behavior_path_planner"></a>

#### 在 behavior_velocity_planner 或 behavior_path_planner 中添加新模块

[behavior_path_planner](https://autowarefoundation.github.io/autoware_universe/main/planning/behavior_path_planner/autoware_behavior_path_planner/) 和 [behavior_velocity_planner](https://autowarefoundation.github.io/autoware_core/main/planning/behavior_velocity_planner/autoware_behavior_velocity_planner/) 等 ROS 节点通过插件提供模块接口。按照这些 ROS 节点定义的接口添加模块，即可动态加载/卸载模块。具体方法请参阅各软件包文档。

<a id="add-a-new-ros-node-in-the-planning-component"></a>

#### 在规划组件中添加新的 ROS 节点

在运动规划中添加模块时，需要将模块创建为 ROS 节点，并集成到规划组件中。当前配置会向上游计算的目标轨迹添加信息，在此过程中引入 ROS 节点即可扩展功能。

<a id="add-or-replace-with-scenarios"></a>

#### 添加或替换场景

当前实现引入了场景级切换逻辑，用于整体切换多个模块，因此可添加新场景（例如高速公路行驶）。

将场景创建为 ROS 节点，并相应调整 scenario_selector ROS 节点，即可完成集成。其优势是可以引入重要的新功能，而不影响车道行驶等其他场景的实现。但它仅支持通过场景切换进行场景级协作，无法在现有规划模块层面协作。

<!--
### Important Parameters

| Package                      | Parameter                                                     | Type   | Description                                                                                                        |
| ---------------------------- | ------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------ |
| `obstacle_stop_planner`      | `stop_planner.stop_position.max_longitudinal_margin`          | double | distance between the ego and the front vehicle when stopping (when `cruise_planner_type:=obstacle_stop_planner`)   |
| `obstacle_cruise_planner`    | `common.safe_distance_margin`                                 | double | distance between the ego and the front vehicle when stopping (when `cruise_planner_type:=obstacle_cruise_planner`) |
| `behavior_path_planner`      | `avoidance.avoidance.lateral.lateral_collision_margin`        | double | minimum lateral margin to obstacle on avoidance                                                                    |
| `behavior_path_planner`      | `avoidance.avoidance.lateral.lateral_collision_safety_buffer` | double | additional lateral margin to obstacle if possible on avoidance                                                     |
| `obstacle_avoidance_planner` | `option.enable_outside_drivable_area_stop`                    | bool   | If set true, a stop point will be inserted before the path footprint is outside the drivable area.                 |

### Notation

#### [1] self-crossing road and overlapped

To support the self-crossing road and overlapped road in the opposite direction, each planning module has to meet the [specifications](https://autowarefoundation.github.io/autoware_universe/main/common/motion_utils/)

Currently, the supported modules are as follows.

- lane_following (in behavior_path_planner)
- detection_area (in behavior_velocity_planner)
- stop_line (in behavior_velocity_planner)
- virtual_traffic_light (in behavior_velocity_planner)
- obstacle_avoidance_planner
- obstacle_stop_planner
- motion_velocity_smoother

#### [2] Size of Path Points

Some functions do not support paths with only one point. Therefore, each modules should generate the path with more than two path points. -->
