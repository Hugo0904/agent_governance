# OLTP / OLAP 一致性檢查加速器

本文件是工作區治理層的通用導讀，用於讓 AI 更快進入「OLTP source of truth 與 OLAP / ClickHouse 查詢結果不一致」的調查流程。具體專案的 database 名稱、table、腳本、商業規則與 secret profile 建立細節，必須放在目標專案自己的 `AGENTS.md`、`agent_rules/` 或等價文件中。

## 觸發時機

遇到下列需求時，先進入本流程：

- 使用者要求檢查 OLTP / OLAP / MySQL / MariaDB / ClickHouse 是否一致。
- 報表數字、局數、金額、狀態、每日彙總與明細資料不一致。
- 使用者提到 CDC、ClickHouse、CL、daily、detail、source of truth、分區或 `FINAL`。
- 使用者要求「一年內」、「指定月份」、「多個 database」的批次一致性檢查。

## 導讀順序

1. 先讀目標 repo 的 `AGENTS.md`。
2. 搜尋目標 repo 內是否已有專案專屬文件：
   - `agent_rules/*DB*`
   - `agent_rules/*CL*`
   - `agent_rules/*CLICKHOUSE*`
   - `agent_rules/*CONSISTENCY*`
3. 搜尋目標 repo 內是否已有只讀檢查腳本：
   - `scripts/*consistency*`
   - `scripts/*clickhouse*`
   - `scripts/*olap*`
   - `scripts/*report*`
4. 若專案已有 runbook 或腳本，以專案文件為準；治理層只補強安全與調查順序。
5. 若專案缺 runbook，先用 `db_inspection.md` 的只讀邊界與宿主提供的 workspace DB access profile 做最小範圍查詢，並把可重用流程回補到專案文件。

## Secret 原則

- 不請使用者把正式 IP、帳號、密碼、token、私鑰貼到聊天。
- 若專案文件定義本機 secret profile，依專案文件引導使用者在本機終端機建立檔案。
- 沒有專案文件時，只能要求使用者建立本機 secret profile 或提供安全的既有連線方式；不要臨時把敏感值寫進治理文件、progress、聊天或腳本參數。
- secret profile 檔案應設為只有擁有者可讀寫；檢查時只確認檔案存在與權限，不輸出內容。

## Seed Memory 配合

- Seed Memory 只保存操作者偏好的調查方法、文件分層與防錯提醒，不保存專案商業邏輯、正式連線資訊、查詢出的敏感資料列或單次 debug 結果。
- 若 Seed resolver 召回 OLTP / OLAP 一致性相關 workflow card，將它視為加速提示：先找專案 runbook、先用聚合縮小差異、先判斷 daily stale / CDC / 分區條件 / 程式後處理。
- Seed Memory 的優先權低於安全規則、當前使用者明確指令、目標專案 `AGENTS.md` 與專案內 `agent_rules`。
- 沒有相關 Seed card 時，仍依本文件與目標專案文件執行；不得因缺少 Seed 就停止檢查。

## 調查順序

1. **確認 source of truth**
   - 先確認哪一側是主資料來源。
   - OLAP / ClickHouse 若由 CDC 或批次同步產生，預設不是修資料的第一站。

2. **確認專案查詢口徑**
   - 從報表 URL、controller、service、querier、config 或平台規則找實際 SQL 條件。
   - 先把 engine、date type、search mode、平台、狀態、分區欄位與調整項拆清楚。

3. **先聚合再 drill down**
   - 月範圍先切日期。
   - 日期差異再切 platform / status / bet type / category。
   - 最後才做 key set compare。

4. **分層比對**
   - OLTP daily vs OLTP detail：判斷每日彙總是否 stale。
   - OLTP detail vs OLAP detail：判斷 CDC / OLAP 明細是否一致。
   - OLAP time-only vs OLAP partitioned：判斷分區條件或特殊資料是否造成查詢口徑差異。
   - raw SQL vs application final value：判斷差異是否來自程式後處理。

5. **只讀結論**
   - 先回報差異類型、範圍、bucket、數量與下一步。
   - 不直接做資料修復、不重跑正式排程、不改 schema，除非使用者另行明確授權。

## 回報格式

```text
範圍：<database/table/date/platform 摘要>
自動化：<使用的專案腳本或查詢方式>
結果：
- OLTP daily vs detail：一致 / 差異 bucket 摘要
- OLTP detail vs OLAP detail：一致 / 差異 bucket 摘要
- OLAP time-only vs partitioned：一致 / 差異 bucket 摘要
- application final value：一致 / 待拆解
判斷：daily stale / CDC 差異 / 分區條件 / SQL 口徑 / 程式後處理 / 需進一步 key compare
下一步：<最小可驗證動作>
```
