<a id="manual-control"></a>

# 手动控制

<a id="description"></a>

## 说明

为操作没有方向盘等驾驶操纵装置的车辆，或开发远程操作系统，需要提供通过 Autoware 手动驾驶车辆的接口。

<a id="requirements"></a>

## 要求

- 发生通信问题时，车辆能够转入安全状态。
- 支持以下指令和状态。
  - 用于纵向控制的踏板、加速度或速度
  - 用于横向控制的转向轮转角
  - 挡位
  - 转向灯
  - 危险警告灯
- 以下功能正在考虑中。
  - 前照灯
  - 雨刷
  - 驻车制动
  - 喇叭

<a id="sequence"></a>

## 时序

```plantuml
{% include 'design/autoware-architecture-v1/interfaces/ad-api/use-cases/manual-control/sequence.plantuml' %}
```

<a id="related-features"></a>

## 相关功能

- [手动控制](../../features/manual-control.md)
- [车辆状态](../../features/vehicle-status.md)
