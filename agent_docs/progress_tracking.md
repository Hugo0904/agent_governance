# 任務日誌機制 (Task Log Tracking)

本文件只定義「任務模式已啟用後」的進度筆記規格，不定義載入時機。
`<TASK_LOG_ROOT>` 由宿主綁定；本治理來源不假設宿主名稱或安裝路徑。

## 1. 建立與更新（強制）
- 任務啟動時由系統建立 `<TASK_LOG_ROOT>/progress/task_YYYYMMDD_<TASK_ID>.md`。
- 每完成一個實質步驟就更新該檔，保持可接手的最新狀態。
- 任務檔必須由系統/Agent 自動維護，不可省略。

## 2. 任務檔格式（強制）
- 必須包含 `任務摘要` 區塊，格式：`任務：此次任務的摘要`。
- 必須使用核取方塊記錄進度：`[x]` 已完成，`[ ]` 未完成。
- 檔案需可讓其他 Agent 或人員直接接手。
- 若執行器可取得本回合 usage，應在該回合條目加入 `- Token 使用量: ...`；若無法取得則可省略。

建議最小模板：
```md
任務：此次任務的摘要

**任務**
- [x] 已完成步驟
- [ ] 下一步
```

## 3. recent_tasks 摘要索引（強制）
- 必須維護：`<TASK_LOG_ROOT>/recent_tasks.md`。
- 摘要預設顯示最後編輯的 50 個任務；這只是摘要上限，不是磁碟保留量。
- 宿主可調整摘要筆數，預設 `50`。
- 每筆格式遵循系統索引輸出：`task_id | updated_at | status | title | file`。
- 每次新增或更新 `<TASK_LOG_ROOT>/progress/task_*.md` 後，必須同步更新 `recent_tasks.md`。

## 4. 任務回顧順序（強制）
- 需要續接舊任務時，先讀 `<TASK_LOG_ROOT>/recent_tasks.md`，再讀
  `<TASK_LOG_ROOT>/progress/` 內的目標任務檔。

## 5. 檔名規範（強制）
- 任務檔名由系統管理，預設為 `task_YYYYMMDD_<TASK_ID>.md`。

## 6. 清理規範（強制）
- 系統必須真正清理 `<TASK_LOG_ROOT>/progress/` 內的任務檔與
  `task_index.json`，不可只縮短摘要。
- 保留最近 60 天內的全部任務；若不足 50 筆，仍保留最後更新的 50 筆。
- 等價規則是保留「60 天時間窗」與「最新 50 筆」的聯集。
- 宿主可調整時間窗與最低保留量，預設分別為 `60` 天與 `50` 筆。
- `<TASK_LOG_ROOT>/recent_tasks.md`、runtime JSON 與非任務檔不列入任務清理目標。
