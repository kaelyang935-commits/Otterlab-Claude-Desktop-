# ⚡ Claude Desktop 斜線指令速查表（CHEATSHEET）

> 一頁掌握常用指令。建議印出放手邊。完整說明見 [課程首頁](README.md)。

---

## 🟢 每天都用（必背）

| 指令 | 用途 |
|---|---|
| `/help` | 顯示說明與所有指令 |
| `/clear` | 開新對話、清空上下文 |
| `/compact` | 摘要壓縮對話以省空間 |
| `/context` | 看上下文用量 |
| `/resume` | 恢復先前對話 |
| `/rewind` | 倒回對話／程式碼到先前某點 |
| `/model` | 切換模型 |
| `/usage` | 查花費與用量 |
| `/diff` | 看未提交的程式碼變更 |
| `/copy` | 複製上一則回覆 |

---

## 🔵 對話與階段

`/clear` 開新 ｜ `/resume` 恢復 ｜ `/branch` 分支 ｜ `/fork` 派背景代理 ｜ `/background` 轉背景 ｜ `/rename` 改名 ｜ `/exit` 離開

## 🧠 上下文與記憶

`/context` 用量 ｜ `/compact` 壓縮 ｜ `/memory` 編輯 CLAUDE.md ｜ `/rewind` 倒回 ｜ `/export` 匯出

## 🔍 程式碼品質

`/code-review` 審查本機 diff ｜ `/simplify` 只整理不抓 bug ｜ `/security-review` 找漏洞 ｜ `/review` 審查 GitHub PR

## ⚙️ 模型

`/model` 換模型 ｜ `/effort` 思考深度 ｜ `/fast` 快速模式 ｜ `/advisor` 第二模型顧問

## 🗂️ 專案

`/init` 建立 CLAUDE.md ｜ `/add-dir` 加目錄 ｜ `/cd` 切目錄

## 🏃 建置執行

`/run` 跑 App ｜ `/verify` 驗證變更 ｜ `/tasks` 看背景工作

## 🛠️ 設定面板（互動）

`/config` 設定 ｜ `/permissions` 權限 ｜ `/agents` 子代理 ｜ `/mcp` MCP ｜ `/hooks` 鉤子 ｜ `/skills` 技能 ｜ `/plugin` 外掛

## ℹ️ 資訊

`/status` 狀態 ｜ `/doctor` 診斷 ｜ `/feedback` 回報 ｜ `/release-notes` 更新說明

## ☁️ 雲端／遠端

`/schedule` 排程 ｜ `/autofix-pr` 自動修 PR ｜ `/ultraplan` 雲端計畫 ｜ `/desktop` 接續到桌面 App ｜ `/teleport` 拉回本機 ｜ `/remote-control` 開放遠端

## ✨ 小工具

`/btw` 問不入歷史的小問題 ｜ `/recap` 一句話摘要 ｜ `/powerup` 互動教學 ｜ `/insights` 使用分析

---

## 🧩 最容易搞混（重點！）

| 別搞混 | 差別 |
|---|---|
| `/clear` ／ `/compact` ／ `/rewind` | 清空 ／ 壓縮 ／ 倒回 |
| `/review` ／ `/code-review` | GitHub PR ／ 本機 diff |
| `/model` ／ `/effort` | 換模型 ／ 思考多深 |
| `/branch` ／ `/fork` | 自己換方向 ／ 派代理做 |

---

## 🟣 自訂指令速記

- 檔案：`.claude/commands/名稱.md` 或 `.claude/skills/名稱/SKILL.md`
- 參數：`$ARGUMENTS`（全部）、`$0` `$1`（第 1、2 個）
- 注入：`` !`git status` `` 先跑 shell 再把輸出帶入
- 設定：`description`、`allowed-tools`、`disable-model-invocation: true`

---

## 🧩 Anthropic 官方 Skills

- 官方 repo：**github.com/anthropics/skills**（文件技能 pdf/docx/pptx/xlsx 為專有授權）
- 結構：資料夾 + `SKILL.md`（`name` ≤64 字、`description` ≤1024 字，description 決定觸發）
- 漸進式揭露三層：中繼資料（啟動）→ `SKILL.md` 本文（觸發時）→ 附帶資源／腳本（需要時）
- 安裝：Claude Code `/plugin marketplace add anthropics/skills` → `/plugin`；App 上傳 `.zip`
- 更多來源：Top 10 來源榜見第 9 課（🛒 市集可一鍵 `add`，📒 清單需自行取用）
- ⚠️ 官方內建技能**別自行替換/修改**（會隨 Claude Desktop 更新並配合模型/harness/loop）；要擴充用外掛或另開新技能
- ⚠️ 第三方技能能執行程式碼，安裝前先審查來源與腳本

---

## 四個運作規則

1. 指令**放開頭** 2. 後面接**參數** 3. 打 `/` 叫**選單** 4. 部分是**互動面板**

---

> 🦦 Otterlab × Claude Desktop 教學　｜　作者：kaelyang935@gmail.com　｜　查證日期：2026-06-25
