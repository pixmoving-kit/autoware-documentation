# Autoware AD API

<a id="overview"></a>

## 概述

Autoware AD API 是从自动驾驶系统外部操作车辆的接口。
[Autoware 的整体接口设计见此处。](../index.md)

<a id="user-stories"></a>

## 用户故事

用户故事是 AD API 所面向的服务场景。AD API 基于这些场景设计。
每个场景均由后文介绍的用例组合实现。
如果有无法覆盖的场景，请讨论新增相应的用户故事。

- [公交服务](stories/bus-service.md)
- [出租车服务](stories/taxi-service.md)

<a id="use-cases"></a>

## 用例

用例是从用户故事中提炼出的局部场景，并经过通用化设计。
服务提供方可以组合这些用例来定义用户故事，并检查 AD API 是否适用于自己的场景。

- [启动与终止](use-cases/launch-terminate.md)
- [初始化位姿](use-cases/initialize-pose.md)
- [切换运行模式](use-cases/change-operation-mode.md)
- [驶向指定位置](use-cases/drive-designated-position.md)
- [上车与下车](use-cases/get-on-off.md)
- [车辆监控](use-cases/vehicle-monitoring.md)
- [车辆操作](use-cases/vehicle-operation.md)
- [系统监控](use-cases/system-monitoring.md)
- [手动控制](use-cases/manual-control/index.md)

<a id="features"></a>

## 功能

- [接口](features/interface.md)
- [运行模式](features/operation_mode.md)
- [路线规划](features/routing.md)
- [定位](features/localization.md)
- [运动](features/motion.md)
- [规划](features/planning-factors.md)
- [感知](features/perception.md)
- [故障安全](features/fail-safe.md)
- [车辆状态](features/vehicle-status.md)
- [车门](features/vehicle-doors.md)
- [协作](features/cooperation.md)
- [心跳](features/heartbeat.md)
- [诊断](features/diagnostics.md)
- [手动控制](features/manual-control.md)
