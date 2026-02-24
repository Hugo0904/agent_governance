# agent_governance

本目錄集中管理可跨機重用的代理與規範文件。

## 結構
- `agents/`：代理定義（例如 Fulla、John、Jerry、Ann、Caster）
- `agent_docs/`：規範與流程文件（進度、委派、路徑、命令）

## 主規範入口
- `<WORKSPACE_ROOT>/AGENTS.md`

## 路徑原則
- 本倉庫只放「可跨機重用」規則，不放機器綁定絕對路徑。
- 路徑映射主檔為：`agent_docs/project_mapping.md`。
- 執行時由 `WORKSPACE_ROOT` 展開 `<WORKSPACE_ROOT>`；若未設定則回退使用 `AI_ALLOWED_ROOT`。
