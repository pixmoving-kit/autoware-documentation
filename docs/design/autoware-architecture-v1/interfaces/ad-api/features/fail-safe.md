<a id="fail-safe"></a>

# 故障安全

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/fail_safe/rti_state') }}
- {{ link_ad_api('/api/fail_safe/list_mrm_description') }}
- {{ link_ad_api('/api/fail_safe/mrm_state') }}
- {{ link_ad_api('/api/fail_safe/mrm_request/send') }}
- {{ link_ad_api('/api/fail_safe/mrm_request/list') }}

<a id="description"></a>

## 说明

此 API 管理与车辆异常相关的行为。
它提供接管请求（RTI）、最小风险操作（MRM）和最小风险状态（MRC）的状态信息。
如下图所示，Autoware 通过门控模块在正常运行命令与异常运行命令之间切换。
出于安全考虑，Autoware 在检测到异常时会切换到 MRM。
不同情境所需的行为不同，因此 MRM 可在各处实现为常规模块中的特定模式，也可实现为独立模块。
故障安全模块根据异常情况选择 MRM 行为，并将门控输出切换为对应命令。

![故障安全架构](fail-safe/architecture.drawio.svg)

<a id="rti-state"></a>

## RTI 状态

RTI 状态表示是否已发出接管请求。如果因某种原因无法继续自动驾驶，Autoware 会请求切换为人工驾驶。RTI 有时也称为 Take Over Request（TOR）。

<a id="mrm-description"></a>

## MRM 说明

Autoware 支持多种 MRM 实现，为不同使用场景提供适当行为。
因此，需要 MRM 行为详细信息时，请使用此 API。该 API 列出的 MRM 行为 ID 会用于 MRM 状态 API。
为保持向后兼容性，MRM 行为表中列出的值予以保留。

<a id="mrm-state"></a>

## MRM 状态

MRM 状态表示 MRM 是否正在运行，以及当前执行的行为。
此状态还提供操作成功或失败的信息。通常，MRM 执行失败时会切换到其他行为。

![MRM 状态](fail-safe/mrm-state.drawio.svg)

| 状态 | 说明 |
| --------- | ---------------------------------------------------------- |
| NONE | MRM 未运行。 |
| OPERATING | 检测到异常，MRM 正在运行。 |
| SUCCEEDED | MRM 成功。车辆处于安全状态。 |
| FAILED | MRM 失败。车辆仍处于不安全状态。 |

**[v1.9.0] 已弃用：请使用 MRM 行为 ID 替代以下常量。**
MRM 行为之间存在依赖关系。例如，可以从舒适停车切换为紧急停车，但不能反向切换。
具体取决于所提供的服务。Autoware 默认支持以下转换。

![MRM 行为](fail-safe/mrm-behavior.drawio.svg)

| 状态 | 值 | 说明 |
| ---------------- | ----- | ------------------------------------------------------------------------- |
| NONE | 1 | MRM 未运行，或正在运行但无需特殊行为。 |
| EMERGENCY_STOP | 2 | 车辆以尽可能大的减速度立即停车。 |
| COMFORTABLE_STOP | 3 | 车辆以舒适的减速度迅速停车。 |
| PULL_OVER | 4 | 车辆移至路边后停车。 |

<a id="mrm-request"></a>

## MRM 请求

MRM 请求允许应用触发 MRM，主要用于应用检测到异常后，希望将车辆转入安全状态的情况。发送 MRM 请求后，Autoware 会尝试执行 MRM。为区分来自多个应用的请求，MRM 请求必须包含用户名作为标识。

由于 MRM 包含多种行为，此功能提供了选择策略。支持的策略请参阅下表。
存在多个请求时，将采用优先级最高的策略。
注意，Autoware 检测到异常时，可能无法按照 MRM 请求中的策略执行。

| 策略 | 说明 |
| -------- | ---------------------------------------------------------- |
| CANCEL | 取消与指定用户关联的 MRM 请求。 |
| DELEGATE | 将 MRM 行为的选择交由 Autoware 处理。 |
