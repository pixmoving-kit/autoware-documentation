<a id="user-story-of-bus-service"></a>

# 公交服务用户故事

<a id="overview"></a>

## 概述

此用户故事描述按指定站点循环运行的公交服务。

<a id="scenario"></a>

## 场景

| 步骤 | 操作 | 用例 |
| ---- | ---------------------------------------------------------- | ----------------------------------------------------------------------------- |
| 1 | 启动自动驾驶系统。 | [启动与终止](../use-cases/launch-terminate.md) |
| 2 | 将车辆从车库驶至等候位置。 | [切换运行模式](../use-cases/change-operation-mode.md) |
| 3 | 启用自主控制。 | [切换运行模式](../use-cases/change-operation-mode.md) |
| 4 | 将车辆驶向下一公交站点。 | [驶向指定位置](../use-cases/drive-designated-position.md) |
| 5 | 乘客上车和下车。 | [上车与下车](../use-cases/get-on-off.md) |
| 6 | 若尚未到达终点站，则返回步骤 4。 | |
| 7 | 将车辆驶至等候位置。 | [驶向指定位置](../use-cases/drive-designated-position.md) |
| 8 | 将车辆从等候位置驶回车库。 | [切换运行模式](../use-cases/change-operation-mode.md) |
| 9 | 关闭自动驾驶系统。 | [启动与终止](../use-cases/launch-terminate.md) |
