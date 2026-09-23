<a id="pull-request-guidelines"></a>

# 拉取请求指南

<a id="general-pull-request-workflow"></a>

## 通用拉取请求流程

Autoware 采用 fork-and-pull 模式。
有关该模式的详细信息，请参阅 [GitHub 文档](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests)。

以下是基于 fork-and-pull 模式的通用拉取请求流程示例。
为 Autoware 贡献时，请参考此流程。

1. 创建 issue。
   - 与维护者讨论解决问题的方案。
   - 创建 issue 之前，请确认[支持指南](../../community/support/support-guidelines.md)。
   - 与其他贡献者讨论时，请遵循[讨论指南](../discussion-guidelines/index.md)。
2. 创建仓库的 fork。（仅首次需要）
3. 按照 issue 中达成一致的方案，在你的 fork 仓库中编写代码。
   - 根据需要编写测试和文档。
   - 编写代码时遵循[编码指南](../coding-guidelines/index.md)。
   - 编写测试时遵循[测试指南](../testing-guidelines/index.md)。
   - 编写文档时遵循[文档指南](../documentation-guidelines/index.md)。
   - 提交修改时遵循[提交指南](commit-guidelines.md)。
   - 如果使用 AI 工具辅助编写修改，请遵循 [AI 贡献政策](../ai-contribution-policy.md)。
4. 测试代码。
   - 建议整理测试结果，因为后续审查过程中需要说明测试结果。
   - 如果不确定应执行哪些测试，请与维护者讨论。
