<a id="autoware-core-debian-package-installation-guide"></a>

# Autoware Core Debian 软件包安装指南

<a id="prerequisites"></a>

## 前提条件

| 项目 | 要求                                                                                               |
| ---- | --------------------------------------------------------------------------------------------------------- |
| OS   | [Ubuntu 22.04](https://releases.ubuntu.com/22.04/)                                                        |
| ROS  | ROS 2 Humble (ROS 2 系统依赖请参阅 [REP-2000](https://www.ros.org/reps/rep-2000.html)) |

<a id="how-to-set-up"></a>

## 配置方法

1. 更新软件包信息。

   ```bash
   sudo apt update
   ```

2. 安装 Autoware Core。

   ```bash
   sudo apt install ros-humble-autoware-core
   ```
