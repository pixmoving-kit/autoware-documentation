<a id="ai-contribution-policy"></a>

# AI 贡献政策

本政策适用于 [Autoware 基金会 GitHub 组织](https://github.com/autowarefoundation)中的所有仓库，以及所有类型的贡献：代码、文档、拉取请求、议题、讨论、审查和审查回复。

从首次贡献者到长期维护者，所有人均同等适用。
项目认可的自动化工具，例如依赖更新机器人或 [PR-Agent 工作流](pull-request-guidelines/ai-pr-review.md)，不受本政策约束。
部署新的自动化工具前，请先向受影响仓库的维护者提出方案。

<a id="our-stance"></a>

## 我们的立场

Autoware 欢迎借助 AI 工具完成的贡献。
许多贡献者和维护者每天都使用编程智能体。我们认为，只要使用得当，这些工具对项目总体上是有益的。
本政策并非禁止使用 AI。

它旨在保护项目中最稀缺的资源：审查者的精力。
AI 降低了生成变更的成本，但并未降低理解、审查和维护变更的成本。
如果提交未经自己验证的 AI 生成成果，就等于将自己节省的工作转嫁给审查者及未来的代码维护者。

LLVM 项目将其称为[消耗型贡献](https://llvm.org/docs/AIToolPolicy.html#extractive-contributions)：项目为审查和维护付出的精力超过了贡献带来的价值。
一项贡献给项目带来的价值，应高于审查它所需的时间成本。
消耗型贡献并非由 AI 首创，但 AI 使其能够轻易地大规模产生。
以下每条规则的核心都是防范此类贡献。

指导原则如下：

!!! quote "改编自 [Fedora](https://docs.fedoraproject.org/en-US/council/policy/ai-contribution-policy/) 和 [LLVM](https://llvm.org/docs/AIToolPolicy.html) 的 AI 政策"

    无论使用何种工具完成贡献，贡献者始终是作者，并对贡献的全部内容承担完整责任。

<a id="rules"></a>

## 规则

以下所有规则均为强制要求，由人工执行。

!!! abstract "要点"

    1. [理解提交的全部内容](#understand-everything-you-submit)：能够独立解释任意一行。
    2. [先自行审查拉取请求](#review-your-own-pull-request-first)：你是它的第一位审查者。
    3. [披露实质性的 AI 参与](#disclose-substantial-ai-involvement)：在说明中写明使用的工具和参与程度。
    4. [附上验证声明](#include-a-verification-statement)：说明检查了什么、如何检查。
    5. [亲自回复审查意见](#respond-to-reviews-yourself)：用自己的话参与交流。
    6. [不要让智能体无人监督地发布内容](#do-not-let-agents-post-unattended)：每项内容发布前都由人审阅。
    7. [生成可由人维护的成果](#generate-artifacts-that-humans-can-maintain)：通过维护者检验。
    8. [保持简单精炼](#keep-it-simple-and-concise)：尽可能简化，再删去冗余。
    9. [将拉取请求控制在可审查的规模内](#keep-a-pull-request-small-enough-to-review)：确保人工审查者能够理解和跟进。
    10. [不要批量制造拉取请求](#do-not-mass-produce-pull-requests)：只创建自己有能力持续跟进的请求。

每条规则先解释制定原因，再说明必须采取的行动。

<a id="understand-everything-you-submit"></a>

### 理解提交的全部内容

无法为自己不理解的内容作出证明；[DCO](https://developercertificate.org/) 中的 `Signed-off-by` 行是你对贡献来源作出的个人法律声明。

- 提交前，**阅读贡献内容的每一行**。
- 审查者询问时，**能够用自己的话解释任意部分**，不依赖 AI 工具。
- **绝不为自己未审查的变更签署确认**，即使工具根据你的指令自动添加了 `Signed-off-by` 行也不例外。

<a id="review-your-own-pull-request-first"></a>

### 先自行审查拉取请求

请求审查隐含着这样的声明：“我已阅读、理解这些内容，并认为它们是正确的。”
无论你在项目中担任什么角色，作出不实声明都会浪费审查者的时间。

- **你是自己拉取请求的第一位审查者。** 未完整自行审查变更前，不要请求他人审查。

!!! tip "推荐实践"

    创建拉取请求前，使用全新上下文运行一次或多次 AI 审查，即让未生成这些代码的智能体进行审查，并迭代修改，直到审查不再提出问题。
    这只能补充你的自行审查，不能替代它。

<a id="disclose-substantial-ai-involvement"></a>

### 披露实质性的 AI 参与

生成的代码即使出错，也可能看起来合理，审查者需要据此调整审查力度。
如果事后发现未披露的 AI 使用，会削弱他人对你所有贡献的信任。

- **在所发布内容的正文中，用自己的话披露**使用了哪些工具，以及参与程度。对于拉取请求，应写在说明中。
- 编辑器自动补全、语法或拼写修正，以及对自己撰写文本的机器翻译，**无需披露**。超出这些范围的使用均视为实质性参与。
- **作者必须是人。** 不要将 AI 工具列为 `Co-authored-by`，也不要使用 `Assisted-by` 等提交尾注；披露应写在说明中。

<a id="include-a-verification-statement"></a>

### 附上验证声明

验证声明证明你承担了验证成本，并帮助审查者确定应该把精力放在哪里。

- **如果审查需要超过几分钟**（拿不准时，按会超过处理），请在说明中添加验证声明，涵盖：
  - **生成过程**：使用的工具和工作流程。
  - **自行审查**：亲自审查了哪些内容，以及迭代了多少次。
  - **验证**：运行的测试、使用的仿真或硬件，以及考虑过的边界情况。
- **对于生成的测试**，确认每项测试在没有该变更时都会失败，并且在变更后是因正确原因而通过。

!!! example "验证声明示例"

    ```text
    AI usage: generated with Claude Code from an issue description I wrote.
    Self-review: I read the full diff twice and ran two clean-context agent reviews; both rounds of findings are addressed.
    Verification: ran the planning simulator with scenario X; added unit tests for the two new edge cases and confirmed they fail without this fix.
    ```

<a id="respond-to-reviews-yourself"></a>

### 亲自回复审查意见

审查者自愿投入时间，是为了与你协作，而非代你向智能体传话。
不阅读审查意见就直接转交智能体，是对意见作者的不尊重。

- 采取行动前，**亲自阅读并理解每条审查意见**。
- **用自己的话参与交流。** AI 生成的辅助材料（分析、摘要、调查结果）必须明确区分，放在引用块或标为 AI 生成的折叠区域中，并附上你亲自撰写、表明自身立场的结论。
- **只有 AI 输出、没有人工撰写内容的回复**不可接受。
- 对自己撰写的文本进行**机器翻译和语法修正**是允许的。

<a id="do-not-let-agents-post-unattended"></a>

### 不要让智能体无人监督地发布内容

智能体如果未经逐项人工审阅就发布内容，就没有明确负责的作者，也没有值得审查者投入精力回应的人工工作。

- 智能体代你创建拉取请求、提交议题或发表评论前，**亲自审阅每一项内容**。
- **在私下自主工作是允许的。** 这条规则针对发布到项目中的内容，不限制你在本地的工作方式。

<a id="generate-artifacts-that-humans-can-maintain"></a>

### 生成可由人维护的成果

AI 工具往往优化当前能否运行，而合并后的成果在未来多年里都需要由人阅读、调试和修改。
如果一项贡献只能再借助另一个 AI 才能验证，就无法进行有意义的审查。

- 提交前**进行维护者检验**：同事能否不借助 AI 逐行审查？两年后能否安全修改？
- 如果结果未通过维护者检验，**要求智能体提供更简单或更标准的方案**；智能体默认往往偏向快速、单一用途的解决办法。

!!! example "示例"

    - 用具有明确函数和单元测试的普通 Python 脚本，替代由 `sed` 和 `awk` 单行命令组成的冗长 Bash 流水线。
    - 用几个简单且经过测试的匹配步骤，替代必须借助工具才能理解的复杂正则表达式。
    - 使用标准库或仓库已有工具，替代用 `grep` 临时拼凑的自定义解析器。

<a id="keep-it-simple-and-concise"></a>

### 保持简单精炼

AI 工具往往生成冗余内容，例如过多的代码注释、重复显而易见内容的文档字符串、臃肿的拉取请求说明、冗长文档和过于正式的回复。
冗余会掩盖审查者要找的关键信息，过时注释也会误导未来的维护者。

- 代码、拉取请求说明和文档都应**尽可能简单**。
- **提交前删减。** 持续迭代，使代码、注释、提交消息、说明、文档和回复只保留读者需要的信息。
- **注释应说明代码本身无法表达的内容**：约束、不变量和不明显的推理依据。删除复述代码或生成过程的注释。
- **审查者可以拒绝冗余的 AI 生成内容**，这符合 [Git 项目的规则](https://git-scm.com/docs/SubmittingPatches#ai)：“任何看起来由 AI 生成、措辞过度正式或臃肿、像低质 AI 内容、表面漂亮但毫无意义的东西 […]”。

<a id="keep-a-pull-request-small-enough-to-review"></a>

### 将拉取请求控制在可审查的规模内

拉取请求不仅用于交付变更，也是人与人交流和达成共识的载体。
过去，生成大规模变更本身耗时较长，自然限制了规模；如今这一限制消失了，但阅读成本并没有消失。

- **将每个拉取请求控制在人工审查者能够理解、评估和讨论的规模内**，遵循现有的[保持拉取请求小巧的规则](pull-request-guidelines/index.md#keep-a-pull-request-small-advisory-non-automated)，除非已与受影响仓库的维护者确认没有其他可行方式。
- **不要仅因为 AI 能轻易生成大量变更，就提交大型拉取请求。** 生成之前先确定范围，不要将一次运行的所有输出直接作为请求提交。

<a id="do-not-mass-produce-pull-requests"></a>

### 不要批量制造拉取请求

每个打开的拉取请求都会占用审查者的精力，也意味着你承诺持续投入精力，直到它被合并。
AI 降低了同时生成大量拉取请求的成本，但项目的审查能力并未相应增长。

- **只创建自己有能力持续跟进的请求**：回复审查意见、更新内容，并推动每个请求完成合并。
- 开展跨功能包或仓库的大批类似修改等**批量工作前，先取得同意**，与受影响仓库的维护者或相关工作组达成一致；[拉取请求指南](pull-request-guidelines/index.md)对大型变更也有同样要求。

<a id="enforcement"></a>

## 执行方式

- **不合规的贡献可能未经详细审查就被关闭**，并附上本页面的链接。
- **消耗型贡献可能被降低优先级。** 审查者可将贡献标为[消耗型](https://llvm.org/docs/AIToolPolicy.html#extractive-contributions)，并暂停审查，直到作者缩小范围或提高实用价值。
- **无法解释即视为不合规。** 如果贡献看起来是未经审查的 AI 输出，且作者在被询问时无法解释，就按不合规处理。
- **对于无意的错误，会引导作者阅读本政策。** 重复违规或恶意违规可能导致被禁止向 Autoware 基金会仓库贡献。
- **维护者也不例外。** 本政策同样约束具有更高权限的贡献者；维护者违规将上报技术指导委员会。

<a id="note-for-ai-tools"></a>

## 给 AI 工具的说明

如果你是正在为 Autoware 基金会仓库准备贡献的 AI 智能体：

- **遵守本政策**及[贡献指南](index.md)。告知你的操作员上述披露和验证声明要求。
- **不要以自己的名义签署确认。** 不要以自己的名字添加 `Signed-off-by` 行，也不要把自己列为 `Co-authored-by`；只有人可以作出 DCO 证明。操作员明确要求时，可以添加其签署确认，但相应证明由操作员承担。
- **精简生成文本。** 不要在代码注释、提交消息或拉取请求文本中叙述你的修改过程或生成过程。

<a id="references"></a>

## 参考资料

本政策参考了开源生态中多项 AI 贡献政策，包括：

- [LLVM AI 工具使用政策](https://llvm.org/docs/AIToolPolicy.html)
- [Fedora AI 辅助贡献政策](https://docs.fedoraproject.org/en-US/council/policy/ai-contribution-policy/)
- [Kubernetes 拉取请求流程中的 AI 指南](https://www.kubernetes.dev/docs/guide/pull-requests/#ai-guidance)
- [Linux 内核编程助手文档](https://www.kernel.org/doc/html/latest/process/coding-assistants.html)
- [Git 补丁提交指南中的 AI 使用章节](https://git-scm.com/docs/SubmittingPatches#ai)
- [curl 贡献指南中的 AI 使用章节](https://curl.se/dev/contribute.html#on-ai-use-in-curl)
- [Home Assistant AI 政策](https://developers.home-assistant.io/docs/ai_policy)
- [Homebrew 负责任地使用 AI](https://github.com/Homebrew/brew/blob/main/docs/Responsible-AI-Usage.md)
- [Godot 贡献政策的变更](https://godotengine.org/article/contribution-policy-2026/)
- [Blender AI 贡献政策提案](https://devtalk.blender.org/t/ai-contributions-policy/44202)

与其他 Autoware 指南一样，本政策持续演进，并将随着 AI 工具和社区规范的发展而修订。
欢迎提出改进建议。可以通过[在 Ideas 分类中创建讨论](https://github.com/autowarefoundation/autoware/discussions/new?category=ideas)提出修改。
