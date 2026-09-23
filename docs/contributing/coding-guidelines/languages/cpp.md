# C++

!!! warning

    编写中

<a id="references"></a>

## 参考资料

如果本页未定义某项规则，请遵循以下指南。

1. <https://docs.ros.org/en/humble/Contributing/Code-Style-Language-Versions.html>
2. <https://www.autosar.org/fileadmin/standards/R22-11/AP/AUTOSAR_RS_CPP14Guidelines.pdf>
3. <https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines>

此外，建议对每个文件运行 Clang-Tidy。
使用方法请参阅[对 ROS 功能包应用 Clang-Tidy](../../../tutorials/others/applying-clang-tidy-to-ros-packages.md)。

请注意，Clang-Tidy 并未覆盖所有规则。

<a id="style-rules"></a>

## 风格规则

<a id="include-header-files-in-the-defined-order-required-partially-automated"></a>

### 按规定顺序包含头文件（必需，部分自动检查）

<a id="rationale"></a>

#### 理由

- 由于间接依赖的存在，C++ 的头文件包含机制在头文件顺序不同时可能产生不同的行为。
- 为减少意外缺陷，应优先包含本地头文件。

<a id="reference"></a>

#### 参考资料

- <https://llvm.org/docs/CodingStandards.html#include-style>

<a id="example"></a>

#### 示例

按以下顺序包含头文件：

- 主模块头文件
- 本功能包头文件
- 其他功能包头文件
- 消息头文件
- Boost 头文件
- C 系统头文件
- C++ 系统头文件

```cpp
// Compliant
#include "my_header.hpp"

#include "my_package/foo.hpp"

#include <package1/foo.hpp>
#include <package2/bar.hpp>

#include <std_msgs/msg/header.hpp>

#include <iostream>
#include <vector>
```

如果正确使用 `""` 和 `<>`，`pre-commit` 中的 `ClangFormat` 会自动对头文件排序。

不要在 `#include` 行之间定义宏，否则会妨碍自动排序。

```cpp
// Non-compliant
#include <package1/foo.hpp>
#include <package2/bar.hpp>

#define EIGEN_MPL2_ONLY
#include "my_header.hpp"
#include "my_package/foo.hpp"

#include <Eigen/Core>

#include <std_msgs/msg/header.hpp>

#include <iostream>
#include <vector>
```

应在 `#include` 行之前定义宏。

```cpp
// Compliant
#define EIGEN_MPL2_ONLY

#include "my_header.hpp"

#include "my_package/foo.hpp"

#include <Eigen/Core>
#include <package1/foo.hpp>
#include <package2/bar.hpp>

#include <std_msgs/msg/header.hpp>

#include <iostream>
#include <vector>
```

如果必须在特定位置定义宏，请在该宏之前添加注释说明原因。

```cpp
// Compliant
#include "my_header.hpp"

#include "my_package/foo.hpp"

#include <package1/foo.hpp>
#include <package2/bar.hpp>

#include <std_msgs/msg/header.hpp>

#include <iostream>
#include <vector>

// For the foo bar reason, the FOO_MACRO must be defined here.
#define FOO_MACRO
#include <foo/bar.hpp>
```

<a id="use-lower-snake-case-for-function-names-required-partially-automated"></a>

### 函数名称使用小写蛇形命名法（必需，部分自动检查）

<a id="rationale_1"></a>

#### 理由

- 与 C++ 标准库一致。
- 与 Python、Rust 等其他编程语言一致。

<a id="exception"></a>

#### 例外

- 对于继承自 Qt 等外部项目类的成员函数，应遵循相应项目的命名约定。

<a id="reference_1"></a>

#### 参考资料

- <https://docs.ros.org/en/humble/The-ROS2-Project/Contributing/Code-Style-Language-Versions.html#function-and-method-naming>

<a id="example_1"></a>

#### 示例

```cpp
void function_name()
{
}
```

<a id="use-upper-camel-case-for-enum-names-required-partially-automated"></a>

### 枚举名称使用大驼峰命名法（必需，部分自动检查）

<a id="rationale_2"></a>

#### 理由

- 与 ROS 2 核心功能包一致。

<a id="exception_1"></a>

#### 例外

- 在 `rosidl` 文件中定义的枚举可以使用其他命名约定。

<a id="reference_2"></a>

#### 参考资料

- <http://wiki.ros.org/CppStyleGuide>（参见“15. Enumerations”）

<a id="example_2"></a>

#### 示例

```cpp
enum class Color
{
  Red, Green, Blue
}
```

<a id="use-lower-snake-case-for-constant-names-required-partially-automated"></a>

### 常量名称使用小写蛇形命名法（必需，部分自动检查）

<a id="rationale_3"></a>

#### 理由

- 与 ROS 2 核心功能包一致。
- 与 `std::numbers` 一致。

<a id="exception_2"></a>

#### 例外

- 在 `rosidl` 文件中定义的常量可以使用其他命名约定。

<a id="reference_3"></a>

#### 参考资料

- <https://en.cppreference.com/w/cpp/numeric/constants>

<a id="example_3"></a>

#### 示例

```cpp
constexpr double gravity = 9.80665;
```

<a id="count-acronyms-and-contractions-of-compound-words-as-one-word-required-partially-automated"></a>

### 将首字母缩写和复合词缩写视为一个单词（必需，部分自动检查）

<a id="rationale_4"></a>

#### 理由

- 在多个缩写连续出现时，可明确单词之间的边界。

<a id="reference_4"></a>

#### 参考资料

- <https://rust-lang.github.io/api-guidelines/naming.html#casing-conforms-to-rfc-430-c-case>

<a id="example_4"></a>

#### 示例

```cpp
class RosApi;
RosApi ros_api;
```

<a id="do-not-use-the-auto-keywords-with-eigens-expressions-required"></a>

### 不要对 Eigen 表达式使用 auto 关键字（必需）

<a id="rationale_5"></a>

#### 理由

- 避免对 `Eigen::Matrix` 或 `Eigen::Vector` 变量使用 auto，否则可能导致缺陷。

<a id="reference_5"></a>

#### 参考资料

- [Eigen、C++11 与 auto 关键字](https://libeigen.gitlab.io/eigen/docs-nightly/TopicPitfalls.html)

<a id="use-rclcpp_-eg-rclcpp_info-macros-instead-of-printf-or-stdcout-for-logging-required"></a>

### 使用 RCLCPP\_\*（例如 RCLCPP_INFO）宏记录日志，而不是 printf 或 std::cout（必需）

<a id="rationale_6"></a>

#### 理由

- 原因包括：
  - 便于统一管理日志级别。例如，使用 RCLCPP\_\* 宏时，只需设置 --log_level 即可统一调整整个应用的日志级别。
  - 可以使用 RCUTILS_CONSOLE_OUTPUT_FORMAT 统一日志格式。
  - 使用 RCLCPP\_\* 宏时，日志会自动记录到 /rosout。这些日志可以保存到 rosbag 中，随后通过回放查看日志数据。

<a id="reference_6"></a>

#### 参考资料

- [Autoware 文档：ROS 节点中的控制台日志](../ros-nodes/console-logging.md)
