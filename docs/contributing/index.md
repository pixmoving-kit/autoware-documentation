<a id="contributing"></a>

# 参与贡献

感谢你有兴趣参与贡献！Autoware 由像你这样的贡献者共同支持，我们欢迎各种类型、各种规模的贡献。

作为贡献者，我们希望你在为 Autoware 及其相关仓库贡献时遵循以下指南。

- [行为准则](#code-of-conduct)
- [开始之前需要了解什么？](#what-should-i-know-before-i-get-started)
  - [Autoware 概念](#autoware-concepts)
  - [为开源项目贡献](#contributing-to-open-source-projects)
- [如何获取帮助？](#how-can-i-get-help)
- [如何参与贡献？](#how-can-i-contribute)
  - [参与讨论](#discussions)
  - [加入工作组](#working-groups)
  - [报告缺陷](#bug-reports)
  - [提交拉取请求](#pull-requests)

与 Autoware 本身一样，这些指南也在不断完善，欢迎随时提出改进建议！你可以[在 Ideas 分类中创建讨论](https://github.com/autowarefoundation/autoware/discussions/new?category=ideas)，提出对指南的修改建议。

<a id="code-of-conduct"></a>

## 行为准则

为确保 Autoware 社区保持开放和包容，请遵守[行为准则](https://github.com/autowarefoundation/autoware/blob/main/CODE_OF_CONDUCT.md)。

如果你认为社区成员违反了行为准则，请发送邮件至 [conduct@autoware.org](mailto:conduct@autoware.org) 进行举报。

<a id="what-should-i-know-before-i-get-started"></a>

## 开始之前需要了解什么？

<a id="autoware-concepts"></a>

### Autoware 概念

以下页面简要介绍了 Autoware，可帮助你从整体上了解其架构和设计：

- [Autoware 架构](../design/index.md)
- [Autoware 概念](../design/autoware-concepts/index.md)

有经验的开发者还应阅读 [Autoware 接口](../design/autoware-architecture-v1/interfaces/index.md)和[各组件页面](../design/autoware-architecture-v1/interfaces/components/index.md)，更详细地了解各组件或模块的输入和输出。

<a id="contributing-to-open-source-projects"></a>

### 为开源项目贡献

如果你刚接触开源项目，建议阅读 GitHub 的[如何为开源做贡献指南](https://opensource.guide/how-to-contribute)，了解人们为什么参与开源、贡献意味着什么，以及其他相关内容。

<a id="how-can-i-get-help"></a>

## 如何获取帮助？

请不要为一般性的支持问题创建 issue，因为我们希望将 GitHub issue 用于已经确认的缺陷报告。请改为在 Q&A 分类中发起讨论。有关 Autoware 支持渠道的更多信息，请参阅[支持指南](../community/support/index.md)。

!!! note

    对于提问或尚未确认的缺陷所创建的 issue，维护者会将其转移到 GitHub Discussions。

<a id="how-can-i-contribute"></a>

## 如何参与贡献？

<a id="discussions"></a>

### 讨论

你可以通过推动和参与讨论来为 Autoware 做贡献，例如：

- [提出增强 Autoware 的新功能](https://github.com/orgs/autowarefoundation/discussions/categories/feature-requests)
- [加入已有讨论并表达意见](https://github.com/orgs/autowarefoundation/discussions)
- 为其他贡献者组织讨论
- [回答问题并支持其他贡献者](https://github.com/autowarefoundation/autoware/discussions/categories/q-a?discussions_q=category%3AQ%26A+is%3Aunanswered)

<a id="working-groups"></a>

### 工作组

Autoware 基金会的[各工作组](https://github.com/autowarefoundation/autoware-projects/wiki#working-group-list)负责完成技术指导委员会设定的目标。这些工作组向所有人开放。加入某个工作组后，你可以了解当前的项目、了解各组如何管理项目，并参与解决有助于推进特定项目的问题。

要查看即将举行的工作组会议安排，请参阅 [Autoware 基金会活动日历](https://calendar.google.com/calendar/u/0/embed?src=autoware.org_6lol0ho5ft0217h8c60pi1fm30@group.calendar.google.com)。

<a id="bug-reports"></a>

### 缺陷报告

在报告缺陷之前，请先搜索相关仓库的 issue。可能已经有人报告了同样的问题，并提供了解决办法。如果无法确定应使用哪个仓库，请在 [Q&A 分类](https://github.com/autowarefoundation/autoware/discussions/new?category=q-a)中创建讨论，向维护者寻求帮助。

报告缺陷时，应提供复现问题所需的最少操作步骤。这有助于我们快速确认问题，并专注于正确的方向。

如果你愿意自行修复缺陷，我们非常欢迎；但在提交拉取请求之前，应先在 issue 中与维护者讨论可行方案。

[创建 issue 很简单](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue#creating-an-issue-from-a-repository)，如果遇到任何问题，可以创建 Q&A 讨论寻求帮助。

<a id="pull-requests"></a>

### 拉取请求

对于以下小规模修改，你可以直接提交拉取请求：

- 小幅更新文档
- 修正拼写错误
- 修复 CI 失败
- 修复编译器或分析工具检测到的警告
- 对单个功能包进行小幅修改

如果拉取请求涉及较大的修改，应遵循以下流程：

1. [创建 GitHub Discussion](https://docs.github.com/en/discussions/collaborating-with-your-community-using-discussions/collaborating-with-maintainers-using-discussions)，提出修改方案。这样可以获取其他成员和 Autoware 维护者的反馈，并确保提议的修改符合 Autoware 的设计理念及当前开发计划。如果不确定应在哪里讨论，请[创建新的 Q&A 讨论](https://github.com/autowarefoundation/autoware/discussions/new?category=q-a)。

2. 在讨论中达成共识后，[创建 issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue)。

3. [创建拉取请求](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)来实现修改，并引用第 2 步创建的 issue。

4. 为新增内容编写文档（如适用）。

大规模修改的示例包括：

- 为 Autoware 添加新功能
- 添加新的文档页面或章节

有关如何提交高质量拉取请求的更多信息，请阅读[拉取请求指南](pull-request-guidelines/index.md)，并且不要忘记检查所需的[许可证声明](license.md)！

如果使用 AI 工具辅助完成贡献，请遵循 [AI 贡献政策](ai-contribution-policy.md)。
