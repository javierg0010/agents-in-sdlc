---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三个不同任务，以改进用户体验并添加功能。云智能体虽以异步、自主的方式工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，你可以查看所有有未关闭拉取请求 (PR) 的智能体任务，了解已执行的操作，还可以引导正在进行的会话。

本课将：

- 浏览智能体页面，监控云智能体任务；
- 引导正在进行的会话，请求添加其他功能。

## 场景

将创建高对比度模式的任务分配给智能体后，团队意识到也可以同时添加亮色模式。既然正在更新网站样式并添加切换功能，同时实现亮色模式也很合理。现在需要引导智能体，使其除了高对比度模式，也添加亮色模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的任务当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档；
   - 为修改游戏生成 API；
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作，例如：
   - 从存储库检出代码；
   - 创建 Copilot 的工作环境；
   - 设置 MCP 服务器；
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 具体步骤会根据 Copilot 当前的工作状态和所采用的方法而有所不同。

4. 同时留意右侧出现的拉取请求 (PR) 窗格。在此可以查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

现在已经了解当前正在执行的任务，接下来要求 Copilot 在实现高对比度模式时也添加亮色模式。

1. 选择有关添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。出现类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    添加高对比度模式时，也请添加亮色模式。此模式也应有一个开关，供用户选择所需的显示模式。
    ```

    ![智能体页面中的云智能体任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 对提示的确认，以及它如何将此要求纳入工作流程。

## 让 Copilot 继续处理任务

和之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在进行的步骤，然后将新要求纳入后续工作流程。

与之前一样，这需要几分钟。此时可以暂停一下，回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以在此跟踪所有进行中和已完成的任务、审查正在执行的工作，还可以重定向正在进行的任务，以调整范围或提供额外指导。

你了解了以下内容：

- 使用 mission control 和智能体页面监控云智能体任务；
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进它创建的拉取请求][next-lesson]。

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
