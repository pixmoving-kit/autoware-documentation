<a id="what-belongs-in-autoware-core"></a>

# 哪些功能包适合纳入 Autoware Core

[Autoware Core](https://github.com/autowarefoundation/autoware_core) 是 Autoware 软件栈中稳定且经过质量保障的基础层。
本页说明功能包进入 Core 所需满足的要求，以及如何从 Universe 迁入 Core。

<a id="what-core-stands-for"></a>

## Core 的定位

Core 功能包构成 Autoware 的稳定基础：能够构建、经过测试、提供稳定接口，并由 Autoware 基金会维护。
Universe 则用于实验、厂商专用代码和快速发展的研究。

将功能包加入 Core 是一项长期承诺。
为支持所有基于 Core 构建系统的用户，该功能包必须持续接受测试、审查和版本发布。
只有达到以下标准，功能包才能纳入 Core。

!!! note "判断标准"

    如果将某个功能包交给没有 GPU、厂商专用软件栈或研究模型的新用户，它仍能构建、通过测试，并仅使用标准化接口通信，那么它适合纳入 Core。
    同时，应有足够多的生态组件依赖它，使基金会有必要保障其稳定性。
    唯一例外是共享 GPU 基础设施，它属于下文介绍的可选 CUDA 层。

<a id="requirements"></a>

## 要求

Core 功能包必须：

- **仅向下依赖。** 不依赖任何 Universe 功能包，只能依赖其他 Core 功能包、通过 rosdep 管理的 ROS 依赖，以及成熟的第三方库。（Universe 依赖 Core，不能反向依赖；参见[仓库结构](../repository-structure.md)图。）
- **使用厂商中立的接口。** 不使用专有功能包或带公司命名空间的功能包（例如 `tier4_*_msgs`）。通过标准消息和 `autoware_component_interface_specs` 通信。
- **默认构建仅使用 CPU。** 标准 Core 功能包不需要 GPU、CUDA 或 TensorRT。GPU 代码只能放在下文介绍的可选 CUDA 层中，不能混入默认功能包。
- **采用宽松许可证。** 使用 Apache 2.0 或兼容许可证，且不包含采用限制性许可证的运行时依赖。
- **通过测试并保持检查通过。** 单元测试、覆盖率检查及完整 Core CI 必须在所有受支持的发行版上通过（目前为 Humble 和 Jazzy）。Core 执行比 Universe 更严格的 clang-tidy 配置，因此仅通过 Universe 的检查还不够。
- **有人维护。** 具有明确的维护者，并在 `CODEOWNERS` 中列出；采用语义化版本，并通过 Core 发布。

功能包还应足够成熟：具有稳定且文档齐全的接口，经过单元测试之外的验证（最好包括实车验证），并具有广泛用途，而非仅针对某个厂商或研究项目。

<a id="getting-into-core"></a>

## 进入 Core 的流程

新工作从 Universe 开始。当功能包明确达到上述标准后，即可晋升到 Core。

晋升需要有计划地移植：清理依赖、将专有消息替换为标准消息，并补充测试。
应先迁移消息、工具和基础层等底层组件，使依赖这些组件的功能包后续迁移时不会出现破坏性问题。
晋升时不改变功能包名称，以保证下游代码继续工作。

<a id="the-cuda-layer"></a>

## CUDA 层

Core 以两种形式提供：默认的纯 CPU 功能包，以及可选的 GPU 层。GPU 层位于 `autoware_core_cuda` 仓库中，构建为独立的 CUDA 镜像。
需要 CUDA 或 TensorRT 的功能包也可以属于 Core，但只能放在这一层中，从而确保默认构建始终无需 GPU。

共享的 CUDA／TensorRT 基础设施（`autoware_tensorrt_common`、`autoware_tensorrt_plugins`、`autoware_cuda_utils`、`autoware_cuda_dependency_meta`）位于此处。
它满足 Core 的质量标准：仅依赖这些功能包、Core 工具、ROS 和一个外部 CMake 模块，不依赖任何 Universe 内容；许多下游 GPU 功能包都基于它构建。
