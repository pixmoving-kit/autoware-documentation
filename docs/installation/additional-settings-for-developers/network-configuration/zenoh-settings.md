<a id="zenoh-settings-for-ros-2-and-autoware"></a>

# ROS 2 与 Autoware 的 Zenoh 设置

Autoware 默认使用 CycloneDDS 作为通信中间件，也兼容 Zenoh 等其他协议。ROS 社区[选择](https://discourse.openrobotics.org/t/ros-2-alternative-middleware-report/33771) Zenoh 作为具有潜力的新中间件替代方案，主要基于以下优势：

- **互联网通信**：与局限于局域网（LAN）的 DDS 不同，Zenoh 可以与云端无缝通信，无需单独的桥接组件。
- **命名空间支持**：Zenoh 允许为每辆车使用命名空间，简化多车管理和网络流量隔离。
- **非组播支持**：Zenoh 可在 5G 等非组播环境中运行，而这是 DDS 的一项主要限制。
- **降低发现开销**：Zenoh 显著减少发现数据包的开销，改善 DDS 在无线环境中的这一已知问题。
- **更高性能**：[一项研究](https://zenoh.io/blog/2023-03-21-zenoh-vs-mqtt-kafka-dds/)表明，Zenoh 的性能通常优于 DDS、MQTT、Kafka 等协议。

以下各节逐步介绍如何通过 Zenoh 运行 Autoware。建议使用自 Autoware 1.7.1 起支持的 **ROS 2 Jazzy**，它包含对 GuardCondition 释放后使用问题的修复，无需额外的临时补丁。

<a id="install-rmw_zenoh"></a>

## 安装 rmw_zenoh

1. 安装 rmw_zenoh

   ```bash
   sudo apt update && sudo apt install ros-jazzy-rmw-zenoh-cpp
   ```

2. 将 rmw_zenoh 设置为默认 RMW 实现

   在 `~/.bashrc` 文件中添加以下内容：

   ```bash
   export RMW_IMPLEMENTATION=rmw_zenoh_cpp
   ```

3. 重新加载 Shell 配置，或打开新的终端：

   ```bash
   source ~/.bashrc
   ```

更多详情请参阅 [rmw_zenoh 仓库](https://github.com/ros2/rmw_zenoh)。

<a id="launch-autoware-with-zenoh"></a>

## 使用 Zenoh 启动 Autoware

1. 启动 Zenoh 路由器：

   ```bash
   # terminal 1
   ros2 run rmw_zenoh_cpp rmw_zenohd
   ```

2. 启动 Autoware：

   ```bash
   # terminal 2
   source <YOUR-AUTOWARE-DIR>/install/setup.bash
   ros2 launch autoware_launch autoware.launch.xml ...
   ```

<a id="logging"></a>

## 日志

Zenoh 使用 Rust 实现，其日志库可通过 `RUST_LOG` 环境变量配置。
您可以指定不同级别（如 `info`、`debug` 或 `trace`），调整日志的详细程度。

<a id="example"></a>

### 示例

启用调试日志并启动 Zenoh 路由器：

```bash
export RUST_LOG=zenoh=debug
ros2 run rmw_zenoh_cpp rmw_zenohd
```

或者通过一条命令，以 info 日志级别启动 Autoware：

```bash
RUST_LOG=zenoh=info ros2 launch autoware_launch autoware.launch.xml ...
```

更多信息请参阅 [rmw_zenoh 日志章节](https://github.com/ros2/rmw_zenoh?tab=readme-ov-file#logging)。
