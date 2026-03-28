---
name: review-issues
description: 整理 tool-use-logs/tool-adoption-log.md 中的問題紀錄，產生摘要報告。可識別高頻問題、待處理項目、客製化覆蓋率。使用時機：開發者說「整理問題」、「看看有哪些問題還沒處理」、「review issues」。
argument-hint: "[--status <開放中|待人工決策|已解決|all>] [--blocking-only]"
disable-model-invocation: false
allowed-tools: Read, Write, Grep
---

# review-issues

整理問題紀錄：$ARGUMENTS

## 步驟 1：讀取並解析日誌

讀取 `tool-use-logs/tool-adoption-log.md` 完整內容。

若檔案不存在，輸出「尚無問題紀錄」並停止。

解析所有 `### ISSUE-NNN` 段落，提取每筆記錄的所有欄位。

## 步驟 2：套用篩選條件

依 `$ARGUMENTS` 中的參數過濾：
- `--status <值>`：只顯示指定狀態（預設：顯示全部）
- `--blocking-only`：只顯示 blocking 問題

若無參數，顯示完整摘要。

## 步驟 3：產生分析報告

```
## 問題紀錄摘要報告

產生時間：<timestamp>
日誌路徑：tool-use-logs/tool-adoption-log.md

### 統計總覽

| 狀態 | 數量 |
|------|------|
| 開放中 | N |
| 待人工決策 | N |
| 已解決 | N |
| 合計 | N |

Blocking 問題：N 件 / 非 Blocking：N 件

### 根因分佈

| 根因分類 | 數量 | 佔比 |
|---------|------|------|
| 使用方式錯誤 | N | N% |
| 工具限制 | N | N% |
| 工具 bug | N | N% |

### 需要關注的項目

#### 待人工決策（需立即處理）
- ISSUE-NNN：<問題描述簡述>

#### 開放中的 Blocking 問題
- ISSUE-NNN：<問題描述簡述>

### 已解決問題摘要
共 N 件，客製化路徑：
- .claude/skills/<name>/SKILL.md（對應 ISSUE-NNN）
```

## 步驟 4：後續建議

根據報告內容，在結尾加入建議：

- 若有「待人工決策」：「建議優先處理以下問題，可使用 `/add-customization ISSUE-NNN` 重新啟動客製化流程。」
- 若有多個相同根因的問題：「發現 N 個相同根因（工具限制）的問題，建議一次性設計覆蓋方案。」
- 若所有問題均已解決：「所有已記錄問題均已處理，工具引入狀態良好。」

## 注意事項

- 此 skill 為唯讀報告，不修改日誌內容
- 若需重新開啟某個「待人工決策」的問題，請使用 `/add-customization <ISSUE-ID>`
