# 專案路徑與特性（通用模板）

本檔為跨機通用模板，不存放機器專屬絕對路徑。

## 套用規則
- 本檔即為路徑映射主檔。
- 執行時由 `WORKSPACE_ROOT` 展開 `<WORKSPACE_ROOT>`。
- 若未設定 `WORKSPACE_ROOT`，回退使用 `AI_ALLOWED_ROOT`。

## 路徑對應規則（模板）

### Web 專案
- `api` → `<WORKSPACE_ROOT>/web/s8_api`
- `agent` → `<WORKSPACE_ROOT>/web/s8_agent`
- `user` → `<WORKSPACE_ROOT>/web/s8_user`
- `midway` → `<WORKSPACE_ROOT>/web/s8_midway`

### Caster 系統專案
- `caster-web` → `<WORKSPACE_ROOT>/web/caster-web`
- `agent (Caster 上下文)` → `<WORKSPACE_ROOT>/web/s8_agent`

### 其他專案
- `payment_assistant` → `<WORKSPACE_ROOT>/web/payment_assistant`
- `data_center` → `<WORKSPACE_ROOT>/web/data_center`

## 專案特性快速指引

### s8_api（Laravel 5.3）
- 架構：Repository pattern + Platform abstraction
- 核心：多平台遊戲 API 統一介面
- 目錄：`app/Platforms/`、`app/Avatars/`、`app/Repositories/`
- 測試：`vendor/bin/phpunit`

### s8_agent（Laravel 5.3）
- 功能：管理後台界面
- 前端：AdminLTE + Vue.js 2.x
- 建置：`npm run dev`、`npm run prod`

### caster-web
- 定位：Caster 系統前端
- 關聯：與 `s8_agent` 共用 Caster 上下文
- 適用代理：`caster-engineer`
