---
title: "练习 4 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应确保 Copilot 可以获取相应指导。[指令文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将查看 Copilot 使用的指令、添加文档标准，并将标准应用到代码中。

本课将：

- 了解存储库指令和按路径限定的指令文件如何传达给智能体；
- 更新指令文件，确保遵循编码标准；
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 和优秀的软件开发团队一样，对开发实践有一系列准则和要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口契约，相关代码发生更改时也应更新注释。
- 应保留现有格式设置和 lint 指导。

通过指令文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和要求。你可以指定编码规范、库以及注释风格等偏好，为建议和代码片段提供明确方向。指令既可以适用于整个存储库，也可以针对特定类型的文件，作为任务级上下文使用。

指令文件分为两种：

- `.github/copilot-instructions.md` 是单个指令文件，会随该存储库的**每个**请求发送给 Copilot。文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及支持情况因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

初始项目已经附带了一些指令文件。更改之前，先浏览现有内容，以便之后观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。这些指令适用于你与此存储库中 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览。这里有 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它用于设置指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并提供示例供 Copilot 参考。

## 更新指令文件以符合团队指导

现有文件是不错的起点，但仍有缺漏。现在修改核心文件 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 将以下内容添加为最后一条项目：

   ```markdown
   - 所有新编写的 TypeScript 代码都应包含 TSDoc 注释，以提供文档说明。
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍然打开，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了指令和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。留意新增的 TSDoc 注释，并确认它们准确说明代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何从指令文件获取上下文，并将新的标准应用于功能中。具体来说，你：

- 了解存储库指令和按路径限定的指令文件如何传达给智能体；
- 更新指令文件，确保遵循编码标准；
- 观察指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件和其他资源集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
