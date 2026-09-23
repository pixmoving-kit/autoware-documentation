<a id="quick-start-demos"></a>

# 快速入门演示

<a id="digital-twin-demos-end-to-end-simulation"></a>

## 数字孪生演示（端到端仿真）

[:fa-cl-s fa-circle-arrow-right: AWSIM 完整演示](digital-twin-demos/awsim-tutorial.md){ .md-button } [:fa-cl-s fa-circle-arrow-right: AWSIM 与 Autoware Core 演示](digital-twin-demos/autoware-core-awsim/index.md){ .md-button }

???+ abstract "概述"

    <div class="grid cards" markdown>

    -   **测试内容：**

        ---

        - 所有组件（演示中涉及的组件）
        - 可测试特定组件（取决于配置）

    -   **仿真内容：**

        ---

        - 传感器（激光雷达、相机、GNSS/INS 等）
        - 车辆动力学
        - NPC（非玩家角色，即其他道路使用者和障碍物）

    </div>

<a id="planning-simulation-demo"></a>

## 规划仿真演示

[:fa-cl-s fa-circle-arrow-right: 规划仿真演示](planning-sim/index.md){ .md-button }

???+ abstract "概述"

    <div class="grid cards" markdown>

    -   **测试内容：**

        ---

        - 规划组件
        - 控制组件

    -   **仿真内容：**

        ---

        - 感知输出（包围框）
          - 可放置虚拟目标，并模拟其简单运动
          - 交通信号灯输出
        - 定位输出
          - 可将自车放置在地图上的任意位置
        - 地图输出
          - 可测试 Lanelet2 地图的有效性

    </div>

<a id="rosbag-replay-simulation-demo"></a>

## Rosbag 回放仿真演示

[:fa-cl-s fa-circle-arrow-right: Rosbag 回放仿真演示](rosbag-replay-simulation.md){ .md-button }

???+ abstract "概述"

    <div class="grid cards" markdown>

    -   **测试内容：**

        ---

        - 传感组件（演示中涉及的组件）
        - 感知组件（演示中涉及的组件）
        - 定位组件（演示中涉及的组件）
        - 其他组件（取决于录制的数据）

    -   **回放内容：**

        ---

        - 左侧、右侧及顶部激光雷达输出（演示中涉及的数据）
        - GNSS/INS 数据（演示中涉及的数据）
        - 车辆状态（演示中涉及的数据）
        - 其他数据（取决于录制的数据）

    </div>

<a id="scenario-simulator-v2-demo"></a>

## Scenario Simulator v2 演示

[:fa-cl-s fa-circle-arrow-right: Scenario Simulator v2 演示](scenario-simulation/scenario-simulator/installation.md){ .md-button }

???+ abstract "概述"

    <div class="grid cards" markdown>

    -   **测试内容：**

        ---

        - 规划组件
        - 控制组件

    -   **仿真内容：**

        ---

        - 感知输出（包围框）
          - 可放置虚拟目标，并模拟其简单运动
          - 交通信号灯输出
        - 定位输出
          - 可将自车放置在地图上的任意位置
        - 地图输出
          - 可测试 Lanelet2 地图的有效性

    </div>
