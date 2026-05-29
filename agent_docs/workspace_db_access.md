# 工作區 DB 存取

本文件用於存放「此 AI 工作區如何查看本地資料庫」的規則，屬於工作區擁有者自己的操作方式，不屬於任何單一專案 repo。

## 目的
- 讓 AI 在需要做本地 / Docker DB 驗證時，知道該去工作區規則找操作方式，而不是把個人操作習慣寫進專案規範。
- 將「是否使用 Docker、如何連 DB、如何取得本機環境資訊」與「專案本身的業務真相」分離。

## 邊界
- 這裡放的是工作區層資訊：
  例如是否透過 Docker 啟動 DB、如何找到容器、如何取得連線設定、哪些命令已被允許。
- 這裡不放專案業務真相：
  例如哪張表是優惠參與 source of truth、哪個欄位代表退水狀態。
- 專案 repo 不應承擔你的個人本機操作方式，除非那是整個團隊共享且穩定的開發流程。
- 專案層文件不可向上定義「去讀 ai-agent-hub / workspace 規範」；只能由 workspace root / hub 這一層往下分流。專案文件只記專案自己的業務真相與工程規則。

## 使用原則
- 先看目標專案的 `config/database.php`、`.env` 與既有 query 結構，再決定怎麼查。
- 若此工作區是用 Docker 提供 DB，優先使用工作區已允許的只讀查詢方式。
- 帳號、密碼、token、容器特有細節，不寫進版控文件；必要時放本機覆寫。

## 本工作區 Docker 只讀查詢方式
- `s8_agent` 在 workspace container 內路徑為 `/var/www/s8_agent`。
- `s8_midway` 在 workspace container 內路徑為 `/var/www/s8_midway`。
- 優先透過 Laravel app bootstrap 執行 `DB::select()`，讓專案自行讀 `.env` / `config/database.php`，不要手動攤開 DB 帳密。
- 不要憑記憶使用 `php artisan tinker --execute`；Laravel 5.3 / 舊版 tinker 不一定支援該選項。若要用 artisan 指令，先用 `php artisan list` 確認存在。
- 查詢必須是只讀 SQL；使用 prepared bindings，不把使用者輸入直接串進 SQL。

### s8_agent 範本
```bash
docker exec -w /var/www/s8_agent laradock-workspace-1 php -r '
require "bootstrap/autoload.php";
$app = require "bootstrap/app.php";
$kernel = $app->make(Illuminate\Contracts\Console\Kernel::class);
$kernel->bootstrap();

$rows = DB::select(
    "select id, sn, status, created_at from user_withdraw where sn = ? limit 1",
    ["<sn>"]
);

echo json_encode($rows, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE) . PHP_EOL;
'
```

### s8_midway 範本
```bash
docker exec -w /var/www/s8_midway laradock-workspace-1 php -r '
require "bootstrap/autoload.php";
$app = require "bootstrap/app.php";
$kernel = $app->make(Illuminate\Contracts\Console\Kernel::class);
$kernel->bootstrap();

$rows = DB::select(
    "select id, flow_state, latest_error_code, latest_error_message from withdraw_order where user_withdraw_id = ? limit 5",
    ["<user_withdraw_id>"]
);

echo json_encode($rows, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE) . PHP_EOL;
'
```

### 資料導向問題的最小查詢模式
- 使用者給明確單號 / id / sn 時，先查主表一筆，再用主鍵查關聯表；不要直接掃 history 全表。
- 例如第三方取款應依序查：`user_withdraw.sn` -> `withdraw_order.user_withdraw_id` -> `withdraw_order_attempts.withdraw_order_id`。
- 回覆只摘要必要欄位，例如筆數、狀態、錯誤碼、錯誤訊息、時間；不要貼出完整 payload 或敏感欄位。
- 若需要看 payload 是否有錯誤資訊，只確認 key / 摘要 / masked 欄位，不回傳 secret、token、sign、bank account 全碼。

## 專案對應
- 若某個專案在此工作區需要特別的 DB 存取方式，可在本文件補「工作區自己的操作備忘」。
- 這些內容屬於 `ai-agent-hub` 或本機覆寫，不應直接寫進該專案的 `AGENTS.md`。
