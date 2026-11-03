---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也要修改多个文件并执行一些操作，例如运行测试。作为开发人员，你通常需要查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再次更新……工作往往会不断延伸。

这正是 Copilot 代理模式发挥作用的地方。

Copilot 代理模式可以在集成开发环境 (IDE) 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来，了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但代理模式和云代理面向不同的使用场景。代理模式在 IDE 中执行任务，支持快速反馈和交互。云代理则像团队中的协作伙伴一样异步工作，通过问题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 代理模式探索项目、确定相关文件并协调多处更改。
- 使用 GitHub Copilot 代理模式跨用户界面 (UI) 和数据层实现新功能。
- 在将更改合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一项练习已经添加了发行商辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改之前，先浏览 Tailspin Toys 网站，了解它目前的功能。

该网站是一个棋盘游戏众筹平台，采用开发人员主题。用户可以浏览游戏列表并查看游戏详情。网站是一个单体 Astro 应用，会在构建时将页面渲染为静态超文本标记语言 (HTML)。页面通过 frontmatter 使用 Drizzle 对象关系映射 (ORM) 直接查询本地 SQLite 数据库，不需要单独的后端应用程序编程接口 (API) 或客户端用户界面 (UI) 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过精简、范围明确的 Astro `<script>` 和标准文档对象模型 (DOM) API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一项练习结束后，你应该已经切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端窗口。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示一个横幅并提供类似以下内容的地址：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，网站服务器现在由 Codespace 托管。

### 浏览网站

网站启动后，可以浏览现有功能。网站的主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、说明、publisher 和类别等信息。

## 使用 Copilot 查看积压问题

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。不同练习所用的模式和模型可能不同，相关步骤会在需要时说明。

网站的初始版本可以正常使用，但还需要添加更多功能。先查看积压的问题。使用项目模板创建项目时，系统会自动创建一组待处理问题。让 GitHub Copilot 为你列出这些问题。

1. 在 Chat 视图的代理下拉列表中选择 **Agent**。**Agent** 代理能够自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊的作者并不偏好某一种模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此示例中采用了这个模型，希望让代码建议相对一致。不过，由于大语言模型 (LLM) 的输出具有概率性，你可能会看到与示例不同的建议。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的模型上下文协议 (MCP) 命令，但仍应该能够完成任务。

3. 向 Copilot 发送以下提示，询问存储库中的积压问题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有问题的命令。
5. 查看生成的问题列表。

注意 Copilot 还根据它判断对用户最有用的程度，为问题排定了优先级。

## 查看指令文件

让代理生成代码之前，先查看用于向 Copilot 提供工作上下文的指令文件。这里将使用 [user interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro。
     - Tailwind CSS。
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用指令，将复杂内容拆分为更易管理的部分，并为 Copilot 提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项更新：

- 在数据层 (`src/lib/`) 中添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为的测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前需要运行测试并确保测试通过。Copilot 代理模式可以代为处理这些任务。现在开始添加此功能。

1. 可以继续当前与 Copilot 的对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 作为指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉列表中仍选中 **Agent**。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍为模型选择了 **Claude Sonnet 4.5**。
5. 使用以下提示，要求 Copilot 根据积压列表中的相关问题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与目标功能相关的文件。你应该会看到它查找数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 选项，因此不必在工作进行期间选择它们。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停一段时间，甚至有测试暂时失败。这没有关系。Copilot 会反复生成代码并运行测试，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你了解了如何：

- 使用 Copilot 代理模式探索项目、确定相关文件并协调多处更改。
- 使用 GitHub Copilot 代理模式跨用户界面 (UI) 和数据层实现新功能。
- 在将更改合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

现在，使用 [Playwright MCP 服务器测试功能][next-lesson]，然后为其创建拉取请求。

### 扩展探索练习 — 实现分页

随着游戏列表不断扩大，网站需要支持分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳做法，包括使用现有指令文件。
- 考虑分页方式：允许用户选择每页大小，还是将其设为固定值。
- 编写提示时，提供 Copilot 所需的指导，确保实现符合预期。
- 你可能需要与 GitHub Copilot 多次交互，要求修改并提供上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
