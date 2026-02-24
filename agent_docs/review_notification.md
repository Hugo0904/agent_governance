# PR Review Slack 通知規則

以下規則供 `/code_review` 的「通知 Slack」流程使用，避免把完整審查長文貼到業務頻道。

## 可調參數

[REVIEW_NOTIFICATION_DELEGATOR]
Ravi
[/REVIEW_NOTIFICATION_DELEGATOR]

[REVIEW_NOTIFICATION_SUMMARY_MAX_CHARS]
220
[/REVIEW_NOTIFICATION_SUMMARY_MAX_CHARS]

## 目標頻道通知模板（固定短版）
可用變數：
- `{mention}`
- `{repo_slug}`
- `{pr_number}`
- `{pr_title}`
- `{pr_url}`
- `{comment_url}`
- `{summary}`

[REVIEW_NOTIFICATION_TARGET_TEMPLATE]
{mention}
PR checked
• Ann 完成：{repo_slug} PR #{pr_number} Review Notification
PR 連結：{pr_url}

### 📋 摘要
{summary}
[/REVIEW_NOTIFICATION_TARGET_TEMPLATE]

## 原則
- 目標頻道只發短版通知，不貼完整審查細節。
- 詳細內容以 GitHub PR comment / review thread 為主。
- 流程通知（委派 / 收到 / 完成）仍走 workflow 頻道。
