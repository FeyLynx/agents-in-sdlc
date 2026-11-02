---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体执行了三项任务，旨在改善用户体验并添加新功能。云智能体会异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，你可以查看所有带有未关闭拉取请求（PR）的智能体任务，探索执行过程，甚至在会话进行时引导智能体。

本课将学习如何：

- 浏览智能体页面并监控云智能体任务。
- 在会话进行时引导智能体添加新功能。

## 场景

分配高对比度模式任务后，团队意识到也应该添加浅色模式。由于正在更新网站样式并添加切换功能，因此顺便实现浅色模式很合适。你将引导智能体，确保它同时添加高对比度和浅色模式。

## 审查 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到你分配给 Copilot 的任务，包括：
    - 为代码库补充文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择其中一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 检出存储库代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行各项任务以完成分配的工作。

> [!NOTE]
> 实际显示的步骤会因 Copilot 当前的工作状态和处理方式而异。

4. 另请查看右侧显示的拉取请求（PR）面板。你可以在这里查看 PR 和更改的文件，以便进一步监控任务。

## 引导云智能体

现在你已了解正在运行的任务，接下来让 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。具体标题取决于 Copilot 使用的名称和任务当前状态。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到 Copilot 完成设置并开始工作。出现类似下图的消息时，就表示已开始处理。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体页面中的任务截图，突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！收到消息时，它会先完成当前步骤，然后将新请求加入工作流程。

整个过程需要几分钟，可以利用这段时间回顾已学和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，也就是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过 mission control，你可以跟踪所有进行中和已完成的任务、审查正在执行的工作，并重定向当前会话以调整范围或补充指导。

本课中，你了解了以下内容：

- 使用 mission control 和智能体页面监控云智能体任务。
- 重定向正在进行的会话，让智能体添加新功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
