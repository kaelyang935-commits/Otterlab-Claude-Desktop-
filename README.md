# 🦦 Otterlab × Claude Desktop 斜線指令（/）實戰教學

> 一套給講師上課、給學生自學的 **Claude Desktop 斜線指令完整教材**。
> 從「打一個 `/` 開始」到「自己寫一個指令」，循序漸進、附範例與練習。

![Level](https://img.shields.io/badge/level-入門到進階-blue)
![Lang](https://img.shields.io/badge/語言-繁體中文-green)
![Topic](https://img.shields.io/badge/主題-Claude%20Desktop%20%2F%20Claude%20Code-purple)

---

## 📌 這是什麼？

這份教材教你 **Claude Desktop（桌面版 App）裡的斜線指令（slash commands）**。

一個你必須先知道的關鍵觀念：

> **「Claude Desktop 的 `/` 指令」本質上就是 Claude Code 的指令系統。**
> Claude 桌面版內建了 Claude Code 引擎（即 Cowork 體驗），所以你在桌面版輸入框打 `/` 看到的指令，跟在終端機執行 `claude` 看到的幾乎完全相同。差異只在少數「互動式面板」的呈現方式與網頁版的限制——這點在 [第 5 課](docs/05-environments.md) 會專門講。

---

## 🎯 適合對象

| 對象 | 你會得到 |
|---|---|
| 🧑‍🎓 **完全新手** | 看懂每個指令在做什麼、最常用的 10 個指令先學會 |
| 👩‍💻 **開發者 / 工程師** | 程式碼審查、背景代理、自訂指令等進階工作流 |
| 🧑‍🏫 **講師 / 教育訓練** | 可直接拿來開課的講義、教學順序、課堂練習與常見誤區 |

---

## 🗂️ 課程大綱

| # | 課程 | 內容重點 | 建議時數 |
|---|---|---|---|
| 1 | [基礎概念與運作規則](docs/01-fundamentals.md) | 什麼是斜線指令、4 個運作規則、3 大類指令 | 30 分 |
| 2 | [內建指令大全](docs/02-builtin-commands.md) | 15 大分類、逐一說明每個內建指令 | 90 分 |
| 3 | [技能指令（Skills）](docs/03-skills.md) | `/code-review`、`/deep-research`、`/loop` 等工作流 | 45 分 |
| 4 | [自訂你自己的指令](docs/04-custom-commands.md) | 檔案格式、frontmatter、參數、動態注入、動手做 | 60 分 |
| 5 | [執行環境差異](docs/05-environments.md) | 桌面 App / 終端機 / 網頁的差別與互通 | 30 分 |
| 6 | [講師教學手冊](docs/06-instructor-guide.md) | 授課順序、常見誤區、評量、教學腳本 | 講師專用 |
| 7 | [Anthropic 官方 Skills（進階專題）](docs/07-anthropic-skills.md) | 官方 GitHub、漸進式揭露、`SKILL.md`、四環境用法、安全 | 60 分 |
| 8 | [找到適合你任務的 GitHub Skill .md](docs/08-find-skill-md.md) | 心法（先官方、缺的找 `.md`）、怎麼挑、熱門來源榜、安全 | 30 分 |
| 9 | [動手做：給 agent 一個 URL 一鍵執行技能](docs/09-run-skill-from-url.md) | 零安裝、找 `.md`、Raw 連結、示範對話、安全 | 20 分 |
| ⚡ | [指令速查表 CHEATSHEET](CHEATSHEET.md) | 一頁掌握所有常用指令（可印出發給學生） | — |

---

## 🚀 快速開始：先學這 10 個指令

如果你只有 5 分鐘，先記住這些，今天就能上手：

| 指令 | 一句話 |
|---|---|
| `/help` | 顯示說明與所有可用指令 |
| `/clear` | 開一段全新的乾淨對話 |
| `/compact` | 對話太長時，摘要壓縮以省下空間 |
| `/context` | 看目前用掉多少上下文 |
| `/model` | 切換要用哪個模型 |
| `/init` | 為專案建立 `CLAUDE.md` 說明檔 |
| `/rewind` | 把對話／程式碼倒回先前某一點 |
| `/diff` | 檢視尚未提交的程式碼變更 |
| `/resume` | 恢復先前的對話 |
| `/usage` | 查看花費與用量 |

> 💡 在輸入框打一個 `/`，就會跳出指令選單；繼續打字會即時篩選。

---

## 🧭 學習路徑

```
入門 ──▶ 第1課 基礎概念 ──▶ 第2課 內建指令（先學「快速開始 10 個」）
                                   │
中階 ───────────────────────────▶ 第3課 技能指令 ──▶ 第5課 環境差異
                                   │
進階 ───────────────────────────▶ 第4課 自訂指令 ──▶ 第7課 官方 Skills(用官方、別替換)
                                   │
技能調用 ──────────────────────────▶ 第8課 找 Skill .md ──▶ 第9課 給 URL 一鍵執行(新手最簡單)
```

---

## 📖 如何使用本教材

**給學生：**
1. 從 [第 1 課](docs/01-fundamentals.md) 開始，照順序讀。
2. 每課結尾都有「課堂練習」，務必親手打一次指令。
3. 把 [速查表](CHEATSHEET.md) 印出來放在手邊。

**給講師：**
1. 直接看 [第 6 課 講師教學手冊](docs/06-instructor-guide.md)，裡面有授課順序、時間分配、常見誤區與評量題。
2. 每課的「課堂練習」可當作隨堂演練或回家作業。
3. 速查表可作為課後發放的講義。

---

## ✅ 版本與正確性

- 內容依據 Anthropic 官方 Claude Code 文件整理，查證日期：**2026-06-25**。
- 指令會隨版本更新；以你裝置上 `/help` 顯示的清單為最終依據。
- 官方文件：<https://code.claude.com/docs/en/commands>

---

## 👤 作者與授權

- **作者：** kaelyang935@gmail.com
- **單位：** Otterlab 🦦
- 歡迎用於教學與內部訓練。轉載請保留出處。

---

> 教材若有錯誤或想補充，歡迎開 Issue 或 Pull Request 一起讓它更好。
