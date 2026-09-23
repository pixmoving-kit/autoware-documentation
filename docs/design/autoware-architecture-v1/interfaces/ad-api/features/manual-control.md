<a id="manual-control"></a>

# 手动控制

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/manual/remote/control_mode/list') }}
- {{ link_ad_api('/api/manual/remote/control_mode/select') }}
- {{ link_ad_api('/api/manual/remote/control_mode/status') }}
- {{ link_ad_api('/api/manual/remote/operator/heartbeat') }}
- {{ link_ad_api('/api/manual/remote/command/pedals') }}
- {{ link_ad_api('/api/manual/remote/command/acceleration') }}
- {{ link_ad_api('/api/manual/remote/command/velocity') }}
- {{ link_ad_api('/api/manual/remote/command/steering') }}
- {{ link_ad_api('/api/manual/remote/command/gear') }}
- {{ link_ad_api('/api/manual/remote/command/turn_indicators') }}
- {{ link_ad_api('/api/manual/remote/command/hazard_lights') }}
- {{ link_ad_api('/api/manual/local/control_mode/list') }}
- {{ link_ad_api('/api/manual/local/control_mode/select') }}
- {{ link_ad_api('/api/manual/local/control_mode/status') }}
- {{ link_ad_api('/api/manual/local/operator/heartbeat') }}
- {{ link_ad_api('/api/manual/local/command/pedals') }}
- {{ link_ad_api('/api/manual/local/command/acceleration') }}
- {{ link_ad_api('/api/manual/local/command/velocity') }}
- {{ link_ad_api('/api/manual/local/command/steering') }}
- {{ link_ad_api('/api/manual/local/command/gear') }}
- {{ link_ad_api('/api/manual/local/command/turn_indicators') }}
- {{ link_ad_api('/api/manual/local/command/hazard_lights') }}

<a id="description"></a>

## 说明

此 API 用于手动控制车辆，并为远程和本地两类操作员提供相同接口。
例如，本地操作员使用操纵杆控制没有驾驶座的车辆；远程操作员则在自动驾驶出现问题时提供远程支持。
当[运行模式](operation_mode.md)为 remote 或 local 时，会使用发送的命令。

<a id="operator-status"></a>

## 操作员状态

应用需要判断操作员是否具备驾驶能力，并通过操作员状态 API 发送这一信息。
手动操作期间，如果操作员无法继续驾驶，Autoware 将执行 MRM，使车辆进入安全状态。
对于 L3 及以下级别，即使在自动驾驶期间也会参考操作员状态。

<a id="control-mode"></a>

## 控制模式

由于可以通过踏板或加速度等多种方式控制车辆，应用必须先选择控制模式。

| 模式 | 说明 |
| ------------ | -------------------------------------------------------------------------- |
| disabled | 初始模式。选择此模式时，所有命令 API 均不可用。 |
| pedals | 使用踏板进行纵向控制。 |
| acceleration | 使用目标加速度进行纵向控制。 |
| velocity | 使用目标速度进行纵向控制。 |

<a id="commands"></a>

## 命令

各模式下可用的命令如下。

| 命令 | disabled | pedals | acceleration | velocity |
| --------------- | :------: | :------: | :----------: | :------: |
| pedals          |    -     | &#x2713; |      -       |    -     |
| acceleration    |    -     |    -     |   &#x2713;   |    -     |
| velocity        |    -     |    -     |      -       | &#x2713; |
| steering        |    -     | &#x2713; |   &#x2713;   | &#x2713; |
| gear            |    -     | &#x2713; |   &#x2713;   | &#x2713; |
| turn_indicators |    -     | &#x2713; |   &#x2713;   | &#x2713; |
| hazard_lights   |    -     | &#x2713; |   &#x2713;   | &#x2713; |
