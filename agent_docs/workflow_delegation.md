# Agent 工作流程與委派規範

## 1. Agent 清單
- 芙拉 / Fulla：`executive-secretary-fulla`
- John：`supreme-code-modifier`
- Jerry：`payment-engineer`
- Ann：`slack-sender`
- Caster：`caster-engineer`
- Ravi：`code-reviewer-ravi`
- Git/PR 助理：`git-pr-assistant`

## 2. 路徑規則
- 代理路徑、專案映射與流程規範以 `agent_governance` 內文件為準。
- PR review 的 reviewer 指派與擴充規範以 `agent_docs/review_governance.md` 為準。

## 3. 模式切換（強制）
1. 使用者說「芙拉」或「呼叫芙拉」：切到 `executive-secretary-fulla`。
2. 保持芙拉模式，直到使用者說「主模式」才切回。
3. 不確定時必問：「您是要找芙拉還是主模式？」

## 4. 委派規則（強制）

### 4.1 芙拉模式
- 芙拉接收技術需求後，一律先委派 John。
- Jerry/Caster/Ravi/Git-PR 助理皆由 John 轉委派。
- 禁止芙拉直接委派 Jerry 或其他技術代理。

### 4.2 主模式
- 可直接依任務內容委派：
  - 內容明顯涉及支付/金流系統 -> Jerry
  - Caster 任務 -> Caster
  - PR Code Review -> 依 `review_governance.md` 決定（預設 Ravi）
  - PR 建立/推送/GitHub PR 操作 -> Git/PR 助理
  - 其他技術任務 -> John 或對應代理

### 4.3 Task Intake 後的內容導向判定（強制）
- 若使用者以「我要執行任務 {task.md}」等語意啟動任務，必須先讀任務檔內容，再決定是否委派或參考特定代理規範。
- 判定依據以任務內容整體脈絡為準，例如涉及的系統、資料流、文件類型、操作對象、風險性質與既有專案結構；不得只依賴單一句型、單一關鍵字或固定廠商代號。
- 若任務內容明顯在處理支付供應商接入、代收/代付流程、商戶設定、支付回調、對帳、金流後台或其他支付資料流，可委派 Jerry，或至少參考 `payment-engineer` 規範完成導讀與提問。
- 若內容只有局部提到金流，但主題其實是文案、排版、一般後台設定或其他非支付核心變更，則不必強制委派 Jerry。
- 若判定存在歧義，必須在導讀階段主動向使用者確認；不可因想避免遺漏而一律硬套某位代理。

## 5. Slack 通知機制（強制）
- 三個時點必通知：委派、接收、完成。
- 呼叫 Ann 時，prompt 開頭必須標明身份（例：`我是 Jerry`）。
- 訊息格式保持精簡：`•` 與 `→`，避免不必要內容。

## 6. Telegram 完成通知（電腦端，強制）
- Telegram 互動與完成通知細節以 `agent_docs/telegram_interaction.md` 為準。
- 本檔僅定義委派責任，不重複 Telegram 呈現格式與互動細節。

## 7. 搜尋規則（強制）
1. 優先使用精確關鍵字（函式/類名）。
2. 限制檔案類型（如 `*.php`、`*.js`）。
3. 限制目錄範圍到目標子目錄。
4. 禁止無限制全專案模糊搜尋。

## 8. 失敗判定
- 未發送必要 Slack 通知：流程未完成。
- 使用無效搜尋策略（大範圍模糊搜尋）：流程不合規。
