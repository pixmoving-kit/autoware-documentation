<a id="console-logging"></a>

# 控制台日志

ROS 2 日志是理解和调试 ROS 节点的强大工具。

本页重点介绍如何设计 Autoware 中的控制台日志，并提供一些实用示例。
要全面了解 ROS 2 日志的工作原理，请参阅[日志文档](https://docs.ros.org/en/humble/Concepts/About-Logging.html)。

<a id="logging-use-cases-in-autoware"></a>

## Autoware 中日志的使用场景

- 开发者通过查看控制台日志调试代码。
- 车辆操作人员根据控制台日志采取适当的避险措施。
- 日志分析人员分析记录在 rosbag 文件中的控制台日志。

为高效支持这些场景，需要清晰且醒目的日志。
为此，下文定义了若干规则。

<a id="rules"></a>

## 规则

<a id="choose-appropriate-severity-levels-required-non-automated"></a>

### 选择适当的严重性级别（必需，非自动检查）

<a id="rationale"></a>

#### 理由

严重性级别选择不当会造成混淆，例如：

- 无用的消息被标记为 `FATAL`。
- 非常重要的错误消息被标记为 `INFO`。

<a id="example"></a>

#### 示例

请参考以下标准：

- **DEBUG：**用于向开发者展示调试信息。请注意，此级别日志默认隐藏。
- **INFO：**用于向操作人员通知事件（初始化期间的周期性通知、状态变化、服务响应等）。
- **WARN：**用于节点仍可继续正常工作，但可能出现非预期行为的情况。
  - 例如，“路径优化失败，但可以使用上一次的数据”“定位得分较低”等。
- **ERROR：**用于节点无法继续正常工作，且将出现非预期行为的情况。
  - 例如，“路径优化失败且路径为空”“车辆将触发紧急停车”等。
- **FATAL：**用于整个系统无法继续正常工作，必须停止系统的情况。
  - 例如，“车辆控制 ECU 无响应”“系统存储崩溃”等。

<a id="filter-out-unnecessary-logs-by-setting-logging-options-required-non-automated"></a>

### 通过日志选项过滤不必要的日志（必需，非自动检查）

<a id="rationale_1"></a>

#### 理由

驱动程序等部分第三方节点可能不遵循 Autoware 的指南。
如果日志过于嘈杂，应过滤不必要的日志。

<a id="example_1"></a>

#### 示例

使用 `--log-level {level}` 选项修改需要显示的最低日志级别：

```xml
<launch>
  <!-- This outputs only FATAL level logs. -->
  <node pkg="demo_nodes_cpp" exec="talker" ros_args="--log-level fatal" />
</launch>
```

如果只想禁用特定输出目标，请使用 `--disable-stdout-logs`、`--disable-rosout-logs` 和/或 `--disable-external-lib-logs` 选项：

```xml
<launch>
  <!-- This outputs to rosout and disk. -->
  <node pkg="demo_nodes_cpp" exec="talker" ros_args="--disable-stdout-logs" />
</launch>
```

```xml
<launch>
  <!-- This outputs to stdout. -->
  <node pkg="demo_nodes_cpp" exec="talker" ros_args="--disable-rosout-logs --disable-external-lib-logs" />
</launch>
```

<a id="use-throttled-logging-when-the-log-is-unnecessarily-shown-repeatedly-required-non-automated"></a>

### 日志无须频繁重复显示时使用限频日志（必需，非自动检查）

<a id="rationale_2"></a>

#### 理由

如果控制台上显示大量日志，人们就可能错过重要消息。

<a id="example_2"></a>

#### 示例

在等待某些消息时，限频日志通常已经足够。
这种情况下，可将约 5 秒作为参考间隔。

```cpp
// Compliant
void FooNode::on_timer() {
  if (!current_pose_) {
    RCLCPP_ERROR_THROTTLE(get_logger(), *get_clock(), 5000, "Waiting for current_pose_.");
    return;
  }
}

// Non-compliant
void FooNode::on_timer() {
  if (!current_pose_) {
    RCLCPP_ERROR(get_logger(), "Waiting for current_pose_.");
    return;
  }
}
```

<a id="exception"></a>

#### 例外

以下情况可以不进行限频。

- 消息确实值得每次都显示。
- 消息级别为 DEBUG。

<a id="do-not-depend-on-rclcppnode-in-core-library-classes-but-depend-only-on-rclcpplogginghpp-advisory-non-automated"></a>

### 核心库类不依赖 rclcpp::Node，仅依赖 rclcpp/logging.hpp（建议，非自动检查）

<a id="rationale_3"></a>

#### 理由

包含可复用算法的核心库类也可能用于非 ROS 平台。
将库移植到其他平台时，依赖越少越好。

<a id="example_3"></a>

#### 示例

```cpp
// Compliant
#include <rclcpp/logging.hpp>

class FooCore {
public:
  explicit FooCore(const rclcpp::Logger & logger) : logger_(logger) {}

  void process() {
    RCLCPP_INFO(logger_, "message");
  }

private:
  rclcpp::Logger logger_;
};

// Compliant
// Note that logs aren't published to `/rosout` if the logger name is different from the node name.
#include <rclcpp/logging.hpp>

class FooCore {
  void process() {
    RCLCPP_INFO(rclcpp::get_logger("foo_core_logger"), "message");
  }
};


// Non-compliant
#include <rclcpp/node.hpp>

class FooCore {
public:
  explicit FooCore(const rclcpp::NodeOptions & node_options) : node_("foo_core_node", node_options) {}

  void process() {
    RCLCPP_INFO(node_.get_logger(), "message");
  }

private:
  rclcpp::Node node_;
};
```

<a id="tips"></a>

## 提示

<a id="use-rqt_console-to-filter-logs"></a>

### 使用 rqt_console 过滤日志

使用 `rqt_console` 可以方便地过滤日志：

```bash
ros2 run rqt_console rqt_console
```

更多信息请参阅 [ROS 2 文档](https://docs.ros.org/en/rolling/Tutorials/Beginner-CLI-Tools/Using-Rqt-Console/Using-Rqt-Console.html)。

<a id="useful-marco-expressions"></a>

### 实用的宏表达式

调试程序时，有时需要查看执行了哪些函数和代码行。
这时可以使用 `__FILE__`、`__LINE__` 和 `__FUNCTION__` 宏：

```cpp
void FooNode::on_timer() {
  RCLCPP_DEBUG(get_logger(), "file: %s, line: %s, function: %s" __FILE__, __LINE__, __FUNCTION__);
}
```

示例输出如下：

> [DEBUG] [1671720414.395456931] [foo]: file: /path/to/file.cpp, line: 100, function: on_timer
