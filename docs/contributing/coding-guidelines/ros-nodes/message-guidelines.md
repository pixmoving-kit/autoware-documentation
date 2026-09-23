<a id="message-guidelines"></a>

# 消息指南

<a id="format"></a>

## 格式

所有消息都应遵循 [ROS 消息描述规范](https://docs.ros.org/en/humble/Concepts/About-ROS-Interfaces.html#background)。

支持以下格式：

- `.msg`
- `.srv`
- `.action`

定义文件应遵循以下风格。

```text
#
# <File description>
#

# <Field description> (required or optional)
# e.g. <Example value>
type field


# <Constants description>
# <Item1 description>
uint16 CONSTANT_ITEM1 = 0
# <Item2 description>
uint16 CONSTANT_ITEM2 = 0


```

在消息顶部，简要说明消息包含什么内容和/或用于什么用途。示例请参阅 [sensor_msgs/msg/Imu.msg](https://github.com/ros2/common_interfaces/blob/master/sensor_msgs/msg/Imu.msg#L1-L13)。

!!! success ""

    虽然未严格检查，但请尽量保持每行不超过 100 个字符。

_示例：_

```text
# Number of times the vehicle performed an emergency brake (required)
# e.g. 10
uint32 count_emergency_brake


# Speed limit on a specific lane (optional)
# If the value is 0.0, the lane has no specific speed limit, so common speed limit in the country is applied
# e.g. 30.0
# default: 0.0
float limit_kmph


```

如果某个字段并非始终必需，应将其注明为 `optional`。

- 原则上，所有字段都应为 `required`，并包含有效值。
- `optional` 字段可能不包含有效值，因此订阅者或客户端必须检查其有效性。此类字段应提供默认值。

用户可以通过执行以下命令了解消息信息：

```bash
ros2 interface show <message> --all-comments
```

仓库中的各 README.md 应仅提供补充说明和外部资源。在 `.msg` 文件中，应引用 README 的网址及对应锚点。

_示例：_

```text
# please refer to https://github.com/autowarefoundation/autoware_msgs/blob/main/README.md#format for the illustrative description of this field
```

<a id="naming"></a>

## 命名

!!! warning ""

    编写中

创建某种消息的集合类型时，请使用 `Array` 后缀。[common_interfaces](https://github.com/ros2/common_interfaces) 中广泛使用此后缀。

<a id="default-units"></a>

## 默认单位

所有字段根据类型默认使用以下单位：

| 类型           | 默认单位      |
| -------------- | ------------- |
| 距离           | 米（m）       |
| 角度           | 弧度（rad）   |
| 时间           | 秒（s）       |
| 速率           | m/s           |
| 速度           | m/s           |
| 加速度         | m/s²          |
| 角速度         | rad/s         |
| 角加速度       | rad/s²        |

!!! warning ""

    如果消息中的某个字段使用上述默认单位，请不要添加表示其类型的后缀或前缀。

<a id="non-default-units"></a>

## 非默认单位

对于非默认单位，请使用以下后缀：

| 类型     | 非默认单位       | 后缀    |
| -------- | ---------------- | ------- |
| 距离     | 纳米             | `_nm`   |
| 距离     | 微米             | `_um`   |
| 距离     | 毫米             | `_mm`   |
| 距离     | 千米             | `_km`   |
| 角度     | 度（deg）        | `_deg`  |
| 时间     | 纳秒             | `_ns`   |
| 时间     | 微秒             | `_us`   |
| 时间     | 毫秒             | `_ms`   |
| 时间     | 分钟             | `_min`  |
| 时间     | 小时（h）        | `_hour` |
| 速度     | km/h             | `_kmph` |

!!! tip ""

    如果此处没有列出你想使用的单位，请[创建 issue 或 PR](https://github.com/autowarefoundation/autoware-documentation/issues)，将其添加到列表中。

<a id="message-field-types"></a>

## 消息字段类型

ROS 接口支持的类型列表[请见此处](https://docs.ros.org/en/humble/Concepts/About-ROS-Interfaces.html#field-types)。

为方便查阅，也复制如下：

| 消息字段类型       | 对应的 C++ 类型  |
| ------------------ | ---------------- |
| `bool`             | `bool`           |
| `byte`             | `uint8_t`        |
| `char`             | `char`           |
| `float32`          | `float`          |
| `float64`          | `double`         |
| `int8`             | `int8_t`         |
| `uint8`            | `uint8_t`        |
| `int16`            | `int16_t`        |
| `uint16`           | `uint16_t`       |
| `int32`            | `int32_t`        |
| `uint32`           | `uint32_t`       |
| `int64`            | `int64_t`        |
| `uint64`           | `uint64_t`       |
| `string`           | `std::string`    |
| `wstring`          | `std::u16string` |

<a id="arrays"></a>

### 数组

对于数组，请使用 `unbounded dynamic array` 类型。

示例：

```text
int32[] unbounded_integer_array
```

<a id="enumerations"></a>

## 枚举

ROS 2 接口不直接支持枚举。

可以定义整数常量，并将其赋给非常量整数参数。

!!! success ""

    常量使用 `CONSTANT_CASE` 命名。

!!! success ""

    为每个常量元素赋予不同的值。

_示例：_

```text
# Classification of error states in Autoware Localization
# Initialization rejected due to unsafe conditions
uint16 ERROR_UNSAFE = 1
# GNSS-based initialization not supported
uint16 ERROR_GNSS_SUPPORT = 2
# GNSS initialization failed
uint16 ERROR_GNSS = 3
# Pose estimation failed
uint16 ERROR_ESTIMATION = 4

# The type of state (required)
# e.g. 1(ERROR_UNSAFE)
uint16 type
```

!!! tip ""

    这些常量应在所属领域内互斥且完整覆盖所有情况，从而使建模清晰、减少歧义。

<a id="example-usages"></a>

## 使用示例

- 默认类型不使用单位后缀：
  - 错误：`float32 path_length_m`
  - 正确：`float32 path_length`
- 不要将单位放在前缀中：
  - 错误：`float32 kmph_velocity_vehicle`
  - 正确：`float32 velocity_vehicle_kmph`
- [如果表中提供了推荐后缀](#non-default-units)，请使用它：
  - 错误：`float32 velocity_vehicle_km_h`
  - 正确：`float32 velocity_vehicle_kmph`
