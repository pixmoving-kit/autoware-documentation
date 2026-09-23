<a id="autowares-design"></a>

# Autoware 设计

<a id="autoware-concepts"></a>

## Autoware 概念

[Autoware 概念页面](autoware-concepts/index.md)介绍 Autoware 的设计理念。读者（服务提供方及所有 Autoware 用户）将了解微自主架构、Core/Universe 架构等 Autoware 开发的基础概念。

<a id="architecture"></a>

## 架构

[Autoware 架构页面](autoware-architecture-v1/index.md)概述组成 Autoware 的各个模块。读者（所有 Autoware 用户）可以从整体上了解这些模块的工作方式。

Core 和 Universe。

Autoware 通过开源软件提供运行时和技术组件。运行时基于机器人操作系统（ROS）。技术组件由贡献者提供，包括但不限于：

- 传感器处理
  - 相机组件
  - 激光雷达组件
  - 毫米波雷达组件
  - GNSS 组件
- 计算
  - 定位组件
  - 感知组件
  - 规划组件
  - 控制组件
  - 日志记录组件
  - 系统监控组件
- 执行
  - 线控驾驶（DBW）组件
- 工具
  - 仿真器组件
  - 建图组件
  - 远程操作组件
  - 机器学习组件
  - 标注组件
  - 标定组件

<a id="autoware-interfaces"></a>

### Autoware 接口

[Autoware 接口页面](autoware-architecture-v1/interfaces/index.md)详细介绍组成 Autoware 的各模块接口。读者（中级开发者）将了解如何为 Autoware 添加新功能，以及如何将自己的模块与 Autoware 集成。

<a id="concern-assumption-and-limitation"></a>

### 考量、假设与限制

微自主架构的缺点是，功能模块化引入的数据传输路径开销会牺牲最终应用的计算性能。也就是说，微自主架构在计算性能与功能模块化之间存在权衡。从技术上，可以通过引入实时能力解决这一问题。这是因为自动驾驶系统并不以绝对运算速度快为根本目标；低延迟计算固然有益，但并非必需。自动驾驶系统必须具备的能力是计算延迟可预测，也就是满足实时性。因此，可以在一定程度上牺牲计算性能，只要延迟足够可预测，能够满足自动驾驶系统给定的时间约束，即通常所说的计算截止时间。
