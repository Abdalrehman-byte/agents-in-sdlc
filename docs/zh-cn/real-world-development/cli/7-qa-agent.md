---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建结合议题需求、quality-checks 技能和 Playwright MCP 的 QA 自定义智能体。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中查看筛选体验。现在将这些能力整合到自定义智能体中，定义清晰的 QA 流程。

本课将学习如何：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可重复使用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，以一致的方式审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调这一 QA 流程，并提供可重复使用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，然后在 Copilot CLI 中选择它。

你使用的这些自定义机制各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何使用这些能力评估需求并报告发现。QA 配置不会取代其他机制，也不需要启动另一个对话。

## 创建 QA 配置

创建功能 PR 前，让 Copilot 创建可重复使用的 QA 配置。该配置将定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 要求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先审查其配置文件，确认 Copilot 捕获了预期的 QA 工作流程和边界。这样可避免你只想验证功能时，配置不完整或范围过大的智能体修改了代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需字段，`name` 为可选字段，但添加名称可让智能体具有清晰的显示名称。
3. 阅读配置指令，确认 QA 会先查看需求、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少任何职责或边界，请先要求默认智能体进行修订，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一个筛选功能对话，然后选择 QA，让它使用已有的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 你刚创建的智能体可能不会立即出现在可用智能体列表中，但它已经存在，上面的命令会启用它。

2. 使用以下提示让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始处理！
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已在工作流中添加了可重复使用的专用角色，并审查了它的工作。本课中，你：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查了可重复使用的质量保证配置。
- 选择了 QA 智能体，并根据筛选议题审查其发现。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备好接受审查。接下来，你将[在功能 PR 中整合这些内容并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
