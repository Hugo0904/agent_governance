# Telegram 互動規則

本文件僅適用於 Telegram 來源任務。

來源判斷：
- 宿主明確標示 task source 為 `telegram`，或
- 等價執行來源標記顯示來自 Telegram adapter。

## 1. 指令互動（強制）
- `/codex`、`/ai` 執行時，使用者可見內容只需回報：已接收 / 處理中、結果、原因或驗證、耗時。
- `task_id`、thread id 與執行流程仍需由宿主內部保存；只有使用者主動查看狀態或需要續接辨識時才顯示。
- 完成後優先提供 Telegram 按鈕續問。
- 保持純文字續問相容（含 `1/2` 這類選項回覆）。

## 2. 任務延續（強制）
- 同一線程續問時，沿用既有 `task_id + task log`。
- 若切換為新會話，應明確建立新 task。
- 文字回覆需可讓使用者快速判斷：
  - 目前線程 / task
  - 任務狀態
  - 下一步操作入口

## 3. 單向通知（電腦端，強制）
- 適用：電腦端發起的專案任務（不論是否由 Telegram 發起）。
- 每次任務完成後，若設定了 Telegram chat，需推送一則完成通知。
- 通知內容保持精簡：需求、完成 / 失敗、結果、原因或驗證、耗時，以及必要的下一步。
- 此通知流程不要求電腦端對話上下文與 Telegram 強制同步。

## 4. 內容風格（強制）
- Telegram 是結果導向的小幫手介面，不是終端機或完整工作台。
- 以可掃讀格式為主，不顯示 command、stdout/stderr、模組載入、內部 hook、完整程式片段或冗長執行過程。
- 完整 output、task context 與歷史仍由宿主 task log / thread history 保存；Telegram 只呈現 bounded digest。
- 優先摘要完成內容、原因、驗證與可執行下一步；失敗時說明阻擋原因。
- 不回傳密鑰、token、密碼、Webhook URL。
