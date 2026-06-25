# 第 7 課（進階專題）｜Anthropic 官方 Skills：GitHub、用法與教學

> 回到：[課程首頁](../README.md) ｜ 延伸自：[第 3 課 技能指令](03-skills.md)、[第 4 課 自訂指令](04-custom-commands.md)

> 本課是進階專題。學會 [第 3 課](03-skills.md)（技能是什麼）與 [第 4 課](04-custom-commands.md)（自己寫指令）後再讀，效果最好。

---

## 🎯 學習目標

1. 知道 **Anthropic 官方 Skills 的 GitHub repo** 在哪、收錄了什麼。
2. 理解 Skills 的核心設計：**漸進式揭露（Progressive Disclosure）**。
3. 看懂 `SKILL.md` 的結構。
4. 會在 **四種環境**（Claude App／Claude Code／API／Agent SDK）使用 Skills。
5. 知道安裝第三方 Skill 的**安全風險**。

---

## 1. 什麼是 Agent Skills？

> **Skill（技能）= 一個資料夾,裡面裝著「教 Claude 做某件事」的說明書、腳本與素材。**

一個 Skill 通常包含:
- **指示（instructions）**:做事的步驟與最佳實務
- **中繼資料（metadata）**:名稱與描述,讓 Claude 知道「何時該用它」
- **附帶資源（選用）**:腳本、範本、參考文件

這跟你在 [第 4 課](04-custom-commands.md) 做的自訂指令是**同一套機制**——官方 Skills 只是「做得很完整、可重用」的範例。

---

## 2. 官方 GitHub Repo

🔗 **https://github.com/anthropics/skills**

- **用途:** Anthropic 官方公開的 Agent Skills 範例與規格,是學習與參考的最佳來源。
- **狀態:** 公開、積極維護。
- **主要結構:**

```
anthropics/skills/
├── skills/                 # 各個技能範例(主要看這裡)
├── spec/                   # Agent Skills 規格說明
├── template/               # 建立新技能用的範本
├── .claude-plugin/         # 外掛中繼資料
├── README.md
└── THIRD_PARTY_NOTICES.md
```

> ⚠️ **授權注意(教學重點):**
> repo 內的**文件類技能**(`pdf`、`docx`、`pptx`、`xlsx`)是 **source-available 的「專有授權(Proprietary)」**——可以閱讀作為參考實作,但**不是開源**,不可任意自行散布。其他範例技能多採開源授權。實際以各技能資料夾內的 `LICENSE` 與 repo 的 `THIRD_PARTY_NOTICES.md` 為準。

---

## 3. Repo 收錄了哪些 Skills?

目前收錄十多個技能,依用途分類(節錄):

| 類別 | 技能 |
|---|---|
| 📄 文件處理（專有授權） | `pdf`、`docx`、`pptx`、`xlsx` |
| 🛠️ 開發 | `claude-api`、`mcp-builder`、`web-artifacts-builder`、`webapp-testing` |
| 🎨 設計創意 | `algorithmic-art`、`canvas-design`、`frontend-design`、`theme-factory`、`brand-guidelines`、`slack-gif-creator` |
| 🏢 企業溝通 | `internal-comms`、`doc-coauthoring` |
| 🧰 工具 | `skill-creator`（用來「建立技能的技能」） |

> 💡 這四個文件技能(pdf／docx／pptx／xlsx)其實就是 Claude 產生 Word／PPT／Excel／PDF 檔背後用的同一套東西。

---

## ⚠️ 重要原則:不要自行更換官方 Skills

> **官方內建的 Skills 不要自己去替換或修改。想擴充能力,請改用「外掛(plugin)」,或在自己的 skills 目錄「另外新增」具名技能。**

**為什麼?**

