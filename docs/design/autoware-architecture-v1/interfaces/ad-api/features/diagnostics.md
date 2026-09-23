<a id="diagnostics"></a>

# 诊断

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/system/diagnostics/struct') }}
- {{ link_ad_api('/api/system/diagnostics/status') }}
- {{ link_ad_api('/api/system/diagnostics/reset') }}

<a id="description"></a>

## 说明

此 API 提供诊断图，其中包含 Autoware 各功能单元的错误级别。
系统根据配置将功能划分为任意单元，并诊断其错误级别。
各功能单元之间存在依赖关系，因此整体结构类似故障树分析（FTA）。
实际上，多个父节点可能共享同一个子节点，因此形成的是有向无环图（DAG）。
下图是此 API 提供的诊断示例。
图中的 `path` 是描述功能单元的任意字符串，`level` 是其错误级别。
错误级别采用与 `diagnostic_msgs/msg/DiagnosticStatus` 相同的取值。

![诊断图树形结构](diagnostics/tree.drawio.svg)

诊断数据包含静态部分和动态部分，为提高效率，API 分别提供这两部分。
下图给出了与上述图示对应的消息示例。
诊断的静态部分以 DiagGraphStruct 形式仅发布一次，其中包含 nodes、diags 和 links。
links 通过节点数组中的索引指定节点之间的依赖关系。
诊断的动态部分以 DiagGraphStatus 形式定期发布。
status 中 nodes 和 diags 的长度与 struct 中相同，相同索引代表同一个功能单元。

![诊断图数据](diagnostics/data.drawio.svg)

某些功能单元的级别可能被锁存。如果异常持续一定时间，级别值将不再自动恢复正常。
使用 input_level 可获取锁存前的级别；使用 latch_level 可判断当前是否已触发锁存。
锁存触发后，如需恢复级别，请使用 reset API。

![诊断级别](diagnostics/level.drawio.svg)
