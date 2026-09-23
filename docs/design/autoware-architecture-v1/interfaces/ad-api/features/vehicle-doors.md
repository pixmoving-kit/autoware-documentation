<a id="vehicle-doors"></a>

# 车门

<a id="related-api"></a>

## 相关 API

- {{ link_ad_api('/api/vehicle/doors/layout') }}
- {{ link_ad_api('/api/vehicle/doors/status') }}
- {{ link_ad_api('/api/vehicle/doors/command') }}

<a id="description"></a>

## 说明

如果车辆提供车门的软件接口，即可使用此功能。
可以用它创建面向乘客的用户界面，或控制公交站点的操作流程。

<a id="layout"></a>

## 布局

车辆的每扇门都分配有一个数组索引，具体分配方式取决于车辆。
layout API 返回这些信息。
description 字段是用于用户界面显示等用途的字符串。
该字符串可任意设置，因此不建议应用将其用于处理逻辑。
使用 roles 字段识别用于上车和下车的车门。
以下是 layout API 返回信息的示例。

| 索引 | 描述 | 角色 |
| ----- | ----------- | --------------- |
| 0 | 右前 | - |
| 1 | 左前 | GET_ON |
| 2 | 右后 | GET_OFF |
| 3 | 左后 | GET_ON, GET_OFF |

<a id="status"></a>

## 状态

status API 提供车门状态数组，其顺序与 layout API 一致。

<a id="control"></a>

## 控制

使用 command API 控制车门。
与 status 和 layout API 不同，此处的数组索引不与车门一一对应。
命令中有专门字段用于指定目标车门索引。