- 官方 Skills 會**隨 Claude Desktop / Claude Code 版本一起更新**,並與當前的 **模型能力、harness(執行框架)、loop(代理迴圈)工程** 整體搭配、共同調校。
- 自行替換或編輯官方技能會有三個問題:
  1. **破壞協調** —— 打亂「模型 × harness × loop × skill」的搭配,行為可能反而變差或出錯。
  2. **被覆蓋** —— 下次更新時,你的改動會被官方版本蓋掉而消失。
  3. **失去後續優化** —— 拿不到官方對該技能的持續修正與強化。

**✅ 正確做法(用「加法」,不要「替換」):**

| 你想做的 | 正確方式 |
|---|---|
| 增加新能力 | 用**外掛市集**安裝(見 [第 9 課](09-skill-sources-ranking.md)),或在 `~/.claude/skills/`、`.claude/skills/` 放**自己命名**的技能 |
| 客製某個流程 | 寫一個**新的、具名**技能(見 [第 4 課](04-custom-commands.md)),別用與官方同名而蓋掉它 |
| 不想要某個內建技能 | 用設定**停用**它,而不是去改它的檔案 |

> 🧠 一句話:**官方件用「原廠」的就好;要加東西,用外掛或自己另開新技能,別動原廠件。**

---

## 4. 核心觀念:漸進式揭露(Progressive Disclosure)

這是 Skills 最重要、也最常被考的設計。Claude **不會一次把整個技能讀進來**,而是**分三層、用到才載**:

| 層級 | 內容 | 何時載入 | Token 成本 |
|---|---|---|---|
| **L1 中繼資料** | `name` + `description` | 啟動時就載入 | 極小（每個技能約 100 tokens） |
| **L2 完整說明** | `SKILL.md` 本文 | 判斷「這次用得到」才載 | 通常 < 5,000 tokens |
| **L3 附帶資源** | 其他 `.md`、腳本、範本 | 需要某個檔／跑某支腳本時才載 | 幾乎無上限 |

> 🧠 **比喻:** 像一本書——你平常只看「書背標題」(L1);需要時才翻開「這一章」(L2);章節裡叫你「去附錄查表」才翻附錄(L3)。
>
> ✅ **好處:** 你可以裝幾十個技能,啟動成本卻很低;腳本「執行」時其程式碼**不佔上下文**,只有輸出佔 token。

---

## 5. `SKILL.md` 解剖

每個技能**必須**有一個 `SKILL.md`,開頭是 YAML frontmatter,後面是 Markdown 本文。

### 必填欄位

```yaml
---
name: pdf
description: 描述「做什麼」+「何時用」。這段話決定 Claude 何時自動啟用這個技能。
---
```

| 欄位 | 規則 | 重點 |
|---|---|---|
| `name` | ≤ 64 字、小寫英數與連字號、不可用保留字(anthropic、claude) | 技能 ID |
| `description` | ≤ 1024 字、第三人稱、不可空白 | **決定觸發**——要寫清楚「能力」+「觸發情境/關鍵字」 |

### ✅ 好描述 vs ❌ 壞描述

✅ 好(明確說出能力與觸發時機):
```
Use this skill whenever the user wants to do anything with PDF files —
extract text/tables, merge, split, rotate, watermark, fill forms, OCR…
If the user mentions a .pdf file or asks to produce one, use this skill.
```

❌ 壞(太模糊,Claude 不知何時該用):
```
Helps with documents
```

### 本文與附帶資源

frontmatter 之後就是 Claude 的「操作手冊」。慣例:

```
pdf/
├── SKILL.md        # 主說明(建議 < 500 行)
├── FORMS.md        # 表單填寫(需要時才載)
├── REFERENCE.md    # 完整 API 參考(需要時才載)
└── scripts/
    └── fill_form.py  # 可執行腳本(只跑,不把程式碼讀進上下文)
```

> 慣例:被參考的檔案在 `SKILL.md` 裡「只連一層」(別層層巢狀),命名要直白(`FORMS.md` 而非 `doc2.md`)。

---

## 6. 四種環境怎麼用 Skills

