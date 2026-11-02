---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能访问相关上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在当前指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的全库标准。
- 使用相同提示重新生成代码，观察新标准如何影响生成结果。
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设定的部分标准。Copilot 具有非确定性。本练习的目的是观察更新指令前后行为的变化趋势，而不是要求输出逐字逐句完全一致。

## 指令文件

### 场景

Tailspin Toys 制定了一系列开发规范和要求，其中包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

借助指令文件，你可以确保 Copilot 了解这些要求，并按相关实践执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。它可以引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，以及希望加入的注释类型。指令既可以适用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本练习的范围。不过，示例项目展示了常见做法。概括来说：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（如单元测试、Astro 组件和数据层）或任务提供指导。
- 使用自然语言，确保说明清晰，并提供代码示例，说明应该和不应该采用的写法。

指令文件没有唯一的正确写法，AI 的使用方式也不止一种。通过实践，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件。探索项目中的文件时，你可能会发现有些文件专门用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务。
>
> Copilot 也可以帮助你生成指令文件。不同产品界面的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应练习会在相关步骤中介绍这些功能。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义指令文件

花些时间查看此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，了解项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下还有 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用于确定指令适用的文件。这里会匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目中有关创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小的可复用文件，也可以为 Copilot 提供生成代码时可参照的示例。（其中的路径相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来的步骤会添加 TSDoc 文档注释和文件头注释规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建一个分支。

1. 返回上一练习使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；每个相关步骤都会说明具体要求。

要观察自定义指令的影响，先在使用当前指令文件的情况下发送提示。稍后更新指令后，再发送相同提示进行比较。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取你希望提供的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是带类型的函数：第一个参数为 `db` 客户端，返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级头部注释。

> [!CAUTION]
> Copilot 具有概率性，因此它有时可能会在没有明确要求的情况下添加文档注释。即使如此也没有问题；本练习关注的是更新指令后行为的一致性变化。

## 添加新的全库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一条存储库编码标准，以提升代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。注意该部分记录了项目编码标准，但还没有代码内文档规则，这也是生成的辅助函数缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级 `.github/copilot-instructions.md` 中，提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合记录刚添加的文档注释规则等全项目标准。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空当前对话。
2. 单击 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 再次发送之前的**同一提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意现在生成的文件会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意现在生成的函数包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到后续练习会继续使用的真实代码中。

## 提交指令并推送分支

指令文件和其他项目资源一样，都应使用源代码管理。现在将指令更新和分支提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础结构更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在应创建拉取请求，并关联待办事项中的文档标准议题。你可以手动完成，也可以让已连接到项目的 GitHub 工具帮助 Copilot 完成：让它查找议题、创建关联该议题的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已为工作区配置 GitHub 工具，因此无需额外设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续练习会介绍这种连接（Model Context Protocol）的工作方式。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础结构：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 提示允许它代表你执行 GitHub 操作，请审查操作内容，并根据需要选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR，并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示允许它代表你执行 GitHub 操作和 shell 命令，请审查命令内容，并根据需要选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结与后续步骤

你探索了 Copilot 如何从指令文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下工作：

- 发送代码生成提示，观察 Copilot 使用**现有**指令时的生成结果。
- 在 `.github/copilot-instructions.md` 中添加新的全库标准。
- 重新发送相同提示，观察生成的代码如何采用新标准。
- 将指令更新和辅助函数基础结构提交并合并到 `main`，供下一练习继续使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
