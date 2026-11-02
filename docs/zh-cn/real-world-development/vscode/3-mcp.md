---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求之前，应确认它在浏览器中确实有效。你将连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，而不是亲自逐项操作应用；随后发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP)，以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当智能体模式能够访问编辑器以外的资源时，它的能力会大幅增强。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信。它是智能体与外部工具和服务交互的一种标准方式。

![示意图展示智能体模式的内部工作方式，以及它如何与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）交互](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体与外部工具和服务提供了通信方式。借助 MCP，AI 智能体可以实时访问最新信息（通过资源）并代表你执行操作（通过工具）。

AI 智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器负责管理智能体与外部工具之间的通信，例如现有 API 或 npm 包等本地工具。每个 MCP 服务器都会提供一组可供 AI 智能体访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体可以创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：提供使用 Playwright 进行浏览器自动化的能力，让 AI 智能体可以访问网页、填写表单和选择按钮。

此外还有许多提供不同工具和资源的 MCP 服务器。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 从安全角度来看，对待 MCP 服务器应像对待项目中的其他依赖项一样。使用前请仔细审查源代码、验证发布者，并考虑安全影响。只使用你信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应能看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板预先配置了 `github` 条目，因此 Copilot 能在上一练习中读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容；VS Code 会通过 HTTP 连接，并在 Copilot 首次使用其工具时让你使用 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可控的浏览器，正好用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列。文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程的 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志让 Playwright 在不显示窗口的情况下运行浏览器，这对于没有桌面环境的 codespace 是必要的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要使用某个服务器的工具时，VS Code 会启动它，并要求你确认是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它立即启动服务器，确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要使用其工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 驱动真实浏览器。你无需亲自在应用中逐项操作来检查更改；智能体可以打开页面、导航、应用筛选条件并读取结果，然后总结观察到的内容。无需离开对话即可快速验证功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着智能体会基于带标签的结构化元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式类似。因此，快速的功能检查也能顺便完成基础的无障碍检查。

连接服务器并启动应用后，使用以下提示让 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行测试并报告结果。根据议题中的验收标准审查总结；如果发现异常，请提出后续问题，或让 Copilot 修复代码后再创建拉取请求。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队审查更改。首先发布 `filtering-vscode` 分支。

1. 打开 codespace 中的 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，包括使用 github.com 和 GitHub 命令行界面 (CLI)。既然你正在使用 GitHub Copilot，就让它帮忙创建 PR 吧！它可以找到相关议题并创建关联该议题的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认智能体下拉菜单中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你使用 GitHub 完成身份验证；请按提示操作。
5. 查看 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接，审查拉取请求，但**暂时不要合并**。

## 总结与后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP)，以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 在发布前使用它通过浏览器手动测试筛选功能。
- 发布了分支，并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
