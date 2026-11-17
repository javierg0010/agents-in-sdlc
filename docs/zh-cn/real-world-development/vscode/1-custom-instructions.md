---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]是提供代码生成指导的重要方式。

本练习将：

- 了解项目专属上下文、编码规范和文档标准如何通过存储库自定义指令和按路径限定的指令文件传达给 Copilot；
- 使用当前指令生成筛选功能的第一个数据切片（publisher 辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 重新运行相同提示，观察生成的代码如何采用新标准；
- 提交指令更新和筛选功能切片到 `main`，供 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设置的所有标准。Copilot 的行为具有不确定性。本练习旨在观察更新指令后行为的*整体变化趋势*，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 和优秀的软件开发团队一样，对开发实践有一系列准则和要求：

- 数据层必须始终有单元测试。
- UI 应采用暗色模式，并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和要求。你可以指定编码规范、库以及注释风格等偏好，为建议和代码片段提供明确方向。指令既可以适用于整个存储库，也可以针对特定类型的文件，作为任务级上下文使用。

指令文件分为两种：

- `.github/copilot-instructions.md` 是单个指令文件，会随该存储库的**每个**请求发送给 Copilot。文件应包含项目级信息，即大多数发送给 Copilot 的聊天或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型创建。你可以为 TypeScript、Astro 等语言，或创建 UI 组件、编写单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

创建指令文件的完整讨论超出了本研讨会的范围。示例项目展示了一种有代表性的做法。总体而言：

- 在 `copilot-instructions.md` 中集中说明项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 针对文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例说明推荐或不推荐的写法。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过实践，你会找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备完善的指令文件。浏览本项目中的文件时，你会看到其中涵盖了多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前学习路径会在相关处说明。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花点时间阅读此存储库提供的指令文件：其中有一个核心文件 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览。这里有 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它用于设置一个相对于存储库根目录的 glob，以确定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 留意本项目专门针对单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并提供示例供 Copilot 参考。链接路径相对于指令文件，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求在代码中编写文档。接下来，你将添加 TSDoc 文档注释和文件注释标头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中打开的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令*之前*先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。每个练习所用的模式和模型可能不同，相关步骤会在适用处说明。

为了观察自定义指令的效果，先在当前指令文件下发送提示。稍后更新指令后，再发送相同提示进行比较。

1. 关闭前面练习中打开的编辑器选项卡，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发布商的列表，且每项都包含名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意生成的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发布商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释标头。

> [!CAUTION]
> Copilot 的行为具有不确定性，即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习重点仍是观察更新指令后行为的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以便 Copilot 生成代码时附带文档。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中记录了项目编码标准，但还没有关于代码文档的规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释标头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以放在存储库级别（`.github/copilot-instructions.md`），提供全局指导；也可以针对特定语言、文件类型或任务，使用 `*.instructions.md` 文件。你刚添加的文档注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 重新发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 专注于正确的文件。
3. 发送之前的**相同提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发布商的列表，且每项都包含名称和 ID。应用文件更改。
   ```

4. 留意现在建议的文件开头有类似以下的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发布商数据访问辅助函数。
    * 提供从数据库检索发布商信息的函数。
    */
   ```

5. 留意现在建议的函数包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发布商的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发布商对象数组。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中，下一练习将在此基础上继续。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，也通过源代码管理进行维护。现在将指令更新和分支提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新以及 Copilot 为辅助函数基础实现修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他支持文件也有更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求并将其关联到待办事项中的文档标准议题。也可以手动操作，不过 Copilot 可以使用已连接到项目的 GitHub 工具代为完成。向 Copilot 提示，让它找到议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已将 GitHub 工具连接到工作区，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成操作。后续练习会介绍这种连接所用的模型上下文协议 (MCP)。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新以及新增的发布商辅助函数基础实现：

   ```
   查找与更新指令文件相关的议题。从当前分支创建新的 PR，并说明该 PR 会关闭此议题，同时还为后续筛选工作添加了发布商辅助函数基础实现。
   ```

4. Copilot 开始查找议题并创建 PR。
5. 如果系统提示你 **Allow** Copilot 代表你执行 GitHub 操作，请审查操作并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 将其合并，并将本地分支切换回 main：

   ```
   将 PR 合并到 main。然后在本地切回 main，并拉取最新代码以保持同步。
   ```

7. 如果系统提示你 **Allow** Copilot 代表你执行 GitHub 操作和运行 shell 命令，请审查操作并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从本项目的指令文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下工作：

- 发送代码生成提示，观察 Copilot 如何根据*现有*指令生成代码；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 重新发送相同提示，观察建议的代码如何采用新标准；
- 将指令更新和辅助函数基础实现提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中应用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
