<a id="datasets"></a>

# 数据集

Autoware 合作伙伴提供用于测试和开发的数据集，可在此下载。

<a id="istanbul-open-dataset"></a>

## 伊斯坦布尔开放数据集

数据集沿以下路线采集，图中标注了隧道和桥梁。
数据集包含的特定区域有：

- 加拉塔大桥（小桥）
- 欧亚隧道（高程变化较大的长隧道）
- 博斯普鲁斯海峡第二大桥（长桥）
- Kagithane-Bomonti 隧道（短隧道）
- 高架桥、道路交叉口、高速公路、密集城区等。

<p align='center'>
    <img src="images/ist_dataset_route_3_resized.png" alt="ist_dataset_route2" width="80%"/>
</p>

<a id="leo-drive-mapping-kit-sensor-data"></a>

### Leo Drive：建图套件传感器数据

此数据集包含通用建图所用便携式建图套件采集的数据。

数据来自以下传感器：

- 1 套 Applanix POS LVX GNSS/INS 系统
- 1 台 Hesai Pandar XT32 激光雷达

**为提供传感器标定信息，已加入 `/tf_static` 话题。**

<a id="data-links"></a>

### 数据链接

- 完整点云地图、角点特征点云地图以及
  面特征点云地图可从此处获取：
  - [https://drive.google.com/drive/folders/1_jiQod4lO6-V2NDEr3d-M3XF_Nqmc0Xf?usp=drive_link](https://drive.google.com/drive/folders/1_jiQod4lO6-V2NDEr3d-M3XF_Nqmc0Xf?usp=drive_link)
  - 导出点云采用 0.2 米和 0.5 米体素网格下采样。

- 与建图数据同步采集的 ROS 2 bag 可从此处获取：
  - [https://drive.google.com/drive/folders/17zXiBeYlM90gQ5hV6EAWaoBTnNFoVPML?usp=drive_link](https://drive.google.com/drive/folders/17zXiBeYlM90gQ5hV6EAWaoBTnNFoVPML?usp=drive_link)
  - 由于数据同步采集，可以将点云地图和 GNSS/INS
    数据视为此 rosbag 的真值数据。

- 此外，建图所用原始数据可从以下链接获取：
  - [https://drive.google.com/drive/folders/1HmWYkxF5XvVCR27R8W7ZqO7An4HlJ6lD?usp=drive_link](https://drive.google.com/drive/folders/1HmWYkxF5XvVCR27R8W7ZqO7An4HlJ6lD?usp=drive_link)
  - 点云以 PCAP 格式采集，经过特征匹配的 GNSS/INS 数据导出为 txt 文件。

<a id="localization-performance-evaluation-with-autoware"></a>

### 使用 Autoware 评估定位性能

使用这些数据对当前 Autoware 进行性能评估的报告见以下链接。

> 报告编写日期为 **2024-08-28**。

- [https://github.com/orgs/autowarefoundation/discussions/5135](https://github.com/orgs/autowarefoundation/discussions/5135)

<a id="topic-list"></a>

### 话题列表

GNSS/INS 数据使用[此仓库](https://github.com/autowarefoundation/applanix)采集。

激光雷达数据使用
[nebula](https://github.com/tier4/nebula/tree/6d55141ef3cf39d5612e34f2646834d6cd4a7ae3)
仓库采集。

| 话题名称 | 消息类型 |
| ----------------------------------------------------- | ----------------------------------------------------- |
| `/applanix/lvx_client/autoware_orientation` | `autoware_sensing_msgs/msg/GnssInsOrientationStamped` |
| `/applanix/lvx_client/imu_raw` | `sensor_msgs/msg/Imu` |
| `/localization/twist_estimator/twist_with_covariance` | `geometry_msgs/msg/TwistWithCovarianceStamped` |
| `/applanix/lvx_client/odom` | `nav_msgs/msg/Odometry` |
| `/applanix/lvx_client/gnss/fix` | `sensor_msgs/msg/NavSatFix` |
| `/clock` | `rosgraph_msgs/msg/Clock` |
| `/pandar_points` | `sensor_msgs/msg/PointCloud2` |
| `/tf_static` | `tf2_msgs/msg/TFMessage` |

<a id="message-explanations"></a>

#### 消息说明

所用传感器驱动既通过标准 ROS 2 消息类型输出，也通过其自定义 ROS 2 消息
类型提供额外信息。以下话题使用标准 ROS 2 消息类型：

- `/applanix/lvx_client/imu_raw`
  - 提供 ENU 坐标系中的 INS 系统输出。由于使用九轴 IMU，`yaw` 值表示
    传感器的航向。

- `/applanix/lvx_client/twist_with_covariance`
  - 提供传感器的 twist 输出。

- `/applanix/lvx_client/odom`
  - 提供传感器相对于 ROS 2 驱动启动位置的位姿。
    使用 `GeographicLib::LocalCartesian` 实现。

    **此话题与轮式里程计无关。**

- `/applanix/lvx_client/gnss/fix`
  - 提供传感器的纬度、经度和高度。

    **高度值为 WGS84 椭球的椭球高。**

- `/pandar_points`
  - 提供激光雷达传感器的点云。

<a id="bus-odd-operational-design-domain-datasets"></a>

## Bus-ODD（运行设计域）数据集

<a id="leo-drive-isuzu-sensor-data"></a>

### Leo Drive：ISUZU 传感器数据

此数据集包含 Bus ODD 项目中使用的五十铃客车采集的数据。

数据来自以下传感器：

- 1 台 VLP16
- 2 台 VLP32C
- 1 套 Applanix POS LV 120 GNSS/INS
- 3 台 Lucid Vision Triton 5.4MP 相机（左、右、前）
- 车辆状态报告

数据还包含 `/tf` 话题，用于提供传感器之间的静态变换。

<a id="required-message-types"></a>

#### 所需消息类型

GNSS 数据通过 `sensor_msgs/msg/NavSatFix` 消息类型提供。

同时还包含 Applanix 原始消息，类型为 `applanix_msgs/msg/NavigationPerformanceGsof50` 和 `applanix_msgs/msg/NavigationSolutionGsof49`。
要回放这些消息，需要构建并 source `applanix_msgs` 功能包。

```bash
# Create a workspace and clone the repository
mkdir -p ~/applanix_ws/src && cd "$_"
git clone https://github.com/autowarefoundation/applanix.git
cd ..

# Build the workspace
colcon build --symlink-install --packages-select applanix_msgs

# Source the workspace
source ~/applanix_ws/install/setup.bash

# Now you can play back the messages
```

同时也请 source Autoware Universe 工作区。

<a id="download-instructions"></a>

#### 下载说明

按照 [AWS 命令行界面（AWS CLI）官方安装指南](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)，在本机安装 AWS CLI：

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

```console
# This will download the entire dataset to the current directory.
# (About 10.9GB of data)
$ aws s3 sync s3://autoware-files/recordings/bags/2022-08-22_leo_drive_isuzu_bags/ ./2022-08-22_leo_drive_isuzu_bags  --no-sign-request

# Optionally,
# If you instead want to download a single bag file, you can get a list of the available files with following:
$ aws s3 ls s3://autoware-files/recordings/bags/2022-08-22_leo_drive_isuzu_bags/ --no-sign-request
   PRE all-sensors-bag1_compressed/
   PRE all-sensors-bag2_compressed/
   PRE all-sensors-bag3_compressed/
   PRE all-sensors-bag4_compressed/
   PRE all-sensors-bag5_compressed/
   PRE all-sensors-bag6_compressed/
   PRE driving_20_kmh_2022_06_10-16_01_55_compressed/
   PRE driving_30_kmh_2022_06_10-15_47_42_compressed/

# Then you can download a single bag file with the following:
aws s3 sync s3://autoware-files/recordings/bags/2022-08-22_leo_drive_isuzu_bags/all-sensors-bag1_compressed/ ./all-sensors-bag1_compressed  --no-sign-request
```

<a id="autocoreai-lidar-ros-2-bag-file-and-pcap"></a>

### AutoCore.ai：激光雷达 ROS 2 bag 文件与 pcap

此数据集包含 Ouster OS1-64 激光雷达的 pcap 文件和 ros2 bag 文件。
pcap 与 ros2 bag 同时录制，但时长略有不同。

[点击此处下载（约 553MB）](https://autoware-files.s3.us-west-2.amazonaws.com/recordings/bags/Lidar_Data_220414_bag_pcap.zip)

[参考 issue](https://github.com/autowarefoundation/autoware_universe/issues/562#issuecomment-1102662448)
