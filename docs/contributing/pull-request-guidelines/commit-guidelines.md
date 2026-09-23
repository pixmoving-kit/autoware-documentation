<a id="commit-guidelines"></a>

# 提交指南

<a id="branch-rules"></a>

## 分支规则

<a id="start-branch-names-with-the-corresponding-issue-numbers-advisory-non-automated"></a>

### 分支名称以对应的 issue 编号开头（建议，非自动检查）

<a id="rationale"></a>

#### 理由

- 开发者可以快速找到对应的 issue。
- 便于工具处理。
- 与 GitHub 的默认行为一致。

<a id="exception"></a>

#### 例外

如果没有对应的 issue，可以忽略此规则。

<a id="example"></a>

#### 示例

```text
123-add-feature
```

<a id="reference"></a>

#### 参考资料

- [GitHub 文档](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-a-branch-for-an-issue)

<a id="use-dash-case-for-the-separator-of-branch-names-advisory-non-automated"></a>

### 分支名称使用 `dash-case` 分隔（建议，非自动检查）

<a id="rationale_1"></a>

#### 理由

- 与 GitHub 的默认行为一致。

<a id="example_1"></a>

#### 示例

```text
123-add-feature
```

<a id="reference_1"></a>

#### 参考资料

- [GitHub 文档](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-a-branch-for-an-issue)

<a id="make-branch-names-descriptive-advisory-non-automated"></a>

### 使用具有描述性的分支名称（建议，非自动检查）

<a id="rationale_2"></a>

#### 理由

- 可以避免名称冲突。
- 开发者可以了解该分支的用途。

<a id="exception_1"></a>

#### 例外

如果已经提交了拉取请求，则不必修改分支名称，因为这需要重新创建拉取请求，会产生额外干扰并浪费时间。  
下次注意即可。

<a id="example_2"></a>

#### 示例

通常以动词开头比较合适。

```text
123-fix-memory-leak-of-trajectory-follower
```

<a id="commit-rules"></a>

## 提交规则

<a id="sign-off-your-commits-required-automated"></a>

### 为提交添加签署声明（必需，自动检查）

开发者必须证明自己编写了所贡献的代码，或拥有向项目提交这些代码的权利。

<a id="rationale_3"></a>

#### 理由

否则会导致复杂的许可证问题。

<a id="example_3"></a>

#### 示例

```bash
git commit -s
```

```text
feat: add a feature

Signed-off-by: Autoware <autoware@example.com>
```

<a id="reference_2"></a>

#### 参考资料

- [GitHub Apps - DCO](https://github.com/apps/dco)
