# 長期 Markdown 變更治理

本文件用於規範「新增 / 修改 / 重組長期 md」時，AI 在真正編輯前必須先完成的治理判斷。  
目的不是拖慢修改，而是避免 AI 一聽到「幫我加進 md」就把內容隨便塞進某個檔案。

## 適用範圍
- `AGENTS.md`
- `agent_governance/agent_docs/*.md`
- `agent_governance/agents/*.md`
- 其他會被重複導讀、作為長期規則來源的 md

## 不適用範圍
- `progress/*.md`
- task 任務檔
- 單次交接備忘

## 編輯前 hard gate（強制）
- 在修改長期 md 前，AI 必須先明確回覆以下四件事：
  - `沉澱價值`：這件事是否值得升級成長期規則，而不是只留在 task / progress。
  - `建設性觀點`：從長期維護、可重用性、減少重複指正、降低 review 成本的角度，說明為何值得寫。
  - `對立角度`：從 root 膨脹、重複入口、token 成本、過早抽象、規則漂移的角度，說明為何不該這樣寫或不該落在這一層。
  - `建議落點`：明確說明應改哪一份文件，以及為什麼不是改到其他層。
- 若上述四件事還說不清楚，不得直接開始改長期 md。
- 若使用者明顯仍在討論設計思路、規則方向、或資訊架構取捨，AI 必須先和使用者完成一輪支持 / 反對角度討論，再進入編輯。

## 落點判斷順序（強制）
1. 先判斷是否屬於長期規則。
2. 若不是長期規則，只記 task / progress。
3. 若是長期規則，先判斷是：
   - 專案自己的規則
   - workspace 通用治理
   - 某個 family 的 leaf
4. 若已存在對應 family `README.md`，優先掛回該 family，不直接新增到 `AGENTS.md`。
5. 只有在 root 缺少入口時，才改 `AGENTS.md`。

## 與 `AGENTS.md` 的邊界（強制）
- `AGENTS.md` 是 root router，不是規則收納箱。
- 下列情況通常不應直接改 `AGENTS.md`：
  - 已存在 family README，可在第二層分流
  - 問題出在 leaf 內容不足，而不是入口缺失
  - 只是新增另一份同主題 leaf
- 下列情況才優先考慮改 `AGENTS.md`：
  - AI 根本不知道要先進哪個主題
  - 第一層載入時機判斷錯了
  - 缺少某個 family 的入口

## Registry 維護（強制）
- 若本次新增 / 搬移 / 重新分層的文件，會影響 required context 的樹狀入口或 leaf 關係，必須同步檢查：
  - `config/context_registry.json`
  - `config/required_context_paths.txt`
- 若新增的是 family README：
  - 補 parent / child 關係到 `context_registry.json`
  - 補候選路徑到 `required_context_paths.txt`
- 若新增的是 leaf：
  - 先補父層 `README.md`
  - 再判斷是否需要進 `context_registry.json` 與 `required_context_paths.txt`
- 不得出現：
  - md 已新增，但 registry 不知道它
  - registry 已指向該 md，但 allow-list 沒放
  - 同一份 leaf 同時被多個父層索引

## md 變更分數累積（強制）
- 長期 md 只要有新增 / 修改 / 重組，就要同步更新：
  - `config/md_change_review_state.json`
- 建議優先使用：
  - `python3 scripts/update_md_change_review_state.py --delta <N> --summary "<本次變更摘要>" --files <files...>`
- `current_edit_count`：
  用來記錄本輪累積幾次 md 變更。
- `current_score`：
  用來記錄本輪變更權重；到達 `score_threshold=10` 就要做整合審查。
- `recent_events`：
  只保留最近一小段有界事件，提供整合審查參考；不可把它當成無上限日誌。
- 事件內容只記：
  - 短摘要
  - 裁切後的檔案名清單
  - 總檔案數
- 不記完整 diff、長篇逐字紀錄、或無上限歷史，避免 state 膨脹與 token 浪費。

## 分數判斷指引（強制）
- `+1`：
  單行修正文案、輕微措辭調整、錯字修正、單點補充。
- `+3`：
  單份長期 md 的小型規則補強、局部段落重寫、少量索引修正。
- `+5`：
  多份 md 連動調整、family / leaf 關係修正、需同步 registry / allow-list 的變更。
- `+10`：
  架構級調整、root / family / leaf 重新分層、matching 策略大幅重整、或一次修改足以需要全面整合確認。
- 若不確定，寧可偏高，不要低估。

## 達門檻後的整合審查（強制）
- 當 `config/md_change_review_state.json.current_score >= score_threshold` 時：
  1. 以 prompt directive 啟用 `ai-engineer`：
     `[ACTIVE_AGENT_FILE] agent_governance/agents/ai-engineer.md`
  2. 以資深 AI 工程師視角做一次整合審查
  3. 先讀 `config/md_change_review_state.json.recent_events`，掌握近期變更脈絡，但只看近期有界事件，不回放完整歷史
  4. 檢查：
     - 是否有重複入口
     - 是否有提示太鬆或太模糊
     - 是否有應落在 family 卻回流 root 的規則
     - registry / allow-list / 文件樹是否漂移
  5. 完成必要整併後，執行：
     - `python3 scripts/update_md_change_review_state.py --complete-review --summary "<整合摘要>"`
- 未完成整合審查，不得直接把達門檻的狀態留著不處理。

## 驗證要求（強制）
- 只要本次變更涉及 required context 樹，就應執行：
  - `python3 scripts/validate_context_registry.py`
- 若驗證失敗，不應把該次 md 變更視為完成。

## 回覆風格（強制）
- AI 不必長篇大論，但必須先給出治理判斷，再開始編輯。
- 若使用者明確指定某個落點，AI 仍應提出至少一個支持理由與一個反對理由，再決定是否照做。
- 若使用者是在提想法，不是在下定稿指令，AI 應先討論結構、風險、替代方案，再修改 md。
- `md_change_review_state.json` 的一般分數累積與未達門檻狀態屬內部維護，不需在任務收尾時主動回報；只有達門檻並啟動或完成整合審查，或使用者主動詢問此機制時，才簡短說明。
