---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。在打开拉取请求 (PR) 之前，应该先确认它能在浏览器中正常工作。无需亲自逐一操作应用，而是连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能；随后发布分支并创建 PR。

本练习将介绍如何：

- 了解模型上下文协议 (MCP)，以及 MCP 服务器如何通过新工具扩展 Copilot 的功能。
- 在工作区中添加 Playwright MCP 服务器。
- 让 Copilot 使用该服务器，在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当代理模式能够访问编辑器之外的资源时，其功能会更强大。MCP 是 Copilot 访问这些资源的方式，是一种让代理与外部工具和服务通信的标准。

![代理模式的内部工作原理及其与上下文、大语言模型 (LLM) 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）交互的示意图](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为人工智能 (AI) 代理提供了与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时与外部工具和服务通信，利用资源访问最新信息，并使用工具代表你执行操作。

MCP 服务器负责访问这些工具和资源，它充当 AI 代理与外部工具和服务之间的桥梁。MCP 服务器负责管理 AI 代理与外部工具之间的通信，例如现有应用程序编程接口 (API) 或 npm 包等本地工具。每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 代理可以创建存储库、更新现有存储库，以及管理问题和拉取请求。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 代理可以访问网页、填写表单和选择按钮。

此外，还有许多其他 MCP 服务器，提供不同的工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 出于安全考虑，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查其源代码、验证发布者并考虑相关安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` 条目由项目模板提供，因此上一项练习中 Copilot 可以读取积压的问题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过超文本传输协议 (HTTP) 连接服务器，Copilot 首次使用其中某个工具时，你需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可控制的浏览器，正好可用于测试刚刚实现的功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 条目旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器。由于 Codespace 没有可显示浏览器窗口的桌面环境，因此必须使用此标志。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装了 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器。Copilot 首次需要使用服务器提供的工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要使用服务器工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 驱动真实浏览器。你无需亲自操作应用来验证更改；代理可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察到的情况。你无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着代理会读取带有标签的结构化元素（按钮、链接和列表项），与辅助技术的处理方式相同。因此，快速功能检查也能兼顾基本的无障碍检查。

服务器已连接且应用正在运行后，请求 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成测试，并报告结果。根据问题中的验收标准检查它的总结。如果发现异常，可以继续提问，或让 Copilot 在创建拉取请求之前修复代码。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 运行。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果它报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，让团队审查。首先发布 `filtering-vscode` 分支。

1. 前往 Codespace 中的 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如在 github.com 上创建，或使用 GitHub 命令行界面 (CLI)。不过，你已经在使用 GitHub Copilot，因此可以让它代为创建 PR。它可以查找相关问题并创建与该问题关联的 PR。

1. 前往 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在代理下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 要求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证，请按提示操作。
5. 留意 Copilot 如何搜索问题、找到正确的问题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中你：

- 了解了模型上下文协议 (MCP)，以及 MCP 服务器如何通过新工具扩展 Copilot 的功能。
- 在工作区中添加了 Playwright MCP 服务器。
- 使用该服务器，在浏览器中手动测试筛选功能。
- 发布了分支，并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等重点任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
