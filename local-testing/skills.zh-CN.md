# 插件技能

预置的 AI 智能体技能，借助 Muggle AI 自动完成测试和开发工作流。

## 本页内容

| 章节 | 说明 |
| :------ | :---------- |
| [什么是插件技能？](#what-are-plugin-skills) | 概览与安装 |
| [可用技能](#available-skills) | 所有技能一览 |
| [/muggle:do](#muggledo) | 质量有保障的开发工作流 |
| [/muggle:test](#muggletest) | 由变更驱动的 E2E 路由 |
| [/muggle:test-feature-local](#muggletest-feature-local) | 快速的单功能本地测试 |
| [/muggle:status](#mugglestatus) | 健康检查 |
| [/muggle:repair](#mugglerepair) | 诊断并修复安装 |
| [/muggle:upgrade](#muggleupgrade) | 更新到最新版本 |
| [更多技能](#more-skills) | 路由、准备、导入、重新生成、PR、反馈、偏好设置 |
| [技能与直接调用 MCP 工具](#skills-vs-direct-mcp-tool-calls) | 两种方式各自的适用场景 |
| [后续步骤](#next-steps) | 延伸阅读 |

## 什么是插件技能？

插件技能是结构化的工作流，用来教 AI 助手如何完成复杂的多步骤任务。你无需手动逐个调用工具，只需调用一个技能，智能体就会自动按工作流执行。

## 安装

技能通过适用于 Claude Code 的 Muggle AI 插件提供：

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

安装后，技能以 `/muggle:*` 命令的形式提供。

## 可用技能

| 技能 | 说明 | 触发示例 |
| :---- | :---------- | :-------------- |
| `/muggle:do` | 质量有保障的开发工作流：需求、编码、测试、通过真实浏览器验证的端到端（E2E）验收测试，以及创建 PR | “添加一个退出登录按钮，并在提交 PR 之前确认它能正常工作” |
| `/muggle:test` | 由变更驱动的 E2E 路由——检测代码变更，将其映射到用户旅程，在本地或远程运行测试生成，将结果发布到控制台并在浏览器中打开，再向 PR 发布 E2E 摘要 | “测试我的改动，并把结果发布到 PR” |
| `/muggle:test-feature-local` | 针对 localhost 运行真实浏览器 E2E 验收测试，验证某项功能是否正常工作——注册流程、结账、表单验证、UI 交互，或任何面向用户的行为 | “在 localhost:3000 上测试我的登录流程” |
| `/muggle:status` | 检查浏览器测试运行器（Electron）、MCP 服务器和身份验证的健康状态 | “检查 muggle 状态” |
| `/muggle:repair` | 诊断并修复损坏的安装 | “修复我的 muggle 安装” |
| `/muggle:upgrade` | 更新到最新版本 | “升级 muggle” |
| `/muggle` | 命令路由，以及所有 Muggle 命令的菜单 | “muggle” |
| `/muggle:test-prepare` | 测试前确认开发服务器和相关服务已启动 | “我的服务都启动了吗？” |
| `/muggle:import` | 将现有的测试、PRD 或规格说明导入 Muggle Test | “导入我的 Playwright 测试” |
| `/muggle:regenerate-missing` | 为没有脚本的测试用例批量重新生成脚本 | “重新生成缺失的测试脚本” |
| `/muggle:pr` | 向 PR 发布带截图的 E2E 可视化演示 | “把可视化演示添加到我的 PR” |
| `/muggle:feedback` | 标记出错的脚本或步骤，以便 Muggle 重新生成 | “这次运行的结果不对” |
| `/muggle:preferences` | 查看、设置或重置 Muggle 测试偏好设置 | “显示我的 muggle 设置” |

## /muggle:do

质量有保障的开发工作流，带领一项任务依次完成需求、编码、测试、通过真实浏览器验证的 E2E 验收测试，以及创建 PR。AI 会编写功能代码并运行单元测试，然后在真实浏览器中以桌面端和移动端视口对应用运行 E2E 测试，排查失败原因，并提交附带证据的 PR。

### 示例

```
/muggle:do "Add a logout button to the header"

REQUIREMENTS  -> Goal: Add logout button
IMPACT        -> frontend repo, src/components/Header.tsx
CODING        -> (writes code)
UNIT_TESTS    -> 12/12 pass
E2E           -> 3/3 test cases pass
OPEN_PRS      -> PR #42 opened
```

特性：

- 基于会话，支持崩溃恢复
- 自动排查：分析失败原因并返回修复（最多 3 轮）
- 通过 `muggle-repos.json` 支持多仓库
- PR 中附带 E2E 结果和截图


## /muggle:test

由变更驱动的 E2E 路由，检测代码变更，确定受影响的用户旅程，并在本地（在 localhost 上使用 Electron 浏览器）或远程（由 Muggle 云端针对预览/预发布环境网址）执行测试生成。执行完成后，它会将结果发布到 Muggle AI 控制台并在你的浏览器中打开，再向你的 PR 发布附带测试用例链接的 E2E 摘要。

### 两种模式

| 模式 | 适用场景 | 执行工具 |
| :--- | :---------- | :------------- |
| **本地** | 开发期间针对 `localhost` 进行测试 | `muggle-local-execute-test-generation` |
| **远程** | 针对预览或预发布环境网址进行测试 | `muggle-remote-workflow-start-test-script-generation` |

该技能会在继续之前询问你要使用哪种模式。如果你的提示词提到“localhost”或“local”，它会选择本地模式。如果提到“staging”、“preview”或非 localhost 网址，它会选择远程模式。

### 示例

```
/muggle:test

"Test my changes and post the results to the PR"

1. Intent: local or remote? -> "local, localhost:3000"
2. Changes detected: signup form refactor, profile page update
3. Auth: authenticated
4. Project: "My App" (matched)
5. Use cases: 2 existing, 1 new -> confirm mapping
6. Test cases: 4 total -> confirm scope
7. Launching browser test runner x4... (approve? y)
8. Results: 3/4 PASS, 1 FAIL
9. Published to Muggle dashboard (opened in browser)
10. E2E results posted to PR #42
```

### 测试运行后的操作

- **发布**：通过 `muggle-local-publish-test-script` 将每次运行发布到 Muggle AI 云端
- **打开**：在你的浏览器中打开 Muggle AI 控制台，方便你查看逐步截图
- **发表评论**：向 PR 发表一条 E2E 结果评论，其中的通过/失败表格里每个测试用例都链接到它在控制台中的详情页

### 何时使用 /muggle:test，何时使用 /muggle:test-feature-local

| 场景 | 推荐技能 |
| :------- | :---------------- |
| 测试受你的变更影响的所有用户旅程 | `/muggle:test` |
| 在预发布环境/预览网址上测试 | `/muggle:test` |
| 将 E2E 结果发布到 PR | `/muggle:test` |
| 在 localhost 上快速测试单个功能 | `/muggle:test-feature-local` |


## /muggle:test-feature-local

针对 localhost 运行真实浏览器 E2E 验收测试，验证某项功能是否正常工作——注册流程、结账、表单验证、UI 交互，或任何面向用户的行为。它会启动一个浏览器来执行测试步骤并截图。

### 示例

```
/muggle:test-feature-local

"Test my login changes on localhost:3000"

1. Auth check
2. Found project: "My App"
3. Found 2 test cases - recommend replay
4. Launching browser test runner... (approve? y)
5. Results: 2/2 PASS
6. Publish to cloud? (y)
```

该技能会处理完整的工作流：身份验证、查找或创建测试用例、在真实浏览器中执行、附带截图报告结果，并可选择将生成的脚本发布到云端。

## /muggle:status

检查你的 Muggle AI 安装的健康状态：

- Electron 浏览器测试运行器的版本和二进制文件完整性
- MCP 服务器的响应情况
- 身份验证状态和令牌过期时间

如果任何一项检查失败，请运行 `/muggle:repair`。

## /muggle:repair

自动诊断并修复损坏的组件：

- Electron 浏览器测试运行器缺失或损坏时重新下载
- 凭据过期时重新进行身份验证
- 报告修复了哪些内容

## /muggle:upgrade

将所有 Muggle AI 组件更新到最新版本，并报告更新前后的版本。

## 更多技能

| 技能 | 作用 |
| :---- | :----------- |
| `/muggle` | 路由和菜单——列出所有 Muggle 命令并分派到对应命令。 |
| `/muggle:test-prepare` | 检查测试所需的开发服务器和相关服务是否正在运行，并可帮你启动尚未运行的服务。 |
| `/muggle:import` | 将现有测试导入 Muggle Test——Playwright、Cypress、Gherkin `.feature` 文件、PRD 或测试计划文档。 |
| `/muggle:regenerate-missing` | 扫描项目中没有有效脚本的测试用例，并通过云端批量生成缺失的脚本。 |
| `/muggle:pr` | 为每个测试用例渲染可视化演示（控制台链接和步骤截图），并发布到 PR。 |
| `/muggle:pr-followup` | 监视一个未关闭的 PR 是否有新提交的评审，并交给 `/muggle:do` 处理。 |
| `/muggle:feedback` | 标记某个生成的脚本——或其中某个具体步骤——执行有误，以便 Muggle 重新生成。也可以列出和删除反馈。 |
| `/muggle:preferences` | 查看、设置或重置控制测试行为的偏好设置，例如自动登录。 |

## 技能与直接调用 MCP 工具

| 方式 | 适用场景 |
| :------- | :---------- |
| **技能** | 常见工作流、多步骤任务、“直接帮我构建”或“直接帮我测试” |
| **直接调用工具** | 精细控制、特定操作、自定义工作流 |

技能在底层调用的是同样的 100 多个 MCP 工具——它们负责自动编排。

## 后续步骤

- **[工具参考](tools-reference.md)**——各个工具的文档
- **[示例工作流](examples.md)**——查看工具和技能的实际用法
- **[本地测试安装与配置](setup.md)**——安装与配置
