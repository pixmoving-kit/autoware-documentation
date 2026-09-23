<a id="routing"></a>

# 路线规划

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/routing/state') }}
- {{ link_ad_api('/api/routing/route') }}
- {{ link_ad_api('/api/routing/clear_route') }}
- {{ link_ad_api('/api/routing/set_route_points') }}
- {{ link_ad_api('/api/routing/set_route') }}
- {{ link_ad_api('/api/routing/change_route_points') }}
- {{ link_ad_api('/api/routing/change_route') }}

<a id="description"></a>

## 说明

此 API 管理目的地和途经点。注意，途经点只表示经过的位置，并不代表停车点。
换言之，Autoware 不支持包含多个停车点的路线，应用需要将其拆分并依次切换。
设置路线有两种方式：一种是使用位姿的通用方法，另一种则依赖具体地图。

<a id="states"></a>

## 状态

![路线状态](routing/state.drawio.svg)

| 状态 | 说明 |
| -------- | -------------------------------------------------- |
| UNSET | 尚未设置路线，等待路线请求。 |
| SET | 已设置路线。 |
| ARRIVED | 车辆已到达目的地。 |
| CHANGING | 正在尝试更改路线。 |

<a id="options"></a>

## 选项

路线设置和更改 API 提供路线选项，允许应用选择与路线规划相关的不同行为。
支持的选项及详情请参阅以下章节。

### allow_goal_modification

**[v1.1.0]** 当目标位置不可达时（例如指定目标位置上存在障碍物），Autoware 会尝试寻找替代目标。应用通过 API 设置路线时，可以选择是否允许 Autoware 在这种情况下调整目标位姿。如果设为 false，Autoware 可能会停滞，直到指定目标位置重新变得可达。
