# Seed Memory 進化論治理

Seed Memory 是 `ai-agent-hub` 的輕量成長層。它保存「理、規則、思維、判斷方式、工具映射」，不保存大量實戰資料，不取代 LLM 原生能力，也不使用 DB。

## 核心定位
- `ai-agent-hub` 是 Seed Core：
  穩定的學習邏輯、治理方式、價值判斷、安全底線與演化規則。
- `seed_memory/` 是 Seed Memory：
  本地、不版控、可複製、可不複製的經驗蒸餾資料夾。
- Seed Pack 是可攜封裝：
  可把 distilled lessons / habits / capability maps 帶到另一個環境，但仍不得覆蓋對方的安全與 hard gate。
- LLM 是廣泛能力與資料來源；Seed 是權威判斷與學習過濾器：
  Seed 負責判斷哪些結果更好、更符合現代、更符合使用者的工作方式，以及如何預防再犯。

## 教育式成長模型
Seed Memory 不是管教系統，也不是硬性命令清單。它更像使用者長期教育 AI 時留下的成長提醒：

- 鼓勵 AI 下次更早問對問題，而不是要求所有情境一律套舊答案。
- 保存使用者明確教過、修正過、偏好過的細部文化與價值排序。
- 將「跌倒後下次怎麼看路」寫成反思問題，而不是禁止未來探索。
- 每張 active card 都應保留適用邊界；若專案規則、當前明確指令或新工具更適合，必須重新判斷。

卡片文案應優先寫成：

> 遇到某類情境時，建議先考慮某個角度，因為使用者曾指出某種風險或價值；但若當前上下文不同，不要硬套。

避免寫成：

> 以後遇到某類情境一律必須使用某個做法。

## 不做的事
- 不把大量 raw progress、任務歷史、payload、客戶資料或市場資料搬進 Seed Memory。
- 不保存 LLM 本來就會的通用能力，例如一般推理、一般程式語法、通用寫作技巧。
- 不建立資料庫、向量庫或無上限記憶索引。
- 不讓卡片數量直接決定 runtime token；資料夾可以成長，但每次載入必須維持固定上限。

## 可保存的內容
一張 Seed Memory card 必須符合至少一項：
- 使用者偏好的工作方式、判斷標準、品質標準。
- 專案族群或工具鏈特有的操作映射。
- 從多次任務蒸餾出的可重用 lesson。
- 新模組 / 新工具能力如何服務既有好習慣。
- 某類錯誤的防再犯判斷，而不是單次錯誤本身。
- LLM 通常能掌握大方向，但未必能穩定掌握的使用者細部文化，例如產品成熟度、UI/UX 人性化、市場語感、操作壓力、溝通密度或本地工作節奏。

每張 card 必須說明 `non_llm_native_reason`：
為什麼這不是 LLM 原生能力，而是 Seed Core 應該保留的個人化或環境化訓練結果。

## 來源分級
Seed intake 必須區分「使用者教導」與「AI 自己推論」。

每張 card 使用 `source_type` 標註來源：
- `user_instruction`：使用者明確要求某個長期方向。
- `user_correction`：使用者指出 AI 的判斷、實作或溝通方式需要修正。
- `user_preference`：使用者表達取捨傾向、風格或工作偏好。
- `user_teaching`：使用者解釋背後價值觀、文化、教育方式或判斷模型。
- `project_culture`：專案族群反覆形成的細部文化，但尚不適合升級成 hard rule。
- `tool_mapping`：新工具 / 新模組如何映射到既有好習慣。
- `seed_core`：Seed 機制自己的核心設計原則。
- `ai_inferred`：AI 從任務結果自行推論出的 lesson。

`ai_inferred` 不可直接成為 active runtime card；只能先進 candidate，等待使用者確認、重複證據或更高權威文件支撐。  
Active card 應具備 `source_summary`，以短句記錄來源訊號，不保存長篇對話、不保存敏感資訊。

## 成長式欄位
Active card 必須至少具備 `source_summary`，並具備 `encouragement` 或 `reflection_question` 其中之一；其他欄位用來讓 Seed 像教導而不是命令：

- `encouragement`：鼓勵下次更好的行為，active card 至少需與 `reflection_question` 擇一填寫。
- `reflection_question`：遇到類似情境時先問自己的問題，active card 至少需與 `encouragement` 擇一填寫。
- `try_when`：什麼情境值得參考。
- `do_not_force_when`：什麼情境不要硬套。
- `growth_signal`：怎樣代表 AI 有把此提醒用好。
- `culture_scope`：此卡保存的是哪個細部文化範圍。

若一張 card 只能寫成單向命令，通常表示它應該是專案 hard rule、測試 checklist、或仍需澄清的 candidate，而不是 Seed active card。

## Preference Layer
Preference Layer 用來學習操作者習性，像長期代理人或秘書逐步理解使用者傾向，但不需要每次都新增 agent md。

Preference 是「傾向」，不是 hard rule：
- 可在多個好結果中協助選擇更符合使用者的方案。
- 不可覆蓋安全、專案規範、品牌、法規、市場目標、可讀性、可維護性或當前使用者明確指令。
- 不可因使用者多次偏好某個選項，就在所有情境無條件套用。
- UI/UX、人性化、市場成熟度與操作感受可作為產品判斷取向，但只在多個方案都合理時協助取捨，不可覆蓋更高權威限制。

