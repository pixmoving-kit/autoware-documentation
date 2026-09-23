<a id="console-settings-for-ros-2"></a>

# ROS 2 控制台设置

<a id="colorizing-logger-output"></a>

## 为日志输出添加颜色

默认情况下，ROS 2 日志器的输出不带颜色。
如需启用彩色输出，请在 `~/.bashrc` 中添加以下内容：

```bash
export RCUTILS_COLORIZED_OUTPUT=1
```

<a id="customizing-the-format-of-logger-output"></a>

## 自定义日志输出格式

默认情况下，ROS 2 日志器不会输出文件名、函数名或行号等详细信息。
如需自定义输出格式，请在 `~/.bashrc` 中添加以下内容：

```bash
export RCUTILS_CONSOLE_OUTPUT_FORMAT="[{severity} {time}] [{name}]: {message} ({function_name}() at {file_name}:{line_number})"
```

更多选项请参阅[此处](https://docs.ros.org/en/rolling/Tutorials/Demos/Logging-and-logger-configuration.html#console-output-formatting)。

<a id="colorized-googletest-output"></a>

## GoogleTest 彩色输出

在 `~/.bashrc` 中添加 `export GTEST_COLOR=1`。

更多详情请参阅 [GoogleTest 高级主题：彩色终端输出](https://google.github.io/googletest/advanced.html#colored-terminal-output)。

使用 `colcon test` 运行测试时，这项设置很有用。
