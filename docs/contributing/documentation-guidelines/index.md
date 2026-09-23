<a id="documentation-guidelines"></a>

# 文档指南

<a id="workflow"></a>

## 工作流程

欢迎为 Autoware 文档作出贡献，并应遵循[贡献指南中的原则](../index.md#pull-requests)。小型、有限的修改可以通过派生仓库并提交拉取请求完成；较大的修改应先通过 GitHub Discussion 与社区及 Autoware 维护者讨论。

小型修改示例：

- 修正拼写或语法错误
- 修复失效链接
- 向已有且范围明确的页面补充内容，例如[故障排查](../../community/support/troubleshooting/index.md)指南。

较大修改示例：

- 添加包含大量细节的新页面，例如教程
- 重新组织现有文档结构

<a id="style-guide"></a>

## 风格指南

应尽量遵循 [Google 开发者文档风格指南](https://developers.google.com/style)。建议阅读其[要点页面](https://developers.google.com/style/highlights)；即使未阅读，也请注意以下关键事项。

- [使用标准美式英语拼写](https://developers.google.com/style/spelling)和标点。
- 文档标题和章节标题[采用句首大写形式](https://developers.google.com/style/capitalization)。
- [使用描述性链接文本](https://developers.google.com/style/link-text)。
- [使用简短的句子](https://developers.google.com/style/translation#write-short,-clear,-and-precise-sentences)，便于理解和翻译。

<a id="tips"></a>

## 提示

<a id="how-to-preview-your-modification"></a>

### 如何预览修改

有两种方式可以在文档网站上预览修改。

<a id="1-using-github-actions-workflow"></a>

#### 1. 使用 GitHub Actions 工作流

请按以下步骤操作。

1. 向仓库创建拉取请求。
2. 在侧栏中添加 `deploy-docs` 标签（见下图）。
3. 等待几分钟，`github-actions` 机器人会通知你该拉取请求的预览网址。

![deploy-docs 标签](images/deploy-docs-label-for-pull-request.png){ width="800" }

<a id="2-running-an-mkdocs-server-in-your-local-environment"></a>

#### 2. 在本地环境运行 MkDocs 服务器

也可以不创建 PR，而是在本机使用 `mkdocs` 命令构建 Autoware 文档网站。
假设你使用 Ubuntu 操作系统，请运行以下命令安装所需的库。

```bash
python3 -m pip install -U $(curl -fsSL https://raw.githubusercontent.com/autowarefoundation/autoware-github-actions/main/deploy-docs/mkdocs-requirements.txt)
```

然后，在文档目录中运行 `mkdocs serve`。

```bash
cd /PATH/TO/YOUR-autoware-documentation
mkdocs serve
```

这将启动 MkDocs 服务器。访问 [http://127.0.0.1:8000/](http://127.0.0.1:8000/) 即可预览网站。
