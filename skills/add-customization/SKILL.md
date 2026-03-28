---
name: add-customization
description: 針對已記錄的 blocking 問題，設計並新增專案層客製化方案。包含確認檢查點，開發者確認後才執行，拒絕則標記待人工決策。絕不修改共用工具本身。
argument-hint: <ISSUE-ID>
disable-model-invocation: true
allowed-tools: Read, Write, Glob, Grep, Bash(date *), Bash(mkdir *)
---

# add-customization

處理問題：$ARGUMENTS

## 步驟 1：讀取問題詳情

從 `tool-use-logs/tool-adoption-log.md` 讀取 `$ARGUMENTS`（例如 ISSUE-003）的完整記錄，確認：
- 問題描述
- 根因分類
- 目前處理方式

若找不到指定的 ISSUE-ID，停止並告知開發者。

## 步驟 2：分析客製化方案

根據根因分類，分析對應策略：

**根因：工具限制**
- 策略 A：在 `.claude/skills/<skill-name>/SKILL.md` 覆蓋同名 skill，加入處理邏輯
- 策略 B：新增補充 skill，在工具 skill 之外提供額外能力
- 策略 C：修改 CLAUDE.md 加入情境說明，讓 Claude 遇到此限制時知道如何繞過

**根因：工具 bug**
- 策略 A：覆蓋有 bug 的 skill，加入 workaround 邏輯
- 策略 B：新增前置/後置驗證 skill

**根因：使用方式錯誤**
- 策略 A：在 `.claude/skills/` 新增包裝 skill，自動修正常見錯誤用法
- 策略 B：更新 CLAUDE.md 加入正確用法說明

對每個可行策略說明：方案描述、影響範圍、優缺點。

## 步驟 3：【確認檢查點】

**必須等待開發者確認後才能繼續。**

向開發者呈現：

```
## 客製化方案建議（ISSUE-NNN）

問題：<問題描述>
根因：<根因分類>

### 建議方案

**方案 1**：<策略描述>
- 將新增：`.claude/skills/<name>/SKILL.md`
- 效果：<預期效果>
- 限制：<已知限制>

**方案 2**（若有）：<策略描述>
...

請選擇要採用的方案（輸入方案編號），或輸入 `no` 拒絕所有方案。
```

等待開發者回應：
- 選擇某方案 → 前往步驟 4
- 回答 `no` → 前往步驟 5

## 步驟 4：執行客製化（確認後）

1. 確認目標目錄存在（`.claude/skills/<name>/`），不存在則建立
2. 若覆蓋現有 skill：
   - 先讀取原始工具的 SKILL.md 內容
   - 在新檔案中保留原始邏輯，以 `<!-- 原始工具邏輯 -->` 和 `<!-- 客製化差異 -->` 區隔
3. 寫入新的 SKILL.md
4. 確認檔案寫入成功

更新 `tool-use-logs/tool-adoption-log.md` 中該 ISSUE 的記錄：
- 處理方式：客製化路徑（`.claude/skills/<name>/SKILL.md`）
- 狀態：已解決

輸出完成報告：
```
## 客製化完成（ISSUE-NNN）

已新增：`.claude/skills/<name>/SKILL.md`
處理方式：<採用的策略>
ISSUE 狀態已更新為：已解決

注意：此客製化覆蓋了共用工具的同名 skill，僅影響本專案。
```

## 步驟 5：拒絕流程

1. 更新 `tool-use-logs/tool-adoption-log.md` 中該 ISSUE：
   - 處理方式：待人工決策
   - 狀態：待人工決策
2. 輸出：「已標記 ISSUE-NNN 為待人工決策。自動化流程停止。」
3. 不做任何其他動作。

## 客製化原則

- **絕不修改**工具原始目錄下的任何檔案
- 所有新增內容在 `.claude/skills/`、`.claude/` 或專案根目錄的 CLAUDE.md 中
- 每次客製化都要更新對應的 ISSUE 狀態
