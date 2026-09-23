<a id="runtime-troubleshooting"></a>

# 运行时故障排查

本页介绍[性能故障排查](performance-troubleshooting.md)未涵盖的运行时错误及解决办法。

<a id="cyclonedds-failed-to-find-a-free-participant-index"></a>

## CycloneDDS：无法找到空闲参与者索引

<a id="symptoms"></a>

### 症状

在 **ROS 2 Jazzy**（Ubuntu 24.04）上使用 `RMW_IMPLEMENTATION=rmw_cyclonedds_cpp` 运行 Autoware 时，某些节点可能启动失败，并出现以下错误：

```text
[component_container_mt-16] Failed to find a free participant index for domain 0
[component_container_mt-16] [ERROR] [rmw_cyclonedds_cpp]: rmw_create_node: failed to create domain, error Error (check_create_domain() at ./src/rmw_node.cpp:1242)
[component_container_mt-16] terminate called after throwing an instance of 'rclcpp::exceptions::RCLError'
[component_container_mt-16]   what():  failed to initialize rcl node: error not set, at ./src/rcl/node.c:252
```

这通常发生在启动使用大量节点的演示时，例如 [Planning Simulator](https://autowarefoundation.github.io/autoware-documentation/main/demos/planning-sim/)。

<a id="cause"></a>

### 原因

CycloneDDS 本身默认使用 `ParticipantIndex=none`，不限制参与者数量。但在 ROS 2 Jazzy 中，**rmw_cyclonedds_cpp** 构建域配置时会显式设置 `ParticipantIndex=auto` 和 `MaxAutoParticipantIndex=32`，覆盖 CycloneDDS 默认值。因此，每台主机只能存在约 32 个 DDS 参与者。Autoware 使用的 DDS 参与者可能超过此限制（例如 100 个或更多），导致新节点无法获取空闲参与者索引，创建域失败。

此行为来自 RMW 实现，参见 [rmw_cyclonedds：check_create_domain()](https://github.com/ros2/rmw_cyclonedds/blob/7cd457de5825d4cb46ec7b081aa00a5392e388d0/rmw_cyclonedds_cpp/src/rmw_node.cpp#L1193-L1199)（第 1193–1199 行）。

<a id="solution"></a>

### 解决办法

在 `cyclonedds.xml` 中添加 `<Discovery>` 配置段，将 `ParticipantIndex` 设为 `none`：

```xml
<Discovery>
  <ParticipantIndex>none</ParticipantIndex>
</Discovery>
```

完整示例见 [CycloneDDS 配置](../../../installation/additional-settings-for-developers/network-configuration/dds-settings.md#cyclonedds-configuration)。

<a id="references"></a>

### 参考资料

- [autowarefoundation/autoware#6759](https://github.com/autowarefoundation/autoware/issues/6759)：此错误的 issue 与讨论。
- [rmw_cyclonedds：check_create_domain()（ParticipantIndex=auto，MaxAutoParticipantIndex=32）](https://github.com/ros2/rmw_cyclonedds/blob/7cd457de5825d4cb46ec7b081aa00a5392e388d0/rmw_cyclonedds_cpp/src/rmw_node.cpp#L1193-L1199)：RMW 覆盖 CycloneDDS 默认配置的位置。
- [Eclipse Cyclone DDS：控制端口号](https://cyclonedds.io/docs/cyclonedds/0.9.1/config.html#controlling-port-numbers)
- [CycloneDDS 配置参考：MaxAutoParticipantIndex](https://cyclonedds.io/docs/cyclonedds/latest/config/config_file_reference.html#cyclonedds-domain-discovery-maxautoparticipantindex)
