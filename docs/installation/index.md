<a id="installation"></a>

# 安装

<a id="target-platforms"></a>

## 目标平台

Autoware 官方支持以下平台。
后续发行版可能增加对其他平台的支持。

<a id="architecture"></a>

### 架构

- amd64
- arm64

<a id="minimum-hardware-requirements"></a>

### 最低硬件要求

!!! info

    Autoware 具有可扩展性，可以通过定制适配分布式硬件或性能较低的硬件。
    以下最低硬件要求仅作为一般建议。
    更多 CPU 核心、更大内存以及更高规格的显卡或 GPU 核心可提升性能。

    运行基本功能不需要 GPU，但启用以下与神经网络有关的功能时必须使用 GPU：
        - 基于激光雷达的目标检测
        - 基于相机的目标检测
        - 交通信号灯检测与分类

- 8 核 CPU
- 16GB 内存
- [可选] NVIDIA GPU（4GB 显存）

有关如何在没有 GPU 的情况下启用目标检测及交通信号灯检测与分类，请参阅[不使用 CUDA 运行 Autoware](../tutorials/others/running-autoware-without-cuda.md)。

<a id="installing-autoware"></a>

## 安装 Autoware

根据使用场景和经验水平，您可以选择不同的 Autoware 安装方式。还可以选择仅包含核心必需软件包的 Autoware Core，或包含完整 Autoware 软件栈的 Autoware Universe。
各类安装方式见下文。如果安装期间遇到问题，请参阅[支持页面](../community/support/index.md)。

???+ abstract "概述"

    <div class="grid cards" markdown>

    -   **Autoware Core**

        ---

        - 仅包含 Autoware 必需的软件包
        - 提供车道行驶和障碍物检测的基本功能
        - 不依赖 NVIDIA
        - 暂不使用神经网络进行目标检测

    -   **Autoware Universe** ⭐

        ---

        - 包含完整的 Autoware 软件栈及第三方软件包
        - 提供基于机器学习的感知与规划能力

    </div>

<a id="1-docker-installation"></a>

### 1. Docker 安装

Autoware 在 GHCR 上发布预构建的 Docker 镜像，使您无需安装依赖即可在主机上方便地运行和开发 Autoware，同时确保开发与部署环境一致。

有关本地开发之外的容器化部署模式和集成方案，请参阅 [Open AD Kit](https://autoware.org/open-ad-kit/)（首个面向自动驾驶的 [SOAFEE Blueprint](https://www.soafee.io/charter/)）及 [Open AD Kit 文档](https://autowarefoundation.github.io/openadkit/)。

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-circle-arrow-right: 使用 Docker 安装 Autoware Universe ⭐](autoware/docker-installation.md){ .md-button style="margin: 5px" }
[:fa-cl-s fa-circle-arrow-right: 使用 Docker 安装 Autoware Core](autoware/core-docker-installation.md){ .md-button style="margin: 5px" }

</div>

<a id="2-source-installation"></a>

### 2. 源码安装

需要更精细地控制安装环境时，可以从源码安装。
此方式适合有经验的用户或需要定制环境的用户。
请注意，您的本地环境可能会引起一些问题。

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-circle-arrow-right: 从源码安装 Autoware Universe ⭐](autoware/source-installation.md){ .md-button style="margin: 5px" }
[:fa-cl-s fa-circle-arrow-right: 从源码安装 Autoware Core](autoware/core-source-installation.md){ .md-button style="margin: 5px" }

</div>

<a id="3-debian-package-installation"></a>

### 3. Debian 软件包安装

Autoware Core 软件包可从 ROS 构建平台获取。
如果您的环境已经配置好 ROS，就可以方便地安装 Autoware 软件包，并与 ROS 生态系统中的其他软件包一起使用。

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-circle-arrow-right: 使用 Debian 软件包安装 Autoware Core](autoware/core-debian-installation.md){ .md-button }

</div>

<a id="extending-autoware-with-community-packages"></a>

## 使用社区软件包扩展 Autoware

[Autoware 索引](autoware/autoware-index.md)收录了用于扩展 Autoware 的社区 ROS 2 软件包。
配置好源码工作空间后，您可以使用生成的 `.repos` 文件，从索引中拉取其他软件包。

<div style="text-align: center;" markdown="1">

[:fa-cl-s fa-cubes: Autoware 索引（社区软件包）](autoware/autoware-index.md){ .md-button style="margin: 5px" }

</div>

<a id="installing-related-tools"></a>

## 安装相关工具

根据要进行的评估，可能还需要其他工具。
例如，运行端到端仿真需要安装合适的仿真器。

更多信息请参阅[此处](related-tools/index.md)。

<a id="additional-settings-for-developers"></a>

## 开发者附加设置

此外，还提供了面向开发者的工具和设置，例如 Shell 或 IDE。

更多信息请参阅[此处](additional-settings-for-developers/index.md)。
