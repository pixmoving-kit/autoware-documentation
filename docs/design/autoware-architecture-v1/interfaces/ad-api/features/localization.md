<a id="localization"></a>

# 定位

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/localization/initialization_state') }}
- {{ link_ad_api('/api/localization/initialize') }}

<a id="description"></a>

## 说明

此 API 管理定位初始化。Autoware 需要一个全局位姿作为定位的初始估计。

<a id="states"></a>

## 状态

![定位初始化状态](localization/state.drawio.svg)

| 状态         | 说明                                                                      |
| ------------- | -------------------------------------------------------------------------------- |
| UNINITIALIZED | 定位尚未初始化，正在等待用作初始估计的全局位姿。 |
| INITIALIZING  | 定位正在初始化。                                                    |
| INITIALIZED   | 定位已初始化。必要时可以再次请求初始化。 |