### 🅰️ Claude App(claude.ai / 桌面版聊天介面)

- **內建技能**:pptx、xlsx、docx、pdf — 開啟程式執行後自動可用。
- **自訂技能**:打包成 `.zip`,到 **Customize(自訂)→ Skills** 上傳:
  點「+」→「+ Create skill」→「Upload a skill」→ 選 `.zip`。
- **條件**:免費與付費方案皆可,但須先到 **設定 → Capabilities** 開啟「Code execution and file creation」,Skills 區塊才會出現。範圍僅限你個人。
- 📖 **完整圖文步驟** 👉 [動手做:在 Claude Desktop 上傳技能](08-upload-skill-on-claude-app.md)

### 🅱️ Claude Code(本課主場景,含桌面 App)

技能用**檔案系統**探索,不必上傳:

| 位置 | 範圍 |
|---|---|
| `~/.claude/skills/<名稱>/SKILL.md` | 個人(所有專案可用) |
| `.claude/skills/<名稱>/SKILL.md` | 專案(隨 repo 進版控,團隊共用) |
| 外掛(plugin) | 以 `/外掛名:技能名` 呼叫 |

**從官方 repo 安裝(外掛市集):**
```
/plugin marketplace add anthropics/skills   ← 加入官方市集
/plugin                                      ← 開選單瀏覽並安裝
/skills                                      ← 列出目前可用技能
/reload-skills                               ← 不重啟即時重新掃描
```

### 🅲 Claude API(Messages API)

需要三個 beta 標頭 + 程式執行工具,並用 `container.skills` 指定技能:

```python
import anthropic
client = anthropic.Anthropic()

resp = client.beta.messages.create(
    model="claude-opus-4-8",
    max_tokens=4096,
    betas=["code-execution-2025-08-25",
           "skills-2025-10-02",
           "files-api-2025-04-14"],
    container={"skills": [
        {"type": "anthropic", "skill_id": "pptx", "version": "latest"}
    ]},
    tools=[{"type": "code_execution_20250825", "name": "code_execution"}],
    messages=[{"role": "user", "content": "幫我做一份再生能源簡報"}],
)
```

- 內建技能 `skill_id`:`pptx`／`xlsx`／`docx`／`pdf`。
- 自訂技能:用 **Skills API**(`POST /v1/skills` 等端點)上傳後,以 `{"type": "custom", "skill_id": ...}` 使用。
- 限制:技能內**無網路**、不能臨時裝套件、單一技能上限約 30MB。

### 🅳 Claude Agent SDK

把技能放進專案的 `.claude/skills/`,SDK 會**自動探索**,行為與 Claude Code 一致,無需額外設定。

### 📊 四環境摘要

| 環境 | 內建技能 | 自訂技能 | 探索方式 |
|---|---|---|---|
| Claude App | ✅(4 個文件技能) | 上傳 `.zip` | 自動 |
| Claude Code | 透過外掛 | `.claude/skills/` 檔案 | 掃描檔案系統 |
| Claude API | ✅(`container`) | Skills API 上傳 | `container` 參數 |
| Agent SDK | — | `.claude/skills/` 檔案 | 自動探索 |

---

## 7. 🛠️ 動手做

### A. 安裝並試用官方技能(Claude Code)
1. `/plugin marketplace add anthropics/skills`
2. `/plugin` → 找一個技能(例如 `slack-gif-creator`)安裝
3. `/skills` 確認它出現
4. 用自然語言觸發它,觀察 Claude 如何讀取 `SKILL.md`

### B. 用 `skill-creator` 做一個自己的技能
1. 在 Claude Code 執行 `/skill-creator`(或安裝官方的 skill-creator)
2. 描述你要的技能(例:「把會議逐字稿整理成行動項目」)
3. 讓它幫你產生 `SKILL.md` 與資料夾結構
4. 放到 `~/.claude/skills/` → `/reload-skills` → 試用

---