5. 创建拉取请求。
   - 创建拉取请求时遵循[拉取请求规则](#pull-request-rules)。
6. 等待拉取请求审查。
   - 审查者会按照[审查指南](review-guidelines.md)审查你的代码。
     - 除审查者之外，也鼓励作者了解审查指南。
   - 如果 [CI 检查](ci-checks.md)失败，请修复错误。
   - 通过[代码所有者](code-owners.md)指南了解代码所有权。
     - 如果拉取请求无人审查，请查看[代码所有者常见问题](code-owners.md#faq)。
7. 处理审查者提出的意见。
   - 如果不理解某条审查意见的含义，请向审查者询问，直到理解为止。
     - 不建议在不理解原因的情况下修改，因为作者应对自己拉取请求的最终内容负责。
   - 如果不赞同某条审查意见，请向审查者询问合理的依据。
     - 审查者有义务让作者理解每条意见的含义。
   - 处理完审查意见后，请再次请求审查，并返回第 6 步。
     - 尽可能避免强制推送，以便审查者只需查看差异。更准确地说，至少应保留截至审查时的提交历史，因为 GitHub Web 界面中的建议修改等操作可能需要 rebase 才能通过 DCO CI。
   - 如果没有新的审查意见，审查者会批准拉取请求，然后进入第 8 步。
8. 合并拉取请求。
   - 如果维护者没有特殊要求，任何具有写入权限的人都可以合并拉取请求。
     - 鼓励作者亲自合并，以增强对自己拉取请求的责任感。
     - 如果作者没有写入权限，请联系审查者或维护者。

<a id="pull-request-rules"></a>

## 拉取请求规则

<a id="use-an-appropriate-pull-request-template-required-non-automated"></a>

### 使用合适的拉取请求模板（必需，非自动检查）

<a id="rationale"></a>

#### 理由

- 模板统一了描述的格式，可以提高审查效率。

<a id="steps-to-use-an-appropriate-pull-request-template"></a>

#### 使用合适的拉取请求模板的步骤

部分仓库可能有多个拉取请求模板。请按以下说明使用合适的模板创建拉取请求：

1. 选择合适的模板，操作方法见[此视频](https://user-images.githubusercontent.com/31987104/184344710-2adee239-799f-4fdf-bfab-be76345bfac1.mp4)。
2. 仔细阅读所选模板，并填写所需内容。
3. 在审查过程中勾选相应复选框。
   - 作者需要填写[审查前检查清单](https://github.com/autowarefoundation/autoware/blob/44c70d33825617b56d8de5e6fe921000238238bd/.github/PULL_REQUEST_TEMPLATE/standard-change.md#pre-review-checklist-for-the-pr-author)和[审查后检查清单](https://github.com/autowarefoundation/autoware/blob/44c70d33825617b56d8de5e6fe921000238238bd/.github/PULL_REQUEST_TEMPLATE/standard-change.md#post-review-checklist-for-the-pr-author)。

<a id="set-appropriate-reviewers-after-creating-a-pull-request-required-partially-automated"></a>

### 创建拉取请求后指定合适的审查者（必需，部分自动完成）

<a id="rationale_1"></a>

#### 理由

- 拉取请求必须由合适的审查者审查，以保持代码库的质量。

<a id="example"></a>

#### 示例

- 对于大多数 ROS 功能包，系统会根据 `package.xml` 中的 `maintainer` 信息自动分配审查者。
- 如果没有自动分配审查者，请按照 [GitHub 文档](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/requesting-a-pull-request-review)中的说明手动分配。
  - 可以查看仓库的 `.github/CODEOWNERS` 文件来确定审查者。
- 如果不确定应指定谁，请询问 `@autoware-maintainers`。
- 如果没有分配审查者的权限，请通过提及审查者来请求审查。

<a id="apply-conventional-commits-to-the-pull-request-title-required-automated"></a>

### 拉取请求标题遵循约定式提交（必需，自动检查）

<a id="rationale_2"></a>

#### 理由

- [约定式提交](https://www.conventionalcommits.org/en/v1.0.0/)可用于生成分类的变更日志，例如通过 [git-cliff](https://github.com/orhun/git-cliff) 生成。

<a id="example_1"></a>

#### 示例

```text
feat(trajectory_follower): add an awesome feature
```

!!! note

    描述部分（此处为 `add an awesome feature`）必须以小写字母开头。

如果修改破坏了某些接口，请如下使用 `!`（破坏性变更）标记：

```text
feat(trajectory_follower)!: remove package
feat(trajectory_follower)!: change parameter names
feat(planning)!: change topic names
feat(autoware_utils)!: change function names
```

对于包含代码的仓库（大多数仓库），类型应采用 [conventional-commit-types 的定义](https://github.com/commitizen/conventional-commit-types/blob/c3a9be4c73e47f2e8197de775f41d981701407fb/index.json)。

对于 [autoware-documentation](https://github.com/autowarefoundation/autoware-documentation) 等文档仓库，请使用以下定义：

- `feat`
  - 添加新页面。
  - 向现有页面添加内容。
- `fix`
  - 修正现有页面的内容。
- `refactor`
  - 将内容移动到其他页面。
- `docs`
  - 更新介绍文档仓库本身的文档。
- `build`
  - 更新文档站点构建工具的配置。
- `!`（破坏性变更）
  - 删除页面。
  - 更改页面的网址。

通常不使用 `perf` 和 `test`。
其他类型的含义与代码仓库相同。

<a id="add-the-related-component-names-to-the-scope-of-conventional-commits-advisory-non-automated"></a>

### 在约定式提交的作用域中添加相关组件名称（建议，非自动检查）

<a id="rationale_3"></a>

#### 理由

- 有助于贡献者找到与自己相关的拉取请求。
- 使变更日志更加清晰。

<a id="example_2"></a>

#### 示例

对于 ROS 功能包，建议添加功能包名称或组件名称。

```text
feat(trajectory_follower): add an awesome feature
refactor(planning, control): use common utils
```

<a id="keep-a-pull-request-small-advisory-non-automated"></a>

### 保持拉取请求规模较小（建议，非自动检查）

<a id="rationale_4"></a>

#### 理由

- 小规模拉取请求便于审查者理解。
- 小规模拉取请求便于维护者回退。

<a id="exception"></a>

#### 例外

如果已与维护者达成一致，确认只能提交较大的拉取请求，那么也是可以接受的。

<a id="example_3"></a>

#### 示例

- 避免在一个拉取请求中开发两个功能。
- 避免在同一个提交中混合不同类型（`feat`、`fix`、`refactor` 等）的修改。

<a id="remind-reviewers-if-there-is-no-response-for-more-than-a-week-advisory-non-automated"></a>

### 超过一周没有回应时提醒审查者（建议，非自动检查）

<a id="rationale_5"></a>

#### 理由

- 作者有责任持续关注自己的拉取请求，直到其合并。

<a id="example_4"></a>

#### 示例

```text
@{some-of-developers} Would it be possible for you to review this PR?
@autoware-maintainers friendly ping.
```
