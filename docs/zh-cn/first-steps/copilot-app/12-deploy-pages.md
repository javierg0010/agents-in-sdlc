---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并验证 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问地址。Copilot 可以准备工作流并引导部署，但你仍需负责审查更改并确认网站已上线。

本课将：

- 让 Copilot 配置 GitHub Pages 部署；
- 在合并前审查工作流和拉取请求；
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已在 GitHub 存储库中，但用户还没有方便的试用方式。使用 GitHub Pages 发布单文件网站，无需添加服务器或运行时依赖，即可通过公共 URL 访问。

## 让 Copilot 准备部署

> [!WARNING]
> 创建拉取请求可能会提交并推送代码。发送此提示前，请检查批准设置和目标存储库。如果选择 **Approve all**，不要认为系统还会显示单独的确认提示。

在 Copilot app 中打开 `space-quiz` 会话，并发送以下提示：

```plaintext
为此存储库准备 GitHub Pages 部署。创建 GitHub Actions 工作流；如有需要，先构建网站，并在更改进入默认分支后将网站部署到 GitHub Pages。如果存储库还没有 README，请添加一个，介绍应用及如何在本地运行。使用这些更改创建拉取请求，并报告 PR 链接以及任何必需的 Pages 设置或权限。不要合并、启用自动合并、更改 Pages 设置或进行部署。完成后停止并等待我审查。
```

## 审查工作流和拉取请求

1. 确认只有在存储库原本没有 README 时，才添加了 README。
2. 审查拉取请求的更改，包括工作流中的构建和部署步骤、触发器及权限。
3. 确认工作流从默认分支部署，而不是从尚未合并的拉取请求部署。
4. 如需修正，请提出要求并审查更新后的差异。授权合并前，等待必需的拉取请求检查通过。

## 授权合并和部署

> [!WARNING]
> 以下提示授权智能体合并已审查的拉取请求、更改存储库 Pages 设置并发布网站。只有在完成审查并确认存储库和拉取请求正确无误后，才发送此提示。根据批准设置，这些操作可能无需再次确认。

将 `<PR_NUMBER>` 替换为已审查的拉取请求编号，然后单独发送以下提示：

```plaintext
我已审查此存储库中的拉取请求 <PR_NUMBER>，并授权合并和部署。使用 GitHub Actions 启用 GitHub Pages，仅在必需检查通过后合并此拉取请求，并监控部署直至成功。报告已上线网站的最终 URL。如果你没有启用 Pages 或合并 PR 的权限，请明确说明我需要批准或执行的操作；网站上线前不要报告成功。
```

![三阶段示意图：已合并的 space-quiz 拉取请求；包含 Build website、Upload artifact 和 Deploy to Pages 步骤且成功运行的 GitHub Actions Deploy website 工作流；以及 yourname.github.io/space-quiz/ 上线的 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## 验证部署

1. 跟进 GitHub Actions 部署工作流，直到运行成功。
2. 打开报告的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有部署成功且网站可以访问后，才能认为任务已完成。

## 总结和后续步骤

Space Quiz 现在已作为网站上线。你亲自验证了部署，而不是只依赖智能体的报告。继续学习[第 13 课：回顾和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
