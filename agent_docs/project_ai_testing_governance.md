# 專案 AI 測試治理

本文件定義「跨專案通用」的 AI 測試治理原則。  
各專案仍需在自己的 `AGENTS.md`、`agent_rules/`、`tests/` 內落地實作。

## 目的
- 讓上層 AI 服務可直接讀取專案內的測試契約，而不是依賴對話記憶。
- 讓未使用 `ai-agent-hub` 的工程師，仍可只靠專案 repo 內文件延續同一套規範。
- 避免測試邏輯只綁定單一 CLI command；CLI 只是測試命令的一種形式。

## 分層原則
- `ai-agent-hub`
  - 放通用方法論、層級設計、命名原則、治理規範。
  - 不承載特定專案 feature 與腳本細節。
- 專案 `AGENTS.md`
  - 放該專案可落地執行的入口規則。
  - 必須讓單獨進 repo 的工程師也能維護測試規範。
- 專案 `agent_rules/AI_TEST_GUIDE.md`
  - 放該專案如何讓 AI 選擇與執行測試。
- 專案 `tests/scripts/<feature>/`
  - 放 feature 相關的 scenario script md。
  - 每個 script 應保持可參數化，不綁死固定帳號。
  - 只保留人看得懂的操作流程與預期結果。
- 專案 `tests/ai_test_registry.php`
  - 作為主 registry。
  - 每個 script 只保留：
    - `id`
    - `feature`
    - `doc`
    - `touches`
    - `tests`

## Registry 設計原則
- 主體應為宣告式資料，不應把流程寫死在自然語言說明中。
- 測試欄位應保持最小，只保留 AI 選測與執行真正需要的資料。
- 測試執行方式應可擴充，不限定為 CLI。
- script 應保持可參數化，不綁死單一帳號或單一平台。
- write 測試應明確標示需使用者確認。
- 測試選擇粒度應為 script，不是命中 feature 就整包全跑。

## Script 角色
- `tests/scripts/<feature>/*.md` 是活的規格（living spec），不只是測試備忘錄。
- script 的責任是定義：
  - 這個功能目前承諾的行為
  - 哪些情境屬於回歸風險
  - AI 修改後應如何驗證沒有改壞
- 若本次需求是「刻意修改業務邏輯」，AI 必須同步更新：
  - 對應 script
  - `tests/ai_test_registry.php`
  - 必要的測試命令
- 若程式邏輯已改，但 script 仍描述舊行為，該任務不應視為完成。

## 推薦欄位
- `project`
- `scripts`
- `scripts.*.id`
- `scripts.*.feature`
- `scripts.*.doc`
- `scripts.*.touches`
- `scripts.*.tests`
- `scripts.*.tests.*.mode`
- `scripts.*.tests.*.command`
- `scripts.*.tests.*.summary`

## AI 執行原則
1. 先讀專案 `AGENTS.md`
2. 再讀 `agent_rules/AI_TEST_GUIDE.md`
3. 再讀 `tests/ai_test_registry.php`
4. 依改動檔案命中 script
5. 判斷本次改動是否屬於高風險，且是否需要測試結果來決定是否繼續修正
6. 若需要，可先自行執行 read-only 測試；若不需要，收尾時再簡短詢問使用者是否要跑本次命中的測試
7. 若有 manual 檢查，在最終回覆提醒

## 重要邊界
- 跨專案 scenario 可以存在於更上層治理文件，但單一 repo 不應直接依賴另一 repo 才能自測。
- 專案內若新增測試能力，應先補 registry 與 guide，再補測試命令。
- 若 AI 同時看得到多個專案，應在各專案各自做同一套 script match 流程；看不到的專案不得假設存在。
- write 測試一律需先詢問使用者，不可自行執行。
- 若 script fail 的原因是「需求刻意改變行為」，AI 應先更新 script 與預期，再決定是否調整測試命令；不可為了讓測試通過而硬刪驗證。
