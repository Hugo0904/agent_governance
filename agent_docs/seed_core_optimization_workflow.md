# Seed Core Optimization Workflow

本檔定義當使用者說「幫我優化 Seed 模型」、「重新規劃 Seed 核心」、「調整 Seed Core」或類似需求時，AI 應如何使用當下可用的 LLM 能力來分析與規劃。

此流程的重點是：**先討論方向，不直接修改核心**。

## 觸發語意

符合以下任一語意時，應載入本檔：

- 使用者要求優化、重新規劃、重整、調整 Seed 模型。
- 使用者要求用目前模型能力檢查 Seed Core 是否更好。
- 使用者提出新的 Seed 理念，希望 AI 轉成可執行機制。
- 使用者要求「幫我重新規劃和調整目前的 seed 核心」。
- 使用者問目前 Seed 架構是否矛盾、模糊、可改得更好。

## 必讀上下文

進入本流程時，至少要參考：

- `agent_governance/agent_docs/seed_core_design.md`
- `agent_governance/agent_docs/seed_memory_governance.md`
- `config/seed_memory_policy.json`
- `scripts/sync_seed_core.py`
- 目前 Seed resolver 輸出的 runtime kernel 與 relevant cards

若需求涉及 context routing，也要參考：

- `config/context_registry.json`
- `config/required_context_paths.txt`

## 不可直接修改

除非使用者明確確認，AI 不得直接修改：

- `seed_core_design.md`
- `seed_memory_governance.md`
- `seed_memory_policy.json`
- `habit_kernel_extension.md`
- `resolve_seed_memory.py`
- `sync_seed_core.py`
- active seed cards

「幫我優化 Seed 模型」預設代表進入討論與規劃，不代表立刻改檔。

## 分析步驟

1. 先用當前使用者原文跑 Seed resolver，讀取 output 與 notes。
2. 讀取 `seed_core_design.md`，確認目前 Seed 的設計目標、runtime kernel 與 policy patch。
3. 判斷使用者的新想法屬於哪一層：
   - `core_design`：Seed 的根本理念、成長方向、保護性判斷空間。
   - `runtime_kernel`：每次 resolver 應短量載入的核心提醒。
   - `retrieval_policy`：召回、弱訊號、否定語境、卡片選擇門檻。
   - `intake_policy`：何時建立 candidate / active card、來源與邊界要求。
   - `card_schema`：card 欄位、成長提示、適用邊界、review 條件。
   - `workflow`：未來如何讓 AI 與使用者討論、確認、同步與驗證。
4. 找出目前設計可能的矛盾、模糊點、過度抽象或 token 成本風險。
5. 提出可比較的優化方向，而不是只給單一結論。

## 回覆使用者時必須包含

在取得修改確認前，回覆應包含：

- 我目前理解的設計目標。
- 這次優化應該落在哪一層。
- 建議調整方向。
- 可能影響的檔案。
- 風險與取捨。
- 明確說明：尚未修改，等待使用者確認。

若使用者已明確說「先不做」、「先討論」、「不要改」，只能分析與提案。

## 取得確認後的修改順序

若使用者明確確認可以修改，才依序執行：

1. 優先修改 `agent_governance/agent_docs/seed_core_design.md`。
2. 若是可同步內容，執行：

```bash
python3 scripts/sync_seed_core.py
```

3. 若需要調整召回或 intake 機制，再修改對應 script / policy。
4. 跑驗證：

```bash
python3 scripts/sync_seed_core.py --dry-run
python3 scripts/review_seed_memory.py --memory-dir seed_memory
python3 scripts/review_seed_memory.py --memory-dir templates/seed_memory
python3 scripts/validate_context_registry.py
```

5. 用至少三種 prompt 模擬：
   - 核心理念 / 優化 Seed 模型。
   - 具體外圍 card 情境。
   - 否定或泛詞情境，確認不誤召回。

## 討論格式建議

優先用以下格式與使用者討論：

```text
我理解這次目標是：
...

我會把它放在：
core_design / runtime_kernel / retrieval_policy / intake_policy / card_schema / workflow

建議方向：
1. ...
2. ...
3. ...

可能影響：
- ...

我目前不會修改檔案；等你確認方向後再動。
```

## 設計底線

- 不把新的理念直接塞成 active card。
- 不讓 Seed Core 因每次對話而無限制膨脹。
- 不因 AI 自行推論就改核心。
- 不把使用者一時說法當成永久規則。
- 不把「討論優化」誤解成「立刻修改」。
