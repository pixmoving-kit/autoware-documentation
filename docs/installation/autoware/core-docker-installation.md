<a id="autoware-core-docker-installation-guide"></a>

# Autoware Core Docker 安装指南

<a id="prerequisites"></a>

## 前提条件

| 项目   | 要求                                                  |
| ------ | ------------------------------------------------------------ |
| Docker | NVIDIA Container Toolkit（推荐，目前并非必需） |

<a id="select-docker-image"></a>

## 选择 Docker 镜像

| 镜像                                                | 说明                                                               |
| ---------------------------------------------------- | ------------------------------------------------------------------------- |
| ghcr.io/autowarefoundation/autoware:core-jazzy       | 仅包含可执行文件的运行时镜像。               |
| ghcr.io/autowarefoundation/autoware:core-devel-jazzy | 包含源代码和构建依赖的开发镜像。 |

使用 ROS 2 Humble 时，将 `jazzy` 替换为 `humble`。标签格式为：`<stage>-<ros_distro>[-<date>|-<version>]`。完整的镜像目录和标签列表见 [`autoware/docker/README.md`](https://github.com/autowarefoundation/autoware/blob/main/docker/README.md)。

<a id="how-to-set-up"></a>

## 设置步骤

拉取你要使用的 Docker 镜像。以下命令以 `core-jazzy` 为例：

```bash
docker pull ghcr.io/autowarefoundation/autoware:core-jazzy
```
