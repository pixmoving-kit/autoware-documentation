<a id="directory-structure"></a>

# 目录结构

本文介绍 Autoware 中 ROS 节点的目录结构。

我们以 `autoware_gnss_poser` 功能包为例。

**请注意，此示例并不对应实际的 `autoware_gnss_poser`，其中包含额外的文件和目录，以展示所有可能的功能包结构。**

<a id="c-package"></a>

## C++ 功能包

<a id="entire-structure"></a>

### 整体结构

- 此处提供整个功能包可能采用的结构参考。
- 功能包不一定包含这里展示的所有目录。

```txt
autoware_gnss_poser
├─ package.xml
├─ CMakeLists.txt
├─ README.md
│
├─ config
│   ├─ gnss_poser.param.yaml
│   └─ another_non_ros_config.yaml
│
├─ schema
│   └─ gnss_poser.schema.json
│
├─ doc
│   ├─ foo_document.md
│   └─ foo_diagram.svg
│
├─ include  # for exporting headers
│   └─ autoware
│       └─ gnss_poser
│           └─ exported_header.hpp
│
├─ src
│   ├─ include
│   │   ├─ gnss_poser_node.hpp
│   │   └─ foo.hpp
│   ├─ gnss_poser_node.cpp
│   └─ bar.cpp
│
├─ launch
│   ├─ gnss_poser.launch.xml
│   └─ gnss_poser.launch.py
│
└─ test
    ├─ test_foo.hpp  # or place under an `include` folder here
    └─ test_foo.cpp
```

<a id="package-name"></a>

### 功能包名称

- Autoware 中的所有功能包都应带有 `autoware_` 前缀。
- 即使功能包导出了节点，其名称也**不应**带有 `_node` 后缀。
- 功能包名称应使用 `snake_case`。

| 功能包名称                       | 是否合规 | 替代名称                     |
| --------------------------------- | --- | ---------------------------- |
| path_smoother                     | ❌  | autoware_path_smoother       |
| autoware_trajectory_follower_node | ❌  | autoware_trajectory_follower |
| autoware_geography_utils          | ✅  | -                            |

<a id="package-folder"></a>

### 功能包文件夹

```txt
autoware_gnss_poser
├─ package.xml
├─ CMakeLists.txt
└─ README.md
```

功能包文件夹的名称应与功能包名称相同。

#### `package.xml`

- 应在 `<name>` 标签内填写功能包名称。
  - `<name>autoware_gnss_poser</name>`

#### `CMakeLists.txt`

