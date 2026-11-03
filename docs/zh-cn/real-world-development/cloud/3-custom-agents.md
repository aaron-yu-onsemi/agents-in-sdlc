---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的人工智能 (AI) 助手。你可以在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义代理，为其提供重点明确的指令、最佳做法、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业经验整理为可复用的代理，例如确保符合 [Web 内容无障碍指南 (WCAG)][wcag] 标准的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面是用于定义代理行为、专业知识和指令的 Markdown 提示词。

### 自定义代理与代理技能的区别

自定义代理和[代理技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，用于指导 AI 执行操作。最简单的区分方式是：**自定义代理是工作者，技能是工具**。

自定义代理拥有自己的上下文窗口，并负责协调技能（甚至其他代理）来完成工作。在本练习中，无障碍自定义代理会依据无障碍指南审查和更新网站；处理过程中，它可以调用拉取请求工作流技能或测试管理技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。和 AI 的其他用法一样，需要根据环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍自定义代理的以下内容：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题中的文字和背景对比度不足，导致难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍功能至关重要，因此你希望尽快实现它。接下来使用自定义代理生成此功能。

## 查看无障碍自定义代理

系统已为你创建了一个无障碍自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 Codespace，然后查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义代理的必需字段。

3. 接着浏览后续各节，查看其中的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳做法。
   - 超文本标记语言 (HTML)、层叠样式表 (CSS) 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 创建并分配问题

任务控制中心是管理环境中所有代理的中心位置。你可以在此将任务分配给 Copilot 云代理、监控任务，并调整正在进行的会话或提供额外指导。现在，先将实现高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建问题对话框。
4. 选择 **Blank issue**，创建新问题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云代理分配界面中突出显示自定义代理和无障碍代理的截图](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。借助自定义代理，可以将团队经验和标准整理为可复用的代理，指导 Copilot 更有效地完成特定类型的工作。

你探索了以下内容：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式。接下来，前往任务控制中心[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为在组织中使用自定义代理做好准备][org-custom-agents]
- [为在企业中使用自定义代理做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云代理][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
