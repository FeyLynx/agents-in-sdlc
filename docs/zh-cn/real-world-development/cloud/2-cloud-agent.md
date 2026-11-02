---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商留下的文档状况不佳，实际上完全缺失。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页上展示相关游戏，也就是同一类别中的其他游戏，方便用户继续浏览。暂时不需要完善的实现，只需要可用于 UX 验收测试的版本。目前这项工作受阻，但其他议题优先级更高。

这些任务很容易被推迟，因此适合分配给 Copilot 云智能体。它可以异步处理任务，让开发人员先处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。就像与开发人员协作一样，你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 会话进行中或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你！

## 明确界定任务范围的重要性

GitHub Copilot 没有魔法，不能替你思考。即使看似简单的操作，深入分析后也会发现复杂性，因此请[谨慎界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段开展工作、学习、尝试并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果你已完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的作用；如果尚未学习，现在可以快速查看。分配任务给 Copilot 云智能体前，先只读查看存储库中已有的指令文件，稍后便能观察它们的影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意它生成的 PR 是否遵循这些指令文件中的 TSDoc 注释、TypeScript 约定和文件头注释。稍后的练习会再次介绍这一点。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境正常。将任务分配给 Copilot 也是如此，它会像软件工程师一样处理任务。

云智能体使用 [GitHub Actions][github-actions] 执行任务。你可以创建[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让它在智能体开始工作前运行，以便安装必要的开发工具和依赖项。为简化实验流程，此工作流已预先配置。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用所需的迁移和填充后的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为执行这些步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 contents: read 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件看起来与其他 GitHub 工作流类似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这便于测试工作流是否成功，而不必等到 Copilot 运行它。

## 添加文档

大家都知道文档很重要，但多数项目要么文档过时，要么完全缺少文档。这类技术债务经常无人处理，会拖慢工作效率，使代码库更难维护，也让新开发人员更难加入项目。Copilot 擅长创建文档，因此很适合将这项任务分配给 Copilot 云智能体。它会在后台生成所需文档，之后的练习中你会回来审查成果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 任务分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会在 PR 中创建待办列表。

> [!NOTE]
> Copilot 生成 PR 待办列表可能需要几分钟。它需要先创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方案。

13. 审查列表及 Copilot 将执行的任务。
14. 向下浏览拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们有时间处理其他工作。现在利用这一特点：再将另一个任务分配给 Copilot 云智能体，然后转而编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务，它同时处理另一组。为设计团队添加相关游戏区域可能不会花太多时间，但这段时间仍可用于其他工作。现在将任务分配给 Copilot 云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

随后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理任务！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！Copilot 云智能体的工作方式类似软件工程师，因此你无需持续监控，等任务完成后再审查即可。现在转而创建并使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理。它可以帮助解决技术债务、开发新功能，或协助将代码从一种框架迁移到另一种框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并支持我们在前面模块中探索过的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
