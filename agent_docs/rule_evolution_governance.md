# 規則演化治理

本文件定義 Canopy 如何把使用者指正、任務經驗、md / json 規則與既有規範整合成可重用的長期行為。

## 目的
- 讓規則可以小步微調，但仍可追溯、可驗證、可被取代。
- 讓 md 保持人類可讀，json 保持機器可排序與檢查。
- 避免新規則只靠口語覆蓋舊規則，造成同層互斥或無限補丁。

## 適用時機
- 使用者要求新增、更新、合併或刪除 AI 規則。
- 討論規則優先權、過時規範、衝突規則、`supersedes` / `deprecated`。
- 需要把單次 correction 轉成可套用到未來任務的治理規則。
- 需要設計 md / json 格式，讓 AI 能讀取後轉換成行為。

## 與既有文件的分工
- `correction_intake.md`：
  定義「被指正後如何判斷是否值得吸收」。
- `md_change_governance.md`：
  定義「長期 md 編輯前的治理 hard gate 與 review score」。
- 本文件：
  定義「被吸收的規則如何排序、取代、合成、退場與防循環」。

## 規則封包
長期規則若可能被覆蓋、合併或排序，應具備下列 metadata。md 可使用 front matter 或段落清單；json 應直接使用同名欄位。

```yaml
rule_id: workspace.rule_evolution.example
status: active
priority_tier: P3_SCOPE_OWNER
scope: project:<project>
owner_doc: <path-to-owning-md-or-json>
supersedes: []
superseded_by: ""
effective_from: "2026-05-11"
review_after: ""
conflict_policy: ask_if_equal
```

必要欄位：
- `rule_id`：穩定 id，不因文案調整而改名。
- `status`：`draft`、`active`、`superseded`、`deprecated`、`archived`。
- `priority_tier`：使用本文件與 `config/rule_evolution_policy.json` 定義的 tier。
- `scope`：`workspace`、`project:<project>`、`language:<language>`、`agent:<agent>`、`task:<task_id>`。
- `owner_doc`：唯一權威落點；同一規則不可同時由多份 leaf 共同擁有。

選用欄位：
- `supersedes`：本規則明確取代哪些規則。
- `superseded_by`：本規則已被哪個規則取代。
- `effective_from`：規則開始生效日期。
- `review_after`：到期檢查日期；到期只代表需檢查，不代表自動失效。
- `conflict_policy`：`higher_priority`、`more_specific`、`newer_supersedes`、`ask_if_equal`。

既有純文字 md 仍有效；若該段規則開始與其他規則衝突，必須補上等價 metadata 或直接改寫為明確的擁有者段落。

## 優先權分級
優先權不是單純「新蓋舊」。排序依序看：安全層級、規則狀態、priority tier、scope specificity、明確取代關係、日期。

| Tier | 用途 | 規則 |
| --- | --- | --- |
| `P0_SAFETY_BOUNDARY` | 安全、密鑰、白名單、不可外洩 | 不可被使用者偏好、專案規則或舊 task 覆蓋 |
| `P1_RUNTIME_HARD_GATE` | required context、task intake、DB read-only、md hard gate | 必須先滿足，失敗時中止或詢問 |
| `P2_ACTIVE_USER_DIRECTIVE` | 當前回合明確要求 | 只在本次任務生效；若要長期化，需走 correction / md change governance |
| `P3_SCOPE_OWNER` | 專案 `AGENTS.md`、專案 `agent_rules/`、特定 agent 定義 | 在不違反 P0/P1 下，覆蓋 workspace 通用建議 |
| `P4_FAMILY_LEAF_RULE` | 語言、DB、review、task 等 family leaf | 同主題內比 root router 更具體 |
| `P5_WORKSPACE_METHOD` | 跨專案方法論與一般工程原則 | 提供預設策略，不覆蓋專案權威規則 |
| `P6_LOCAL_PREFERENCE` | 本機偏好、文風、操作習慣 | 可被當前任務或專案規則覆蓋 |
| `P7_EXAMPLE_OR_HISTORY` | progress、task 歷史、範例 | 只能當參考，不可單獨推翻 active 規則 |

