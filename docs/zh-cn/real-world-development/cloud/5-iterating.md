---
title: "练习 5：迭代优化 GitHub Copilot 的工作成果"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你使用 GitHub Copilot 完成了多个任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在来检查代码更改，并在需要时向 Copilot 提供反馈，进一步改进结果。

### 场景

前面多次强调过，加入生成式 AI 并不会改变软件设计和开发运维 (DevOps) 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。接下来，在为团队其他成员开启审查之前，先查看 GitHub Copilot 关于添加文档、相关游戏功能和无障碍功能的建议。

## 安全性与 GitHub Copilot 云代理

由于 Copilot 云代理会在没有监督的情况下异步执行任务，因此它受到一些安全约束，以确保操作安全。这些约束包括：

- Copilot 仅对存储库具有读取权限，对自己使用的分支具有写入权限。
- 云代理在 GitHub Actions 中运行，并在其中创建独立的临时环境进行工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议 (MCP) 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云代理生成的第一个拉取请求 (PR)，其中包含为代码添加的文档。通过 GitHub.com 上的标准 PR 界面完成此操作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 对外部资源的访问受到限制，包括对外部 MCP 服务器的调用。如有需要，可以[自定义或禁用 Copilot 云代理防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更完整标题的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟，可以趁此休息一下，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新添加的 TypeScript 文档注释 (TSDoc) 和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流在拉取请求的检查区域中排队运行。如果一切正常，单体 Astro 应用的项目检查应该会通过。完成此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动另一个会话来处理修改。由于结果具有非确定性，我们无法提供固定的提示词。可以要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建一个介绍 Astro 应用结构的 README。

1. 在评论中请求修改生成的文档，并像提及其他用户一样使用 **@copilot**。可以使用上面的建议，也可以提出其他希望 Copilot 为代码库补充的文档要求。
2. 选择 **View Session**，观察 Copilot 执行修改。注意 Copilot 会启动一个新会话来完成更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

你可以继续迭代，直到满意为止。确认满意后，可以将 PR 从草稿转换为准备审查状态，然后合并。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在回到 Copilot 为解决“在游戏详情页显示相关游戏”问题而生成的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或更完整标题的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. 你应该会看到工作流在拉取请求的检查区域中排队运行。如果一切正常，单体 Astro 应用的项目检查应该会通过。完成此过程可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。前往 Codespace，打开终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能。和之前一样，你可以继续与 Copilot 云代理迭代并要求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，这项任务是在更新自定义指令之前分配的）。和之前一样，可以在 **Conversation** 选项卡添加评论来提出这些要求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过自定义无障碍代理实现的功能。此 PR 应该同时包含你在练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或更完整标题的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，特别留意：
   - 用于切换模式的用户界面 (UI) 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的层叠样式表 (CSS) 或样式更改。
   - 无障碍属性（无障碍富互联网应用程序 (ARIA) 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流在拉取请求的检查区域中排队运行。如果一切正常，单体 Astro 应用的项目检查应该会通过。完成此过程可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。前往 Codespace，打开终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认它们按预期工作并能在页面重新加载后保留设置。

请注意，自定义无障碍代理如何引导 Copilot 按照无障碍最佳做法实现这些功能。如果发现无障碍方面的问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云代理最适合处理实际积压工作，让你腾出时间处理其他事务。可以在自己的存储库中创建几个问题并分配给 Copilot，以养成这种协作方式。可以考虑以下任务：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你完成了云代理流程。在这些练习中，你：

- **检查了此存储库提供的自定义指令文件**，以便之后观察它们对云代理输出的影响。
- **将问题分配给 Copilot 云代理**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义代理**来实现无障碍功能，包括高对比度和浅色模式切换。
- **将 Copilot 代理页面用作任务控制中心**，在会话进行期间监控并引导无障碍功能的实现。
- **审查并迭代了云代理创建的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

云代理流程已完成。如果想继续探索，其他流程可以补充本次实践的内容：

- 🖥️ **[VS Code 流程](../../vscode/)** — 直接在集成开发环境 (IDE) 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[命令行界面 (CLI) 流程](../../cli/)** — 从终端交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、代理技能、自定义质量保证 (QA) 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接功能。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率问题分配给云代理。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳做法][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
