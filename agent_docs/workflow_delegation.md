# Agent 工作流程與委派規範

## Agent 列表與路徑
- 芙拉 / Fulla：`executive-secretary-fulla`（`<AI_AGENT_HUB_ROOT>/agent_governance/agents/executive-secretary-fulla.md`）
- John：`supreme-code-modifier`（`<AI_AGENT_HUB_ROOT>/agent_governance/agents/supreme-code-modifier.md`）
- Jerry：`payment-engineer`（`<AI_AGENT_HUB_ROOT>/agent_governance/agents/payment-engineer.md`）
- Ann：`slack-sender`（`<AI_AGENT_HUB_ROOT>/agent_governance/agents/slack-sender.md`）
- Caster：`caster-engineer`（`<AI_AGENT_HUB_ROOT>/agent_governance/agents/caster-engineer.md`）
- Ravi（PR/Code Review）：`code-reviewer-ravi`（`<AI_AGENT_HUB_ROOT>/agent_governance/agents/code-reviewer-ravi.md`）
- Git/PR 助理：`git-pr-assistant`（`<AI_AGENT_HUB_ROOT>/agent_governance/agents/git-pr-assistant.md`）

## 路徑規則
- 代理路徑與專案映射以 `agent_governance` 內文件為主。

## 模式切換
1. 用戶說「芙拉」或「呼叫芙拉」時，切換至 *executive-secretary-fulla* 模式。
2. 保持芙拉模式，直到用戶說「主模式」才切回。
3. 不確定時，主動詢問：「您是要找芙拉還是主模式？」

## 技術工作委派

### 情境 A: 用戶呼叫「芙拉」時
- 芙拉接收需求後，一律先委派給 John（`supreme-code-modifier`）。
- John 依任務內容判斷是否轉委派。
- 涉及支付 / 金流：John 轉委派給 Jerry（`payment-engineer`）。
- 涉及 Caster 系統：John 轉委派給 Caster（`caster-engineer`）。
- 涉及 PR Code Review：John 轉委派給 Ravi（`code-reviewer-ravi`）。
- 涉及 PR 建立 / 推送 / GitHub PR 操作：John 可轉委派給 Git/PR 助理（`git-pr-assistant`）。
- 其他專業領域：John 轉委派給對應代理。
- 一般程式碼修改：John 自行處理。
- 禁止芙拉直接委派給 Jerry 或其他專業代理（必須透過 John）。

### 情境 B: 用戶使用主模式時
- 可直接判斷並委派對應代理。
- 金流任務：直接委派給 Jerry（`payment-engineer`）。
- Caster 系統任務：直接委派給 Caster（`caster-engineer`）。
- PR Code Review：直接委派給 Ravi（`code-reviewer-ravi`）。
- PR 建立 / 推送 / GitHub PR 操作：可委派給 Git/PR 助理（`git-pr-assistant`）。
- 其他技術任務：直接委派給 John 或對應代理。

## Slack 通知機制（強制）
- 必須三個時點：委派、接收、完成。
- 訊息格式：簡潔文字 + `•` 與 `→`，避免過多 emoji。
- 執行方式：使用 Task 工具調用 `slack-sender`（Ann）。
- 委派標註規則：呼叫 Ann 時，prompt 開頭須標明身份（例如「我是 Jerry」）。

**範例（金流委派）**
1. 芙拉委派：`• 芙拉委派 → John：Tendoorpay 金流整合`
2. John 接收：`• John 收到委派：Tendoorpay 金流整合`
3. John 轉委派：`• John 轉委派 → Jerry：Tendoorpay 金流開發`
4. Jerry 接收：`• Jerry 收到委派：Tendoorpay 金流開發`
5. Jerry 完成：`• Jerry 完成：Tendoorpay 金流開發`
6. John 完成：`• John 完成：Tendoorpay 金流整合`

## 搜尋規則（禁止無效搜尋）
1. 關鍵字優先：先用精確函式 / 類名。
2. 限制檔案類型：`*.php`、`*.js`。
3. 限制目錄範圍：指定目標資料夾。
4. 禁止行為：整個專案無限制搜尋、模糊關鍵字。

## 違反流程後果
- 未發送 Slack 通知：視為溝通失職。
- 無效搜尋（大範圍、模糊）：視為效率低落。
