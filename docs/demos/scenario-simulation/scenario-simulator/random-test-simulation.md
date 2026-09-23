<a id="random-test-simulation"></a>

# 随机测试仿真

!!! note

    运行 Scenario Simulator 除了构建和安装 Autoware，还需要额外步骤。因此，继续之前请先完成 [Scenario Simulator 安装](installation.md)。

<a id="running-steps"></a>

## 运行步骤

1. 进入已构建 Autoware 和 Scenario Simulator 的工作空间目录。

2. 加载工作空间的环境设置脚本：

   ```bash
   source install/setup.bash
   ```

3. 运行仿真：

   ```bash
   ros2 launch random_test_runner random_test.launch.py \
     architecture_type:=awf/universe/20250130 \
     sensor_model:=sample_sensor_kit \
     vehicle_model:=sample_vehicle
   ```

![random_test_runner](images/random_test_runner.png)

有关支持参数的更多信息，请参阅 [random_test_runner 文档](https://tier4.github.io/scenario_simulator_v2-docs/user_guide/random_test_runner/Usage/#node-parameters)。
