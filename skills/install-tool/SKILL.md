---
name: install-tool
description: 引導開發者將外部 AI 工具（含大量 skill）引入專案。讀取工具 README/安裝指引，逐步執行安裝，遇到問題自動記錄後根據 blocking 程度決定繼續或暫停。使用時機：開發者說「安裝工具」、「引入工具」、「setup tool」。
argument-hint: <tool-name-or-path>
disable-model-invocation: true
allowed-tools: Read, Bash, Glob, Grep, Write, Skill(log-install-issue)
---

# install-tool

安裝目標工具：$ARGUMENTS

## 步驟 1：讀取安裝指引

1. 在 `$ARGUMENTS` 指定的路徑或目錄中尋找 `README.md`、`INSTALL.md`、`docs/setup.md`（按優先序）
2. 若找不到，讀取工具根目錄下所有 Markdown 檔案，提取安裝相關段落
3. 將安裝步驟整理成清單，向開發者確認即將執行的動作

## 步驟 2：確認目前環境

執行環境前置檢查：
- 確認相依執行環境版本符合要求（Node.js / Python / 其他）
- 記錄初始環境狀態備用

## 步驟 3：逐步執行安裝

依安裝指引逐步執行，每一步完成後確認狀態。

**遇到問題時的處理規則：**

- **非 blocking 問題**（警告訊息、非必要相依、選用功能失敗）：
  - 呼叫 `/log-install-issue <問題描述> blocking:false`
  - 繼續執行下一步，不中斷

- **blocking 問題**（必要指令失敗、必要檔案缺失、權限錯誤）：
  - 呼叫 `/log-install-issue <問題描述> blocking:true`
  - 停止安裝，向開發者報告，等待指示

## 步驟 4：安裝完成報告

安裝成功完成後輸出：

```
## 安裝完成報告

工具名稱：<name>
安裝路徑：<path>
安裝時間：<timestamp>

### 已安裝的 Skills
<列出偵測到的 skill 清單>

### 記錄的問題
<引用 tool-use-logs/tool-adoption-log.md 中本次記錄的問題數量與摘要>

### 下一步建議
- 使用 /log-usage-issue 記錄使用過程中的問題
- 使用 /review-issues 定期整理問題紀錄
```

## 注意事項

- 安裝過程中**不修改**工具本身的任何檔案
- 所有客製化必須在專案的 `.claude/skills/` 下以覆蓋方式進行
- 若 `tool-use-logs/tool-adoption-log.md` 不存在，在第一次記錄問題前建立它
