<a id="integration-testing"></a>

# 集成测试

集成测试是软件测试中的一个阶段，将各个软件模块组合起来作为一个整体进行测试。
集成测试在单元测试之后、确认测试之前进行。

集成测试的输入是一组已经通过单元测试的独立模块。
根据定义的集成测试计划对这组模块进行测试，
输出是一组已正确集成、可进入系统测试的软件模块。

<a id="value-of-integration-testing"></a>

## 集成测试的价值

集成测试用于确定独立开发的软件模块相互连接后能否正常工作。
在 ROS 2 中，软件模块称为节点。
对单个节点进行测试是一种特殊的集成测试，通常称为组件测试。

集成测试有助于发现以下类型的错误：

- 节点间的交互不兼容，例如话题不匹配、消息类型不同，或 QoS 设置不兼容。
- 单元测试未覆盖的边界情况，例如关键时序问题、网络通信延迟、磁盘 I/O 故障，以及其他可能在生产环境中出现的问题。
- 系统在 CPU 或内存负载较高时可能出现的问题，例如 `malloc` 失败。可以使用 `stress` 和 `udpreplay` 等工具，结合真实数据测试节点的性能。

ROS 2 支持编写包含大量节点的复杂自动驾驶应用。
因此，人们投入了大量工作来提供集成测试框架，帮助开发者测试 ROS 2 节点之间的交互。

<a id="integration-test-framework"></a>

## 集成测试框架

典型的集成测试框架包括三个部分：

1. 一组带参数的可执行程序，它们协同工作并产生输出。
2. 一组预期输出，用于与可执行程序的实际输出进行匹配。
3. 一个启动器，用于启动测试、将输出与预期输出比较，并判断测试是否通过。

