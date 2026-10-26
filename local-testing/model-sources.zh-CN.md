# 模型来源

Studio 是运行你本地测试的浏览器引擎，它会调用 AI 模型来选择每一步操作、总结所发生的情况，并判定结果。模型来源决定了在你的计算机上由谁的模型来处理这些调用。

## 来源

| 来源 | 模型 | 付费方 | 状态 |
| :----- | :---- | :------- | :----- |
| Token 钱包（默认） | Muggle 指定的模型 | 从你的 Token 钱包中扣除 | ✅ 可用 |
| 本地或自托管模型 | 你的 OpenAI 兼容端点 | 你自己——不会从你的 Token 钱包中扣除任何费用 | ✅ 在 `@muggleai/works` 5.11.0 及更高版本中可用 |
| 你自己的 Anthropic API 密钥 | Muggle 指定的模型，使用你的密钥 | 由 Anthropic 向你收费 | 尚未发布 |
| Claude Pro 或 Max 订阅 | — | — | ❌ 不提供——参见[常见问题](#faq) |

模型来源仅适用于在你计算机上进行的运行。云端 E2E 运行始终使用 Muggle 指定的模型和你的 Token 钱包。

## 本地或自托管模型

### 要求

| 要求 | 详情 |
| :---------- | :------ |
| `@muggleai/works` | 5.11.0 或更高版本。可用 `muggle --version` 查看。 |
| 端点 | 在 `/v1` 下提供 OpenAI 兼容 Chat Completions API 的服务器——Ollama、LM Studio、vLLM 或 Text Generation Inference |
| 模型 | 必须支持图像输入：Studio 在每一步都会发送一张截图。视觉模型包括 Llama 3.2 Vision、Qwen2.5-VL、LLaVA 和 MiniCPM-V。 |

测试质量取决于你选择的模型。

### 配置

在 `~/.muggle-ai/preferences.json` 中添加一个 `llmEnv` 块，放在顶层、与 `preferences` 并列——不要放在它里面：

```json
{
  "version": 1,
  "preferences": {
    "autoLogin": "ask"
  },
  "llmEnv": {
    "MUGGLE_LLM_PROVIDER": "local",
    "MUGGLE_LLM_BASE_URL": "http://localhost:11434/v1",
    "MUGGLE_LLM_MODEL": "llama3.2-vision"
  }
}
```

下一次本地运行就会读取该配置，你的 AI 助手无需重启。使用 `/muggle:preferences` 修改偏好设置时会保留该块。删除该块即可改回使用你的 Token 钱包。

| 设置项 | 必填 | 默认值 | 说明 |
| :------ | :------: | :------ | :---------- |
| `MUGGLE_LLM_PROVIDER` | ✓ | — | `local` |
| `MUGGLE_LLM_BASE_URL` | ✓ | — | 端点网址，以 `/v1` 结尾 |
| `MUGGLE_LLM_MODEL` | ✓ | — | 端点所提供模型的 ID |
| `MUGGLE_LLM_API_KEY` | | — | 端点要求的密钥（如果端点会校验密钥） |
| `MUGGLE_LLM_MODEL_PREDICTION` | | `MUGGLE_LLM_MODEL` | 选择每一步操作的模型 |
| `MUGGLE_LLM_MODEL_SUMMARIZER` | | `MUGGLE_LLM_MODEL` | 总结步骤和运行的模型 |
| `MUGGLE_LLM_MODEL_EVAL` | | `MUGGLE_LLM_MODEL` | 评判候选用户旅程的模型 |
| `MUGGLE_LLM_MAX_TOKENS` | | `8192` | 每次回复的最大 Token 数 |
| `MUGGLE_LLM_TEMPERATURE` | | `0.1` | 采样温度 |
| `MUGGLE_LLM_TOP_P` | | 端点默认值 | 核采样 |
| `MUGGLE_LLM_TIMEOUT_MS` | | `60000` | 单次请求的超时时间（毫秒）；模型较慢时请调高 |

你指定的每个模型都必须支持图像输入。图像检查只覆盖 `MUGGLE_LLM_MODEL`。

### 端点

| 运行时 | 默认 `MUGGLE_LLM_BASE_URL` |
| :------ | :---------------------------- |
| Ollama | `http://localhost:11434/v1` |
| LM Studio | `http://localhost:1234/v1` |
| vLLM | `http://localhost:8000/v1` |
| Text Generation Inference | `http://<host>:<port>/v1` |

将 `MUGGLE_LLM_MODEL` 设置为服务器所提供的确切 ID。大多数服务器会在 `<base URL>/models` 列出其模型 ID。

### 环境变量

每个设置项也可以设为 Muggle MCP 服务器的环境变量，例如 MCP 配置中 `env` 块里的一项（参见[本地测试安装与配置](local-testing/setup.md)）。在那里设置的变量会覆盖 `llmEnv` 中的同名设置。

你的 AI 助手可以读取 `preferences.json`。如果你的端点需要密钥，请将 `MUGGLE_LLM_API_KEY` 设置为环境变量，而不是写在该文件中。

### 行为

- **图像检查。** 在一次运行首次调用模型之前，Studio 会向你的模型发送一张小图片。如果该请求失败，运行就会停止。
- **不回退到云端。** 如果你的端点无法访问或返回错误，运行就会失败。Studio 绝不会切换到 Muggle 的模型。
- **计费。** 由你的端点处理的调用不会从你的 Token 钱包中扣费。

### 数据处理

模型调用会发送到你的端点。本地模式并不会让 Studio 离线运行：

- 运行测试仍需登录 Muggle Test；Studio 运行的是存储在你账户中的测试用例。
- Studio 目前会将每次模型调用的文本记录（提示词和模型的回复，不含截图）发送给 Muggle，其中也包括由你的端点处理的调用。
- 与所有本地运行一样，是否上传本地结果取决于 `autoPublishLocalResults` 偏好设置。

### 已知问题

部分内部调用请求的模型 ID 是 `gpt-4o-mini-2024-07-18`，而不是你的模型：包括修复格式错误的模型回复，以及在每次运行结束时或某个步骤无法达成目标时记录运行记忆。不提供该 ID 的端点会以 404 拒绝这些调用。此时该次运行的记忆不会被保存，而无法达成目标的步骤会报告这个 404 错误。

在修复发布之前，请让你的模型同时以该 ID 提供服务：

| 运行时 | 临时解决方法 |
| :------ | :--------- |
| Ollama | `ollama cp <your-model> gpt-4o-mini-2024-07-18` |
| vLLM | 将 `gpt-4o-mini-2024-07-18` 添加到 `--served-model-name` |

### 故障排除

来自你端点的错误可能会出现在前缀 `Failed to check or debit tokens:` 之后，即使实际上并没有扣费。请阅读该前缀后面的内容。

| 症状 | 原因 | 解决方法 |
| :------ | :---- | :-- |
| 每次运行都立即失败：`Local LLM provider selected but no baseURL is configured`（或 `no model`） | 缺少 `MUGGLE_LLM_BASE_URL` 或 `MUGGLE_LLM_MODEL` | 添加该设置项 |
| `Model "<id>" does not accept image input: Connection error` | 端点未运行，或网址错误 | 启动服务器；确认网址以 `/v1` 结尾 |
| `Model "<id>" does not accept image input: 404 …` | 端点不提供该 ID | 使用 `<base URL>/models` 中列出的 ID |
| `Model "<id>" does not accept image input`，且为其他原因 | 该模型不支持视觉输入 | 换用视觉模型 |
| 提及 `gpt-4o-mini-2024-07-18` 的 404 错误 | [已知问题](#known-issue) | 采用临时解决方法 |
| 请求超时 | 模型或硬件速度慢 | 调高 `MUGGLE_LLM_TIMEOUT_MS` |
| 运行仍从你的 Token 钱包扣费 | `llmEnv` 被放在了 `preferences` 内部、`MUGGLE_LLM_PROVIDER` 不是 `local`，或 `@muggleai/works` 版本低于 5.11.0 | 修正该配置块或升级 |

## 常见问题

**我可以使用自己的 Claude Pro 或 Max 订阅吗？**
目前不行。Anthropic 的条款禁止第三方产品处理订阅凭据：“Anthropic 不允许第三方开发者在其自己的应用中提供 Claude.ai 登录，也不允许代表其用户通过 Free、Pro 或 Max 套餐的凭据转发请求”，并且开发者“不得收集、存储或中转 Claude.ai 凭据或会话令牌”（[Claude Code 法律与合规](https://code.claude.com/docs/en/legal-and-compliance)）。Muggle Test 绝不会要求你登录 Claude 或提供会话令牌。

同一条款允许你使用自己的订阅登录未经修改的 Claude Code。但 Muggle Test 的模型调用目前并不经由它运行。

**我可以使用自己的 Anthropic API 密钥吗？**
暂时不行。相关支持正在开发中，尚未包含在任何已发布的 `@muggleai/works` 版本中。

**本地模型适用于云端 E2E 运行吗？**
不适用。云端 E2E 运行始终使用 Muggle 指定的模型和你的 Token 钱包。

## 后续步骤

| 目标 | 资源 |
| :--- | :------- |
| `preferences.json` 中的其他设置 | [偏好设置](local-testing/preferences.md) |
| MCP 服务器环境变量 | [本地测试安装与配置](local-testing/setup.md) |
| Token 钱包如何计费 | [订阅套餐](billing/subscription-plans.md) |
