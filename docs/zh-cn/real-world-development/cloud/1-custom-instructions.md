---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解某些背景信息，就需要确保 Copilot 能获取这些上下文。[指令文件][instruction-files]用于提供这些指引，让 Copilot 不仅知道你要它做什么，也能了解你希望它如何完成。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 实验不同，本实验不会先后运行提示词来展示差异——Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查本实验中云智能体生成的拉取请求时，你会看到指令文件发挥的作用。

## 指令文件

### 场景

和任何优秀的开发团队一样，Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它是一项强大的功能，可以引导 Copilot 提供更符合需求的建议和代码片段。你可以为整个存储库或特定文件类型创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：单个指令文件，适用于存储库中的**每个**请求。此文件应包含项目级信息，例如适用于 Copilot 大多数聊天或 CLI 请求的上下文，包括技术栈、项目概述、最佳实践和全局指引。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型创建的文件。你可以为特定编程语言（如 TypeScript 或 Astro），或特定任务（如创建 UI 组件或一组单元测试）提供指引。

> [!NOTE]
> 在 IDE 中工作时，指令文件仅用于 Copilot Chat 代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

关于如何创建指令文件的完整介绍超出了本工作坊的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 在 `copilot-instructions.md` 中集中说明项目级指引，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件提供针对文件类型（单元测试、Astro 组件、数据层）或特定任务的指引。
- 使用自然语言，确保指引清晰，并提供代码应如何编写（以及不应如何编写）的示例。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过实践，你会找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护一套完善的指令文件。浏览此项目中的文件时，你可能会注意到，其中包含多种任务的指引，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同界面提供的方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前界面的相关课程会在适当位置说明。
>
> 想寻找模板或入门示例？可以浏览 [awesome-copilot][awesome-copilot]，其中收录了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件——其中包含一个核心 `copilot-instructions.md` 文件，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，了解项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，决定哪些文件适用这些指令。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 留意此项目中有关创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意它链接到的其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供参考示例。（其中的路径相对于指令文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求在代码中添加文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来是本课中唯一需要编辑文件的步骤：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释，并为每个文件添加注释头。云智能体将在本实验后续处理你分配的议题时使用此规则。

编辑前，先创建用于工作的分支（云智能体会从议题目标分支读取指令，但仍要单独提交这些更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库的编码标准添加文档要求，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。注意该部分记录了项目的编码标准，但尚未规定代码文档要求；这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建指令文件提供全局指引，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指引。存储库级文件适合放置项目级标准，例如刚刚添加的文档注释规则。

## 提交、推送并合并指令更新

云智能体会从议题的目标分支读取指令。下一练习中，你将把议题分配给 Copilot；Copilot 会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云智能体才能获取这些指令。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。下一练习中，云智能体处理分配给它的议题时就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main` 分支，也可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了让工作坊更接近真实项目中的工作方式。

## 总结与后续步骤

你了解了 Copilot 如何获取此项目的指令文件上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。接下来几节中，云智能体创建拉取请求时，你会看到这些标准发挥作用。

接下来[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
