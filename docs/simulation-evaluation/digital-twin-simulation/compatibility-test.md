<a id="compatibility-test"></a>

# 兼容性测试

兼容性测试工具用于验证数字孪生仿真器能否与 Autoware 正常协作。

<a id="simulator_compatibility_test-package"></a>

## `simulator_compatibility_test` 功能包

- 此工具基于 **ROS 2 Humble** 构建。
- 截至 **2023 年 1 月 26 日**，仅使用 [**MORAI SIM：Drive 演示**](../../demos/digital-twin-demos/MORAI_Sim-tutorial.md)进行过测试。
- 完整源代码与说明请参阅：
  [autoware_tools/simulator/simulator_compatibility_test](https://github.com/autowarefoundation/autoware_tools/tree/main/simulator/simulator_compatibility_test#simulator_compatibility_test)

!!! note

    测试套件包含手动和自动测试用例，检查仿真器是否正确发布及接收 Autoware 所需消息（例如控制模式、挡位、速度、转向、转向灯和危险警告灯）。
    这些测试通过发送控制命令并检查仿真器报告的状态，验证双向通信。

!!! info

    MORAI SIM 之外的仿真器也可以使用**通用手动测试**（`test_sim_common_manual_testing`）。

!!! tip

    开发者可以参照 MORAI SIM 测试集，创建针对特定仿真器的自动化版本，扩展这些测试。