Preference card 必須具備：
- `confidence`：信心分數，建議 0.0 到 1.0。
- `use_when`：什麼情境下可採用此偏好。
- `do_not_use_when`：什麼情境下不應採用此偏好。
- `validation`：套用偏好前要檢查什麼。
- `override_by`：哪些更高權威可以覆蓋此偏好。
- `non_llm_native_reason`：為什麼這是操作者個人化習性，而不是 LLM 原生能力。

若偏好已經穩定變成一個明確職能，例如固定的 UI designer、reviewer、payment engineer 工作方式，才考慮升級成 agent md；否則留在 Preference Layer。

## Runtime 載入原則
- 每次最多載入一段短 kernel extension。
- 每次最多載入少量最相關 lesson cards。
- 每次最多載入少量 capability maps。
- 預設總量應維持微量 token；若命中太多，寧可少載入。
- Seed Memory 低於 P0 safety、P1 hard gate、必讀 context 與當前使用者明確指令。

## 建議資料夾
```text
seed_memory/
  manifest.json
  habit_kernel_extension.md
  lessons/
    candidates.jsonl
    active.jsonl
    archived.jsonl
  preferences/
    candidates.jsonl
    active.jsonl
    archived.jsonl
  distilled/
    README.md
  capability_maps/
    tools.jsonl
  evals/
    behavior_checks.md
```

`seed_memory/` 預設不納入 Git 版控；可自行壓縮或複製成 Seed Pack。

## Card 生命週期
- `draft`：尚未成為 runtime 依據。
- `active`：可被 resolver 選入 runtime。
- `distilled`：已被整合成更高階 kernel 或 distilled 文件，原 card 可保留但不再優先載入。
- `deprecated`：保留歷史脈絡，不再驅動新任務。
- `archived`：不參與選擇。

## 反覆驗證與汰換
- Seed Memory 不把當下最佳做法視為永久真理。
- Active card 應盡量具備：
  - `review_after`：到期後需重新檢查是否仍是好做法。
  - `replace_when`：出現什麼條件時應被取代。
  - `evidence`：為什麼此 lesson 目前值得採用。
- 到期 card 不會自動失效；它會進入 review queue，等待比較是否有更好的做法。
- 若新工具、新模組版本或新專案規則能更好地服務同一個核心習慣，應新增 replacement，並把舊 card 標為 `deprecated` 或 `distilled`。
- 若一張 card 只是在重述 LLM 原生能力，review 時應移除或封存。

## 成長流程
1. 從使用者指正、要求、偏好、教導或成功模式辨識可學習內容。
2. 先標註 `source_type`；若只是 AI 自行推論，預設只能進 candidate。
3. 排除 LLM 原生能力、單次資訊與敏感脈絡。
4. 抽象成鼓勵式 lesson / habit / preference / capability mapping，補上 `reflection_question` 或 `encouragement`。
5. 若是操作者傾向，寫入 `seed_memory/preferences/candidates.jsonl` 或 `preferences/active.jsonl`。
6. 若是 lesson / habit / capability mapping，寫入 `seed_memory/lessons/candidates.jsonl`、`lessons/active.jsonl` 或 `capability_maps/tools.jsonl`。
7. 定期把多張同主題 cards 蒸餾成更短的 kernel extension 或 preference profile。
8. 舊 card 被取代時，標成 `distilled`、`deprecated` 或移到 `archived.jsonl`。

## 自動學習檢查
- 任務收尾時，AI 應主動檢查是否有可重用 seed lesson，而不是等待使用者說「記起來」。
- 觸發來源包含：
  - 使用者指正 AI。
  - AI 發現自己重複犯同類錯誤。
  - 任務出現可重用成功模式。
  - 新工具 / 新模組能力能映射到既有好習慣。
- 若 lesson 涉及安全、專案 hard rule 或跨專案規則，應升級到對應治理 md，不只留在 Seed Memory。
- 若 lesson 仍不確定、可能只對單次任務成立、或含敏感脈絡，只能寫入 candidate 或 progress，不可直接 active。
- 若 lesson 只是 AI 從任務結果做出的事後總結，且沒有使用者教導或專案文化來源，只能寫入 candidate。
- 若使用者正在教育 AI 如何看待事情，例如「不要硬性限制發展」、「多從 UI/UX 與人性化市場角度判斷」，優先轉成 preference / judgment card，並保留彈性邊界。
- 自動學習不是自動相信；Seed 必須先做 LLM 原生能力排除、敏感資訊排除、scope 判斷與可驗證性判斷。

## 與規則演化的關係
- `rule_evolution_governance.md` 管理長期規則的優先權、取代、衝突與防循環。
- Seed Memory 管理可攜的學習蒸餾；它可引用規則演化，但不能替代長期治理 md。
- 若某張 card 已經變成跨專案 hard rule，應提升到 `agent_governance/agent_docs/`，並從 Seed Memory 降級或封存。

## 驗證要求
涉及 Seed Memory runtime 的變更，至少檢查：

```bash
python3 scripts/resolve_seed_memory.py --repo-root . --prompt "<prompt>" --output /tmp/seed.txt --notes-output /tmp/seed_notes.txt
python3 scripts/review_seed_memory.py --memory-dir seed_memory
```

涉及 required context 樹時，仍需執行：

```bash
python3 scripts/validate_context_registry.py
```
