<a id="communicating-across-multiple-computers-with-cyclonedds"></a>

# 使用 CycloneDDS 实现多机通信

<a id="configuring-cyclonedds"></a>

## 配置 CycloneDDS

可以通过多种方式设置 `~/cyclonedds.xml` 文件中的 Interfaces 配置段，实现同一网络内多台计算机之间的通信。

<a id="automatically-determine-the-network-interface-convenient"></a>

### 自动选择网络接口（便捷方式）

使用此设置时，CycloneDDS 会自动选择最合适的网络接口。

```xml
<Interfaces>
  <NetworkInterface autodetermine="true" priority="default" multicast="default" />
</Interfaces>
```

<a id="manually-set-the-network-interface-recommended"></a>

### 手动设置网络接口（推荐）

使用此设置时，您可以手动指定要使用的网络接口。

```xml
<Interfaces>
  <NetworkInterface autodetermine="false" name="enp38s0" priority="default" multicast="default" />
</Interfaces>
```

!!! warning

    请将 `enp38s0` 替换为实际的网络接口名称。

!!! note

    可以使用 `ifconfig` 命令查找网络接口名称。

<a id="time-synchronization"></a>

## 时间同步

为确保不同计算机上的节点保持同步，需要同步各计算机的时间。

可以使用 `chrony` 同步计算机之间的时间。

更多信息请参阅此帖子：[多机 AWSIM + Autoware 测试 #3813](https://github.com/orgs/autowarefoundation/discussions/3813)

!!! warning

    正在编写
