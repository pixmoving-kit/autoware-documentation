<a id="how-is-autoware-coreuniverse-different-from-autowareai-and-autowareauto"></a>

# Autoware Core/Universe 与 Autoware.AI、Autoware.Auto 有什么不同？

Autoware 是全球首个面向自动驾驶车辆的“一体化”开源软件。
自 2015 年首次发布以来，Autoware 已推出多个基于不同理念的版本，每个版本都旨在改进软件。

## Autoware.AI

[Autoware.AI](https://github.com/Autoware-AI/autoware.ai) 是首个基于 ROS 1 发布的 Autoware 发行版。该仓库包含覆盖自动驾驶技术各方面的多种功能包，包括传感器处理、执行、定位、建图、感知和规划。

虽然它成功吸引了众多开发者和贡献，但由于以下原因，进一步提升 Autoware.AI 的能力比较困难：

- 缺少具体架构设计，积累了大量技术债务，例如模块之间耦合紧密、职责不清。
- 各功能包采用不同的编码标准，测试覆盖率很低。

此外，项目未明确定义搭载 Autoware 的自动驾驶车辆可以在哪些条件下运行，也未明确支持的用例或场景，例如是否能绕过静止车辆。

基于 Autoware.AI 开发中的经验教训，Autoware.Auto 采用了不同的开发流程，开发基于 ROS 2 的 Autoware 版本。

!!! warning

    Autoware.AI 目前处于维护模式，将于 2022 年底结束生命周期。

## Autoware.Auto

[Autoware.Auto](https://gitlab.com/autowarefoundation/autoware.auto/AutowareAuto) 是第二个基于 ROS 2 发布的 Autoware 发行版。在转向 ROS 2 的过程中，项目决定不直接将 Autoware.AI 从 ROS 1 移植到 ROS 2，而是从头重写代码库，采用规范的工程实践，包括定义目标用例和 ODD（例如自主代客泊车 [AVP]、货物配送等）、合理设计架构、编写设计文档和测试代码。

Autoware.Auto 开发初期进展顺利，但在完成 AVP 和货物配送 ODD 项目后，逐渐出现了以下问题：

- 新工程师的参与门槛过高。
  - 将新功能合入 Autoware.Auto 需要大量工作，因此研究人员和学生难以参与开发。
  - 结果，大多数 Autoware.Auto 开发者都来自 Autoware 基金会成员公司，能够将研究论文中的前沿功能引入项目的人很少。
- 大规模架构变更过于困难。
  - 为尝试实验性架构，既要保持主分支稳定，又要确保每项变更满足持续集成要求，开销很大。

## Autoware Core/Universe

为解决 Autoware.Auto 开发中的问题，Autoware 基金会决定创建名为 Autoware Core/Universe 的新架构。

Autoware Core 继承 Autoware.Auto 的原有方针，保持稳定且经过充分测试的代码库。同时引入 Autoware Universe 这一新概念，作为 Core 的扩展，带来以下优势：

- 用户可以方便地用 Universe 中的对应组件替换 Core 组件，从而使用新的定位或感知算法等更先进功能。
- Universe 的代码质量要求相对宽松，便于新开发者、学生和研究人员贡献，但仍严于 Autoware.AI 的要求。
- Universe 中对更广泛 Autoware 社区有价值的先进功能，会经过审查，并考虑纳入主要的 Autoware Core 代码库。

这样既能满足自动驾驶系统稳定、安全的首要要求，也能同时使用第三方贡献者开发的前沿功能。有关 Autoware Core/Universe 设计的更多信息，请参阅 [Autoware 概念文档](../autoware-concepts/index.md)。
