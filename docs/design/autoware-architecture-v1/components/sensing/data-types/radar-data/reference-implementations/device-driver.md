<a id="device-driver-for-radars"></a>

# 雷达设备驱动程序

<a id="interface-for-radar-devices"></a>

## 雷达设备接口

雷达驱动程序将雷达通信数据转换为 ROS 2 话题。
考虑以下通信类型：

- CAN
- CAN-FD
- Ethernet

![draw.io figure](image/radar-communication.drawio.svg)

<a id="software-interface"></a>

## 软件接口

Autoware 雷达驱动程序支持 `ros-perception/radar_msgs/msg/RadarScan.msg` 和 `autoware_auto_perception_msgs/msg/TrackedObjects.msg`。

![draw.io figure](image/radar-driver.drawio.svg)

ROS 驱动程序接收来自雷达设备的数据。

- 扫描（点云）
- 跟踪目标
- 诊断结果

ROS 驱动程序从雷达设备接收以下数据。

- 时间同步
- 自车状态，通常为计算跟踪目标所需

<a id="radar-driver-example"></a>

## 雷达驱动程序示例

- [ARS408 驱动程序](https://github.com/tier4/ars408_driver)
