<a id="support-guidelines"></a>

# 支持指南

本页介绍我们提供的支持渠道。

!!! warning

    寻求帮助前，请先仔细搜索并阅读本文档网站。
    参与讨论时，也请遵循[讨论指南](../../contributing/discussion-guidelines/index.md)。

根据所需帮助的类型选择合适的资源，并阅读下文各节的详细说明。

- [文档网站](#documentation-sites)
  - 收集信息。
- [GitHub Discussions](#github-discussions)
  - 提问或尚未确认的缺陷 → [Q&A](https://github.com/orgs/autowarefoundation/discussions/categories/q-a)。
  - [功能请求](https://github.com/orgs/autowarefoundation/discussions/categories/feature-requests)。
  - [设计讨论](https://github.com/orgs/autowarefoundation/discussions/categories/design)。
- [GitHub Issues](#github-issues)
  - 已确认的缺陷。
  - 已确认的任务。
- [Discord](#discord)
  - 贡献者之间的即时交流。
- [ROS Discourse](#ros-discourse)
  - 需要广泛公告的一般性话题。

<a id="guidelines-for-autoware-community-support"></a>

## Autoware 社区支持指南

如果遇到 Autoware 问题，请按以下步骤寻求帮助：

<a id="1-search-for-existing-issues-and-questions"></a>

### 1. 搜索已有的 issue 和问题

创建新的 issue 或提问之前，请先检查是否已经有人报告或询问过该问题。可使用以下资源：

- **[Issues](https://github.com/autowarefoundation/autoware/issues)**

  请注意，Autoware 包含多个仓库，列表见 [autoware.repos](https://github.com/autowarefoundation/autoware/blob/main/repositories/autoware.repos)。
  建议跨所有仓库搜索。

- **[问题](https://github.com/autowarefoundation/autoware/discussions/categories/q-a)**

<a id="2-create-a-new-question-thread"></a>

### 2. 创建新的提问讨论

如果现有 issue 或问题都无法解决你的问题，请创建新的提问讨论：

- **[提问](https://github.com/autowarefoundation/autoware/discussions/categories/q-a)**

  如果一周内没有得到回答，请在帖子中提及 `@autoware-maintainers`，提醒维护者。

<a id="3-participate-in-other-discussions"></a>

### 3. 参与其他讨论

也欢迎在其他分类中发起或参与讨论：

- **[功能请求](https://github.com/autowarefoundation/autoware/discussions/categories/feature-requests)**
- **[设计讨论](https://github.com/autowarefoundation/autoware/discussions/categories/design)**

<a id="additional-resources"></a>

### 其他资源

如果不确定如何创建讨论，请参阅 [GitHub 关于创建新讨论的文档](https://docs.github.com/en/discussions/quickstart#creating-a-new-discussion)。

<a id="documentation-sites"></a>

## 文档网站

[文档指南](docs-guide.md)列出了实用的文档网站。
请访问这些网站，查看是否有与你的问题相关的信息。

请注意，文档网站并不总是最新或完全准确的。
如果发现 Autoware 文档中存在错误、表述不清或内容缺失，欢迎按照[贡献指南](../../contributing/index.md)提交拉取请求。

## GitHub Discussions

[GitHub Discussions 页面](https://github.com/orgs/autowarefoundation/discussions)是提问和讨论 Autoware 相关话题的主要场所。

| 分类 | 说明 |
| :--------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------- |
| [公告](https://github.com/orgs/autowarefoundation/discussions/categories/announcements) | Autoware 维护者发布的官方更新和新闻 |
| [设计](https://github.com/orgs/autowarefoundation/discussions/categories/design) | Autoware 系统与软件设计讨论 |
| [功能请求](https://github.com/orgs/autowarefoundation/discussions/categories/feature-requests) | 新功能和改进建议 |
| [综合讨论](https://github.com/orgs/autowarefoundation/discussions/categories/general) | Autoware 的一般性讨论 |
| [想法](https://github.com/orgs/autowarefoundation/discussions/categories/ideas) | 头脑风暴与创新想法分享 |
| [投票](https://github.com/orgs/autowarefoundation/discussions/categories/polls) | 社区投票与调查 |
| [问答](https://github.com/orgs/autowarefoundation/discussions/categories/q-a) | 社区和开发者的提问与回答 |
| [成果展示](https://github.com/orgs/autowarefoundation/discussions/categories/show-and-tell) | 项目与成果展示 |
| [TSC 会议](https://github.com/orgs/autowarefoundation/discussions/categories/tsc-meetings) | TSC（技术指导委员会）会议纪要与讨论 |
| [工作组活动](https://github.com/orgs/autowarefoundation/discussions/categories/working-group-activities) | 工作组活动更新 |
| [工作组会议](https://github.com/orgs/autowarefoundation/discussions/categories/working-group-meetings) | 工作组会议纪要与讨论 |

!!! warning

    GitHub Discussions 不适合跟踪任务或缺陷，请使用 GitHub Issues。

## GitHub Issues

GitHub Issues 是在 Autoware 各仓库中跟踪已确认缺陷、任务和改进的指定平台。

请遵循以下指南，以高效跟踪和解决问题：

<a id="reporting-bugs"></a>

### 报告缺陷

如果遇到已经确认的缺陷，请在对应的 Autoware 仓库中创建 issue 报告。
请提供复现步骤、预期结果和实际结果等详细信息，以帮助维护者及时处理。

<a id="tracking-tasks"></a>

### 跟踪任务

GitHub Issues 也用于管理以下任务：

- **重构：**提议重构现有代码，以提高效率、可读性或可维护性。请清楚说明建议重构的内容及原因。
- **新功能：**如果已经通过讨论确认新功能的必要性，请使用 issue 跟踪开发，说明功能目的、可能的设计和预期影响。
- **文档：**提议修改文档，以纠正错误、更新过时内容或添加新章节。请说明需要哪些修改，以及这些修改为何重要。

<a id="creating-an-issue"></a>

### 创建 issue

创建新 issue 时，请遵循以下要求：

1. **选择正确的仓库**：如果不确定应使用哪个仓库，请在 [Q&A 分类](https://github.com/autowarefoundation/autoware/discussions/categories/q-a)中发起讨论，向维护者寻求指导。
2. **使用清楚简洁的标题**：在标题中概括问题或任务，便于快速识别。
3. **提供详细描述**：包含理解问题背景和范围所需的全部信息，必要时附上截图、错误日志和代码片段。
4. **提及相关贡献者**：提及可能受影响或对此感兴趣的贡献者或团队。

<a id="linking-issues-and-pull-requests"></a>

### 关联 issue 与拉取请求

开始处理 issue 时，请通过提及 issue 编号，将相关拉取请求与其关联。
这样有助于保持清晰、可追溯的开发历史。

更多信息请参阅[拉取请求指南](../../contributing/pull-request-guidelines/index.md)。

!!! warning

    GitHub Issues 不用于一般提问或尚未确认的缺陷。如果为此创建 issue，
    通常会转移到 GitHub Discussions，以进一步澄清问题。

## Discord

[![Discord](https://img.shields.io/discord/953808765935816715?label=Join%20Autoware%20Discord&style=for-the-badge)](https://discord.gg/Q94UsPvReQ)

Autoware 提供 Discord 服务器，供贡献者轻松交流。

Autoware Discord 服务器适合以下活动：

- 向社区介绍自己。
- 与贡献者聊天。
- 进行快速的非正式意见调查。

请注意，这里并不适合寻求具体问题的帮助。

## ROS Discourse

如果希望与整个 ROS 社区广泛讨论某个话题，请在 [ROS Discourse 的 Autoware 分类](https://discourse.ros.org)发帖。

!!! warning

    不要在 ROS Discourse 上发布缺陷相关问题！
