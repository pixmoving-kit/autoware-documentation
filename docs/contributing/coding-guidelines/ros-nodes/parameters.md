<a id="parameters"></a>

# 参数

Autoware 的 ROS 节点会声明参数，并在节点启动时通过参数文件提供参数值。参数文件中应包含所有预期参数及其对应值。根据具体应用，可能需要修改参数值。

有关参数的更多信息，请参阅 ROS 官方文档：

- [理解 ROS 2 参数](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.html)
- [关于 ROS 2 参数](https://docs.ros.org/en/humble/Concepts/About-ROS-2-Parameters.html)

<a id="workflow"></a>

## 工作流程

使用 [declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb) 函数的 ROS 功能包应：

- 使用 [declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb)，且不提供默认值。
- 创建参数文件。
- 创建 schema 文件。

此流程的目的是建立一个经过验证的唯一权威来源，既用于向 ROS 节点传递参数，也用于网页文档。这样可以降低使用无效参数值的风险，并简化文档维护。具体实现方式如下：

- 如果参数文件中缺少预期参数，[declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb) 会抛出异常。
- schema 在 CI 中验证参数文件，并渲染参数表，如下图所示。

  ```mermaid
  flowchart TD
      NodeSchema[Schema file: *.schema.json]
      ParameterFile[Parameter file: *.param.yaml]
      WebDocumentation[Web documentation table]

      NodeSchema -->|Validation| ParameterFile
      NodeSchema -->|Generate| WebDocumentation
  ```

注意：由于运行时不进行验证，参数值仍可能在修改后绕过验证。

<a id="declare-parameter-function"></a>

## 参数声明函数

[declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb) 函数负责在节点启动时设置参数值。

```cpp
declare_parameter<INSERT_TYPE>("INSERT_PARAMETER_1_NAME"),
declare_parameter<INSERT_TYPE>("INSERT_PARAMETER_N_NAME")
```

由于未提供 _default_value_，如果传入的 `*.param.yaml` 文件中缺少某个参数，该函数就会抛出异常。为 [declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb) 函数选择下表 _C++ 类型_列中的类型，替换 _INSERT_TYPE_。

| ParameterType 枚举        | C++ 类型                   |
| ------------------------- | -------------------------- |
| `PARAMETER_BOOL`          | `bool`                     |
| `PARAMETER_INTEGER`       | `int64_t`                  |
| `PARAMETER_DOUBLE`        | `double`                   |
| `PARAMETER_STRING`        | `std::string`              |
| `PARAMETER_BYTE_ARRAY`    | `std::vector<uint8_t>`     |
| `PARAMETER_BOOL_ARRAY`    | `std::vector<bool>`        |
| `PARAMETER_INTEGER_ARRAY` | `std::vector<int64_t>`     |
| `PARAMETER_DOUBLE_ARRAY`  | `std::vector<double>`      |
| `PARAMETER_STRING_ARRAY`  | `std::vector<std::string>` |

此表根据 [Parameter Type](https://github.com/ros2/rcl_interfaces/blob/humble/rcl_interfaces/msg/ParameterType.msg) 和 [Parameter Value](https://github.com/ros2/rcl_interfaces/blob/humble/rcl_interfaces/msg/ParameterValue.msg) 整理。

示例：_Lidar Apollo Segmentation TVM Nodes_ 的[声明函数](https://github.com/autowarefoundation/autoware_universe/blob/f85c90b56ed4c7d6b52e787570e590cff786b28b/perception/lidar_apollo_segmentation_tvm_nodes/src/lidar_apollo_segmentation_tvm_node.cpp#L38)

<a id="parameter-file"></a>

## 参数文件

参数文件保持精简，无须向用户提供描述、类型等额外信息，因为这些信息由关联的 schema 文件提供。为 ROS 节点编写参数文件时，可从以下模板开始。

```yaml
/**:
  ros__parameters:
    INSERT_PARAMETER_1_NAME: INSERT_PARAMETER_1_VALUE
    INSERT_PARAMETER_N_NAME: INSERT_PARAMETER_N_VALUE
```

注意：使用 `/**` 而非明确的节点命名空间，可以将参数文件传递给经过[重映射](https://design.ros2.org/articles/static_remapping.html)的 ROS 节点。

要使模板适用于 ROS 节点，请为所有参数替换对应的 `INSERT_PARAMETER_..._NAME` 和 `INSERT_PARAMETER_..._VALUE`。每次调用 [declare_parameter(...)](https://docs.ros.org/en/ros2_packages/humble/api/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb) 都接受一个参数作为输入。所有参数文件都应使用 `.param.yaml` 后缀，以确保正确应用自动格式化。

Autoware 为 ROS 功能包提供以下两类参数文件：

- **节点参数文件**
  - 节点参数文件存储 Autoware 各功能包提供的典型参数。
    - 例如，[`behavior_path_planner` 的参数](https://github.com/autowarefoundation/autoware_universe/tree/245242cee866de2d113e89c562353c5fc17f1f98/planning/behavior_path_planner/config)
  - Autoware 中的节点只要声明了 ROS 参数，就必须提供参数文件。
  - 对于 `FOO_package`，参数应存放在 `FOO_package/config` 中。
  - 各功能包的 launch 文件必须默认加载节点参数：

```xml
<launch>
  <arg name="foo_node_param_path" default="$(find-pkg-share FOO_package)/config/foo_node.param.yaml" />

  <node pkg="FOO_package" exec="foo_node">
    ...
    <param from="$(var foo_node_param_path)" />
  </node>
</launch>
```

- **启动参数文件**
  - 用户为自己的车辆创建 launch 功能包时，应复制 launch 文件使用的各节点参数文件，将其作为“启动参数文件”。
  - 随后针对用户的具体车辆定制启动参数文件。
    - 例如，[存储在 `autoware_launch` 下的 `behavior_path_planner` 定制参数](https://github.com/autowarefoundation/autoware_launch/tree/5fa613b9d80bf4f0db77efde03a43f7ede6bac86/autoware_launch/config)
  - 启动参数文件存储在 `autoware_launch` 功能包下。

### sync-params

尽管 `autoware_launch` 不直接使用节点参数文件，但在节点实现中添加或删除参数后，仍必须更新节点参数文件。每个节点参数文件都是对应启动参数文件的主文件。

在大多数情况下，只要正确更新了对应的节点参数文件，[sync-params](https://github.com/autowarefoundation/autoware_launch/actions/workflows/sync-params.yaml) 工作流就会自动更新启动参数文件。

由 sync-params 工作流管理的启动参数文件带有以 `# This file is managed by sync-params workflow` 开头的 sync-params 注释头。
默认情况下，除注释头之外，启动参数文件与对应的节点参数文件完全一致。

<a id="adding-or-removing-parameters-with-sync-params-workflow"></a>

#### 使用 sync-params 工作流添加或删除参数

一般来说，如果只是添加或删除某些节点参数，不应为 `autoware_launch` 单独创建 PR。此时请按以下步骤操作：

1. **首先**合并包含节点参数文件更新的节点 PR。
2. 对所需类别运行 sync-params 工作流。可用类别见 [sync-param 配置文件](https://github.com/autowarefoundation/autoware_launch/blob/main/.github/sync-params.yaml)。
3. 工作流会创建 PR；如果已经存在相应 PR，则更新该 PR。PR 会带有 [`tag:sync-params`](https://github.com/autowarefoundation/autoware_launch/pulls?q=is%3Aopen+is%3Apr+label%3Atag%3Async-params) 标签。

<a id="overriding-parameters-with-sync-params-workflow"></a>

#### 使用 sync-params 工作流覆盖参数

某些字段需要与对应节点参数文件中的值不同。可以为这些字段添加 `# {OVERRIDE}` 或 `# {OVERRIDE: <reason>}` 注释标记，使其保留不同于上游节点参数文件的值。

如果像下例一样直接修改启动参数文件中的参数，下次运行 sync-params 工作流时，它会尝试将参数恢复为 `foo: 42`，与节点参数文件保持一致。

```patch
 # NG
-foo: 42
+foo: 40
```

要使修改持续保留，请创建 PR，按以下方式更新启动参数文件：

```patch
 # OK
-foo: 42
+foo: 40 # {OVERRIDE}
```

如果该行已有注释，请将标记放在现有注释之前：

```patch
 # OK
-bar: [1, 2, 3, 4] # existing comment
+bar: [5, 6, 7, 8] # {OVERRIDE} existing comment
```

**提示**：尽可能使节点参数文件与预期的启动参数文件一致，从而减少需要覆盖的参数。

<a id="updating-sync-params-configurations"></a>

#### 更新 sync-params 配置

要添加一组新的节点参数文件与启动参数文件配对，请按照下例更新 [sync-param 配置文件](https://github.com/autowarefoundation/autoware_launch/blob/main/.github/sync-params.yaml)。

```yaml
perception: # category
  - repository: autowarefoundation/autoware_universe # the source repository
    ref: main
    files:
      - # node parameter file path in the source repository
        source: perception/autoware_image_object_locator/config/bbox_object_locator.param.yaml
        # List of launch parameter file paths in autoware_launch repository
        variants:
          - path: autoware_launch/config/perception/object_recognition/detection/camera_vru_detection/near_range_camera_vru_detector.param.yaml
```

然后在本地运行 `python .github/scripts/sync_params.py perception`（将 `perception` 替换为相应类别），这就是 sync-params 工作流底层使用的脚本。
同一类别中的其他文件也可能被更新，但只应提交刚添加的文件，并丢弃其他修改。其他文件应通过自动生成的 sync-params PR 处理。

要删除某个配对，只需从 sync-param 配置文件中移除相应启动参数文件条目，并删除 sync-params 注释头。

如果需要指定新的上游，例如节点功能包已移至其他仓库，请先删除旧配对，再使用新的上游重新添加该启动参数文件。

## JSON Schema

[JSON Schema](https://json-schema.org/understanding-json-schema/about) 用于验证参数文件，确保其结构和内容正确。在云原生开发中，使用 JSON Schema 进行此类验证被视为最佳实践。为 ROS 节点定义 schema 时，应以下面的 schema 模板为起点。

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "INSERT_TITLE",
  "type": "object",
  "definitions": {
    "INSERT_ROS_NODE_NAME": {
      "type": "object",
      "properties": {
        "INSERT_PARAMETER_1_NAME": {
          "type": "INSERT_TYPE",
          "description": "INSERT_DESCRIPTION",
          "default": "INSERT_DEFAULT",
          "INSERT_BOUND_CONDITION(S)": INSERT_BOUND_VALUE(S)
        },
        "INSERT_PARAMETER_N_NAME": {
          "type": "INSERT_TYPE",
          "description": "INSERT_DESCRIPTION",
          "default": "INSERT_DEFAULT",
          "INSERT_BOUND_CONDITION(S)": INSERT_BOUND_VALUE(S)
        }
      },
      "required": ["INSERT_PARAMETER_1_NAME", "INSERT_PARAMETER_N_NAME"],
      "additionalProperties": false
    }
  },
  "properties": {
    "/**": {
      "type": "object",
      "properties": {
        "ros__parameters": {
          "$ref": "#/definitions/INSERT_ROS_NODE_NAME"
        }
      },
      "required": ["ros__parameters"],
      "additionalProperties": false
    }
  },
  "required": ["/**"],
  "additionalProperties": false
}
```

schema 文件路径为 `INSERT_PATH_TO_PACKAGE/schema/`，文件名为 `INSERT_NODE_NAME.schema.json`。要使模板适用于 ROS 节点，请替换各个 `INSERT_...`，并添加全部参数 `1..N`。

示例：_基于图像投影的融合 - Pointpainting_ 的 [schema](https://github.com/autowarefoundation/autoware_universe/blob/main/perception/autoware_image_projection_based_fusion/schema/pointpainting.schema.json)

<a id="attributes"></a>

### 属性

参数具有多种属性，其中有些是必需的，有些是可选的。在适用时强烈建议提供可选属性，因为这些属性能提供有用的参数信息，并确保参数值位于规定范围内。

<a id="required"></a>

#### 必需属性

- 名称（name）
- 类型（type）
  - 参见 [JSON Schema 类型](http://json-schema.org/understanding-json-schema/reference/type.html)
- 描述（description）

<a id="optional"></a>

#### 可选属性

- 默认值（default）
  - 经过测试和验证的值，参见 [JSON Schema 默认值](https://json-schema.org/understanding-json-schema/reference/generic.html)
- 边界（bound(s)）
  - 取决于类型，例如[整数](https://json-schema.org/understanding-json-schema/reference/numeric.html#integer)、[范围](https://json-schema.org/understanding-json-schema/reference/numeric.html#range)和[大小](https://json-schema.org/understanding-json-schema/reference/object.html#size)。

<a id="tips-and-tricks"></a>

## 提示与技巧

采用成熟的标准可以使用通用工具。以下示例介绍如何在 VS Code 中将 schema 关联到参数文件，从而为开发者提供自动补全、参数边界验证等便捷功能。

在项目根目录下创建 `.vscode` 文件夹，并创建两个文件；`extensions.json` 的内容为：

```json
{
  "recommendations": ["redhat.vscode-yaml"]
}
```

`settings.json` 的内容为：

```json
{
  "yaml.schemas": {
    "./INSERT_PATH_TO_PACKAGE/schema/INSERT_NODE_NAME.schema.json": "**/INSERT_NODE_NAME/config/*.param.yaml"
  }
}
```

RedHat YAML 扩展支持使用 JSON Schema 验证 YAML 文件，`"yaml.schemas"` 设置会将 `*.schema.json` 文件与 `config/` 文件夹中的所有 `*.param.yaml` 文件关联起来。
