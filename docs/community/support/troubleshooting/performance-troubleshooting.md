<a id="performance-troubleshooting"></a>

# 性能故障排查

总体症状：

- Autoware 运行速度低于预期。
- 消息延迟出现在 RViz2 中。
- 点云滞后。
- 相机图像滞后。
- 点云或标记在 RViz2 中闪烁。
- 多个订阅者使用同一发布者时，消息频率下降。

<a id="diagnostic-steps"></a>

## 诊断步骤

<a id="check-if-multicast-is-enabled"></a>

### 检查是否启用组播

<a id="target-symptoms"></a>

#### 目标症状

- 多个订阅者使用同一发布者时，消息频率下降。

<a id="diagnosis"></a>

#### 诊断

确认网络接口已启用组播。

例如，运行以下命令时：

```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
```

如果出现错误消息 `selected interface "{your-interface-name}" is not multicast-capable: disabling multicast`，则需要修复。

<a id="solution"></a>

#### 解决办法

请按照 [ROS 2 和 Autoware 的 DDS 设置](../../../installation/additional-settings-for-developers/network-configuration/dds-settings.md)操作，

尤其是[在 `lo` 上启用 `multicast`](../../../installation/additional-settings-for-developers/network-configuration/enable-multicast-for-lo.md) 一节。

<a id="check-the-compilation-flags"></a>

### 检查编译选项

<a id="target-symptoms_1"></a>

#### 目标症状

- Autoware 运行速度低于预期。
- 点云滞后。
- 多个订阅者使用同一发布者时，消息频率进一步下降。

<a id="diagnosis_1"></a>

#### 诊断

检查 `~/.bash_history`，查看是否存在未带 `-DCMAKE_BUILD_TYPE=Release` 或 `-DCMAKE_BUILD_TYPE=RelWithDebInfo` 选项的 `colcon build` 命令。

即使一开始使用了这些选项，如果之后在同一工作区编译时省略了这些选项，最终构建结果仍会较慢。

此外，各节点整体运行较慢，尤其是 `pointcloud_preprocessor` 节点。

问题示例：[issue2597](https://github.com/autowarefoundation/autoware_universe/issues/2597#issuecomment-1491789081)

<a id="solution_1"></a>

#### 解决办法

- 删除 `autoware` 主目录中的 `build`、`install` 文件夹，也可选择删除 `log`。
- 使用 `Release` 或 `RelWithDebInfo` 构建类型编译 Autoware：

  ```bash
  colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
  # Or build with debug flags too (comparable performance but you can debug too)
  colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo
  ```

<a id="check-the-dds-settings"></a>

### 检查 DDS 设置

<a id="target-symptoms_2"></a>

#### 目标症状

- Autoware 运行速度低于预期。
- 消息延迟出现在 RViz2 中。
- 点云滞后。
- 相机图像滞后。
- 多个订阅者使用同一发布者时，消息频率下降。

<a id="check-the-rmw-ros-middleware-implementation"></a>

#### 检查 RMW（ROS 中间件）实现

<a id="diagnosis_2"></a>

##### 诊断

运行以下命令，检查使用的中间件：

```bash
echo $RMW_IMPLEMENTATION
```

返回结果应为 `rmw_cyclonedds_cpp`。如果不是，请执行下述解决办法。

如果使用其他 DDS 中间件，我们可能尚未提供官方支持。

<a id="solution_2"></a>

##### 解决办法

在 `~/.bashrc` 文件中单独添加一行 `export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp`。

详情请参阅 [CycloneDDS 配置](../../../installation/additional-settings-for-developers/network-configuration/dds-settings.md#cyclonedds-configuration)。

<a id="check-if-the-cyclonedds-is-configured-correctly"></a>

#### 检查 CycloneDDS 配置是否正确

<a id="diagnosis_3"></a>

##### 诊断

运行以下命令，检查 `CycloneDDS` 的 `.xml` 配置文件：

```bash
echo $CYCLONEDDS_URI
```

返回结果应为指向 `CycloneDDS` `.xml` 配置文件的有效路径。

还要检查文件内容是否配置正确：

```bash
cat ${CYCLONEDDS_URI#file://}
```

这应会在终端中打印 `.xml` 文件内容。

<a id="solution_3"></a>

##### 解决办法

按照 [CycloneDDS 配置](../../../installation/additional-settings-for-developers/network-configuration/dds-settings.md#cyclonedds-configuration)操作，并确认：

- `~/.bashrc` 中包含一行 `export CYCLONEDDS_URI=file:///absolute_path_to_your/cyclonedds.xml`。
- `cyclonedds.xml` 使用文档提供的配置。

<a id="check-the-linux-kernel-maximum-buffer-size"></a>

#### 检查 Linux 内核的最大缓冲区大小

<a id="diagnosis_4"></a>

##### 诊断

[验证 sysctl 设置](../../../installation/additional-settings-for-developers/network-configuration/dds-settings.md#validate-the-sysctl-settings)

<a id="solution_4"></a>

##### 解决办法

[调整系统级网络设置](../../../installation/additional-settings-for-developers/network-configuration/dds-settings.md#tune-system-wide-network-settings)

<a id="check-if-localhost-only-communication-for-dds-is-enabled"></a>

### 检查 DDS 是否启用仅本机通信

- 如果使用多机配置，请跳过此检查。
- 为 DDS 启用仅本机通信，可以减少网络流量，并避免与网络中其他设备的潜在冲突，从而改善 ROS 性能。

<a id="target-symptoms_3"></a>

#### 目标症状

- 看到不应存在的话题。
- 看到不属于本机的点云。
  - 它们可能来自同一网络中另一台运行 ROS 2 的计算机。
- 点云或标记在 RViz2 中闪烁。
  - 另一台机器上的发布者可能在与你的节点相同的话题上发布。
  - 从而导致闪烁。

<a id="diagnosis_5"></a>

#### 诊断

运行：

```bash
cat ${CYCLONEDDS_URI#file://}
```

返回结果应指向 [ROS 2 和 Autoware 的 DDS 设置：CycloneDDS 配置](../../../installation/additional-settings-for-developers/network-configuration/dds-settings.md#cyclonedds-configuration)中所述的文件。

<a id="solution_5"></a>

#### 解决办法

按照 [ROS 2 和 Autoware 的 DDS 设置：启用仅本机通信](../../../installation/additional-settings-for-developers/network-configuration/dds-settings.md#enable-localhost-only-communication)操作。

同时确认以下命令返回空行：

```bash
echo $ROS_LOCALHOST_ONLY
```
