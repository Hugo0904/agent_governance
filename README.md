# agent_governance

本目錄集中管理可跨機重用的代理與規範文件。

## 結構
- `agents/`：代理定義（例如 Fulla、John、Jerry、Ann、Caster、AI Engineer）
- `agent_docs/`：規範與流程文件（進度、委派、路徑、命令）
  - DB 規範目錄：`agent_docs/db/README.md`
  - 語言規範目錄：`agent_docs/languages/README.md`
  - 長期 Markdown 變更治理：`agent_docs/md_change_governance.md`
  - PR review 治理：`agent_docs/review_governance.md`
  - Task 規範目錄：`agent_docs/task/README.md`
  - Task intake 流程：`agent_docs/task_intake_workflow.md`
  - Task 文件契約：`agent_docs/task_document_contract.md`
  - 客製化擴充索引：`agent_docs/custom_extensions.md`

## Markdown 治理補充
- `agents/ai-engineer.md`：
  用於長期 md 治理、樹狀索引整合、context routing 與 token 成本檢查。
- `config/md_change_review_state.json`：
  用於追蹤長期 md 變更的加權分數；達門檻後需啟動整合審查並重置。

## 主規範入口
- `<WORKSPACE_ROOT>/AGENTS.md`

## 路徑原則
- 本倉庫只放「可跨機重用」規則，不放機器綁定絕對路徑。
- 專案路徑映射屬於客製化擴充，啟用時參考：`agent_docs/project_mapping.md`（由 `agent_docs/custom_extensions.md` 索引）。
- 執行時由 `WORKSPACE_ROOT` 展開 `<WORKSPACE_ROOT>`；若未設定則回退使用 `AI_ALLOWED_ROOT`。
