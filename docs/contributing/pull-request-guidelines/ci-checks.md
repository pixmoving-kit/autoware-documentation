<a id="ci-checks"></a>

# CI 检查

Autoware 会对拉取请求执行多项检查。
检查结果会显示在拉取请求页面底部，如下图所示。

![CI 检查](images/ci-checks.png)

如果显示 ❌ 标记，请点击 `Details` 按钮，调查失败原因。

如果显示 `Required` 标记，则必须解决错误后才能合并拉取请求。
如果没有该标记，则该检查是可选的，但仍建议修复问题。

以下各节介绍 Autoware 中常见的 CI 检查。  
请注意，部分仓库的配置可能不同。

## DCO

开发者来源证明（Developer Certificate of Origin，DCO）是一种轻量方式，用于让贡献者证明其编写了所贡献的代码，或拥有向项目提交这些代码的权利。

此工作流检查拉取请求是否满足 `DCO` 要求。  
你需要确认[要求的事项](https://developercertificate.org/)，并使用 `git commit -s` 提交。

更多信息请参阅 [GitHub App 页面](https://github.com/apps/dco)。

## semantic-pull-request

此工作流检查拉取请求是否遵循[约定式提交](https://www.conventionalcommits.org/en/v1.0.0/)。

详细规则请参阅[拉取请求规则](index.md#pull-request-rules)。

## pre-commit

[pre-commit](https://pre-commit.com/) 是在提交时运行格式化工具或代码检查工具的工具。

此工作流检查拉取请求是否能够通过 `pre-commit` 检查而不出现错误。

如果仓库启用了 `pre-commit.ci - pr` 工作流，它会通过 [pre-commit.ci](https://pre-commit.ci/) 尽可能自动修复错误。  
如果仍有错误，请手动修复。

你可以在本地环境中使用以下命令运行 `pre-commit`：

```bash
pre-commit run -a
```

也可以在仓库中安装 `pre-commit`，使其在提交之前自动运行：

```bash
pre-commit install
```

由于难以在检测错误时完全避免误报，因此部分作业放在单独的配置文件中，并标记为可选。  
要执行这些检查，请使用 `--config` 选项：

```bash
pre-commit run -a --config .pre-commit-config-optional.yaml
```

## spell-check-differential

此工作流使用 [CSpell](https://github.com/streetsidesoftware/cspell) 和[我们的词典文件](https://github.com/tier4/autoware-spell-check-dict/blob/main/.cspell.json)检测拼写错误。
由于难以在检测错误时完全避免误报，此工作流是可选的，但仍建议尽可能消除拼写错误。

如果需要使用词典中未收录的词语，可以采用以下方式。

- 如果该词仅在少量文件中使用，可以通过[文档内设置“cspell:ignore”](https://cspell.org/configuration/document-settings/)抑制检查。
- 如果该词在仓库中广泛使用，可以创建本地 cspell JSON 文件，并将其传递给 [spell-check action](https://github.com/autowarefoundation/autoware-github-actions/tree/main/spell-check)。
- 如果该词是通用词语，且可能在多个仓库中使用，可以向 [tier4/autoware-spell-check-dict](https://github.com/tier4/autoware-spell-check-dict) 或 [tier4/cspell-dicts](https://github.com/tier4/cspell-dicts) 提交拉取请求，更新词典。

## build-and-test-differential

此工作流对拉取请求执行 `colcon build` 和 `colcon test` 检查。  
为了加快 CI，它仅检查修改过的功能包及其依赖项，而非全部功能包。

## build-and-test-differential-self-hosted

此工作流是 `build-and-test-differential` 的 `ARM64` 版本。  
你需要添加 `ARM64` 标签才能运行此工作流。

作为补充说明，由于 GitHub 托管运行器不支持 ARM 机器，我们使用 AWF 提供的自托管运行器。  
有关自托管运行器的详细信息，请参阅 [GitHub 文档](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners)。

## deploy-docs

此工作流为拉取请求部署文档预览站点。  
你需要添加 `deploy-docs` 标签才能运行此工作流。
