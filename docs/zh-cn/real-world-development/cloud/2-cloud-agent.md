---
title: "练习 2 - GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期搁置的功能请求。GitHub Copilot 云代理能够自主执行更新代码、添加功能等任务。代理完成工作后，会创建一个草稿拉取请求，供开发人员审查。这样，团队可以把繁琐的任务交给代理，加快开发进程，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot 云代理：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定问题的范围。
- 将问题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上，应用完全缺少文档。首先，团队希望为应用中所有导出的函数添加 TypeScript 文档注释（TSDoc）。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页中显示相关游戏，也就是同一类别中的其他游戏。目前不需要完善的实现，只需要一个可用于用户体验验收测试的版本。虽然这是一个阻碍事项，但当前还有优先级更高的问题。

这些任务很容易被降级为低优先级，正适合交给 Copilot 云代理。代理可以在后台异步处理任务，让开发人员先处理其他工作，再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云代理

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以在后台执行任务，方式与开发人员相似。和与开发人员协作一样，你可以通过多种方式交办任务，包括[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话进行期间或结束后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法工具，使用它并不意味着可以省略思考。即使是看似简单的操作，深入分析后也会发现其复杂性，因此要[认真界定分配给 Copilot 云代理的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其对 Copilot 的引导作用。如果你已经完成自定义指令的练习，就会看到 `.github/copilot-instructions.md` 的实际效果；如果还没看过，现在正好快速浏览一下。分配任务给 Copilot 云代理之前，只读查看此存储库中已有的指令文件，以便稍后观察它们的影响。

在 GitHub 网页界面中打开以下文件，或者在已经启动的 Codespace 中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它会将这些指令限定在 `**/*.test.ts` 文件中。

接下来，把 `Code lacks documentation` 问题分配给云代理时，观察生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。稍后的练习会再次讨论这些内容。

## 为 GitHub Copilot 云代理设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保项目状态正常。将任务分配给 Copilot 时也一样，因为它的工作方式与软件工程师类似。

云代理使用 [GitHub Actions][github-actions] 执行任务时所需的环境。你可以创建一个[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，以便在代理开始工作前运行。这样可以让代理使用所需的开发工具和依赖项。此工作流已预先配置好，以便顺利完成实验，并让你了解相关功能。它确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用的迁移并填充数据后的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

这个文件看起来和其他 GitHub 工作流文件相同，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于验证工作流是否成功，而不必等待 Copilot 运行它。

## 添加文档

虽然大家都知道文档很重要，但多数项目的信息要么过时，要么完全缺失。这类技术债务常常无人处理，降低工作效率，也增加维护代码库或帮助新开发人员入门的难度。Copilot 擅长编写文档，因此很适合把这项问题交给 Copilot 云代理。它会在后台生成所需的文档。之后的练习会再回来审查它的成果。

1. 在新的浏览器选项卡中，打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建问题对话框。
4. 选择 **Blank issue**，创建新问题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，其标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻，然后刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 创建待办事项列表可能需要几分钟。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理问题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解现有状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云代理的主要目标之一是异步执行任务，让我们腾出时间处理其他工作。接下来，我们会利用这一点：先把另一项任务交给 Copilot 云代理，再着手为应用编写功能代码。

## 为游戏详情页添加相关游戏部分

前面提到过，GitHub Copilot 云代理的一大优点是可以拆分工作：你专注于一组任务时，它可以处理另一组任务。即使为设计团队添加相关游戏部分不一定要花很长时间，省下来的时间仍可用于处理其他工作。现在就把这项任务交给 Copilot 云代理。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建问题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

随后，你应该会在问题的第一条评论中看到一组 👀，这表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情表示正在处理问题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。Copilot 云代理的工作方式与软件工程师类似，因此你不必一直监控它；等它完成后再审查即可。现在，我们来创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云代理][copilot-agents]。你可以将问题分配给云代理，让它异步处理任务。Copilot 可用于处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

你探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定问题的范围。
- 将问题分配给 Copilot。

云代理正在后台处理任务，现在可以开始创建和使用自定义代理。[Copilot 云代理还可以使用模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并且可以使用前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳做法][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