在 Autoware 中，我们使用 [launch_testing](https://github.com/ros2/launch/tree/master/launch_testing) 框架。

<a id="smoke-tests"></a>

### 冒烟测试

Autoware 提供了专门用于冒烟测试的 API。
要使用此框架，请在 `package.xml` 中添加：

```xml
<test_depend>autoware_testing</test_depend>
```

并在 `CMakeLists.txt` 中添加：

```cmake
if(BUILD_TESTING)
  find_package(autoware_testing REQUIRED)
  add_smoke_test(${PROJECT_NAME} ${NODE_NAME})
endif()
```

这样会添加冒烟测试，以确保节点可以：

1. 使用默认参数文件启动。
2. 通过标准 `SIGTERM` 信号终止。

完整的 API 文档，
请参阅[功能包设计页面](https://github.com/autowarefoundation/autoware_core/blob/main/testing/autoware_testing/design/autoware_testing-design.md)。

!!! note

    此 API 并不适用于所有冒烟测试场景。
    如果需要向节点传入特定文件位置（例如地图文件），或需要在节点启动前执行准备工作，则无法使用此 API。
    这种情况下，请使用[下方组件测试章节](#integration-test-with-a-single-node-component-test)中的手动方案。

<a id="integration-test-with-a-single-node-component-test"></a>

### 单节点集成测试：组件测试

最简单的场景是单个节点。
在这种情况下，集成测试通常称为组件测试。

要为现有节点添加组件测试，
可以参考 [`autoware_map_loader` 功能包](https://github.com/autowarefoundation/autoware_core/tree/main/map/autoware_map_loader)中 `lanelet2_map_loader` 的示例
（在[此 PR](https://github.com/autowarefoundation/autoware_universe/pull/1056) 中添加）。

在 [`package.xml`](https://github.com/autowarefoundation/autoware_core/blob/main/map/autoware_map_loader/package.xml) 中添加：

```xml
<test_depend>ros_testing</test_depend>
```

在 [`CMakeLists.txt`](https://github.com/autowarefoundation/autoware_core/blob/main/map/autoware_map_loader/CMakeLists.txt) 中，
添加或修改 `BUILD_TESTING` 部分：

```cmake
if(BUILD_TESTING)
  add_ros_test(
    test/lanelet2_map_loader_launch.test.py
    TIMEOUT "30"
  )
  install(DIRECTORY
    test/data/
    DESTINATION share/${PROJECT_NAME}/test/data/
  )
endif()
```

除了使用 `add_ros_test` 命令，我们还使用 `install` 命令安装测试所需的数据。

!!! note

    - `TIMEOUT` 参数以秒为单位；详细信息请参阅 [add_ros_test.cmake 文件](https://github.com/ros2/ros_testing/blob/master/ros_testing/cmake/add_ros_test.cmake)。
    - `add_ros_test` 命令会在独立的 `ROS_DOMAIN_ID` 中运行测试，避免并行运行的测试相互干扰。

要创建测试，
可以阅读 [launch_testing 快速入门示例](https://github.com/ros2/launch/tree/master/launch_testing#quick-start-example)，
也可以按照以下步骤操作。

以 [`test/lanelet2_map_loader_launch.test.py`](https://github.com/autowarefoundation/autoware_core/blob/main/map/autoware_map_loader/test/lanelet2_map_loader_launch.test.py) 为例，
首先导入依赖项：

```python
import os
import unittest

from ament_index_python import get_package_share_directory
import launch
from launch import LaunchDescription
from launch_ros.actions import Node
import launch_testing
import pytest
```

然后创建启动描述，用于启动被测节点。
请注意，此处会查找 [`test_map.osm`](https://github.com/autowarefoundation/autoware_core/blob/main/map/autoware_map_loader/test/data/test_map.osm) 文件的路径，并将其传递给节点，
而[冒烟测试 API](#smoke-tests) 无法完成这一操作：

```python
@pytest.mark.launch_test
def generate_test_description():

    lanelet2_map_path = os.path.join(
        get_package_share_directory("autoware_map_loader"), "test/data/test_map.osm"
    )

    lanelet2_map_loader = Node(
        package="autoware_map_loader",
        executable="autoware_lanelet2_map_loader",
        parameters=[{"lanelet2_map_path": lanelet2_map_path}],
    )

    context = {}

    return (
        LaunchDescription(
            [
                lanelet2_map_loader,
                # Start test after 1s - gives time for the map_loader to finish initialization
                launch.actions.TimerAction(
                    period=1.0, actions=[launch_testing.actions.ReadyToTest()]
                ),
            ]
        ),
        context,
    )
```

!!! note

    - 由于节点需要时间处理输入的 lanelet2 地图，我们使用 `TimerAction` 将测试启动延迟 1 秒。
    - 在上面的示例中，`context` 为空，但可以用它向测试用例传递对象。
    - 你可以在 [ROS 2 context_launch_test.py](https://github.com/ros2/launch/blob/humble/launch_testing/test/launch_testing/examples/context_launch_test.py) 测试示例中找到使用 `context` 的例子。

最后，在节点可执行程序关闭后执行测试（`post_shutdown_test`）。
这里验证节点启动时没有错误，并且正常退出。

```python
@launch_testing.post_shutdown_test()
class TestProcessOutput(unittest.TestCase):
    def test_exit_code(self, proc_info):
        # Check that process exits with code 0: no error
        launch_testing.asserts.assertExitCodes(proc_info)
```

<a id="running-the-test"></a>

## 运行测试

接着上面的示例，首先构建功能包：

```console
colcon build --packages-up-to autoware_map_loader
source install/setup.bash
```

然后可以手动执行组件测试：

```console
ros2 test src/core/autoware_core/map/autoware_map_loader/test/lanelet2_map_loader_launch.test.py
```

也可以将其作为整个功能包测试的一部分执行：

```console
colcon test --packages-select autoware_map_loader
```

确认测试已经执行，例如：

```console
$ colcon test-result --all --verbose
...
build/autoware_map_loader/test_results/autoware_map_loader/test_lanelet2_map_loader_launch.test.py.xunit.xml: 1 test, 0 errors, 0 failures, 0 skipped
```

<a id="next-steps"></a>

### 后续步骤

[单节点集成测试：组件测试](#integration-test-with-a-single-node-component-test)中介绍的简单测试可以从多个方向扩展，例如测试节点的输出。

<a id="testing-the-output-of-a-node"></a>

#### 测试节点输出

要在节点运行期间进行测试，
请在 `*launch.test.py` 中添加 Python `unittest.TestCase` 的子类，创建一个[_运行期测试_](https://github.com/ros2/launch/tree/foxy/launch_testing#active-tests)。
需要编写一些样板代码，创建节点并订阅特定话题，以访问输出，例如：

```python
import unittest

class TestRunningDataPublisher(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        cls.context = Context()
        rclpy.init(context=cls.context)
        cls.node = rclpy.create_node("test_node", context=cls.context)

    @classmethod
    def tearDownClass(cls):
        rclpy.shutdown(context=cls.context)

    def setUp(self):
        self.msgs = []
        sub = self.node.create_subscription(
            msg_type=my_msg_type,
            topic="/info_test",
            callback=self._msg_received
        )
        self.addCleanup(self.node.destroy_subscription, sub)

    def _msg_received(self, msg):
        # Callback for ROS 2 subscriber used in the test
        self.msgs.append(msg)

    def get_message(self):
        startlen = len(self.msgs)

        executor = rclpy.executors.SingleThreadedExecutor(context=self.context)
        executor.add_node(self.node)

        try:
            # Try up to 60 s to receive messages
            end_time = time.time() + 60.0
            while time.time() < end_time:
                executor.spin_once(timeout_sec=0.1)
                if startlen != len(self.msgs):
                    break

            self.assertNotEqual(startlen, len(self.msgs))
            return self.msgs[-1]
        finally:
            executor.remove_node(self.node)

    def test_message_content():
        msg = self.get_message()
        self.assertEqual(msg, "Hello, world")
```

<a id="references"></a>

## 参考资料

- [colcon](https://github.com/ros2/ros2/wiki/Colcon-Tutorial) 用于构建和运行测试。
- [launch testing](https://github.com/ros2/launch/tree/master/launch_testing) 用于启动节点并运行测试。
- [测试指南](index.md)介绍了 Autoware 中执行的不同测试类型，并提供相应指南的链接。
