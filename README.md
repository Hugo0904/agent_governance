# agent_governance

本目錄集中管理可跨機重用的代理與規範文件。

## 結構
- `agents/`：代理定義（例如 Fulla、John、Jerry、Ann、Caster、AI Engineer）
- `agent_docs/`：規範與流程文件（進度、委派、路徑、命令）
  - DB 規範目錄：`agent_docs/db/README.md`
  - 語言規範目錄：`agent_docs/languages/README.md`
  - 長期 Markdown 變更治理：`agent_docs/md_change_governance.md`
  - 規則演化治理：`agent_docs/rule_evolution_governance.md`
  - PR review 治理：`agent_docs/review_governance.md`
  - Task 規範目錄：`agent_docs/task/README.md`
  - Task intake 流程：`agent_docs/task_intake_workflow.md`
  - Task 文件契約：`agent_docs/task_document_contract.md`

## Markdown 治理補充
- `agents/ai-engineer.md`：
  用於長期 md 治理、樹狀索引整合、context routing 與 token 成本檢查。
- 宿主可提供 machine-readable context registry、md change review state 與 rule evolution policy；本倉庫只定義它們應支援的治理行為，不指定實體路徑或命令。

## 宿主整合邊界
- 本倉庫只定義可重用的代理角色、治理文件與可選擴充內容，不指定宿主專案的主規範入口。
- 安裝位置、目標 workspace、絕對路徑、環境變數與 placeholder 展開方式，均由採用本套件的宿主系統負責。
- 專案映射、本機操作資料與客製化啟用清單屬於宿主設定，不放入本倉庫。
- 若需調整某個宿主如何發現、載入或限制本套件，應修改宿主整合規範，不應把宿主本機規則寫入本倉庫。
