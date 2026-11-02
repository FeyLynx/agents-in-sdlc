---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多项改善用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在检查代码更改，并在需要时向 Copilot 提供反馈，进一步完善成果。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在允许团队其他成员参与审查前，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改。

## 安全与 GitHub Copilot 云智能体

Copilot 云智能体会在无人监督的情况下异步执行任务，因此设置了一些安全限制，以确保操作安全：

- Copilot 只能读取存储库，并且只能写入它用于处理任务的分支。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，访问外部资源受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），其中包含添加到代码中的文档。你将通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或内容更完善的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟，可以稍作休息或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    检查更改时，留意 TSDoc 文档注释、TypeScript 约定和文件头注释。这些要求来自你在练习 2 开始时审查的自定义指令文件。

6. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准的指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应能在拉取请求的检查区域看到工作流已加入队列。正常情况下，单个 Astro 应用的项目检查都会通过。完成这些检查可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像 @提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，这里无法提供固定的提示文本。你可以要求 Copilot：

- 在每个代码文件顶部添加简短说明用途的头部注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建说明 Astro 应用结构的 README。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上述建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 执行修改。注意 Copilot 会启动新会话来应用更改。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

你可以持续迭代，直到满意为止。完成后，可将 PR 从草稿状态改为就绪，然后合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似于 **Show related games on the game details page** 或内容更完善的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准的指示器。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应能在拉取请求的检查区域看到工作流已加入队列。正常情况下，单个 Astro 应用的项目检查都会通过。完成这些检查可能需要几分钟。
9. **可选：** 也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！和之前一样，可以通过评论请求云智能体进一步修改。例如，可以调整显示的相关游戏数量，或要求添加文件头注释和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令！）。和之前一样，在 **Conversation** 选项卡中添加评论即可请求 Copilot 修改；它会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含你在练习 3 中分配的高对比度模式，以及在练习 4 的 mission control 中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似于 **Add high contrast mode to website** 或内容更完善的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，尤其关注以下方面：
   - 用于切换模式的 UI 组件。
   - 使用本地存储持久保存用户偏好的方式。
   - 高对比度和浅色模式相关的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准的指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应能在拉取请求的检查区域看到工作流已加入队列。正常情况下，单个 Astro 应用的项目检查都会通过。完成这些检查可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认它们正常工作且在页面重新加载后仍能保留设置。

请注意，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进空间，可以像之前一样在评论中提及 **@copilot**，请求它进行修改。

## 可选练习 — 继续委派任务

将真实待办事项交给云智能体处理，自己转而处理其他工作，是云智能体最适合的使用方式。可以在自己的存储库中创建更多议题并分配给 Copilot，养成这种工作习惯。以下是一些建议：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体练习系列。在这些课程中，你：

- **检查了存储库自带的自定义指令文件，** 以便稍后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体，** 并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义智能体，** 实现高对比度和浅色模式切换等无障碍功能。
- **使用 Copilot 智能体页面作为 mission control，** 监控并引导无障碍会话。
- **审查并迭代改进云智能体的拉取请求，** 通过提及 `@copilot` 请求修改并批准工作流。

## 回顾与后续步骤

云智能体练习系列已完成。若要继续探索，其他练习系列会通过不同环境补充本系列的内容：

- 🖥️ **[VS Code 练习系列](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习系列](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体集成。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
