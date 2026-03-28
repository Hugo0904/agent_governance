# PR Review 治理規範

## 1. 適用範圍（強制）
- 僅適用 PR / code review 任務。
- 非 review 任務不得導讀本檔。

## 2. 雙層檢核（強制）
- 每次 review 必須同時遵守：
  1. 全域 reviewer 規範（`agent_governance/agents/code-reviewer-*.md`）
  2. 目標專案規範（專案 `AGENTS.md` 與 `agent_rules/*.md`）
- 若規範衝突：專案規範優先；未定義時回退全域規範。

## 3. Reviewer 指派策略（強制）
- 預設 reviewer：`code-reviewer-ravi`。
- 若任務明確指定 reviewer，依指定執行。
- 若專案規範有領域 reviewer 映射，優先使用映射。
- 若無法判斷，先回退 Ravi，再於回覆中標註「可補充領域 reviewer」。

## 4. 審查輸出格式（強制）
- 先列 Findings（依嚴重度排序，附檔案與行號）。
- 再列 Open Questions / Assumptions。
- 最後才放簡短 Summary。

## 5. 程式註解準則（強制）
- 不新增「描述字面行為」的低價值註解。
- 只在必要時新增「原因 / 取捨 / 風險」類註解。

## 6. 擴充約定（強制）
- 新增領域 reviewer 時，檔名使用：`agents/code-reviewer-<domain>.md`。
- 並在本檔補上：
  1. 適用領域
  2. 觸發條件（關鍵字 / 專案映射）
  3. 回退策略（通常回退 Ravi）
