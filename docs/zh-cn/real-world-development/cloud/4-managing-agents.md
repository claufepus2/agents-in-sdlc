---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

最近两节练习中，你要求 Copilot 云智能体处理了三项不同任务，旨在改善用户体验并添加功能。云智能体虽然设计为异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，你可以查看所有有未关闭拉取请求 (PR) 的智能体任务，了解已执行的操作，甚至引导进行中的会话。

本课将学习如何：

- 浏览智能体页面并监控云智能体任务。
- 在会话进行过程中引导智能体添加新功能。

## 场景

将任务分配给智能体以实现高对比度模式后，团队意识到也可以趁机添加浅色模式。既然已经开始更新网站样式并添加切换功能，顺便实现这个功能也很合理。现在引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 留意主面板和左侧面板中的任务列表。你应该会看到已分配给 Copilot 的任务，包括：
    - 更新代码库的文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作。这些操作可能包括：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成所分配任务的各个步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作状态和所采用的方案而异。

4. 另外，留意右侧显示的拉取请求 (PR) 面板。你可以在此查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

查看当前正在执行的任务后，现在请 Copilot 在实现高对比度模式的同时添加浅色模式切换开关。

1. 选择提及添加高对比度模式的会话。确切标题会因 Copilot 采用的名称和当前工作状态而异。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到状态显示已完成设置并开始工作。看到类似下方的消息时，就说明已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务屏幕截图，突出显示了“Steer active session while copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认提示词，并将其纳入工作流程。

## 让 Copilot 完成工作

和之前一样，Copilot 会开始处理更新后的任务！它会在完成你发送消息时正在执行的具体步骤后，将新请求加入工作流程。

和之前一样，此过程需要几分钟。此时可以暂停一下，回顾至今学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面——监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以使用这个 mission control 跟踪所有正在进行和已完成的任务、审查已完成的工作，甚至重定向进行中的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 浏览 mission control 和智能体页面，监控云智能体任务。
- 重定向进行中的会话，要求添加新功能。

Copilot 正在完成功能开发，接下来可以[迭代优化 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
