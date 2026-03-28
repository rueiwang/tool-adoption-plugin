---
name: log-install-issue
description: 記錄安裝階段發生的問題到 tool-use-logs/tool-adoption-log.md。供 install-tool 呼叫，也可手動觸發。記錄後不中斷安裝流程。
argument-hint: "<問題描述> [blocking:true|false]"
disable-model-invocation: true
allowed-tools: Read, Write, Bash(date *)
---

# log-install-issue

記錄安裝問題：$ARGUMENTS

## 步驟 1：解析輸入

從 `$ARGUMENTS` 提取：
- **問題描述**：主要敘述文字
- **是否 blocking**：若包含 `blocking:true` 則為是，預設為否

若輸入資訊不足以完成分類，向開發者詢問：
1. 這個問題有沒有阻止安裝繼續進行？

## 步驟 2：根因分類

根據問題描述判斷根因，選擇最符合的一項：

| 根因分類 | 判斷依據 |
|---------|---------|
| 使用方式錯誤 | 指令格式錯誤、缺少必要參數、版本不符預期用法 |
| 工具限制 | 工具不支援此環境/功能、文件說明的已知限制 |
| 工具 bug | 行為與文件描述不符、在預期輸入下產生錯誤輸出 |

若無法確定，標記為「工具限制」並在描述中說明不確定性。

## 步驟 3：產生問題 ID

讀取 `tool-use-logs/tool-adoption-log.md`：
- 若檔案不存在：建立 `tool-use-logs/` 目錄與檔案，加入標頭，從 ISSUE-001 開始
- 若檔案存在：找出最大的 ISSUE-NNN 編號，遞增一

## 步驟 4：寫入日誌

將以下格式附加到 `tool-use-logs/tool-adoption-log.md`：

```markdown
### ISSUE-NNN

- **問題描述**：<描述>
- **發生時機**：安裝階段
- **是否 blocking**：是 | 否
- **根因分類**：<分類>
- **處理方式**：未處理
- **記錄時間**：<YYYY-MM-DD HH:MM>
- **狀態**：開放中
```

## 步驟 5：回應摘要

輸出簡短確認：
- 已記錄為 ISSUE-NNN
- 是否 blocking
- 若非 blocking：「繼續安裝」
- 若 blocking：「安裝已暫停，請查看 tool-use-logs/tool-adoption-log.md」

## 注意事項

- 此 skill 只做記錄，不採取任何修復行動
- 寫入日誌後立即回傳控制權
