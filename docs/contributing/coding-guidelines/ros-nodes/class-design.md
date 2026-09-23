<a id="class-design"></a>

# 类设计

我们以 `autoware_gnss_poser` 功能包为例。

<a id="namespaces"></a>

## 命名空间

```cpp
namespace autoware::gnss_poser
{
...
} // namespace autoware::gnss_poser
```

- 所有内容都应位于 `autoware::gnss_poser` 命名空间中。
- 结束大括号必须带有注明命名空间名称的注释。（通过 `cpplint` 自动检查）

<a id="classes"></a>

## 类

<a id="nodes"></a>

### 节点

#### `gnss_poser_node.hpp`

```cpp
class GNSSPoser : public rclcpp::Node
{
  public:
    explicit GNSSPoser(const rclcpp::NodeOptions & node_options);
  ...
}
```

#### `gnss_poser_node.cpp`

```cpp
GNSSPoser::GNSSPoser(const rclcpp::NodeOptions & node_options)
: Node("gnss_poser", node_options)
{
  ...
}
```

- 类名应使用 `CamelCase`。
- 节点类应继承 `rclcpp::Node`。
- 构造函数必须标记为 explicit。
- 构造函数必须接受 `rclcpp::NodeOptions` 参数。
- 默认节点名称：
  - 不应带有 `autoware_` 前缀。
  - **不应**带有 `_node` 后缀。
    - **理由：**节点名称在运行时使用，而 `ros2 node list` 的输出本来就只包含节点。添加 `_node` 是多余的。
  - **示例：**`gnss_poser`。

<a id="component-registration"></a>

##### 组件注册

```cpp
...
} // namespace autoware::gnss_poser

#include <rclcpp_components/register_node_macro.hpp>
RCLCPP_COMPONENTS_REGISTER_NODE(autoware::gnss_poser::GNSSPoser)
```

- 应在 `gnss_poser_node.cpp` 文件的末尾、命名空间之外注册组件。

<a id="libraries"></a>

### 库

!!! warning

    编写中
