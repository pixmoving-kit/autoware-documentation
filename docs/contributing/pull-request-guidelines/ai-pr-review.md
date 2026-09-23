<a id="ai-pr-review"></a>

# AI PR 审查

我们已在 Autoware Universe 仓库中启用 [Codium-ai/pr-agent](https://github.com/Codium-ai/pr-agent/tree/main)。

<a id="the-workflow"></a>

## 工作流

工作流：[pr-agent.yaml](https://github.com/autowarefoundation/autoware_universe/blob/main/.github/workflows/pr-agent.yaml)

<a id="additional-links-for-the-workflow-maintainers"></a>

### 面向工作流维护者的其他链接

- [可用模型列表](https://github.com/Codium-ai/pr-agent/blob/main/pr_agent/algo/__init__.py)

<a id="how-to-use"></a>

## 使用方法

创建 PR 时或创建后，在 PR 中添加 `tag:pr-agent` 标签。

等待以下两个 PR-Agent 作业均成功完成：

- `prevent-no-label-execution-pr-agent / prevent-no-label-execution`
- `Run pr agent on every pull request, respond to user comments`

!!! warning

    如果同时添加多个标签，`prevent-no-label-execution` 可能无法正确处理。

    例如，先添加 `tag:pr-agent`，等待其就绪后，再根据需要添加 `tag:run-build-and-test-differential`。

然后，你可以选择使用以下文档中的命令：

```text
/review: Request a review of your Pull Request.
/describe: Update the PR description based on the contents of the PR.
/improve: Suggest code improvements.
/ask Could you propose a better name for this parameter?: Ask a question about the PR or anything really.
/update_changelog: Update the changelog based on the PR's contents.
/add_docs: Generate docstring for new components introduced in the PR.
/help: Get a list of all available PR-Agent tools and their descriptions.
```

- [官方文档](https://pr-agent-docs.codium.ai/tools/)
- [使用指南](https://pr-agent-docs.codium.ai/usage-guide/automations_and_usage/#online-usage)

使用时，[像这样在 PR 中发表评论](https://github.com/Codium-ai/pr-agent/pull/229#issuecomment-1695021901)。

一分钟内，你应该会看到评论下出现 👀 表情回应。

随后机器人会回复审查意见、描述或答案。

!!! info

    请每次只发布一条与 PR-Agent 相关的评论。
