---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]，为代码生成提供指导。

本练习将介绍如何：

- 通过存储库自定义指令和按路径限定范围的指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在当前指令生效时，生成用于筛选功能的首个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能切片提交到 `main`，供 Copilot 在下一项练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分设定的标准不一致。Copilot 的输出具有非确定性。本练习的重点是观察更新指令后行为变化的趋势，而不是要求输出逐字逐句一致。

## 指令文件

### 场景

Tailspin Toys 像其他优秀的软件开发团队一样，制定了一系列开发规范和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应采用深色模式并具有现代感。
- 应以 TypeScript 文档注释 (TSDoc) 的形式在代码中添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，可以确保 Copilot 了解这些要求，并据此完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。借助自定义指令，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至注释风格。指令既可以适用于整个存储库，也可以针对特定类型文件提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库提出请求时，都会将这个唯一的指令文件发送给 Copilot。文件应包含适用于大多数聊天或命令行界面 (CLI) 请求的项目级信息，例如所用技术栈、项目概况、最佳做法和全局指导。
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建这类文件，例如为 TypeScript 或 Astro 提供指南，或指导创建 UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳做法

完整讨论如何创建指令文件超出了本练习的范围。不过，示例项目展示了一种有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明应该和不应该采用的写法。

创建指令文件和使用 AI 都没有唯一的方法。通过不断试验，你会找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览此项目中的文件时，你可能会看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。各个界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应的练习会在适用时介绍具体操作。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收录了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览此项目中的自定义指令文件

花点时间阅读此存储库提供的指令文件：其中有一个核心 `copilot-instructions.md` 文件，以及若干适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 留意此项目中有关创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。路径相对于指令文件本身，而不是存储库根目录，因此可以拆分较大的指令集以便复用，并为 Copilot 提供参考示例。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前尚未要求添加代码文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

接下来按照最佳做法，为更改创建一个分支。

1. 返回上一项练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令之前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。不同练习所用的模式和模型可能不同，相关步骤会在需要时说明。

为了观察自定义指令的效果，先在当前指令文件生效时发送提示。稍后更新指令后，再发送相同的提示以比较结果。

1. 关闭前面练习中打开的编辑器选项卡，确保 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的代理下拉列表中选择 **Agent**，以便 Copilot 可以修改文件。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个经过类型定义的函数，以 `db` 客户端作为第一个参数，并返回经过类型定义的 publisher 数组。这些做法来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> 由于 Copilot 的输出具有概率性，它有时可能会在没有要求的情况下添加文档注释。即使如此也没有问题；重点是观察更新指令后的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加代码文档标准，以改进 Copilot 生成的建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，它应该位于文件开头附近（约第 27 行）。留意该部分介绍了项目的编码标准，但尚未规定代码文档要求。这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建指令文件以提供全局指导，也可以针对特定语言、文件类型或任务创建 `*.instructions.md` 文件。存储库级文件适合放置项目范围内的标准，例如刚刚添加的文档注释规则。

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话内容。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意现在建议的文件是否以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意现在建议的函数是否包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

现在，你已引导 Copilot 遵循新的项目标准，并将其应用于实际代码，为下一项练习打下基础。

## 提交指令并推送分支

指令文件和其他资源一样，也需要使用源代码管理。现在，将指令更新和代码更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，需要创建拉取请求并关联积压列表中的文档标准问题。可以手动完成，但也可以让 Copilot 使用已连接到项目的 GitHub 工具代为处理。向 Copilot 提示，让它查找问题、创建用于解决该问题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已经为工作区配置了 GitHub 工具，因此无需额外设置。Copilot 首次使用 GitHub 工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。稍后的练习会介绍这种连接的工作原理，即模型上下文协议 (MCP)。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux），打开 Copilot Chat 视图，并确保在代理选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的问题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 将开始查找问题并创建 PR。
5. 如果 Copilot 提示你**允许**它代表你执行 GitHub 操作，请检查命令并酌情选择 **Allow**。
6. 创建 PR 后，要求 Copilot 合并 PR 并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示你允许它代表你执行 GitHub 操作和运行 shell 命令，请检查命令并酌情选择 **Allow**。

现在，你已经借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何从项目中的指令文件获取上下文，然后在 VS Code 中使用 Copilot Chat 来：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定范围的指令文件传递给 Copilot。
- 发送代码生成提示，并观察 Copilot 在**现有**指令下生成的内容。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同的提示，观察建议的代码如何采用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一项练习继续使用。

接下来，在[代理模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳做法][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