## 8. 撰寫最佳實務(出自官方指南)

- **描述要好**:第三人稱、寫出能力 + 觸發關鍵字(這是觸發成敗關鍵)。
- **精簡**:假設 Claude 已懂通用概念;每段都問「真的需要嗎?」;**範例優於長篇解釋**;本文 < 500 行。
- **善用漸進式揭露**:細節拆到獨立檔、確定性操作寫成腳本(只跑不佔上下文)。
- **別做**:放時效性資訊、用詞不一致、給太多選項(「可用 A 或 B 或 C…」應給一個預設)、用 Windows 反斜線路徑。
- **要測試**:用 Haiku／Sonnet／Opus 各測一次,看 Claude 實際怎麼走你的結構。

---

## 9. ⚠️ 安全提醒(務必教給學生)

> **技能擁有「執行工具與腳本」的能力。安裝來路不明的第三方技能,等同在你的環境執行別人的程式碼。**

- 只安裝**可信來源**的技能(官方 `anthropics/skills`、你自己或團隊寫的)。
- 安裝前**先讀 `SKILL.md` 與其腳本**,看它會做什麼、要哪些權限。
- 善用 `allowed-tools` 限制技能可用的工具。
- 在 Claude Code 用 `claude plugin validate` 檢查外掛/技能結構。

---

## 10. 重要連結

| 資源 | 連結 |
|---|---|
| 官方 Skills GitHub | <https://github.com/anthropics/skills> |
| Agent Skills 總覽 | <https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview> |
| 快速上手 | <https://platform.claude.com/docs/en/agents-and-tools/agent-skills/quickstart> |
| 最佳實務 | <https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices> |
| Claude Code 的 Skills | <https://code.claude.com/docs/en/skills> |
| 工程部落格(發表文) | <https://anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills> |

---

## 📝 課堂練習

1. **觀念題:** 用一句話解釋「漸進式揭露」的三層,各在什麼時候載入。
2. **判斷題:** 我把官方的 `pptx` 技能直接拿去商業產品散布,可以嗎?為什麼?
3. **描述練習:** 替一個「把 CSV 轉成圖表」的技能,寫一段「好的」`description`(要含能力 + 觸發情境)。
4. **實作:** 用 `/plugin marketplace add anthropics/skills` 加入市集,安裝任一技能並 `/skills` 列出它。
5. **安全題:** 朋友傳你一個 `.zip` 技能叫你裝,你裝之前會先做哪兩件事?

<details>
<summary>👉 點此看參考答案</summary>

1. L1 中繼資料(name/description)啟動就載;L2 `SKILL.md` 本文在判斷用得到時載;L3 附帶資源(腳本/參考檔)在需要時才載。
2. **不行**。`pptx` 等四個文件技能是 source-available 的**專有授權**,僅供參考,不可任意散布。
3. 範例:`Convert CSV/tabular data into charts (bar, line, pie). Use when the user has a .csv file or asks to visualize/plot data.`
5. (a) 先讀它的 `SKILL.md` 與腳本看會做什麼;(b) 確認來源可信,必要時用 `allowed-tools` 限制權限。

</details>

---

## 🔑 重點回顧

- 官方 repo:**github.com/anthropics/skills**(文件類技能為專有授權,非開源)。
- Skills = 資料夾 + `SKILL.md`(`name`/`description` 必填,description 決定觸發)。
- 核心是**漸進式揭露**三層,啟動成本低、可擴充。
- 四環境:App 上傳 `.zip`、Claude Code 放 `.claude/skills/`、API 用 `container.skills`、SDK 自動探索。
- **安裝第三方技能前務必審查**——它能執行程式碼。

---

> 想知道**還有哪些技能來源與最新排名**? 👉 [第 9 課 Skill 來源榜](09-skill-sources-ranking.md)
>
> 回到 [課程首頁](../README.md) ｜ 複習 [速查表](../CHEATSHEET.md)
