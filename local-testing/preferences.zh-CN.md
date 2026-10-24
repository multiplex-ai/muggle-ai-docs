# 偏好设置

偏好设置控制 Muggle Test 在测试期间的行为——例如是否复用你上次使用的项目、是否显示浏览器窗口、是否自动发布结果等。这些偏好设置由 `@muggleai/works` 的技能和工具读取，存储在本地的 `~/.muggle-ai/preferences.json` 中。

## 设置值

询问你的 AI 助手，或使用 `/muggle:preferences` 技能：

> “显示我的 muggle 偏好设置”
>
> “将 autoLogin 设置为 always”
>
> “重置 muggle 偏好设置”

大多数偏好设置接受以下三个值之一：

| 值 | 行为 |
| :----- | :------- |
| `ask` | 每次都询问你（默认） |
| `always` | 自动执行，不询问 |
| `never` | 自动跳过，不询问 |

少数偏好设置使用不同的取值，已在表中注明。

## 可用的偏好设置

| 偏好设置 | 取值 | 默认值 | 控制内容 |
| :--------- | :----- | :------ | :------- |
| `autoLogin` | always / ask / never | ask | 工具需要身份验证时，直接复用已保存的凭据，不再询问 |
| `autoSelectProject` | always / ask / never | ask | 不询问，直接复用此仓库上次使用的项目 |
| `autoSelectLocalHost` | always / ask / never | ask | 复用上一次本地运行所用的开发服务器网址 |
| `showElectronBrowser` | always / ask / never | ask | 在本地测试期间显示 Electron 浏览器窗口 |
| `openTestResultsAfterRun` | always / ask / never | ask | 本地运行结束后，在控制台中打开该次运行的结果页面 |
| `defaultExecutionMode` | local / remote / ask | ask | 当技能同时支持两种执行模式时，默认使用哪一种 |
| `autoPublishLocalResults` | always / ask / never | ask | 将本地结果上传到云端，便于团队查看 |
| `suggestRelatedUseCases` | always / ask / never | ask | 创建或运行用户旅程后，推荐相关的用户旅程 |
| `suggestRelatedTestCases` | always / ask / never | ask | 创建或运行测试用例后，推荐相关的测试用例 |
| `autoDetectChanges` | always / ask / never | ask | 扫描本地 git 更改，并将其映射到受影响的测试用例 |
| `postPRVisualWalkthrough` | always / ask / never | ask | 测试结束后，在 PR 中发布带截图的可视化演示评论 |
| `autoCreatePR` | always / ask / never | ask | 不询问，直接推送分支并创建 PR |
| `checkForUpdates` | always / ask / never | ask | 在会话开始时检查是否有更新的 Muggle Test 版本 |
| `verboseOutput` | always / ask / never | ask | 在技能和工具运行期间显示详细的进度日志 |
| `autoUseWorktree` | always / ask / never | ask | 创建 git worktree 来隔离较复杂的工作 |
| `autoRebase` | always / ask / never | ask | 在启动开发服务器或运行测试之前，变基到默认分支 |
| `autoCleanup` | always / ask / never | ask | PR 合并后，移除 worktree、删除分支并清理产物 |
| `autoE2ETest` | always / ask | always | 在每个 `/muggle:do` 周期结束时运行 E2E 验收测试 |

## 重置

> “重置我的 muggle 偏好设置”

这会将所有偏好设置恢复为默认值——大多数为 `ask`，`autoE2ETest` 为 `always`。

## 后续步骤

| 目标 | 资源 |
| :--- | :------- |
| 查看读取这些偏好设置的技能 | [插件技能](local-testing/skills.md) |
| 设置本地测试 | [本地测试安装与配置](local-testing/setup.md) |
| 使用你自己的模型运行本地测试（`llmEnv` 块） | [模型来源](local-testing/model-sources.md) |
