---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。智能体完成工作后，会创建一个草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

你将使用 Copilot 云智能体探索以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初负责创建网站的承包商没有留下完善的文档，事实上，你会发现项目完全缺少文档。团队希望先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队也准备改进游戏发现体验。他们希望在每款游戏的详细信息页面显示相关游戏，也就是同一类别中的其他游戏。现在不需要完善的实现，只需要能用于验收测试用户体验的版本。目前这项工作被其他更高优先级的事项阻塞。

这两项工作都可能很快被降级为低优先级，因此很适合分配给 Copilot 云智能体。Copilot 云智能体可以异步处理它们，让开发人员继续完成其他工作，之后再审查结果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以在后台执行任务，方式与人类开发人员相似。与人类协作一样，有多种方式可以开始工作，其中包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在 Copilot 会话进行期间或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你！

## 明确界定说明范围的重要性

GitHub Copilot 没有魔法，使用它并不意味着可以跳过思考。看似简单的操作也可能包含复杂之处，因此请务必[谨慎界定云智能体任务的范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、试验并不断调整。生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

之前的练习介绍了自定义说明以及它们如何指导 Copilot。如果你已完成自定义说明练习，就已经看到 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。向 Copilot 云智能体分配任务前，先只读查看存储库中已有的说明文件，之后便能观察它们的影响。

在 GitHub Web 界面或已运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其注意 TypeScript 规范和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将这些说明限定为 `**/*.test.ts` 文件。

下一部分将把 `Code lacks documentation` 议题分配给云智能体。请留意生成的拉取请求是否采用了 TSDoc 文档注释、TypeScript 规范和文件注释头；这些要求来自说明文件。后续练习会再次介绍如何审查 PR。

## 为 GitHub Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保项目状态良好。将任务分配给 Copilot 时也一样，因为它会以类似软件工程师的方式工作。

云智能体使用 [GitHub Actions][github-actions] 环境执行工作。你可以创建一个[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中进行配置，并在云智能体开始工作前运行，以便它能够访问所需的开发工具和依赖项。本实验已提前完成设置，以便顺利学习。该工作流会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用程序使用的、已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤仅授予所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件相似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器。它可让你从存储库的 **Actions** 选项卡手动运行工作流，这样无需等待 Copilot 运行，即可确认工作流是否成功。

## 添加文档

每个人都知道文档很重要，但多数项目中的文档要么已过时，要么完全缺失。这类技术债务经常得不到处理，进而降低工作效率、增加维护代码库的难度，也让新开发人员更难加入团队。幸运的是，Copilot 擅长创建文档，因此这是分配给 Copilot 云智能体的理想议题。它会在后台生成所需文档。后续练习中，你会回来审查它的工作。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR）。其标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，你应该会看到 Copilot 创建的待办事项列表。

> [!NOTE]
> Copilot 生成待办事项列表可能需要几分钟。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表和它准备完成的任务。
14. 向下滚动拉取请求时间线，你会看到 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停下来思考并决定合适的方案，以及创建代码。

这通常需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来充分利用这一点：先给 Copilot 云智能体分配另一项任务，然后为应用程序编写代码并添加功能。

## 向游戏详情页面添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，而它处理另一组。为设计团队添加相关游戏区域可能不需要很长时间，但仍会占用你处理其他工作的时间。现在就将此任务分配给 Copilot 云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理任务！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！GitHub Copilot 云智能体的工作方式类似软件工程师，你无需一直监控它，完成后再审查结果即可。接下来，我们将了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理工作。你可以使用 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

Copilot 云智能体正在后台处理任务。现在可以开始创建和使用自定义智能体了。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，并且可以使用自定义说明；我们已在之前的模块中介绍过这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
