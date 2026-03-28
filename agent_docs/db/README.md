# DB 規範目錄

本目錄作為 DB / 資料驗證主題的第二層索引。  
`AGENTS.md` 應只判斷是否進入本主題；具體讀哪份 DB 文件，由本檔往下分流。

## 何時進入本目錄
- 需求涉及資料不一致、報表數字、歷史資料分佈、schema / migration 影響。
- 問題是否成立，必須靠既有資料狀態而不是只看 code 命名推論。
- 需要判斷是否應主動做只讀 DB 驗證。

## 子文件索引
- `agent_governance/agent_docs/db_inspection.md`
  何時可以主動查看 DB、查詢邊界、只讀原則、檢查順序。
- `agent_governance/agent_docs/workspace_db_access.md`
  只有在已判定需要 DB 驗證，且還需要知道本工作區的本機 / Docker 存取方式時才往下讀。

## 邊界
- 本目錄只做分流，不定義單一專案的資料表真相。
- 若只是語法、命名、重構、UI 或文案問題，不應因為看見 `db` 字樣就進入本目錄。