- [`project()`](https://cmake.org/cmake/help/latest/command/project.html) 命令应使用功能包名称。
  - **示例：**`project(autoware_gnss_poser)`

<a id="exporting-a-composable-node-component-executables"></a>

##### 导出可组合节点组件的可执行程序

为遵循最佳实践并提高系统效率，建议优先使用可组合节点组件。

这种方式便于在 ROS 环境中部署和维护。

```cmake
ament_auto_add_library(${PROJECT_NAME} SHARED
  src/gnss_poser_node.cpp
)

rclcpp_components_register_node(${PROJECT_NAME}
  PLUGIN "autoware::gnss_poser::GNSSPoser"
  EXECUTABLE ${PROJECT_NAME}_node
)
```

- 如果构建的是：
  - **仅一个可组合节点组件**，则可执行程序名称应以 `${PROJECT_NAME}` 开头。
  - **多个可组合节点组件**，则可执行程序名称由开发者自行决定。
- 所有可组合节点组件的可执行程序都应带有 `_node` 后缀。

<a id="exporting-a-standalone-node-executable-without-composition-discouraged-for-most-cases"></a>

##### 导出不使用组合机制的独立节点可执行程序_（大多数情况下不推荐）_

独立可执行程序的使用**应限于**调试或工具等特定需求场景。

对于常规运行场景，通常优先选择[导出可组合节点组件的可执行程序](#exporting-a-composable-node-component-executables)，因为它在 ROS 生态系统中更具灵活性和可扩展性。

假设：

- `src/gnss_poser.cpp` 包含 `GNSSPoser` 类。
- `src/gnss_poser_node.cpp` 包含 `main` 函数。
- 未注册可组合节点组件。

```cmake
ament_auto_add_library(${PROJECT_NAME} SHARED
  src/gnss_poser.cpp
)

ament_auto_add_executable(${PROJECT_NAME}_node src/gnss_poser_node.cpp)
```

- 节点可执行程序：
  - 应带有 `_node` 后缀。
  - 应以 `${PROJECT_NAME} 开头。

<a id="config-and-schema"></a>

### `config` 和 `schema`

```txt
autoware_gnss_poser
│─ config
│   ├─ gnss_poser.param.yaml
│   └─ another_non_ros_config.yaml
└─ schema
    └─ gnss_poser.schema.json
```

#### `config`

- ROS 参数文件使用 `.param.yaml` 扩展名。
- 非 ROS 参数文件使用 `.yaml` 扩展名。

**理由：**ROS 参数与非 ROS 参数使用不同的检查规则。

#### `schema`

放置参数定义文件。详情请参阅[参数](./parameters.md)。

### `doc`

```txt
autoware_gnss_poser
└─ doc
    ├─ foo_document.md
    └─ foo_diagram.svg
```

放置文档文件，并在 README 文件中添加指向这些文件的链接。

<a id="include-and-src"></a>

### `include` 和 `src`

- 除非确实需要导出头文件，否则不应在功能包目录下设置 `include` 目录。
- 大多数情况下，请遵循[不导出头文件](#not-exporting-headers)的结构。
- 导出头文件的库功能包可以遵循[导出头文件](#exporting-headers)的结构。

<a id="not-exporting-headers"></a>

#### 不导出头文件

```txt
autoware_gnss_poser
└─ src
    ├─ include
    │   ├─ gnss_poser_node.hpp
    │   └─ foo.hpp
    │─ gnss_poser_node.cpp
    └─ bar.cpp

OR

autoware_gnss_poser
└─ src
    ├─ gnss_poser_node.hpp
    ├─ gnss_poser_node.cpp
    ├─ foo.hpp
    └─ bar.cpp
```

- 导出节点的源文件：
  - 应带有 `_node` 后缀。
    - **理由：**与其他源文件区分。
  - **不应**带有 `autoware_` 前缀。
    - **理由：**避免冗长。
- 有关如何组织 `gnss_poser_node.hpp` 和 `gnss_poser_node.cpp` 文件的更多信息，请参阅[类设计](./class-design.md)。
- `src` 下源文件的组织方式由开发者自行决定。
  - **注意：**`src` 下的 `include` 文件夹是可选的。

<a id="exporting-headers"></a>

#### 导出头文件

```txt
autoware_gnss_poser
└─ include
    └─ autoware
        └─ gnss_poser
            └─ exported_header.hpp
```

- `autoware_gnss_poser/include` 文件夹应**仅**包含 `autoware` 文件夹。
  - **理由：**安装 ROS Debian 软件包时，头文件会复制到 `/opt/ros/$ROS_DISTRO/include/` 目录。采用这种结构可避免与非 Autoware 功能包发生冲突。
- `autoware_gnss_poser/include/autoware` 文件夹应**仅**包含 `gnss_poser` 文件夹。
  - **理由：**同样，这种结构用于避免与其他功能包发生冲突。
- `autoware_gnss_poser/include/autoware/gnss_poser` 文件夹应包含需要导出的头文件。

**注意：**如果在 `CMakeLists.txt` 中使用了 `ament_auto_package()` 命令，并且存在 `autoware_gnss_poser/include` 文件夹，
则 [ament_auto_package.cmake](https://github.com/ament/ament_cmake/blob/79cc237f8eb819edf4c1c624b56451e0a05a45f8/ament_cmake_auto/cmake/ament_auto_package.cmake#L62-L66) 会将该 `include` 文件夹导出到 `install` 文件夹。

**参考资料：**<https://docs.ros.org/en/humble/How-To-Guides/Ament-CMake-Documentation.html#adding-targets>

### `launch`

```txt
autoware_gnss_poser
└─ launch
    ├─ gnss_poser.launch.xml
    └─ gnss_poser.launch.py
```

- 此处可以放置多个 launch 文件。
- 除非有特殊原因，否则请使用 `.launch.xml` 扩展名。
  - **理由：**虽然 `.launch.py` 更灵活，但会降低可读性。
- launch 文件名避免使用 `autoware_` 前缀。
  - **理由：**避免冗长。

### `test`

```txt
autoware_gnss_poser
└─ test
    ├─ test_foo.hpp  # or place under an `include` folder here
    └─ test_foo.cpp
```

放置测试源文件。详情请参阅[单元测试](../../testing-guidelines/unit-testing.md)。

<a id="python-package"></a>

## Python 功能包

!!! warning

    编写中
