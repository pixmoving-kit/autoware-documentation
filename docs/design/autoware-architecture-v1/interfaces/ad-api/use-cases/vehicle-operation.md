<a id="vehicle-operation"></a>

# 车辆操作

<a id="request-to-intervene"></a>

## 接管请求

接管请求（RTI）要求操作员切换到手动驾驶模式，也称 Take Over Request（TOR）。
RTI 接口目前仍在讨论中。现阶段可以认为，当 MRM 状态不是 NORMAL 时，即请求手动驾驶。
详情请参阅[故障安全](../features/fail-safe.md)。

<a id="request-to-cooperate"></a>

## 协作请求

协作请求（RTC）允许操作员在自动驾驶模式下辅助决策。
Autoware 通常根据自身决策驾驶车辆，但在复杂情况下，操作员可能希望自行作出决定。
RTC 只覆盖决策，无需切换操作模式，因此车辆可以继续自动驾驶，与接管请求不同。
详情请参阅[协作](../features/cooperation.md)。
