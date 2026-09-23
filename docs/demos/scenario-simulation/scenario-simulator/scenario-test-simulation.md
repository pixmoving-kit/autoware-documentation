<a id="scenario-test-simulation"></a>

# 场景测试仿真

!!! note

    运行场景仿真器需要在构建和安装 Autoware 的基础上完成一些额外步骤，因此继续操作前，请确保已完成[场景仿真器安装](installation.md)。

<a id="running-steps"></a>

## 运行步骤

1. 进入已构建 Autoware 和场景仿真器的工作空间目录。

2. 使用 source 命令加载工作空间设置脚本：

   ```bash
   source install/setup.bash
   ```

3. 运行仿真：

   ```bash
   ros2 launch scenario_test_runner scenario_test_runner.launch.py \
     architecture_type:=awf/universe/20250130 \
     record:=false \
     scenario:='$(find-pkg-share scenario_test_runner)/scenario/sample.yaml' \
     sensor_model:=sample_sensor_kit \
     vehicle_model:=sample_vehicle \
     use_custom_centerline:=true \
     rviz_config:=$(ros2 pkg prefix autoware_launch)/share/autoware_launch/rviz/scenario_simulator.rviz
   ```

![场景测试运行器](images/scenario_test_runner.png)

[参考视频教程](https://user-images.githubusercontent.com/102840938/206996920-758b62ae-270a-497c-8a72-f9e4867df695.mp4)
