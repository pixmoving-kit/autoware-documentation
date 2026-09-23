<a id="operation-mode"></a>

# 运行模式

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/operation_mode/state') }}
- {{ link_ad_api('/api/operation_mode/change_to_autonomous') }}
- {{ link_ad_api('/api/operation_mode/change_to_stop') }}
- {{ link_ad_api('/api/operation_mode/change_to_local') }}
- {{ link_ad_api('/api/operation_mode/change_to_remote') }}
- {{ link_ad_api('/api/operation_mode/enable_autoware_control') }}
- {{ link_ad_api('/api/operation_mode/disable_autoware_control') }}

<a id="description"></a>

## 说明

如下图所示，Autoware 假定车辆接口具有两种模式：Autoware 控制和直接控制。
在直接控制模式下，通过方向盘和踏板等设备操作车辆。
如果车辆不支持直接控制模式，则始终将其视为 Autoware 控制模式。
Autoware 控制模式下有四种运行模式。

| 模式 | 说明 |
| ---------- | ----------------------------------------------------------------------------- |
| Stop | 保持车辆停止。 |
| Autonomous | 自主控制车辆。 |
| Local | 在车辆附近，使用操纵杆等设备手动控制车辆。 |
| Remote | 通过云端 Web 应用手动控制车辆。 |

![运行模式架构](operation_mode/architecture.drawio.svg)

<a id="states"></a>

## 状态

<a id="autoware-control-flag"></a>

### Autoware 控制标志

`is_autoware_control_enabled` 标志表示车辆是否由 Autoware 控制。
如果能够通过软件切换控制方式，就可以使用 enable 和 disable API。
如果车辆不支持模式切换，或只能通过硬件切换，这些 API 将始终调用失败。

<a id="operation-mode-and-change-flags"></a>

### 运行模式与切换标志

`operation_mode` 状态表示启用 Autoware 控制后使用哪一类命令。
可以使用 `change_to_*` 标志检查是否能够切换到各个模式。

<a id="transition-flag"></a>

### 切换过程标志

在某些情况下，例如超速时切换到自动驾驶模式，Autoware 可能无法保证安全。
为此提供了 `is_in_transition` 标志，切换模式期间该标志为 true。
发起模式切换的操作员应在该标志为 true 时负责确保安全。模式切换完成后，该标志变为 false。
