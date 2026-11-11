---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

启用 GitHub Copilot 智能体模式后，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会实时在你的计算机上进行，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以内联审查每个差异、接受或拒绝单项更改，并通过后续提示词引导对话以完善或扩展工作，全程无需离开编辑器。

本练习将学习如何：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行过程中引导智能体，在高对比度模式之外添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出一项新需求——他们也希望为觉得默认主题太暗的用户添加浅色模式切换开关。无需从头开始新对话，你可以引导现有会话扩展功能。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体采取的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内联显示差异，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 在智能体工作时滚动聊天内容，查看每次工具调用。你会看到文件读取、文件编辑以及它请求运行的终端命令。
3. 智能体提出文件编辑时，聊天中会内联显示差异：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的完整文件列表。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式的实现。完成后应会显示总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式就绪后，你将继续在同一对话中添加浅色模式切换开关。由于继续使用同一聊天会话，智能体会保留先前工作的上下文，包括修改的组件、使用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，探索已经完成的更改并提出浅色模式的新增内容。
3. 新编辑出现时，像处理高对比度模式一样逐项审查，并对每个差异选择 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期（例如尝试以你不喜欢的方式重构开关 UI），可以选择聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清性的后续消息。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示词要求调整（“简化 CSS——使用 CSS 变量，而不要重复编写选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板中”）或扩展范围（“为两个开关添加键盘快捷键”）。

## 审查工作文件

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 检查所有已更改文件的列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认其中体现了自定义智能体推荐的无障碍实践，例如 ARIA 属性、键盘导航、语义化 HTML 和通过本地存储持久化设置。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课中会发布这些更改。

## 总结与后续步骤

你使用 Copilot Chat 智能体模式监控并引导正在进行的会话——逐项审查文件编辑、接受或拒绝更改，并通过后续提示词扩展工作，同时保留上下文。

本课介绍了以下内容：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 通过后续提示词在会话进行过程中引导智能体。

在[下一课][next-lesson]中，你将把无障碍功能作为拉取请求发布，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
