<a id="introduction"></a>

### 简介

<a id="related-links"></a>

#### 相关链接

数据采集文档 → [伊斯坦布尔开放数据集](../../../datasets/index.md#istanbul-open-dataset)

<a id="purpose"></a>

#### 目的

本测试旨在观察当前 Autoware 默认 NDT 定位系统在城市环境中的表现，并评估结果。我们希望发现定位的不足，了解哪些场景需要改进。

<a id="test-environment"></a>

#### 测试环境

测试数据采集于伊斯坦布尔，包含隧道、桥梁等可能对定位造成挑战的场景。完整路线在下图中标出。
![伊斯坦布尔数据集路线](https://github.com/user-attachments/assets/e04c77b4-5192-4faf-839c-12429138708f)

<a id="test-dataset-map"></a>

#### 测试数据集与地图

测试与建图使用相同数据集，其中包含用于通用建图的便携式建图套件采集的数据。

数据来自以下传感器：

- 1 套 Applanix POS LVX GNSS/INS 系统
- 1 台 Hesai Pandar XT32 激光雷达

测试与建图数据可在[伊斯坦布尔开放数据集](../../../datasets/index.md#istanbul-open-dataset)中获取。

> <span style="color:green">**注意！**</span> </br>  
> 由于所有测试中都没有来自车辆的速度源，因此将 GNSS/INS 的 twist 消息作为线速度和角速度源输入 ekf_localizer。</br>
> 为了解隧道中 GNSS/INS 误差增大时，这是否会进一步增大误差及其对系统的影响，还测试了仅向 ekf_localizer 提供 NDT 位姿、不提供该速度的隧道定位情况。</br>  
> 测试视频[见此处](https://youtu.be/6On130bjQUY?si=vumtij7a66WBIV3z)。</br>
> 从视频可见，不提供速度时，隧道内定位恶化得更快。
> 预计如果使用车辆线速度与 IMU 信息融合后的 Twist 消息（/localization/twist_estimator/twist_with_covariance）替代 GNSS/INS Twist 消息，隧道中的性能会改善。但现有数据无法完成此测试。

<a id="expected-tests"></a>

#### 预期测试

1.) 首先，找出定位完全失效的位置，并粗略检查定位表现。

2.) 提取指标，具体观察定位误差增大的程度和性能水平。

<a id="how-to-reproduce-tests"></a>

### 如何复现测试

<a id="test-with-raw-data"></a>

#### 使用原始数据测试

如果使用原始数据测试，请遵循 `Test With Raw Data`（使用原始数据测试）的说明。由于此测试需要重新执行全部输入数据预处理，应将 sensor kit 和 individual params 仓库切换到测试分支。具体步骤如下。

<a id="installation"></a>

##### 安装

如果 `gdown` 命令不可用，请先安装：

```bash
sudo apt-get -y install pipx
python3 -m pipx ensurepath
pipx install gdown
```

1.) 下载并解压测试地图文件。

- 也可以手动下载[地图](https://drive.google.com/file/d/1WPWmFCjV7eQee4kyBpmGNlX7awerCPxc/view?usp=drive_link)。

```bash
mkdir ~/autoware_ista_map
gdown --id 1WPWmFCjV7eQee4kyBpmGNlX7awerCPxc -O ~/autoware_ista_map/
```

> <span style="color:green">**注意！**</span></br>  
> 还需要将 `lanelet2_map.osm` 文件添加到 autoware_ista_map 文件夹。目前尚未为此地图创建 lanelet 文件，
> 因此可将任意 `lanelet2_map.osm` 文件放入该文件夹以运行。

2.) 下载测试 rosbag 文件。

- 也可以手动下载 [rosbag 文件](https://drive.google.com/drive/folders/1BMPcUhjq_BCLi521X88WpujoOiEi3_CJ?usp=drive_link)。

```bash
mkdir ~/autoware_ista_data
gdown --id 1uta5Xr_ftV4jERxPNVqooDvWerK0dn89 -O ~/autoware_ista_data/
```

<a id="prepare-autoware-to-test"></a>

##### 准备 Autoware 测试环境

1.) 检出 autoware_launch：

```bash
cd ~/autoware/src/launcher/autoware_launch/
git remote add autoware_launch https://github.com/meliketanrikulu/autoware_launch.git
git remote update
git checkout evaluate_localization_issue_7652
```

2.) 检出 individual_params：

