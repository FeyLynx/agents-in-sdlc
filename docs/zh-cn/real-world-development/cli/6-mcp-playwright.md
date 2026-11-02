---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面提到过，编写代码不只是写代码。我们还需要处理数据、外部服务，并让 Copilot 使用更多自动化工具。MCP 服务器可以让 Copilot 超越 CLI 内置功能，使用更多工具和服务。

本课将学习如何：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 在尚未配置时添加 Playwright MCP 服务器。
- 让智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证界面更新还需要实际与界面交互。你希望 Copilot 能像用户一样使用正在开发的网站，从而进一步自动化更改流程，并更有信心地确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体与外部工具和服务提供了通信方式。借助 MCP，AI 智能体可以实时访问外部工具和服务，从而获取最新信息并代表你执行操作。

AI 智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁，每个 MCP 服务器都会提供一组可供 AI 智能体访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：提供使用 Playwright 进行浏览器自动化的能力。

此外还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，帮助发现并参与生态系统。

> [!CAUTION]
> 对待 MCP 服务器应像对待项目中的其他依赖项一样。使用前请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能符合预期。现在使用 Copilot 中配置好的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示，让 Copilot 调用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它一般会自行判断。不过，既然你知道要使用哪个工具，明确指出也有好处！这样可以提高结果的一致性，还能节省一些 token。

2. 观察 Copilot 在浏览器中执行的各项操作，确认功能是否正常。
3. 阅读报告，确保所有行为均符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，并向你报告结果。

## 总结与后续步骤

恭喜，你已通过 Copilot CLI 使用 Playwright MCP 服务器在真实浏览器中探索功能！本课中，你：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 在尚未配置时添加了 Playwright MCP 服务器。
- 让智能体驱动浏览器并探索筛选功能。

接下来，你将[创建 QA 自定义智能体][next-lesson]，把技能和浏览器工具整合到一个专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
