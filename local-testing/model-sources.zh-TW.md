# 模型來源

Studio 是執行你本機測試的瀏覽器引擎，它會呼叫 AI 模型來選擇每個步驟、摘要所發生的情況，並判定結果。模型來源決定在你的電腦上由誰的模型處理這些呼叫。

## 來源

| 來源 | 模型 | 付費者 | 狀態 |
| :----- | :---- | :------- | :----- |
| Token 錢包（預設） | Muggle 指定的模型 | 從你的 Token 錢包扣除 | ✅ 可用 |
| 本機或自行託管模型 | 你的相容 OpenAI 端點 | 你自己——不會從你的 Token 錢包扣除任何費用 | ✅ `@muggleai/works` 5.11.0 以上版本可用 |
| 你自己的 Anthropic API 金鑰 | Muggle 指定的模型，使用你的金鑰 | 由 Anthropic 向你收費 | 尚未推出 |
| Claude Pro 或 Max 訂閱 | — | — | ❌ 不提供——請參閱[常見問題](#faq) |

模型來源僅適用於在你電腦上進行的執行。雲端 E2E 執行一律使用 Muggle 指定的模型與你的 Token 錢包。

## 本機或自行託管模型

### 需求

| 需求 | 詳細資訊 |
| :---------- | :------ |
| `@muggleai/works` | 5.11.0 以上版本。可用 `muggle --version` 查看。 |
| 端點 | 在 `/v1` 下提供相容 OpenAI 的 Chat Completions API 的伺服器——Ollama、LM Studio、vLLM 或 Text Generation Inference |
| 模型 | 必須支援影像輸入：Studio 每個步驟都會傳送一張螢幕截圖。視覺模型包括 Llama 3.2 Vision、Qwen2.5-VL、LLaVA 與 MiniCPM-V。 |

測試品質取決於你選擇的模型。

### 設定

在 `~/.muggle-ai/preferences.json` 中新增 `llmEnv` 區塊，放在最上層、與 `preferences` 並列——不要放在它裡面：

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

下一次本機執行就會套用，不需要重新啟動你的 AI 助理。使用 `/muggle:preferences` 變更偏好設定時，會保留這個區塊。刪除此區塊即可改回使用你的 Token 錢包。

| 設定 | 必填 | 預設值 | 說明 |
| :------ | :------: | :------ | :---------- |
| `MUGGLE_LLM_PROVIDER` | ✓ | — | `local` |
| `MUGGLE_LLM_BASE_URL` | ✓ | — | 端點網址，以 `/v1` 結尾 |
| `MUGGLE_LLM_MODEL` | ✓ | — | 端點提供的模型 ID |
| `MUGGLE_LLM_API_KEY` | | — | 端點要求的金鑰（若端點會檢查金鑰） |
| `MUGGLE_LLM_MODEL_PREDICTION` | | `MUGGLE_LLM_MODEL` | 選擇每個步驟的模型 |
| `MUGGLE_LLM_MODEL_SUMMARIZER` | | `MUGGLE_LLM_MODEL` | 摘要步驟與執行的模型 |
| `MUGGLE_LLM_MODEL_EVAL` | | `MUGGLE_LLM_MODEL` | 評估候選使用者旅程的模型 |
| `MUGGLE_LLM_MAX_TOKENS` | | `8192` | 每次回覆的 Token 上限 |
| `MUGGLE_LLM_TEMPERATURE` | | `0.1` | 取樣溫度 |
| `MUGGLE_LLM_TOP_P` | | 端點預設值 | 核取樣 |
| `MUGGLE_LLM_TIMEOUT_MS` | | `60000` | 每個請求的逾時時間（毫秒）；模型較慢時請調高 |

你指定的每個模型都必須支援影像輸入。影像檢查只涵蓋 `MUGGLE_LLM_MODEL`。

### 端點

| 執行環境 | 預設 `MUGGLE_LLM_BASE_URL` |
| :------ | :---------------------------- |
| Ollama | `http://localhost:11434/v1` |
| LM Studio | `http://localhost:1234/v1` |
| vLLM | `http://localhost:8000/v1` |
| Text Generation Inference | `http://<host>:<port>/v1` |

將 `MUGGLE_LLM_MODEL` 設為伺服器提供的確切 ID。大多數伺服器會在 `<base URL>/models` 列出其模型 ID。

### 環境變數

每項設定也可以設為 Muggle MCP 伺服器的環境變數，例如 MCP 設定中 `env` 區塊裡的一個項目（請參閱[本機測試安裝與設定](local-testing/setup.md)）。在該處設定的變數會覆寫 `llmEnv` 中的同名設定。

你的 AI 助理能讀取 `preferences.json`。如果你的端點需要金鑰，請將 `MUGGLE_LLM_API_KEY` 設為環境變數，而不要寫在檔案中。

### 行為

- **影像檢查。** 在一次執行首度呼叫模型之前，Studio 會傳送一張小圖片給你的模型。如果該請求失敗，執行就會停止。
- **不會改用雲端。** 如果你的端點無法連線或回傳錯誤，執行就會失敗。Studio 絕不會改用 Muggle 的模型。
- **計費。** 由你的端點處理的呼叫，不會從你的 Token 錢包扣款。

### 資料處理

模型呼叫會傳送到你的端點。本機模式並不會讓 Studio 離線：

- 執行測試仍須登入 Muggle Test；Studio 執行的是儲存在你帳戶中的測試案例。
- Studio 目前會將每次模型呼叫的文字記錄（提示詞與模型的回覆，不含螢幕截圖）傳送給 Muggle，其中也包括由你的端點處理的呼叫。
- 與任何本機執行相同，是否上傳本機結果取決於 `autoPublishLocalResults` 偏好設定。

### 已知問題

部分內部呼叫請求的模型 ID 是 `gpt-4o-mini-2024-07-18`，而不是你的模型：包括修復格式錯誤的模型回覆，以及在每次執行結束時或某個步驟無法達成目標時記錄執行記憶。不提供該 ID 的端點會以 404 拒絕這些呼叫。此時該次執行的記憶不會儲存，而無法達成目標的步驟會回報這個 404 錯誤。

在修正推出之前，請讓你的模型同時以該 ID 提供服務：

| 執行環境 | 暫時解決方法 |
| :------ | :--------- |
| Ollama | `ollama cp <your-model> gpt-4o-mini-2024-07-18` |
| vLLM | 將 `gpt-4o-mini-2024-07-18` 加入 `--served-model-name` |

### 疑難排解

來自你端點的錯誤可能會出現在前綴 `Failed to check or debit tokens:` 之後，即使實際上並未扣款。請閱讀前綴後面的內容。

| 症狀 | 原因 | 解決方法 |
| :------ | :---- | :-- |
| 每次執行都立即失敗：`Local LLM provider selected but no baseURL is configured`（或 `no model`） | 缺少 `MUGGLE_LLM_BASE_URL` 或 `MUGGLE_LLM_MODEL` | 新增該設定 |
| `Model "<id>" does not accept image input: Connection error` | 端點未執行，或網址錯誤 | 啟動伺服器；確認網址以 `/v1` 結尾 |
| `Model "<id>" does not accept image input: 404 …` | 端點不提供該 ID | 使用 `<base URL>/models` 中列出的 ID |
| `Model "<id>" does not accept image input`，且為其他原因 | 該模型不支援視覺輸入 | 改用視覺模型 |
| 提到 `gpt-4o-mini-2024-07-18` 的 404 錯誤 | [已知問題](#known-issue) | 套用暫時解決方法 |
| 請求逾時 | 模型或硬體速度太慢 | 調高 `MUGGLE_LLM_TIMEOUT_MS` |
| 執行仍從你的 Token 錢包扣款 | `llmEnv` 放在 `preferences` 內、`MUGGLE_LLM_PROVIDER` 不是 `local`，或 `@muggleai/works` 版本早於 5.11.0 | 修正該區塊或升級 |

## 常見問題

**我可以使用自己的 Claude Pro 或 Max 訂閱嗎？**
目前不行。Anthropic 的條款禁止第三方產品處理訂閱憑證：「Anthropic 不允許第三方開發人員在自己的應用程式中提供 Claude.ai 登入，也不允許代表其使用者透過 Free、Pro 或 Max 方案的憑證轉送請求」，而且開發人員「不得收集、儲存或中介 Claude.ai 憑證或工作階段權杖」（[Claude Code 法律與法規遵循](https://code.claude.com/docs/en/legal-and-compliance)）。Muggle Test 絕不會要求你登入 Claude 或提供工作階段權杖。

相同條款允許你以自己的訂閱登入未經修改的 Claude Code。不過，Muggle Test 的模型呼叫目前並不會透過它執行。

**我可以使用自己的 Anthropic API 金鑰嗎？**
目前還不行。相關支援仍在開發中，尚未包含在任何已發布的 `@muggleai/works` 版本中。

**本機模型適用於雲端 E2E 執行嗎？**
不適用。雲端 E2E 執行一律使用 Muggle 指定的模型與你的 Token 錢包。

## 後續步驟

| 目標 | 資源 |
| :--- | :------- |
| `preferences.json` 中的其他設定 | [偏好設定](local-testing/preferences.md) |
| MCP 伺服器環境變數 | [本機測試安裝與設定](local-testing/setup.md) |
| Token 錢包如何計費 | [訂閱方案](billing/subscription-plans.md) |