```bash
cd ~/autoware/src/param/autoware_individual_params/
git remote add autoware_individual_params https://github.com/meliketanrikulu/autoware_individual_params.git
git remote update
git checkout evaluate_localization_issue_7652
```

3.) 检出 sample_sensor_kit_launch：

```bash
cd ~/autoware/src/sensor_kit/sample_sensor_kit_launch/
git remote add sample_sensor_kit_launch https://github.com/meliketanrikulu/sample_sensor_kit_launch.git
git remote update
git checkout evaluate_localization_issue_7652
```

4.) 编译更新的功能包：

```bash
cd ~/autoware
colcon build --symlink-install --packages-select sample_sensor_kit_launch autoware_individual_params autoware_launch common_sensor_launch
```

<a id="launch-autoware"></a>

##### 启动 Autoware

```bash
source ~/autoware/install/setup.bash
ros2 launch autoware_launch logging_simulator.launch.xml map_path:=~/autoware_ista_map/ vehicle_model:=sample_vehicle sensor_model:=sample_sensor_kit
```

<a id="run-rosbag"></a>

##### 运行 Rosbag

```bash
source ~/autoware/install/setup.bash
ros2 bag play ~/autoware_ista_data/rosbag2_2024_09_11-17_53_54_0.db3
```

<a id="localization-test-only"></a>

#### 仅测试定位

如果只想查看定位性能，请遵循 `Localization Test Only`（仅测试定位）的说明。为此，专门创建了第二个测试 bag 文件和单独的 launch 文件。可按照以下步骤测试。

<a id="installation_1"></a>

##### 安装

如果 `gdown` 命令不可用，请先安装：

```bash
sudo apt-get -y install pipx
python3 -m pipx ensurepath
pipx install gdown
```

1.) 下载并解压测试地图文件。

- 也可以手动下载[地图](https://drive.google.com/file/d/1WPWmFCjV7eQee4kyBpmGNlX7awerCPxc/view?usp=drive_link)。

```bash
mkdir ~/autoware_ista_map
gdown --id 1WPWmFCjV7eQee4kyBpmGNlX7awerCPxc -O ~/autoware_ista_map/
```

> <span style="color:green">**注意！**</span></br>
> 还需要将 `lanelet2_map.osm` 文件添加到 autoware_ista_map 文件夹。目前尚未为此地图创建 lanelet 文件，
> 因此可将任意 `lanelet2_map.osm` 文件放入该文件夹以运行。

2.) 下载测试 rosbag 文件。

- 也可以手动下载[定位 rosbag 文件](https://drive.google.com/file/d/1yEB5j74gPLLbkkf87cuCxUgHXTkgSZbn/view?usp=sharing)。

```bash
mkdir ~/autoware_ista_data
gdown --id 1yEB5j74gPLLbkkf87cuCxUgHXTkgSZbn -O ~/autoware_ista_data/
```

<a id="prepare-autoware-to-test_1"></a>

##### 准备 Autoware 测试环境

1.) 检出 autoware_launch：

```bash
cd ~/autoware/src/launcher/autoware_launch/
git remote add autoware_launch https://github.com/meliketanrikulu/autoware_launch.git
git remote update
git checkout evaluate_localization_issue_7652
```

2.) 编译更新的功能包：

```bash
cd ~/autoware
colcon build --symlink-install --packages-select autoware_launch
```

<a id="launch-autoware_1"></a>

##### 启动 Autoware

```bash
source ~/autoware/install/setup.bash
ros2 launch autoware_launch urban_environment_localization_test.launch.xml map_path:=~/autoware_ista_map/
```

<a id="run-rosbag_1"></a>

##### 运行 Rosbag

```bash
source ~/autoware/install/setup.bash
ros2 bag play ~/autoware_ista_data/rosbag2_2024_09_12-14_59_58_0.db3
```

<a id="test-results"></a>

### 测试结果

<a id="test-1-simple-test-of-localization"></a>

#### 测试 1：定位的简单测试

