<a id="unit-testing"></a>

# 单元测试

单元测试是测试的第一个阶段，用于验证类、函数等源代码单元。
通常，通过验证某个代码单元在不同输入下的输出来测试它。
单元测试有助于确保代码按预期运行，并防止意外的行为变化。

Autoware 使用 `ament_cmake` 框架构建和运行测试。
同一框架也用于分析测试结果。

`ament_cmake` 提供了一系列便捷函数，方便在基于 CMake 的功能包中注册测试，并确保生成与 JUnit 兼容的结果文件。
它目前支持 `pytest`、`gtest` 和 `gmock` 等多种测试框架。

为防止并行运行的测试在发布和订阅 ROS 话题时相互干扰，
建议使用 [`ament_cmake_ros`](https://github.com/ros2/ament_cmake_ros/tree/master/ament_cmake_ros/cmake) 中的命令，在隔离环境中运行测试。

下文提供了结合 `colcon test` 使用 `ament_add_ros_isolated_gtest` 的示例。
其他测试均遵循类似模式。

<a id="create-a-unit-test-with-gtest"></a>

## 使用 gtest 创建单元测试

在 `my_cool_pkg/test` 中创建 `gtest` 代码文件 `test_my_cool_pkg.cpp`：

```cpp
#include "gtest/gtest.h"
#include "my_cool_pkg/my_cool_pkg.hpp"
TEST(TestMyCoolPkg, TestHello) {
  EXPECT_EQ(my_cool_pkg::print_hello(), 0);
}
```

在 `package.xml` 中添加以下行：

```xml
<test_depend>ament_cmake_ros</test_depend>
```

接下来，在 `CMakeLists.txt` 的 `BUILD_TESTING` 下添加条目，以编译测试源文件：

```cmake
if(BUILD_TESTING)

  ament_add_ros_isolated_gtest(test_my_cool_pkg test/test_my_cool_pkg.cpp)
  target_link_libraries(test_my_cool_pkg ${PROJECT_NAME})
  target_include_directories(test_my_cool_pkg PRIVATE src)  # For private headers.
...
endif()
```

这会自动将测试与 `gtest` 提供的默认 main 函数链接。
被测代码通常位于另一个 CMake 目标中（示例中为 `${PROJECT_NAME}`），需要添加其共享对象以进行链接。
如果测试源文件包含 `src` 目录中的私有头文件，则需要使用 `target_include_directories()` 函数将该目录添加到头文件搜索路径中。

要注册新的 `gtest` 测试项，请使用宏 `TEST ()` 包裹测试代码。
`TEST ()` 是一个预定义宏，用于帮助生成最终测试代码，
同时注册一个可供执行的 `gtest` 测试项。
测试用例名称应使用 CamelCase，因为 gtest 在创建测试可执行程序时，会在测试夹具名称和测试用例类名之间插入下划线。

`gtest/gtest.h` 还包含 `gtest` 的预定义宏，例如 `ASSERT_TRUE(condition)`、
`ASSERT_FALSE(condition)`、`ASSERT_EQ(val1,val2)`、`ASSERT_STREQ(str1,str2)`、`EXPECT_EQ()` 等。
如果条件不满足，`ASSERT_*` 会中止测试，
而 `EXPECT_*` 会将测试标记为失败，但继续检查下一个测试条件。

!!! info

    有关 `gtest` 及其功能的更多信息，请参阅 [gtest 仓库](https://github.com/google/googletest)。

在示例 `CMakeLists.txt` 中，`ament_add_ros_isolated_gtest` 是 `ament_cmake_ros` 中的预定义宏，用于简化 `gtest` 代码的添加。
详细信息可参阅 [ament_add_gtest.cmake](https://github.com/ros2/ament_cmake_ros/tree/master/ament_cmake_ros/cmake)。

<a id="build-test"></a>

## 构建测试

<!-- cspell:ignore Testfile -->

默认情况下，`colcon` 会编译所有必需的测试文件（`ELF`、`CTestTestfile.cmake` 等）：

```console
cd ~/workspace/
colcon build --packages-select my_cool_pkg
```

测试文件生成在 `~/workspace/build/my_cool_pkg` 下。

<a id="run-test"></a>

## 运行测试

要运行某个功能包的全部测试，请执行：

```console
$ colcon test --packages-select my_cool_pkg

Starting >>> my_cool_pkg
Finished <<< my_cool_pkg [7.80s]

Summary: 1 package finished [9.27s]
```

测试命令的输出中包含所有测试结果的简要报告。

要获取所有已执行测试按作业划分的信息，请执行：

```console
$ colcon test-result --all

build/my_cool_pkg/test_results/my_cool_pkg/copyright.xunit.xml: 8 tests, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/cppcheck.xunit.xml: 6 tests, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/lint_cmake.xunit.xml: 1 test, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/my_cool_pkg_exe_integration_test.xunit.xml: 1 test, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml: 1 test, 0 errors, 0 failures, 0 skipped
build/my_cool_pkg/test_results/my_cool_pkg/xmllint.xunit.xml: 1 test, 0 errors, 0 failures, 0 skipped

Summary: 18 tests, 0 errors, 0 failures, 0 skipped
```

在 `~/workspace/log/test_<date>/<package_name>` 目录中可以找到所有原始测试命令、`std_out` 和 `std_err`。
此外，`~/workspace/log/latest_*/` 目录中还包含指向最近一次功能包级构建和测试输出的符号链接。

要在测试运行时打印详细信息，请使用 `--event-handlers console_cohesion+` 选项，将详细信息直接输出到控制台：

```console
$ colcon test --event-handlers console_cohesion+ --packages-select my_cool_pkg

...
test 1
    Start 1: test_my_cool_pkg

1: Test command: /usr/bin/python3 "-u" "~/workspace/install/share/ament_cmake_test/cmake/run_test.py" "~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml" "--package-name" "my_cool_pkg" "--output-file" "~/workspace/build/my_cool_pkg/ament_cmake_gtest/test_my_cool_pkg.txt" "--command" "~/workspace/build/my_cool_pkg/test_my_cool_pkg" "--gtest_output=xml:~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml"
1: Test timeout computed to be: 60
1: -- run_test.py: invoking following command in '~/workspace/src/my_cool_pkg':
1:  - ~/workspace/build/my_cool_pkg/test_my_cool_pkg --gtest_output=xml:~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml
1: [==========] Running 1 test from 1 test case.
1: [----------] Global test environment set-up.
1: [----------] 1 test from test_my_cool_pkg
1: [ RUN      ] test_my_cool_pkg.test_hello
1: Hello World
1: [       OK ] test_my_cool_pkg.test_hello (0 ms)
1: [----------] 1 test from test_my_cool_pkg (0 ms total)
1:
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test case ran. (0 ms total)
1: [  PASSED  ] 1 test.
1: -- run_test.py: return code 0
1: -- run_test.py: inject classname prefix into gtest result file '~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml'
1: -- run_test.py: verify result file '~/workspace/build/my_cool_pkg/test_results/my_cool_pkg/test_my_cool_pkg.gtest.xml'
1/5 Test #1: test_my_cool_pkg ...................   Passed    0.09 sec

...

100% tests passed, 0 tests failed out of 5

Label Time Summary:
copyright     =   0.49 sec*proc (1 test)
cppcheck      =   0.20 sec*proc (1 test)
gtest         =   0.05 sec*proc (1 test)
lint_cmake    =   0.18 sec*proc (1 test)
linter        =   1.34 sec*proc (4 tests)
xmllint       =   0.47 sec*proc (1 test)

Total Test time (real) =   7.91 sec
...
```

<a id="code-coverage"></a>

## 代码覆盖率

简单来说，
代码覆盖率指标衡量的是测试期间执行（覆盖）了多少程序代码。

在 Autoware 仓库中，[Codecov](https://app.codecov.io/gh/autowarefoundation/autoware_universe/) 用于自动计算每个未关闭拉取请求的覆盖率。

有关代码覆盖率指标的更多信息，请参阅 [Codecov 文档](https://docs.codecov.com/docs/about-code-coverage)。
