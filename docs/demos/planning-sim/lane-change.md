<a id="lane-change-scenario"></a>

# 变道场景

1. 下载并解压 Nishishinjuku 地图。

   ```bash
   mkdir -p ~/autoware_data/maps
   wget -P ~/autoware_data/maps/ 'https://github.com/autowarefoundation/AWSIM/releases/download/v1.1.0/nishishinjuku_autoware_map.zip'
   unzip -d ~/autoware_data/maps ~/autoware_data/maps/nishishinjuku_autoware_map.zip
   ```

2. 使用以下命令加载 Nishishinjuku 地图并启动 Autoware：

   ```bash
   source ~/autoware/install/setup.bash
   ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/autoware_data/maps/nishishinjuku_autoware_map vehicle_model:=sample_vehicle sensor_model:=sample_sensor_kit
   ```

   ![打开 Nishishinjuku 地图](images/lane-change/open-nishishinjuku-map.png)

3. 分别在相邻车道上设置初始位姿和目标位姿。

   ![设置位置和目标](images/lane-change/set-position-and-goal.png)

4. 启用自车自动驾驶。车辆将沿规划路径完成变道。

   ![变道](images/lane-change/lane-changing.png)
