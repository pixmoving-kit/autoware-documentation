<a id="autoware-concepts"></a>

# Autoware 概念

Autoware 的理念是提供开放、灵活的平台，加速自动驾驶系统的开发与部署。以下详细介绍其关键原则。

**另请参阅：**

- [开源理念](../../contributing/open-source-philosophy.md)
- [Autoware 系统能力](../autoware-system-capabilities.md)

<a id="microautonomy-architecture-conceptual-overview"></a>

## 微自主架构：概念概述

**微自主架构（Microautonomy architecture）**是 Autoware 将“自动驾驶”拆分为许多小型、可替换能力的方式，
而非构建单一的整体式软件栈。
每项能力（例如目标检测、行为规划、车道级路线规划）都是具有明确输入和
输出的模块，因此可像搭积木一样，为不同车辆和使用场景组合系统。

!!! question "什么是微自主架构？"

    它是一种**基于组件的自主系统设计**，通过_组合多个小型自主模块_构建驾驶行为，而非依赖单一、固定的流水线。
    因此，无需重写整个系统即可方便地组合、替换和升级各部分。

Autoware 的模块通过明确的接口连接，使你能够**替换或扩展单个
组件**，同时保持系统其他部分不变。例如，可以将默认目标检测替换为
专门检测施工锥桶的自定义神经网络，而下游的跟踪、规划和控制模块
仍可照常工作。

!!! example "可组合性示例"

    - 从默认感知流水线开始
    - 接入面向特殊目标（例如锥桶、叉车）的专用检测器
    - 保持相同的规划器和控制器。它们只接收“目标”，无需关心目标的检测方式

总体而言，这些接口分为两类：

- **内部组件接口**连接 Autoware 内部模块（例如感知 → 规划 → 控制）。
- **外部 AD API**向外部系统开放 Autoware 的能力（例如车队管理、云服务、
  信息娱乐系统）。

!!! success "对开发者与合作伙伴的价值"

    - 可以**复用** Autoware 核心模块，只定制产品特有的部分。
    - 可以**逐步演进**软件栈，每次替换一个模块。
    - 合作伙伴可以**围绕共享、稳定的接口协作**，贡献能够接入共同
      生态系统的组件。

<a id="core-universe-repository-model"></a>

## Core 与 Universe 仓库模式

![Autoware 生态系统](images/autoware_ecosystem.png)

Autoware 的软件生态分为两层：**Autoware Core** 和 **Autoware Universe**。
两者共同兼顾**质量保障**与**社区驱动的创新**。

<a id="autoware-core-the-quality-assured-base"></a>

### Autoware Core：经过质量保障的基础层

[**Autoware Core**](https://github.com/autowarefoundation/autoware_core) 包含由 Autoware 基金会（AWF）维护的基础功能包。
这些功能包遵循严格的开发标准，包括单元测试、集成测试、性能验证和实车测试。
Core 是用户构建自动驾驶系统时可以信赖的**稳定、可用于生产环境的平台**。

!!! success "Core 提供："

    - 经过审查、可靠的基础
    - 一致的 API 和行为
    - 持续维护、经过测试且具有版本管理的发布

<a id="autoware-universe-the-community-innovation-layer"></a>

### Autoware Universe：社区创新层

[**Autoware Universe**](https://github.com/autowarefoundation/autoware_universe) 是更广泛的开源功能包集合，由个人、公司和研究团队贡献。
这些功能包由原作者拥有和维护，并采用各自的质量要求和开发实践。

贡献可以采用两种形式：

- 直接合入由 AWF 托管的 Universe 仓库，或
- 托管在外部，并作为 Universe 生态的一部分列出。

Universe 是**用于实验的空间**，让新想法、算法和硬件适配成果能够快速共享。

!!! example "Universe 支持："

    - 快速原型开发和实验
    - 共享专用模块
    - 为全球社区贡献提供入口

当 Autoware Universe 中有潜力的功能包展现出足够的成熟度、稳定性和实用性时，可被**纳入 Autoware Core**。
由此形成从创新 → 标准化 → 生产应用的自然演进流程。
功能包必须满足的具体要求见[哪些功能包适合纳入 Autoware Core](core-package-inclusion-criteria.md)。

!!! info

    更多详情请参阅[🔗 仓库结构](../repository-structure.md)文档。
