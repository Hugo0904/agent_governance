# 專案路徑與特性

## 路徑對應規則

### Web 專案
- `api` → `/Users/shawn/Code/web/s8_api`（Laravel 5.3 API）
- `agent` → `/Users/shawn/Code/web/s8_agent`（Laravel 5.3 管理後台）
- `user` → `/Users/shawn/Code/web/s8_user`（用戶端）
- `midway` → `/Users/shawn/Code/web/s8_midway`（中間層）

### Caster 系統專案
- `caster-web` → `/Users/shawn/Code/web/caster-web`（Caster 系統前端）
- `agent (Caster 上下文)` → `/Users/shawn/Code/web/s8_agent`（Caster 管理後台）

### 其他專案
- `payment_assistant` → `/Users/shawn/Code/web/payment_assistant`
- `data_center` → `/Users/shawn/Code/web/data_center`

## 專案特性快速指引

### s8_api（Laravel 5.3）
- 架構：Repository pattern + Platform abstraction
- 核心：多平台遊戲 API 統一介面（77+ 平台）
- 目錄：`app/Platforms/`、`app/Avatars/`、`app/Repositories/`
- 測試：`vendor/bin/phpunit`

### s8_agent（Laravel 5.3）
- 功能：管理後台界面
- 前端：AdminLTE + Vue.js 2.x
- 建置：`npm run dev`、`npm run prod`
- 架構：多租戶賭博平台管理系統

### caster-web
- 定位：Caster 系統前端
- 關聯：與 `s8_agent` 共用 Caster 上下文
- 適用代理：`caster-engineer`
