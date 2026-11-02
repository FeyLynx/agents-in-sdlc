---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能访问相关上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，本练习不会比较更新指令前后的提示效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示影响。稍后审查云智能体生成的拉取请求时，你会看到指令文件的作用。

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

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本练习的范围。不过，示例项目展示了常见做法。概括来说：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（如单元测试、Astro 组件和数据层）或特定任务提供指导。
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

## 添加新的全库标准

接下来只需编辑一次：添加一条项目级规则，要求代码中使用 TSDoc 文档注释，并为文件添加头部注释。后续本练习系列中，云智能体处理分配给它的议题时会采用这条规则。

开始编辑前，创建并切换到新分支（云智能体会读取议题目标分支中的指令，但仍应规范提交更改）：

1. 在 codespace 终端中运行以下命令：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一条存储库编码标准，以提升代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。注意该部分记录了项目编码标准，但尚无代码内文档规则，这也是生成的辅助函数缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级 `.github/copilot-instructions.md` 中，提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合记录刚添加的文档注释规则等全项目标准。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。在下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并完成合并。之后，云智能体处理下一练习中分配的议题时就会读取这些指令。

> [!TIP]
> 如果希望在本次研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你探索了 Copilot 如何从指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的全库标准。稍后的练习中，你会看到云智能体创建的拉取请求如何遵循该标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
