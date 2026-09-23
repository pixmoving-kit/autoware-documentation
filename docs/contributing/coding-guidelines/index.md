<a id="coding-guidelines"></a>

# 编码指南

!!! warning

    编写中

<a id="common-guidelines"></a>

## 通用指南

目前请参阅以下链接：

- <https://docs.ros.org/en/humble/Contributing/Developer-Guide.html>

同时，请牢记以下原则。

- 保持一致性。
- 尽可能采用自动化，例如使用简单的检查来验证格式、语法等。
- 使用英语编写注释和文档。
- 对于过于复杂（内聚性低）的函数，应适当拆分为较小的函数（例如，[Google 风格指南](https://google.github.io/styleguide/cppguide.html#Write_Short_Functions)建议单个函数少于 40 行）。
- 尽量减少使用作用域较大的成员变量或全局变量。
- 尽可能将大型拉取请求拆分成更小、更易管理的 PR（一些研究建议修改少于 200 行，例如[这篇文章](https://opensource.com/article/18/6/anatomy-perfect-pull-request)）。
- 代码审查时，不要在细枝末节的分歧上花费过多时间。详情请参阅：
  - <https://en.wikipedia.org/wiki/Law_of_triviality>
  - <https://steemit.com/programming/@emrebeyler/code-reviews-and-parkinson-s-law-of-triviality>
- 请遵循各编程语言的指南。
  - [C++](./languages/cpp.md)
  - [Python](./languages/python.md)
  - [Shell 脚本](./languages/shell-scripts.md)

<a id="autoware-style-guide"></a>

## Autoware 风格指南

有关 Autoware 特有的风格规范，请参阅以下要求：

- 功能包名称使用 `autoware_` 前缀。
  - 参见[目录结构指南：功能包名称](./ros-nodes/directory-structure.md#package-name)
  - 参见[为功能包添加 autoware\_ 前缀](https://github.com/orgs/autowarefoundation/discussions/4097)
- 将实现放在 `autoware` 命名空间中。
  - 参见[类设计指南：命名空间](./ros-nodes/class-design.md#namespaces)
  - 参见[为功能包添加 autoware\_ 前缀，选项 3：](https://github.com/orgs/autowarefoundation/discussions/4097#discussioncomment-8384169)
- 需要导出的头文件必须放在 `PACKAGE_NAME/include/autoware/` 目录中。
  - 参见[目录结构指南：导出头文件](./ros-nodes/directory-structure.md#exporting-headers)
- 在 `CMakeLists.txt` 中使用 `autoware_package()`。
  - 参见 [autoware_cmake README](https://github.com/autowarefoundation/autoware_cmake/tree/main/autoware_cmake)
