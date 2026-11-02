---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员的工作流程往往包括查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次更新……这样的步骤可能会不断重复。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。接下来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增加，因此需要让用户按类别和发行商筛选。你已在上一练习中添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先浏览 Tailspin Toys 网站，了解它目前的功能。

这是一个以开发者为主题、为桌游提供众筹服务的网站，用户可以浏览游戏列表并查看游戏详情。整个网站是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库；它没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则使用少量作用域受限的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端窗口。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，从终端选择 localhost URL 会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，网站服务器现在通过该地址运行！

### 浏览网站

网站运行后，可以探索它的功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；每个相关步骤都会说明具体要求。

网站的初始实现已经可以正常工作，但我们还希望添加新功能。先查看待办事项。根据模板设置项目时，系统已自动创建一组议题；现在让 GitHub Copilot 帮你查看这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将它列在说明中，希望让代码建议保持相对一致。不过，LLM 具有概率性，你可能会收到与说明不同的建议。这完全正常，符合预期。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行命令以列出所有议题。
5. 检查生成的议题列表。

请注意，Copilot 还会根据它认为对用户最有用的程度为议题排序。

## 审查指令文件

让智能体生成代码前，先查看将用于向 Copilot 提供上下文的指令文件。接下来会使用[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中说明了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 浏览文件中有关如何添加功能的整体指导，包括：
   - 应用架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用指导、将复杂内容拆分成更容易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项独立更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 为筛选行为添加或更新测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前还必须运行并通过测试。Copilot 智能体模式可以替你执行这些任务，现在就来添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开启新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单仍选中 **Agent**。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并查找相关文件。它应该能找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 选项，因此不必在工作进行时立即选择。

7. Copilot 提示时，选择 **Continue** 以运行测试。

   ![Copilot Chat 面板中的对话框提示用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且没有检测到错误。

   ![Copilot 智能体模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 记住，始终都应审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，探索新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习中，我们使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了新功能。我们学习了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 让 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，然后为它创建拉取请求。

### 额外探索练习 - 实现分页

随着游戏列表不断扩大，网站需要支持分页。使用本练习中学到的技能，提示 Copilot 为网站添加分页。编写提示时可考虑以下事项：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页方式：允许用户选择每页条目数，还是使用固定数量。
- 编写提示时，提供足够指导，让 Copilot 按照你期望的方式实现功能。
- 你可能需要与 GitHub Copilot 多次交互，要求它修改并提供上下文。这正是使用 Copilot 时的正常工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
