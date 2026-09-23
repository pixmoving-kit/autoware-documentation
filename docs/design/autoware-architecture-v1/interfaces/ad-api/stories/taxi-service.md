<a id="user-story-of-bus-service"></a>

# 出租车服务用户故事

<a id="overview"></a>

## 概述

此用户故事描述接载乘客并将其送至目的地的出租车服务。

<a id="scenario"></a>

## 场景

| 步骤 | 操作 | 用例 |
| ---- | ---------------------------------------------------------- | ----------------------------------------------------------------------------- |
| 1 | 启动自动驾驶系统。 | [启动与终止](../use-cases/launch-terminate.md) |
| 2 | 将车辆从车库驶至等候位置。 | [切换运行模式](../use-cases/change-operation-mode.md) |
| 3 | 启用自主控制。 | [切换运行模式](../use-cases/change-operation-mode.md) |
| 4 | 将车辆驶至接客位置。 | [驶向指定位置](../use-cases/drive-designated-position.md) |
| 5 | 乘客上车。 | [上车与下车](../use-cases/get-on-off.md) |
| 6 | 将车辆驶至目的地。 | [驶向指定位置](../use-cases/drive-designated-position.md) |
| 7 | 乘客下车。 | [上车与下车](../use-cases/get-on-off.md) |
| 8 | 将车辆驶至等候位置。 | [驶向指定位置](../use-cases/drive-designated-position.md) |
| 9 | 若有新的请求，则返回步骤 4。 | |
| 10 | 将车辆从等候位置驶回车库。 | [切换运行模式](../use-cases/change-operation-mode.md) |
| 11 | 关闭自动驾驶系统。 | [启动与终止](../use-cases/launch-terminate.md) |