同一 tier 內再看 scope specificity：
`project:<name>` > `language:<name>` / `agent:<name>` > `workspace` > `task history` > `example`。

## 衝突解法
遇到兩條規則互斥時，按下列順序處理：

1. 排除 `archived` 規則；`deprecated` 只能當歷史背景。
2. 若任一方屬於 `P0_SAFETY_BOUNDARY`，安全規則勝出。
3. 若任一方屬於 `P1_RUNTIME_HARD_GATE`，先滿足 hard gate。
4. 比較 `priority_tier`；高 tier 勝出。
5. 同 tier 比較 `scope`；較具體 scope 勝出。
6. 檢查 `supersedes` / `superseded_by`；明確取代關係勝出。
7. 日期只在同 tier、同 scope、同等權威且有明確變更脈絡時作為輔助。
8. 仍無法判定時，停止修改並詢問使用者；不可透過再新增一條模糊規則來繞過衝突。

## 過時規範處理
- 舊規範不會只因為時間久而自動失效。
- 要讓舊規範退場，必須至少滿足一項：
  - 新規則明確列出 `supersedes`。
  - 舊規則改成 `superseded` 並標明 `superseded_by`。
  - 舊段落被刪除或改寫，且 progress / md change state 記錄摘要。
- 不可只新增「補充說明」放在另一份 md，讓兩份文件同時保持 active 並互相打架。
- 如果舊規範在專案 leaf，新規則也應優先回到同一 leaf 或其父層 README；不要直接回流 root。

## 合成流程
每次要把新資訊轉成長期規則時，使用固定流程：

1. `intake`：
  讀取使用者需求與相關現有 md / json，確認是否為長期規則。
2. `classify`：
  判斷 priority tier、scope、owner_doc、是否涉及 safety / hard gate。
3. `normalize`：
  把口語要求改成可檢查條件句，不直接保留一次性案例。
4. `merge`：
  搜尋同 scope / 同主題既有規則；能改寫舊規則就不要新增平行規則。
5. `supersede`：
  若新規則取代舊規則，同步標記 `supersedes` / `superseded_by` 或移除舊文案。
6. `validate`：
  執行 registry 驗證、必要的 grep 檢查與 md change review score 更新。
7. `report`：
  回覆落點、衝突處理、驗證結果與仍需人工決策的部分。

## 防循環規則
- 同一輪任務中，不得為同一衝突新增超過一條 active override 規則。
- 若第二次合成後仍衝突，停止並要求使用者選擇權威來源。
- 不建立 A supersedes B、B 又 supersedes A 的互相取代關係。
- 不用 `review_after` 或日期自動改變優先權；到期只觸發重新檢查。
- 不把 `progress` 中的歷史結果升級成 active 規則，除非經過 intake / classify / merge。
- 不為了讓 resolver 命中而把大量關鍵字塞進 root；優先調整正確 family 或 leaf 的 routing。

## md 與 json 的使用邊界
- md 是主要規範來源，負責說明規則、背景、判斷流程與人工可讀的例外。
- json 是排序與檢查輔助，負責保存 priority tier、status、conflict policy 這類結構化資料。
- json 不應保存敏感資訊、完整案例 payload、帳密、webhook 或一次性內部資料。
- 若 md 與 json 不一致，以 md_change_governance 的 hard gate 先停下來修正來源，不由 AI 任意猜測。

## 最小驗證
涉及本文件或規則 registry 的變更，收尾前至少檢查：

```bash
python3 scripts/validate_context_registry.py
python3 scripts/resolve_required_context.py --repo-root . --prompt "<rule-evolution prompt>" --context-output /tmp/context.txt --notes-output /tmp/notes.txt --task-mode-output /tmp/mode.txt
```

若本次新增 / 修改長期 md，仍需同步更新 `config/md_change_review_state.json`。
