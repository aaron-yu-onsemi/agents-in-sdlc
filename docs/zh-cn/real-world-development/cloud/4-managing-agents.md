---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前两项练习中，你让 Copilot 云代理处理了三项不同任务，用于改善用户体验并添加功能。云代理虽然能够异步、自主地工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，你可以查看所有具有未关闭拉取请求 (PR) 的代理任务、了解代理执行的操作，甚至引导进行中的会话。

本课将介绍如何：

- 使用代理页面监控云代理任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将任务分配给代理以创建高对比度模式后，团队意识到也可以趁此机会添加浅色模式。网站样式和切换功能已经在更新，因此加入此功能合乎情理。你希望引导代理在添加高对比度模式的同时，也实现浅色模式。

## 查看 Copilot 云代理任务

查看当前分配给 Copilot 云代理的所有任务的状态。

1. 前往 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 为修改游戏生成应用程序编程接口 (API)。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置模型上下文协议 (MCP) 服务器。
    - 执行完成分配任务所需的各项操作。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作进度和执行方式而有所不同。

4. 同时留意右侧显示的拉取请求 (PR) 窗格。在此可以查看 PR 和更改的文件，以便进一步监控。

## 引导云代理

现在已经查看了正在处理的任务，接下来要求 Copilot 在实现高对比度模式时也添加浅色模式。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。出现类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中突出显示“Steer active session while Copilot is working”对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会开始处理更新后的任务。收到新消息时，它会先完成当前步骤，然后将新请求纳入工作流程。

这可能需要几分钟，可以趁此机会暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，它是监控和引导 GitHub Copilot 云代理任务的中心位置。通过这个任务控制中心，你可以跟踪所有正在进行和已完成的任务、查看代理正在执行的工作，还可以调整进行中的任务范围或提供额外指导。

你探索了以下内容：

- 探索任务控制中心和代理页面，以监控云代理任务。
- 引导正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来，开始[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
