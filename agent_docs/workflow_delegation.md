# Agent 工作流程與委派規範

## 定位
- 本文件定義角色如何被選擇、委派與驗證，不維護固定主管階級或專案路徑。
- 宿主可提供 machine-readable role registry；角色來源只宣告可重用能力與責任契約。

## 選擇順序
1. 當前使用者明確指定的 active role。
2. 宿主依 task intent、domain、project evidence 與角色 contract 得到的穩定單一候選。
3. 候選接近或證據衝突時，由當回合 LLM 選最多一個，或判定 `no_role_needed`。
4. 沒有專業角色能明顯改善任務時，直接由主 agent 執行。

不得只因出現單一關鍵字就載入角色，也不得把多個候選全文合併。角色內容低於安全、當前明確指令與目標專案較近規則。

## 委派契約
- 只有工作能獨立驗證、責任清楚且平行化確實有收益時才委派。
- 每個委派必須提供：目的、輸入、限制、輸出、驗證與升級條件。
- 領域工作可直接交給對應角色，不需經過固定中間角色。
- coordinator 可以整理 owner 與依賴，但不得替 domain role 做專業決策。
- workflow role 只在明確操作意圖成立時使用，例如 code review、Git delivery 或 Slack notification。

## Task Intake 後的判定
- 若使用者以任務檔啟動工作，先完成 task intake，再以任務全文判斷角色。
- 判斷需綜合系統、資料流、操作、風險與專案結構，不以廠商名稱或局部詞彙硬套角色。
- 領域只被局部提及、主要工作不在該領域時，不載入完整 domain role。

## 通知
- 通知只在使用者、上游流程或外部交接確實要求時發送，不是所有委派的固定三次儀式。
- 發送行為使用 `slack-sender` 等 workflow role，並以工具回傳證明實際送達。
- Telegram 或手機通道的呈現遵守 `agent_docs/telegram_interaction.md`，預設只回報結果、原因與必要下一步。

## 驗證與失敗
- 宿主應留下 bounded selection trace：狀態、候選、證據、選定角色與 context 字數，不保存 prompt 或所有角色全文。
- 角色找不到、已 deprecated、與 closer project rule 衝突或 context 超限時，需明確降級或停止，不得假稱已套用。
- 角色選擇錯誤應修正 registry taxonomy、contract 或 eval；不可只為當次 prompt 新增一條角色關鍵字。
