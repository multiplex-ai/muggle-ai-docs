# 本地测试工具参考

`@muggleai/works` 包中本地测试工具的完整参考。

> **架构说明：** 本地测试采用**云端优先**的方式：
> - **`muggle-remote-*` 工具**：身份验证及所有实体管理（项目、用户旅程、测试用例、密钥）
> - **`muggle-local-*` 工具**：仅用于本地执行、查看结果和发布

## 本页内容

| 章节 | 说明 |
| :------ | :---------- |
| [工具命名空间](#tool-namespaces) | 远程与本地工具前缀 |
| [本地工具类别](#local-tool-categories) | 状态、执行、结果、脚本、发布 |
| [身份验证工具](#authentication-tools-muggle-remote-auth-) | 登录、退出登录、状态、轮询 |
| [远程数据工具](#remote-data-tools-muggle-remote-) | 获取云端实体 |
| [状态工具](#status-tools) | 检查服务状态和会话 |
| [执行工具](#execution-tools) | 生成和回放测试脚本 |
| [运行结果工具](#run-result-tools) | 查看运行结果 |
| [测试脚本工具](#test-script-tools) | 查看本地生成的脚本 |
| [发布工具](#publishing-tools) | 将脚本上传到云端 |
| [工作流：使用云端实体进行本地测试](#workflow-local-testing-with-cloud-entities) | 推荐的端到端工作流 |
| [工具选择指南](#tool-selection-guide) | 常见任务速查 |
| [后续步骤](#next-steps) | 延伸阅读 |

## 工具命名空间

| 命名空间 | 用途 | 示例 |
| :-------- | :------ | :------- |
| `muggle-remote-auth-*` | 身份验证 | `muggle-remote-auth-login`、`muggle-remote-auth-status` |
| `muggle-remote-*` | 云端实体管理 | `muggle-remote-project-create`、`muggle-remote-test-case-get`、`muggle-remote-action-script-get` |
| `muggle-local-*` | 本地执行与结果 | `muggle-local-execute-test-generation` |

## 本地工具类别

| 类别 | 工具 | 用途 |
| :------- | :---- | :------ |
| **状态** | `muggle-local-check-status`、`muggle-local-list-sessions` | 检查本地服务状态 |
| **执行** | `muggle-local-execute-test-generation`、`muggle-local-execute-replay` | 运行测试生成和回放 |
| **结果** | `muggle-local-run-result-list`、`muggle-local-run-result-get` | 查看运行结果 |
| **脚本** | `muggle-local-test-script-list`、`muggle-local-test-script-get` | 查看本地生成的脚本 |
| **发布** | `muggle-local-publish-test-script` | 将生成的脚本上传到云端 |
| **控制** | `muggle-local-cancel-execution` | 取消正在进行的执行 |

---

## 身份验证工具（muggle-remote-auth-*）

身份验证工具位于 `muggle-remote-*` 命名空间中，因为它们需要与云端交互。

### muggle-remote-auth-status

检查当前的身份验证状态。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| （无） | — | — | — |

**返回：**
- `authenticated` - 是否已登录
- `email` - 用户邮箱（已通过身份验证时）
- `expiresAt` - Token 过期时间

---

### muggle-remote-auth-login

启动设备码（Device Code）身份验证流程。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `waitForCompletion` | boolean | 否 | 等待浏览器中完成登录（默认：true） |
| `timeoutMs` | number | 否 | 超时时间，单位为毫秒（默认：120000） |

**返回：**
- `verificationUri` - 在浏览器中打开的网址
- `userCode` - 需要输入的用户码
- `deviceCode` - 用于轮询的设备码（配合 `muggle-remote-auth-poll` 使用）

**示例提示词：**
> “登录 Muggle Test”

---

### muggle-remote-auth-poll

轮询设备码身份验证是否完成。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `deviceCode` | string | 否 | 来自 `muggle-remote-auth-login` 的设备码 |

**返回：**
- `status` - `complete`、`pending`、`expired` 或 `error`
- `email` - 用户邮箱（完成时）

---

### muggle-remote-auth-logout

清除已存储的凭据。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| （无） | — | — | — |

---

## 远程数据工具（muggle-remote-*）

用于获取云端实体的远程工具，通常在本地执行之前使用。

### muggle-remote-action-script-get

按 ID 获取完整的操作脚本内容。回放时必需——使用测试脚本中的 `actionScriptId` 获取带元素标签的完整可执行步骤。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `actionScriptId` | string | 是 | 来自 `testScript.actionScriptId` 的操作脚本 ID |

**返回：** 操作步骤数组，包含回放所需的元素标签。

**用法：** 在 `muggle-remote-test-script-get` 之后调用此工具获取脚本内容，然后再运行 `muggle-local-execute-replay`。

---

## 状态工具

### muggle-local-check-status

检查 Muggle Test 本地服务的状态。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| （无） | — | — | — |

**返回：**
- `dataDirectory` - 本地数据存储路径
- `sessionsDirectory` - 会话存储路径
- `authenticated` - 是否已通过身份验证

---

### muggle-local-list-sessions

列出所有已存储的测试会话。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `limit` | number | 否 | 返回的最大会话数（默认：10） |

---

## 执行工具

### muggle-local-execute-test-generation

启动真实浏览器访问你的 Web 应用，生成 E2E 验收测试脚本。浏览器会在你的应用中导航，执行测试用例中的步骤（例如注册、填写表单、点击完成各个流程），并生成带截图、可回放的测试脚本。可用它为任意用户流程创建新的浏览器测试。

**重要：** 先调用 `muggle-remote-test-case-get` 获取测试用例详情，再将其传入此工具。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `testCase` | object | 是 | 来自 `muggle-remote-test-case-get` 的测试用例详情 |
| `localUrl` | string | 是 | 要测试的本地网址（例如 `http://localhost:3000`） |
| `approveElectronAppLaunch` | boolean | 是 | 必须为 `true` 才能继续 |
| `timeoutMs` | number | 否 | 超时时间，单位为毫秒（默认：300000） |

**testCase 对象：**
- `id` - 云端测试用例 ID
- `title` - 测试用例标题
- `goal` - 测试要验证的内容
- `expectedResult` - 预期结果
- `precondition` - 前置条件（可选）
- `instructions` - 分步指令（可选）

**执行过程：**
1. 启动浏览器引擎
2. 导航到 `localUrl`
3. AI 根据测试用例探索并记录操作
4. 脚本保存在本地

**示例工作流：**
```
1. muggle-remote-test-case-get(testCaseId) → returns test case object
2. muggle-local-execute-test-generation({ testCase: {...}, localUrl: "http://localhost:3000", approveElectronAppLaunch: true })
```

---

### muggle-local-execute-replay

在真实浏览器中回放现有的 E2E 验收测试脚本，验证你的应用是否仍然正常工作——可用于代码变更后的回归测试。浏览器会执行每个已保存的步骤并截图，方便你查看发生了什么。

**重要：** 回放前需要分别调用两次获取：
1. `muggle-remote-test-script-get` 获取脚本元数据（包含 `actionScriptId`）
2. `muggle-remote-action-script-get` 获取完整的操作脚本内容

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `testScript` | object | 是 | 来自 `muggle-remote-test-script-get` 的测试脚本元数据 |
| `actionScript` | array | 是 | 来自 `muggle-remote-action-script-get` 的操作脚本步骤 |
| `localUrl` | string | 是 | 要测试的本地网址 |
| `approveElectronAppLaunch` | boolean | 是 | 必须为 `true` 才能继续 |
| `timeoutMs` | number | 否 | 超时时间，单位为毫秒（默认：180000） |
| `showUi` | boolean | 否 | 执行期间显示浏览器界面（默认：false，以无头模式运行） |

**testScript 对象：**
- `id` - 云端测试脚本 ID
- `name` - 脚本名称
- `testCaseId` - 所属测试用例 ID
- `actionScriptId` - 用于获取操作脚本内容的 ID

**网址重写：**
- 操作脚本中的原始云端网址会自动替换为 `localUrl`

**示例工作流：**
```
1. muggle-remote-test-script-get(testScriptId) → returns testScript with actionScriptId
2. muggle-remote-action-script-get(testScript.actionScriptId) → returns actionScript array
3. muggle-local-execute-replay({ testScript, actionScript, localUrl: "http://localhost:3000", approveElectronAppLaunch: true })
```

---

### muggle-local-cancel-execution

取消正在进行的执行。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | 是 | 要取消的运行 ID |

---

## 运行结果工具

### muggle-local-run-result-list

列出运行结果。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `cloudTestCaseId` | string | 否 | 按云端测试用例 ID 筛选 |
| `limit` | number | 否 | 最大结果数（默认：20） |

---

### muggle-local-run-result-get

获取运行结果详情，包括截图。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | 是 | 运行结果 ID |

**返回：**
- `id` - 运行 ID
- `runType` - `generation` 或 `replay`
- `status` - 通过/失败结果
- `executionTimeMs` - 耗时
- `testScriptId` - 关联的脚本（如果已生成）
- `errorMessage` - 错误详情（如果失败）

---

## 测试脚本工具

### muggle-local-test-script-list

列出本地生成的测试脚本。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `cloudTestCaseId` | string | 否 | 按云端测试用例 ID 筛选 |

---

### muggle-local-test-script-get

获取测试脚本详情，包括操作脚本步骤。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `testScriptId` | string | 是 | 本地测试脚本 ID |

---

## 发布工具

### muggle-local-publish-test-script

将本地生成的测试脚本上传到云端。

| 参数 | 类型 | 必填 | 说明 |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | 是 | 测试生成得到的运行结果 ID |
| `cloudTestCaseId` | string | 是 | 要发布到的云端测试用例 ID |

**返回：** 一个 `viewUrl`，用于在 MuggleTest 控制台中打开已发布的测试脚本。使用 `open "<viewUrl>"`（macOS）或等效命令，在浏览器中查看脚本详情。

**前提：** 通过 `muggle-remote-auth-login` 完成身份验证

---

## 工作流：使用云端实体进行本地测试

推荐的工作流使用云端工具（`muggle-remote-*`）管理实体，使用本地工具（`muggle-local-*`）执行：

```
┌─────────────────────────────────────────────────────────────┐
│                     Entity Management                        │
│                  (muggle-remote-* tools)                     │
├─────────────────────────────────────────────────────────────┤
│  muggle-remote-auth-login       → Authenticate               │
│  muggle-remote-project-list     → Find/create project        │
│  muggle-remote-use-case-list    → Find/create use case       │
│  muggle-remote-test-case-list   → Find/create test case      │
│  muggle-remote-test-case-get    → Fetch test case details    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                     Local Execution                          │
│                 (muggle-local-* tools)                       │
├─────────────────────────────────────────────────────────────┤
│  muggle-local-execute-test-generation → Generate locally     │
│  muggle-local-run-result-get          → View results         │
│  muggle-local-publish-test-script     → Publish to cloud     │
└─────────────────────────────────────────────────────────────┘
```

### 示例：在 localhost:3000 上测试登录功能

```
1. muggle-remote-auth-status                         # Check auth
2. muggle-remote-auth-login                          # Login if needed
3. muggle-remote-project-list                        # Find project
4. muggle-remote-use-case-list(projectId)            # Find login use case
5. muggle-remote-test-case-list-by-use-case(ucId)    # Find login test case
6. muggle-remote-test-case-get(testCaseId)           # Get full details
7. [Ask user for approval]
8. muggle-local-execute-test-generation({
     testCase: { ...from step 6... },
     localUrl: "http://localhost:3000",
     approveElectronAppLaunch: true
   })
9. muggle-local-run-result-get(runId)           # View results
10. muggle-local-publish-test-script(runId, testCaseId)  # Publish
```

---

## 工具选择指南

### 测试执行

| 你想要…… | 使用此流程 |
| :------------- | :------------ |
| 生成测试脚本 | `muggle-remote-test-case-get` → `muggle-local-execute-test-generation` |
| 回放现有脚本 | `muggle-remote-test-script-get` → `muggle-local-execute-replay` |
| 停止正在运行的测试 | `muggle-local-cancel-execution` |
| 查看测试结果 | `muggle-local-run-result-get` |

### 实体管理（使用 muggle-remote-* 工具）

| 你想要…… | 使用此工具 |
| :------------- | :------------ |
| 创建/列出项目 | `muggle-remote-project-create`、`muggle-remote-project-list` |
| 创建/列出用户旅程 | `muggle-remote-use-case-create-from-prompts`、`muggle-remote-use-case-list` |
| 创建/列出测试用例 | `muggle-remote-test-case-create`、`muggle-remote-test-case-list` |
| 获取测试用例详情 | `muggle-remote-test-case-get` |
| 获取测试脚本详情 | `muggle-remote-test-script-get` |

### 身份验证

| 你想要…… | 使用此工具 |
| :------------- | :------------ |
| 检查身份验证状态 | `muggle-remote-auth-status` |
| 登录 | `muggle-remote-auth-login` |
| 轮询登录是否完成 | `muggle-remote-auth-poll` |
| 退出登录 | `muggle-remote-auth-logout` |

---

## 后续步骤

- **[智能体技能](skills.md)**——自动完成多步骤测试的预置工作流
- **[示例工作流](examples.md)**——看看这些工具的实际用法
- **[本地测试安装与配置](setup.md)**——安装与配置
- **[本地测试概览](overview.md)**——了解架构
- **[云端 E2E 验收工具](mcp/mcp-api-reference.md)**——muggle-remote-* 工具的完整参考
