# Own Anthropic API Key

> **Not yet released.** No released version of `@muggleai/works` includes this feature. Until it ships, local runs use your token wallet or a local model — see [Model Sources](local-testing/model-sources.md).

Run your local tests on Muggle's designated models with your own Anthropic API key. Anthropic bills your key; nothing is debited from your token wallet. The key applies only to runs on your computer — Cloud E2E runs always use your token wallet.

## Requirements

| Requirement | Details |
| :---------- | :------ |
| `@muggleai/works` | The first release that includes `muggle llm-source` (not yet released) |
| Anthropic API key | Created in the [Claude Console](https://platform.claude.com/) |

## Setting the Key

Run in your own terminal:

```bash
muggle llm-source
```

Choose **My own Anthropic API key**, then paste the key at the hidden prompt. An empty prompt cancels.

The key is accepted only at that prompt — never as a command-line argument and never in chat with your AI assistant — so it stays out of shell history and conversation transcripts. `muggle init` asks the same question in the terminal. Your AI assistant's `/muggle:preferences` skill can switch sources for you, but sends you to `muggle llm-source` to enter a key.

## Storage

| Item | Location |
| :--- | :------- |
| The key | `~/.muggle-ai/llm-credentials.json` |
| The source choice | `llmEnv` block in `~/.muggle-ai/preferences.json` |

The key stays out of `preferences.json`, which your AI assistant reads. On macOS and Linux the credentials file is readable only by your user account. Every display of the source masks the key to its last four characters.

## Checking and Switching

| Task | Command |
| :--- | :------ |
| Show the source in effect | `muggle llm-source --show` |
| Change the source | `muggle llm-source` |
| Return to your token wallet | `muggle llm-source --use muggle-credit` — also deletes the stored key |

The token wallet option is labeled **Muggle AI credit** in the prompt.

## Key from the Environment

To supply the key without storing it — in CI, for example — set `MUGGLE_LLM_PROVIDER=anthropic` and `MUGGLE_LLM_API_KEY=<your key>` as environment variables of the Muggle MCP server. Variables set there override the stored choice.

## Billing

- Anthropic bills calls on your key under your agreement with Anthropic.
- Nothing is debited from your token wallet for those calls.
- Cloud E2E runs always use Muggle's designated models and your token wallet.

## Next Steps

| Goal | Resource |
| :--- | :------- |
| Run local tests on a local or self-hosted model | [Model Sources](local-testing/model-sources.md) |
| Why a Claude Pro or Max subscription is not an option | [Model Sources — FAQ](local-testing/model-sources.md) |
