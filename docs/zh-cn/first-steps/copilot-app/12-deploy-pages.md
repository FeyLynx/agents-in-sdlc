---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并验证 Space Quiz 是否已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问地址。Copilot 可以准备工作流并指导部署，但仍需自行审查更改并确认网站已上线。

本课将学习如何：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已在 GitHub 存储库中，但其他人还需要一种简单的方式来试用它。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖项，即可通过公开 URL 访问。

## 让 Copilot 部署网站

在 `space-quiz` 的 Copilot app 会话中发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功运行的 GitHub Actions“Deploy website”工作流，其中包含“Build website”“Upload artifact”和“Deploy to Pages”步骤；以及已上线的 Space Quiz，网址为 yourname.github.io/space-quiz/。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示会要求智能体创建并合并拉取请求，以及更改存储库的 Pages 设置。继续操作前，请审查差异、部署权限和 GitHub 的审批提示。

## 审查工作流并验证部署

1. 确认仅当存储库中尚无 README 时才会添加。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤。
3. 仅在所需的拉取请求检查通过后合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告的 URL，确认 Space Quiz 可以正常加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有部署成功且网站可以正常加载后，才能将任务视为完成。

## 总结与后续步骤

Space Quiz 现已作为实时网站发布，你也已亲自验证部署，而不是仅依赖智能体的报告。继续学习[第 13 课：审查与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
