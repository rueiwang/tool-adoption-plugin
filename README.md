# tool-adoption-plugin

AI 工具引入輔助套件。引導安裝外部工具、記錄問題、管理專案客製化，保留開發者對關鍵決策的控制權。

## 安裝

```bash
claude plugin add github:<username>/tool-adoption-plugin
```

## Skills

| Skill | 指令 | 說明 |
|-------|------|------|
| install-tool | `/install-tool <path>` | 引導安裝工具，遇問題自動記錄 |
| log-install-issue | `/log-install-issue <描述>` | 記錄安裝問題 |
| log-usage-issue | `/log-usage-issue <描述>` | 記錄使用問題，含根因分類與 blocking 判斷 |
| add-customization | `/add-customization <ISSUE-ID>` | 設計客製化方案，含確認檢查點 |
| review-issues | `/review-issues` | 產生問題統計報告 |

## 工作流程

```
/install-tool <tool>
  ├─ 非 blocking 問題 → 記錄後繼續
  └─ blocking 問題 → 記錄後暫停

/log-usage-issue <描述>
  ├─ 非 blocking → 記錄後繼續
  └─ blocking → 記錄 → 詢問是否客製化
                  ├─ yes → /add-customization → 確認檢查點 → 執行
                  └─ no  → 標記待人工決策

/review-issues → 問題統計 + 後續建議
```

## 問題日誌

執行時自動在專案目錄下建立 `tool-use-logs/tool-adoption-log.md`。

## 客製化原則

- 絕不修改共用工具原始檔案
- 客製化內容寫入專案的 `.claude/skills/` 覆蓋同名 skill
- 所有客製化操作需經開發者確認
