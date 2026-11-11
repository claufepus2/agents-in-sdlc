---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 一起完成了多项任务，旨在改善用户体验并添加功能。你要求 Copilot 为代码添加文档、为设计团队构建可供迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在来检查代码更改；必要时向 Copilot 提供反馈，进一步完善工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。接下来，在为团队其他成员启用审查前，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改。

## 安全和 GitHub Copilot 云智能体

Copilot 云智能体会异步且无人监督地执行任务，因此我们设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 只有存储库的读取权限，并且**只能**写入用于编写代码的分支。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流都必须经过人工批准才能运行。
- [默认情况下，云智能体无法访问外部资源](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。你将使用 GitHub.com 上的标准 PR 界面完成此操作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下，Copilot 只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息一下或回顾至今学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到提示，表明有一些工作流正在等待批准。
8. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流已加入拉取请求的检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在拉取请求评论或代码内联评论中提及 Copilot。Copilot 会读取这些评论，并启动另一个会话来处理请求。由于结果具有非确定性，我们无法给出固定的提示词。可以要求 Copilot 更新以下内容：

- 为每个代码文件开头添加简短注释，说明文件的用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的想法，或提出其他你希望 Copilot 在代码库文档中完成的更改。
2. 选择 **View Session**，观察 Copilot 执行工作。留意 Copilot 会启动新会话来完成更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转换为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来返回 Copilot 创建的 PR，查看它如何解决在游戏详情页显示相关游戏的议题。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到提示，表明有一些工作流正在等待批准。
7. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流已加入拉取请求的检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 并启动终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！和之前一样，你可以通过迭代来请求 Copilot 云智能体更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住——分配此任务时，你**尚未**更新自定义指令！）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查由无障碍自定义智能体实现的无障碍功能。此 PR 应该同时包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的 mission control 中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其关注以下内容：
   - 用于切换模式的 UI 组件。
   - 使用本地存储持久化用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到提示，表明有一些工作流正在等待批准。
8. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流已加入拉取请求的检查队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 并启动终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能符合预期，并且在页面重新加载后仍然保留设置。

请注意，无障碍自定义智能体如何帮助 Copilot 根据无障碍最佳实践实现这些功能。如果发现无障碍问题或有改进建议，可以像之前审查其他 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习——继续委派任务

云智能体最适合处理待办列表中的真实任务，让你可以将注意力转移到其他工作。为培养这种工作习惯，可以为存储库创建几个议题并分配给 Copilot。可以考虑：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体实验。在这些课程中，你：

- **检查了此存储库自带的自定义指令文件**，以便之后了解它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**处理无障碍功能，添加高对比度和浅色模式切换开关。
- **将 Copilot 智能体页面用作 mission control**，监控并引导无障碍会话。
- **审查并迭代优化云智能体创建的拉取请求**，标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

你已完成云智能体实验。如果想继续探索，其他实验可以补充此处学到的内容：

- 🖥️ **[VS Code 实验](../../vscode/)** — 在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 实验](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge 以及可选的 `/delegate` 云智能体桥接，交付两个经过审查的更改。

也可以在自己的存储库中尝试以下后续操作：

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
