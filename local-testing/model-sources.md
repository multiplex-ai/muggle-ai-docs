# Model Sources

Studio, the browser engine that runs your local tests, calls an AI model to choose each step, summarize what happened, and judge the result. The model source decides whose model serves those calls on your computer.

## Sources

| Source | Model | Who pays | Status |
| :----- | :---- | :------- | :----- |
| Token wallet (default) | Muggle's designated models | Debited from your token wallet | ✅ Available |
| Local or self-hosted model | Your OpenAI-compatible endpoint | You — nothing is debited from your token wallet | ✅ Available in `@muggleai/works` 5.11.0 and later |
| Your own Anthropic API key | Muggle's designated models, on your key | Anthropic bills you | Not yet released |
| Claude Pro or Max subscription | — | — | ❌ Not offered — see [FAQ](#faq) |

The source applies only to runs on your computer. Cloud E2E runs always use Muggle's designated models and your token wallet.

## Local or Self-Hosted Model

### Requirements

| Requirement | Details |
| :---------- | :------ |
| `@muggleai/works` | 5.11.0 or later. Check with `muggle --version`. |
| Endpoint | A server exposing the OpenAI-compatible chat completions API under `/v1` — Ollama, LM Studio, vLLM, or Text Generation Inference |
| Model | Must accept image input: Studio sends a screenshot with every step. Vision models include Llama 3.2 Vision, Qwen2.5-VL, LLaVA, and MiniCPM-V. |

Test quality depends on the model you choose.

### Configuration

Add an `llmEnv` block to `~/.muggle-ai/preferences.json`, at the top level beside `preferences` — not inside it:

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

The next local run picks it up; your AI assistant does not need a restart. Changing preferences with `/muggle:preferences` keeps the block. Delete the block to return to your token wallet.

| Setting | Required | Default | Description |
| :------ | :------: | :------ | :---------- |
| `MUGGLE_LLM_PROVIDER` | ✓ | — | `local` |
| `MUGGLE_LLM_BASE_URL` | ✓ | — | Endpoint URL, ending in `/v1` |
| `MUGGLE_LLM_MODEL` | ✓ | — | Model id the endpoint serves |
| `MUGGLE_LLM_API_KEY` | | — | Key the endpoint expects, if it checks one |
| `MUGGLE_LLM_MODEL_PREDICTION` | | `MUGGLE_LLM_MODEL` | Model that chooses each step |
| `MUGGLE_LLM_MODEL_SUMMARIZER` | | `MUGGLE_LLM_MODEL` | Model that summarizes steps and runs |
| `MUGGLE_LLM_MODEL_EVAL` | | `MUGGLE_LLM_MODEL` | Model that judges candidate use cases |
| `MUGGLE_LLM_MAX_TOKENS` | | `8192` | Maximum tokens per reply |
| `MUGGLE_LLM_TEMPERATURE` | | `0.1` | Sampling temperature |
| `MUGGLE_LLM_TOP_P` | | endpoint default | Nucleus sampling |
| `MUGGLE_LLM_TIMEOUT_MS` | | `60000` | Per-request timeout in milliseconds; raise it for slow models |

Every model you name must accept images. The image check covers only `MUGGLE_LLM_MODEL`.

### Endpoints

| Runtime | Default `MUGGLE_LLM_BASE_URL` |
| :------ | :---------------------------- |
| Ollama | `http://localhost:11434/v1` |
| LM Studio | `http://localhost:1234/v1` |
| vLLM | `http://localhost:8000/v1` |
| Text Generation Inference | `http://<host>:<port>/v1` |

Set `MUGGLE_LLM_MODEL` to the exact id the server serves. Most servers list their ids at `<base URL>/models`.

### Environment Variables

Each setting can also be an environment variable of the Muggle MCP server, such as an entry in the `env` block of your MCP config (see [Local Testing Setup](local-testing/setup.md)). A variable set there overrides the same name in `llmEnv`.

Your AI assistant can read `preferences.json`. If your endpoint needs a key, set `MUGGLE_LLM_API_KEY` as an environment variable rather than in the file.

### Behavior

- **Image check.** Before a run's first model call, Studio sends your model a small image. If that request fails, the run stops.
- **No cloud fallback.** If your endpoint is unreachable or returns an error, the run fails. Studio never switches to Muggle's models.
- **Billing.** Calls served by your endpoint are not debited from your token wallet.

### Data Handling

Model calls go to your endpoint. Local mode does not take Studio offline:

- Running a test still requires signing in to Muggle Test; Studio runs the test cases stored in your account.
- Studio currently sends Muggle a text record of every model call — the prompt and the model's reply, without screenshots — including calls your endpoint serves.
- Uploading local results follows the `autoPublishLocalResults` preference, as for any local run.

### Known Issue

Some internal calls request the model id `gpt-4o-mini-2024-07-18` instead of yours: repairing a malformed model reply, and recording run memory at the end of each run or when a step cannot reach its goal. An endpoint that does not serve that id rejects them with a 404. The run's memory is then not saved, and a step that cannot reach its goal reports the 404.

Until a fix ships, serve your model under that id as well:

| Runtime | Workaround |
| :------ | :--------- |
| Ollama | `ollama cp <your-model> gpt-4o-mini-2024-07-18` |
| vLLM | Add `gpt-4o-mini-2024-07-18` to `--served-model-name` |

### Troubleshooting

Errors from your endpoint can appear after the prefix `Failed to check or debit tokens:` even though nothing is debited. Read the text after it.

| Symptom | Cause | Fix |
| :------ | :---- | :-- |
| Every run fails immediately: `Local LLM provider selected but no baseURL is configured` (or `no model`) | `MUGGLE_LLM_BASE_URL` or `MUGGLE_LLM_MODEL` missing | Add the setting |
| `Model "<id>" does not accept image input: Connection error` | Endpoint not running, or wrong URL | Start the server; check the URL ends in `/v1` |
| `Model "<id>" does not accept image input: 404 …` | The endpoint does not serve that id | Use an id from `<base URL>/models` |
| `Model "<id>" does not accept image input` with any other cause | The model has no vision support | Switch to a vision model |
| 404 naming `gpt-4o-mini-2024-07-18` | [Known issue](#known-issue) | Apply the workaround |
| Requests time out | Slow model or hardware | Raise `MUGGLE_LLM_TIMEOUT_MS` |
| Runs still debit your token wallet | `llmEnv` placed inside `preferences`, `MUGGLE_LLM_PROVIDER` not `local`, or `@muggleai/works` older than 5.11.0 | Fix the block or upgrade |

## FAQ

**Can I use my Claude Pro or Max subscription?**
No. Anthropic's terms reserve subscription sign-in for Anthropic's own applications: "Anthropic does not permit third-party developers to offer Claude.ai login into their own applications, or to route requests through Free, Pro, or Max plan credentials on behalf of their users" ([Claude Code legal and compliance](https://code.claude.com/docs/en/legal-and-compliance)). Muggle Test does not offer it.

**Can I use my own Anthropic API key?**
Not yet. Support is in development and not in any released version of `@muggleai/works`.

**Does a local model apply to cloud E2E runs?**
No. Cloud E2E runs always use Muggle's designated models and your token wallet.

## Next Steps

| Goal | Resource |
| :--- | :------- |
| Other settings in `preferences.json` | [Preferences](local-testing/preferences.md) |
| MCP server environment variables | [Local Testing Setup](local-testing/setup.md) |
| How the token wallet is billed | [Subscription Plans](billing/subscription-plans.md) |
