# 客製化擴充索引

本檔用於管理「非通用、與特定專案族群綁定」的擴充規範。

## 角色
- 本檔是客製化擴充的第二層索引。
- `AGENTS.md` 只需判斷是否進入客製化主題；具體讀哪份客製化文件，由本檔往下分流。

## 啟用條件
- 由 `.env` 參數 `AI_CUSTOM_EXTENSIONS_ENABLED` 控制。
- 啟用時才可導讀本檔與以下擴充內容。
- 停用時，核心流程應僅使用通用治理檔案。

## 客製化 agent_docs
- 專案路徑與特性：`agent_governance/agent_docs/project_mapping.md`

## 客製化 agents（專案族群）
- `agent_governance/agents/supreme-code-modifier.md`
- `agent_governance/agents/payment-engineer.md`
- `agent_governance/agents/caster-engineer.md`
- `agent_governance/agents/game-platform-api-integrator-yaris.md`

## 使用原則
- 本檔僅作為擴充入口索引，不取代核心治理規範。
- 若部署環境與上述專案族群無關，建議停用客製化擴充或改寫本檔內容。
