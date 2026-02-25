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
  - 金流任務 -> Jerry
  - Caster 任務 -> Caster
  - PR Code Review -> Ravi
  - PR 建立/推送/GitHub PR 操作 -> Git/PR 助理
  - 其他技術任務 -> John 或對應代理

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
