---
title: "练习 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一分支。现在应一起审查这些更改并创建拉取请求。星级评分 PR 是你手动合并的；这一次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 在常规项目中，可能会将功能、指令更新、技能更新和 QA 智能体拆分成几个独立的 PR。为了简化研讨会流程，筛选和质量工作流一直使用同一对话和分支，所有更改都会进入这个 PR。

本课将：

- 了解 Agent Merge 以及它如何自动管理合并流程；
- 检查完整的功能更改和验证证据；
- 创建筛选功能 PR；
- 审查完成后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你使用 Copilot 规划、实现并验证了一项功能。现在 Tailspin Toys 希望自动完成剩余 PR 工作，同时由开发人员掌控合并授权。

## 认识 Agent Merge

**Agent Merge** 可自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的事项，例如修复持续集成 (CI) 检查失败、响应审查评论，以及在需要时变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

到目前为止，你都是自己选择 **Merge pull request**。Agent Merge 可以代为执行这项操作。启用 Agent Merge 前，请先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码都已完成，现在一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入 `/agent`，打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、存储库设置或权限要求。如果遇到阻碍，请阅读报告的原因，并在存储库允许时手动完成审查过的合并。

## 总结和后续步骤

你已自动完成开发流程中的多个部分，包括生成代码、测试和验证代码，现在还包括拉取请求流程。本课中你：

- 了解了 Agent Merge 以及它如何自动管理合并流程；
- 检查了完整的功能更改和验证证据；
- 创建了筛选功能 PR；
- 审查完成后启用 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型和共享，并了解可选的云端委托。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