简单测试了定位在城市环境中的工作表现，并录制了结果视频。
[测试视频在此](https://youtu.be/DQsk3rY7NjY?si=NkJYIZoPd6HqLIHp)。
[<img src="https://github.com/user-attachments/assets/e0f7edbf-0596-4806-8dcd-b4156584e4c0" width="60%">](https://youtu.be/Bk4Oyk6FOg0?t=6")

从视频可见，车辆沿欧亚隧道行驶时，纵向存在定位误差。正如预期，基于 NDT 的定位在这里无法正常工作。但由于 NDT 得分未能检测到这一退化，直到隧道末端定位才完全失效。在隧道出口处定位完全失效，车辆驶出隧道后，我重新初始化了定位。

随后进入桥梁场景。这是连接博斯普鲁斯海峡的一座桥，也是测试路线中最长的桥。我们原本认为 NDT 定位在此也可能受到干扰，但未观察到异常。

此后还有另一条隧道（Kagithane-Bomonti），定位表现与欧亚隧道类似。但在该隧道出口，定位能够自行恢复，无须重新初始化。

<a id="test-1-summary"></a>

##### 测试 1 小结

总体而言，测试路线中定位出现明显退化的位置是隧道，这符合预期。我们也曾担心桥梁会出现问题，但未观察到桥梁上的定位退化。需要提醒的是，这些测试仅基于一组数据。

<a id="test-2-comparing-results-with-ground-truth"></a>

#### 测试 2：与真值比较结果

1.) NDT 得分如何变化？

沿整个路线，仅在最长的欧亚隧道中的一小段，NDT 得分低于预期值。但路线中的两条隧道均出现了明显定位退化。因此，本测试表明，无法仅凭 NDT 得分检查这些问题。下图展示了 NDT 得分沿路线的变化。查看图像时，请记住 NDT 得分阈值采用 2.3。

![NDT 最近体素变换似然](https://github.com/user-attachments/assets/d2fcd062-856b-4dce-bd4a-8a1f49b835f4)

NDT 得分（最近体素变换似然）阈值 = 2.3

2.) 与真值比较

真值：这些测试使用后处理后的 GNSS/INS 数据作为真值。由于该真值数据的误差在隧道环境中也会减小，因此评估这些区域时需要考虑真值误差。

测试中，我将 NDT 和 EKF 位姿与真值比较，并展示结果。下面以 png 格式分享结果。如果希望更详细地查看数据，我还创建了可执行文件，用于可视化和进一步分析，可从[此处](https://drive.google.com/drive/folders/1ges_2q8qljgLfwjZbv2Bn35Rv5awhd_U?usp=sharing)获取。目前链接中仅有 Ubuntu 版本，之后计划添加 Windows 版本。
请按以下步骤操作：

```bash
cd /your/path/show_evaluation_ubuntu
./pose_main
```

也可以通过修改 /configs/evaluation_pose.yaml 更新配置。

二维轨迹：
![二维轨迹](https://github.com/user-attachments/assets/4eebd34f-dc87-4a2d-a4f9-07f068a7d8d7)

二维误差：
![二维误差](https://github.com/user-attachments/assets/01f1a5a0-6fbd-4ae2-aa86-4ce7b5cdf141)

三维轨迹：
![三维轨迹](https://github.com/user-attachments/assets/310b5d5f-430b-4f7a-b4fd-93063fc2ea8b)

三维误差：
![三维误差](https://github.com/user-attachments/assets/6ab4a1cf-ad69-4847-8747-26efb3e9ce8e)

横向误差：

![横向误差](https://github.com/user-attachments/assets/50ffba9e-b1c2-4407-90bc-002886540f9a)

纵向误差：
![纵向误差](https://github.com/user-attachments/assets/617ca024-7c29-4556-bb62-c1e54b19fd38)

滚转角、俯仰角、偏航角：
![滚转俯仰偏航角](https://github.com/user-attachments/assets/68eef655-189a-4552-a99c-ab89df2136af)

滚转角、俯仰角、偏航角误差：
![滚转俯仰偏航角误差](https://github.com/user-attachments/assets/071cd440-d64a-49a8-bf0a-86278e174048)

X-Y-Z：
![XYZ](https://github.com/user-attachments/assets/6d946f18-98f7-4297-bb1b-fad936fd1328)

X-Y-Z 误差：

![XYZ 误差](https://github.com/user-attachments/assets/612559cc-a973-4096-9a76-3e27d87539fd)
