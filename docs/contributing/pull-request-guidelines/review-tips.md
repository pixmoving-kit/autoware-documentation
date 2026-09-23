<a id="review-tips"></a>

# 审查技巧

<a id="toggle-annotations-or-review-comments-in-the-diff-view"></a>

## 在差异视图中切换注解或审查评论的显示

审查时，差异视图中可能会出现注解或审查评论。

要切换注解的显示，请按 `A` 键。

切换前：

![按 A 键之前](images/before-press-a.png)

切换后：

![按 A 键之后](images/after-press-a.png)

要切换审查评论的显示，请按 `I` 键。

其他键盘快捷键请参阅 [GitHub 文档](https://docs.github.com/en/get-started/using-github/keyboard-shortcuts)。

<a id="view-code-in-the-web-based-visual-studio-code"></a>

## 在网页版 Visual Studio Code 中查看代码

你可以在浏览器中打开 `Visual Studio Code`，通过功能丰富的界面查看代码。
使用时，在任意仓库或拉取请求页面按 `.` 键即可。

更详细的用法请参阅 [github/dev](https://github.com/github/dev)。

<a id="check-out-the-branch-of-a-pull-request-quickly"></a>

## 快速检出拉取请求的分支

在 fork-and-pull 模式下，检出拉取请求的分支通常比较麻烦。

```bash
# Copy the user name and the fork URL.
git remote add {user-name} {fork-url}
git checkout {user-name}/{branch-name}
git remote rm {user-name} # To clean up
```

你可以使用 [GitHub CLI](https://cli.github.com/) 简化操作，只需运行 `gh pr checkout {pr-number}`。

可以从拉取请求页面的右上角复制该命令。

![gh-pr-checkout](images/gh-pr-checkout.png)
