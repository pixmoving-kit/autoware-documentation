<a id="dds-settings-for-ros-2-and-autoware"></a>

# ROS 2 与 Autoware 的 DDS 设置

<a id="enable-localhost-only-communication"></a>

## 启用仅限本机的通信

1. [为 `lo` 启用 `multicast`](./enable-multicast-for-lo.md)
2. 确保 `.bashrc` 中**没有** `export ROS_LOCALHOST_ONLY=1`。
   - 更多信息请参阅[关于 `ROS_LOCALHOST_ONLY` 环境变量](#about-ros_localhost_only-environment-variable)。

<a id="tune-dds-settings"></a>

## 调整 DDS 设置

Autoware 使用 DDS 进行节点间通信。[ROS 2 文档](https://docs.ros.org/en/humble/How-To-Guides/DDS-tuning.html)建议用户调整 DDS 设置，以充分发挥其能力。

!!! note

    CycloneDDS 是 Autoware 推荐使用且测试最充分的 DDS 实现。

!!! warning

    如果不调整这些设置，Autoware 将无法接收点云或图像等大体积数据。

<a id="tune-system-wide-network-settings"></a>

### 调整系统级网络设置

启动 Autoware 前，设置配置文件路径，并增大 Linux 内核的最大缓冲区大小。

```bash
# Increase the maximum receive buffer size for network packets
sudo sysctl -w net.core.rmem_max=2147483647  # 2 GiB, default is 208 KiB

# IP fragmentation settings
sudo sysctl -w net.ipv4.ipfrag_time=3  # in seconds, default is 30 s
sudo sysctl -w net.ipv4.ipfrag_high_thresh=134217728  # 128 MiB, default is 256 KiB
```

如需永久生效，执行：

```bash
sudo nano /etc/sysctl.d/10-cyclone-max.conf
```

将以下内容粘贴到文件中：

```bash
# Increase the maximum receive buffer size for network packets
net.core.rmem_max=2147483647  # 2 GiB, default is 208 KiB

# IP fragmentation settings
net.ipv4.ipfrag_time=3  # in seconds, default is 30 s
net.ipv4.ipfrag_high_thresh=134217728  # 128 MiB, default is 256 KiB
```

各参数的详细说明见 [ROS 2 文档](https://docs.ros.org/en/humble/How-To-Guides/DDS-tuning.html#cross-vendor-tuning)。

<a id="validate-the-sysctl-settings"></a>

#### 验证 sysctl 设置

```console
user@pc$ sysctl net.core.rmem_max net.ipv4.ipfrag_time net.ipv4.ipfrag_high_thresh
net.core.rmem_max = 2147483647
net.ipv4.ipfrag_time = 3
net.ipv4.ipfrag_high_thresh = 134217728
```

<a id="cyclonedds-configuration"></a>

### CycloneDDS 配置

将以下内容保存为 `~/cyclonedds.xml`。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CycloneDDS xmlns="https://cdds.io/config" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://cdds.io/config https://raw.githubusercontent.com/eclipse-cyclonedds/cyclonedds/master/etc/cyclonedds.xsd">
  <Domain Id="any">
    <General>
      <Interfaces>
        <NetworkInterface autodetermine="false" name="lo" priority="default" multicast="default" />
      </Interfaces>
      <AllowMulticast>default</AllowMulticast>
      <MaxMessageSize>65500B</MaxMessageSize>
    </General>
    <Discovery>
      <ParticipantIndex>none</ParticipantIndex>
    </Discovery>
    <Internal>
      <SocketReceiveBufferSize min="10MB"/>
      <Watermarks>
        <WhcHigh>500kB</WhcHigh>
      </Watermarks>
    </Internal>
  </Domain>
</CycloneDDS>
```

!!! note "在 ROS 2 Jazzy（Ubuntu 24.04）中使用 CycloneDDS 时"

    在 ROS 2 Jazzy 中，rmw_cyclonedds_cpp 的默认最大 Participant Index 约为 32，运行大量节点（例如规划仿真器）时可能出现“Failed to find a free participant index for domain 0”错误。添加上面的 `<Discovery>` 配置段，并将 `ParticipantIndex` 设置为 `none`，即可避免此错误。详情请参阅[运行时故障排查：CycloneDDS 无法找到空闲 participant index](../../../community/support/troubleshooting/runtime-troubleshooting.md#cyclonedds-failed-to-find-a-free-participant-index)。

然后在 `~/.bashrc` 文件中添加以下内容。

```bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp

export CYCLONEDDS_URI=file:///absolute/path/to/cyclonedds.xml
# Replace `/absolute/path/to/cyclonedds.xml` with the actual path to the file.
# Example: export CYCLONEDDS_URI=file:///home/user/cyclonedds.xml
```

更多详情可参阅 [Eclipse Cyclone DDS：运行时配置文档](https://github.com/eclipse-cyclonedds/cyclonedds/tree/a10ced3c81cc009e7176912190f710331a4d6caf#run-time-configuration)。

!!! warning

    `RMW_IMPLEMENTATION` 变量可能已通过 [Ansible/RMW Implementation](https://github.com/autowarefoundation/autoware/tree/main/ansible/roles/rmw_implementation#manual-installation) 设置。

    请检查并在必要时删除重复行。

<a id="additional-information"></a>

## 补充信息

<a id="about-ros_localhost_only-environment-variable"></a>

### 关于 `ROS_LOCALHOST_ONLY` 环境变量

以前，我们通过设置 `export ROS_LOCALHOST_ONLY=1` 来启用仅限本机的通信。
但由于[一个尚未解决的问题](https://github.com/ros2/rmw_cyclonedds/issues/370)，此方法目前无法正常工作。

!!! warning

    请勿在 `~/.bashrc` 中设置 `export ROS_LOCALHOST_ONLY=1`。

    这样做会导致 RMW 错误。

    如果已经设置，请从 `~/.bashrc` 中删除。

<a id="about-ros_domain_id-environment-variable"></a>

### 关于 `ROS_DOMAIN_ID` 环境变量

也可以设置 `export ROS_DOMAIN_ID=3(or any number 1 to 255)`（默认值为 `0`），以避免同一网络中其他 ROS 2 节点的干扰。

但由于可用范围只有 `255` 这么大，除非确保每个参与者的 domain ID 都唯一，否则仍可能与同一网络中的其他计算机互相干扰。

另一个问题是，使用 ROS 2 [launch_testing](https://github.com/ros2/launch/blob/a317c54bbbf2dfeec35fbb6d2b5913939d02750d/launch_testing/README.md) 框架运行测试时，
默认会使用随机 domain ID 隔离不同测试，即使这些测试运行在同一台计算机上。
更多详情请参阅[此 PR](https://github.com/ros2/launch/pull/251)。
