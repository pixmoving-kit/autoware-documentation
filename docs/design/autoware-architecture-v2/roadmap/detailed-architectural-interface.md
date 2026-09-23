<a id="detailed-architectural-interface"></a>

# 详细架构接口

<a id="requirements-and-considerations"></a>

## 要求与考量

为在 Autoware 开源项目中引入端到端自动驾驶技术，我们为新的 Autoware E2E 架构实现和软件接口设定了以下高层软件设计要求：

<a id="1-define-a-robust-consistent-interface-across-evolutionary-steps"></a>

### 1. 在各演进阶段定义稳健且一致的接口

如前文[自动驾驶软件栈架构](autonomous-driving-stack-architecture.md)所述，我们计划通过以下演进步骤实现端到端自动驾驶。

![分阶段技术演进](media/detailed_architecture_figure1.png)

<p align="center"><strong>图 1：</strong>分阶段技术演进</p>

我们希望避免在各阶段对核心接口进行颠覆性修改，确保用户无需大幅重写代码即可逐步采用新的端到端 AI 模块。此外，一些用户可能希望在特定场景中保留传统规则规划器，例如需要确定性规划的场景。通过保持接口一致，这些用户可以按需在端到端 AI 与传统方法之间便捷切换。

<a id="2-introduce-a-framework-for-minimum-safety-guarantees"></a>

### 2. 引入提供最低安全保障的框架

端到端 AI 模型的一个主要挑战是其黑盒性质，这使得验证生成轨迹是否安全、有效变得困难。为此，架构必须加入后处理阶段，对神经网络输出施加安全约束。

不同用户和应用的安全要求可能不同。因此，框架应便于定制，让开发者能够定义并执行针对具体领域的安全策略。

<a id="3-support-user-defined-behavior-preferences"></a>

### 3. 支持用户定义的行为偏好

在自动驾驶中，许多决策本身并没有唯一确定的答案。例如，变道时机往往取决于驾驶员偏好和具体情境，保持当前车道和变道都可能是有效选择。

某些 E2E 规划器，例如 Diffusion Drive，可以在这些场景中生成多条可行轨迹，但车辆最终必须确定一种驾驶操作。因此，架构必须提供让用户影响决策的机制，在存在多个有效选项时，让用户偏好指导行为选择。

<a id="generator-selector-framework"></a>

## 生成器—选择器框架

为灵活兼容这些不同方法，我们提出生成器—选择器框架。该框架包含两个核心组件：

- 生成器：生成车辆可跟踪的候选轨迹。
- 选择器：从候选轨迹中选出最安全、最优的一条。

![生成器—选择器框架](media/detailed_architecture_figure2.png)

<p align="center"><strong>图 2：</strong>生成器—选择器框架</p>

该框架能够统一处理采用不同程度端到端 AI 技术的自动驾驶架构，同时根据安全性和性能作出最终选择。

<a id="generator"></a>

### 生成器

生成器可以是任何能够生成车辆可跟踪候选轨迹的自动驾驶软件栈。例如：

- 传统机器人软件栈：可以原样复用现有 Autoware 软件栈。
- 专有 AI 模型：Autoware 基金会成员公司可以使用内部研发的模型
- 开源端到端 AI：Autoware E2E 模型

多个生成器可以并行执行，也可以根据情境选择性启用。

<a id="selector"></a>

### 选择器

选择器负责两项主要功能：

- 安全门控（安全保障）
  - 验证黑盒生成器（例如神经网络）的输出，确保最低安全水平。（例如，结合高精地图检查是否遵守交通信号。）
- 排序（轨迹评估与选择）
  - 评估多个生成器的输出并排序，然后选择最佳轨迹。
    - 示例：
      - 有高精地图时使用基于机器人技术的方法，否则回退到端到端 AI 模型
      - 根据安全、舒适或遵守规则等驾驶策略为轨迹评分

这些选择器功能以插件形式实现，允许开发者定制安全要求并加入偏好，为自己的使用场景选择合适轨迹。

<a id="reference-links"></a>

### 参考链接

- GitHub 讨论：
  - <https://github.com/orgs/autowarefoundation/discussions/5033>
  - <https://github.com/orgs/autowarefoundation/discussions/6301>
- 提议的架构接口：<https://github.com/tier4/new_planning_framework/wiki>
- 引入新生成器—选择器框架的迁移计划：[启用生成器—选择器规划框架 #6292](https://github.com/autowarefoundation/autoware.universe/pull/6292)
