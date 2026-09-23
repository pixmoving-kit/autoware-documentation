<a id="autoware-documentation"></a>

# Autoware 文档

**Autoware** 是全球领先的开源自动驾驶框架。Autoware 提供全面、可用于生产环境的软件栈，旨在加速自动驾驶车辆在不同平台和应用场景中的**商业化部署**。

<div style="text-align: center;">
<iframe
  width="800"
  height="450"
  src="https://www.youtube.com/embed/7XP5Pq11Yi8?si=je1235R4ZFayw7cj&controls=0&autoplay=1&mute=1&loop=1&playlist=7XP5Pq11Yi8"
  title="YouTube 视频播放器"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  referrerpolicy="strict-origin-when-cross-origin"
  allowfullscreen>
</iframe>
</div>

---

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-circle-down: 安装 Autoware](../installation/index.md){ .md-button style="margin: 5px" }
[:fa-cl-s fa-gamepad: 快速入门演示](../demos/index.md){ .md-button style="margin: 5px" }
[:fa-cl-s fa-book: 教程](../tutorials/index.md){ .md-button style="margin: 5px" }
[:fa-cl-s fa-compass-drafting: 架构设计](../design/index.md){ .md-button style="margin: 5px" }
[:fa-cl-s fa-circle-question: 支持](../community/support/index.md){ .md-button style="margin: 5px" }
[:fa-cl-s fa-circle-plus: 参与贡献](../contributing/index.md){ .md-button style="margin: 5px" }

</div>

---

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-cubes: Autoware 索引（社区软件包）<span class="aw-badge-new">新增</span>](../installation/autoware/autoware-index.md){ .md-button .md-button--primary style="margin: 5px" }

</div>

---

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-gem: Autoware Core 文档](https://autowarefoundation.github.io/autoware_core/main/){ .md-button style="margin: 5px" }
[:fa-cl-s fa-star: Autoware Universe 文档](https://autowarefoundation.github.io/autoware_universe/main/){ .md-button style="margin: 5px" }
[:fa-cl-s fa-screwdriver-wrench: Autoware Tools 文档](https://autowarefoundation.github.io/autoware_tools/main/){ .md-button style="margin: 5px" }
[:fa-cl-s fa-house: Autoware 主站](https://autoware.org/autoware-overview){ .md-button style="margin: 5px" }

</div>

---

<a id="capabilities"></a>

## 核心能力

Autoware 覆盖从传感到控制的完整自动驾驶软件栈。

| 领域 | 主要功能 |
| :------------------- | :------------------------------------------------------------------------------------------------------------- |
| **🤖 人工智能与学习** | 支持**端到端（E2E）驾驶模型**、基于机器学习的感知，以及数据驱动的轨迹预测。 |
| **👁️ 感知** | **多传感器融合**（激光雷达、相机、毫米波雷达）、交通信号灯识别和动态目标跟踪。 |
| **📍 定位** | 高精度 **NDT 匹配**结合 GNSS/IMU 里程计，在已建图环境中实现稳健定位。 |
| **🧠 规划** | 面向交叉路口和变道的**行为规划**，以及实时**动态障碍物避让**。 |
| **🚗 控制** | 精确的**轨迹跟踪**，以及适用于线控平台的标准化车辆接口。 |
| **🎮 仿真** | 用于验证的**数字孪生仿真**（AWSIM）、Rosbag 回放和场景仿真。 |

<a id="validated-use-cases"></a>

## 已验证的应用场景

Autoware 的设计不依赖特定平台，可支持以下应用：

- **自动驾驶出租车**与城市出行
- **货物配送**与物流
- **自动驾驶巴士**接驳服务
- **私家车（POV）**
