---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委派到云端的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们可以访问高级功能、查看内部运行信息并设置其他选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来看看其他实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解一些其他功能：管理上下文、切换模型、共享会话，以及选择是否将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会接近模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统指令、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口逐渐填满时自动压缩上下文。若要自行决定压缩时机，请使用 `/compact`。如果要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有不同偏好。Copilot CLI 支持查看并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和分享经验有助于所有人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送给他人或发布之前，先审查导出内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队无意公开的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会内容已完成。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet` 可以将独立子任务交给多个子智能体并行执行。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务交给异步工作的 Copilot 云智能体；它可能会创建拉取请求。

这些命令是可选的，因为它们可能创建额外的工作树或远程工作。请先准备范围明确的新任务，并通过常规工作流审查结果。若要深入了解异步智能体工作，请继续学习[云智能体研讨会][cloud-workshop]。

## 总结与后续步骤

使用 Copilot CLI 斜杠命令可以进行配置、共享会话并查看内部运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 查看了 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览了可用模型。
- 了解了如何通过 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树和委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索！接下来，[回顾已学内容][next-lesson]并了解后续学习资源。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
