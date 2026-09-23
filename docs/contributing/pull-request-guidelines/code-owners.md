<a id="code-owners"></a>

# 代码所有者

Autoware 项目使用多个 `CODEOWNERS` 文件指定仓库各部分的所有者。有关代码所有者的详细说明，请参阅 [GitHub 的代码所有者文档](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)。

<a id="purpose-and-function-of-the-codeowners-file"></a>

## `CODEOWNERS` 文件的用途与作用

`CODEOWNERS` 文件通过以下方式在拉取请求（PR）管理中发挥重要作用：

- **自动请求审查**：自动将 PR 分配给负责的个人或团队。
- **强制合并审批**：在指定代码所有者或仓库维护者批准之前阻止合并 PR，确保进行充分审查。
- **维持质量控制**：要求熟悉相关内容的个人或团队进行审查，有助于保持代码质量与一致性。

<a id="locating-codeowners-files"></a>

## `CODEOWNERS` 文件的位置

`CODEOWNERS` 文件位于 Autoware 项目各仓库的 `.github` 目录中。[`autoware.repos` 文件](https://github.com/autowarefoundation/autoware/blob/main/repositories/autoware.repos)列出了这些仓库及其目录。

<a id="maintenance-of-codeowners"></a>

## `CODEOWNERS` 的维护

通常由仓库维护者负责更新 `CODEOWNERS` 文件。如需提出修改，请提交修改该文件的 PR。

<a id="special-case-for-the-autoware-universe-repository"></a>

### Autoware Universe 仓库的特殊情况

在 [autoware_universe](https://github.com/autowarefoundation/autoware_universe) 仓库中，`CODEOWNERS` 文件由 CI 自动维护。

[此工作流](https://github.com/autowarefoundation/autoware_universe/actions/workflows/update-codeowners-from-packages.yaml)根据仓库中各功能包 `package.xml` 文件里的 `maintainer` 信息，更新 `CODEOWNERS` 文件。

要修改 `autoware_universe` 仓库中某个功能包的代码所有者：

1. 通过 PR 修改 `package.xml` 文件中的 `maintainer` 信息。
2. 合并后，CI 工作流会在 UTC 午夜运行（也可由维护者手动触发），更新 `CODEOWNERS` 文件并创建 PR。
3. 随后需要维护者合并 CI 生成的 PR，才能完成更新。
   - **自动生成的 PR 示例：**[chore: update CODEOWNERS #6866](https://github.com/autowarefoundation/autoware_universe/pull/6866)

<a id="responsibilities-of-code-owners"></a>

## 代码所有者的职责

代码所有者应及时审查分配给自己的 PR。
如果 PR **超过一周**仍未得到审查，维护者可能会介入审查，并可能将其合并。

<a id="faq"></a>

## 常见问题

<a id="unreviewed-pull-requests"></a>

### 拉取请求无人审查

如果你的 PR 尚未得到审查：

- 🏹 **直接联系代码所有者**：在 PR 中发表评论，提醒相关所有者。
- ⏳ **一周后跟进**：如果一周后仍未审查，请在 PR 下评论，并提及 `@autoware-maintainers`。
- 📢 **必要时升级处理**：如果你的请求一直未获回应，可以在 [Autoware Discord 频道](../../community/support/support-guidelines.md#discord)中发送消息，寻求进一步处理 🚨。请记住，维护者通常同时承担多项职责，感谢你的耐心🙇。

<a id="pr-author-is-the-only-code-owner"></a>

### PR 作者是唯一的代码所有者

如果你是唯一的代码所有者，并且创建了 PR：

- 通过提及 `@autoware-maintainers` 请求审查。
- 维护者会考虑增加维护人员，以避免此类冲突。

<a id="non-code-owners-reviewing-prs"></a>

### 非代码所有者审查 PR

任何人都可以审查 PR：

- 你可以审查任何拉取请求并提供反馈。
- 你的审查可能不足以满足合并要求，但有助于代码所有者和维护者做出决定。
- 如果认为拉取请求已经可以合并，可以在拉取请求的评论中提及代码所有者和维护者。
