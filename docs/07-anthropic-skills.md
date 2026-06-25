# 第 7 課（進階專題）｜Anthropic 官方 Skills：概念與用法

> 上一課：[第 6 課 講師手冊](06-instructor-guide.md)（學生可從 [第 5 課](05-environments.md) 直接過來）｜ 下一課：[第 8 課 找 Skill .md](08-find-skill-md.md)
> 延伸自：[第 3 課 技能指令](03-skills.md)、[第 4 課 自訂指令](04-custom-commands.md) ｜ 回到 [課程首頁](../README.md)

> 本課是進階專題。學會 [第 3 課](03-skills.md)（技能是什麼）與 [第 4 課](04-custom-commands.md)（自己寫指令）後再讀，效果最好。

---

## 🎯 學習目標

1. 知道 **Anthropic 官方 Skills 的 GitHub repo** 在哪、收錄了什麼。
2. 理解 Skills 的核心設計：**漸進式揭露（Progressive Disclosure）**。
3. 看懂 `SKILL.md` 的結構。
4. 建立正確心法：**官方技能自動用、別替換；缺的就找一份 `.md` 來用**。

---

## 1. 什麼是 Agent Skills？

> **Skill（技能）= 一個資料夾，裡面裝著「教 Claude 做某件事」的說明書、腳本與素材。**

一個 Skill 通常包含：
- **指示（instructions）**：做事的步驟與最佳實務
- **中繼資料（metadata）**：名稱與描述，讓 Claude 知道「何時該用它」
- **附帶資源（選用）**：腳本、範本、參考文件

這跟你在 [第 4 課](04-custom-commands.md) 做的自訂指令是**同一套機制**——官方 Skills 只是「做得很完整、可重用」的範例。

---

## 2. 官方 GitHub Repo

🔗 **https://github.com/anthropics/skills**

- **用途：** Anthropic 官方公開的 Agent Skills 範例與規格，是學習與參考的最佳來源。
- **狀態：** 公開、積極維護。
- **主要結構：**

```
anthropics/skills/
├── skills/                 # 各個技能範例(主要看這裡)
├── spec/                   # Agent Skills 規格說明
├── template/               # 建立新技能用的範本
├── README.md
└── THIRD_PARTY_NOTICES.md
```

> ⚠️ **授權注意(教學重點):**
> repo 內的**文件類技能**(`pdf`、`docx`、`pptx`、`xlsx`)是 **source-available 的「專有授權(Proprietary)」**——可以閱讀作為參考實作，但**不是開源**，不可任意自行散布。其他範例技能多採開源授權。實際以各技能資料夾內的 `LICENSE` 與 repo 的 `THIRD_PARTY_NOTICES.md` 為準。

---

## 3. Repo 收錄了哪些 Skills?

目前收錄十多個技能，依用途分類(節錄):

| 類別 | 技能 |
|---|---|
| 📄 文件處理（專有授權） | `pdf`、`docx`、`pptx`、`xlsx` |
| 🛠️ 開發 | `claude-api`、`mcp-builder`、`web-artifacts-builder`、`webapp-testing` |
| 🎨 設計創意 | `algorithmic-art`、`canvas-design`、`frontend-design`、`theme-factory`、`brand-guidelines`、`slack-gif-creator` |
| 🏢 企業溝通 | `internal-comms`、`doc-coauthoring` |
| 🧰 工具 | `skill-creator`（用來「建立技能的技能」） |

> 💡 這四個文件技能(pdf／docx／pptx／xlsx)其實就是 Claude 產生 Word／PPT／Excel／PDF 檔背後用的同一套東西。

---

## ⚠️ 重要原則：不要自行更換官方 Skills

> **官方內建的 Skills 不要自己去替換或修改。想擴充能力，請「另外新增」——找一份適合的 skill `.md`，或在自己的 skills 目錄放具名的新技能。**

**為什麼?**

- 官方 Skills 會**隨 Claude Desktop / Claude Code 版本一起更新**，並與當前的 **模型能力、harness(執行框架)、loop(代理迴圈)工程** 整體搭配、共同調校。
- 自行替換或編輯官方技能會有三個問題：
  1. **破壞協調** —— 打亂「模型 × harness × loop × skill」的搭配，行為可能反而變差或出錯。
  2. **被覆蓋** —— 下次更新時，你的改動會被官方版本蓋掉而消失。
  3. **失去後續優化** —— 拿不到官方對該技能的持續修正與強化。

**✅ 正確做法(用「加法」，不要「替換」):**

| 你想做的 | 正確方式 |
|---|---|
| 增加新能力 | **找一份適合的 skill `.md`** 來用(見 [第 8 課](08-find-skill-md.md))，或在 `~/.claude/skills/` 放**自己命名**的技能 |
| 客製某個流程 | 寫一個**新的、具名**技能(見 [第 4 課](04-custom-commands.md))，別用與官方同名而蓋掉它 |
| 不想要某個內建技能 | 用設定**停用**它，而不是去改它的檔案 |

> 🧠 一句話：**官方件用「原廠」的就好；要加東西，另外找一份 `.md` 或自己另開新技能，別動原廠件。**

---

## 4. 核心觀念：漸進式揭露(Progressive Disclosure)

這是 Skills 最重要、也最常被考的設計。Claude **不會一次把整個技能讀進來**，而是**分三層、用到才載**:

| 層級 | 內容 | 何時載入 | Token 成本 |
|---|---|---|---|
| **L1 中繼資料** | `name` + `description` | 啟動時就載入 | 極小（每個技能約 100 tokens） |
| **L2 完整說明** | `SKILL.md` 本文 | 判斷「這次用得到」才載 | 通常 < 5,000 tokens |
| **L3 附帶資源** | 其他 `.md`、腳本、範本 | 需要某個檔／跑某支腳本時才載 | 幾乎無上限 |

> 🧠 **比喻：** 像一本書——你平常只看「書背標題」(L1)；需要時才翻開「這一章」(L2)；章節裡叫你「去附錄查表」才翻附錄(L3)。
>
> ✅ **好處：** 你可以裝幾十個技能，啟動成本卻很低；腳本「執行」時其程式碼**不佔上下文**，只有輸出佔 token。

---

## 5. `SKILL.md` 解剖

每個技能**必須**有一個 `SKILL.md`，開頭是 YAML frontmatter，後面是 Markdown 本文。

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

❌ 壞(太模糊，Claude 不知何時該用):
```
Helps with documents
```

### 本文與附帶資源

frontmatter 之後就是 Claude 的「操作手冊」。慣例：

```
pdf/
├── SKILL.md        # 主說明(建議 < 500 行)
├── FORMS.md        # 表單填寫(需要時才載)
├── REFERENCE.md    # 完整 API 參考(需要時才載)
└── scripts/
    └── fill_form.py  # 可執行腳本(只跑,不把程式碼讀進上下文)
```

> 慣例：被參考的檔案在 `SKILL.md` 裡「只連一層」(別層層巢狀)，命名要直白(`FORMS.md` 而非 `doc2.md`)。

---

## 6. 官方技能怎麼用?(很簡單)

**結論先講：官方技能「內建就會用」，你幾乎不用做任何設定。**

- **日常任務** → 直接講你要做的事，Claude 會在合適時**自動**使用對應的官方技能(做 Word／PPT／Excel／PDF、審查程式碼…)。
- **官方沒有、又很具體的任務** → 去 GitHub **找一份適合的 skill `.md`**(見 [第 8 課](08-find-skill-md.md))，再用**最簡單的方式**叫 agent 執行：把那份 `.md` 的網址貼給它，說「讀這個並照做」(見 [第 9 課](09-run-skill-from-url.md))。
- **想做一個自己專屬的技能** → 用 `/skill-creator` 幫你產生，或照 [第 4 課](04-custom-commands.md) 自己寫，放進 `~/.claude/skills/`。

> 🧠 一句話：**官方的自動用；缺的，找一份 `.md` 貼網址給 agent 跑；要專屬的，自己做一個。**
> 不必學上傳 `.zip`、也不必碰外掛市集——對學員來說，記住上面三句就夠了。

---

## 7. 撰寫最佳實務(出自官方指南)

- **描述要好**：第三人稱、寫出能力 + 觸發關鍵字(這是觸發成敗關鍵)。
- **精簡**：假設 Claude 已懂通用概念；每段都問「真的需要嗎?」；**範例優於長篇解釋**；本文 < 500 行。
- **善用漸進式揭露**：細節拆到獨立檔、確定性操作寫成腳本(只跑不佔上下文)。
- **別做**：放時效性資訊、用詞不一致、給太多選項(「可用 A 或 B 或 C…」應給一個預設)、用 Windows 反斜線路徑。
- **要測試**：用 Haiku／Sonnet／Opus 各測一次，看 Claude 實際怎麼走你的結構。

---

## 8. ⚠️ 安全提醒(務必教給學生)

> **技能擁有「執行工具與腳本」的能力。使用來路不明的第三方技能，等同在你的環境執行別人的程式碼。**

- 只使用**可信來源**的技能(官方 `anthropics/skills`、你自己或團隊寫的)。
- 使用前**先讀 `SKILL.md` 與其腳本**，看它會做什麼；不確定就先請 agent「**先摘要、不要執行**」。
- 善用 `allowed-tools` 限制技能可用的工具。

---

## 9. 重要連結

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

1. **觀念題：** 用一句話解釋「漸進式揭露」的三層，各在什麼時候載入。
2. **判斷題：** 我把官方的 `pptx` 技能直接拿去商業產品散布，可以嗎?為什麼?
3. **描述練習：** 替一個「把 CSV 轉成圖表」的技能，寫一段「好的」`description`(要含能力 + 觸發情境)。
4. **實作：** 到 [第 8 課](08-find-skill-md.md) 的來源找一個你用得到的技能，打開它的 `SKILL.md` 看 `description` 適不適合你的任務。
5. **安全題：** 你想用一個第三方技能的 `.md`，在叫 agent「照做」前會先做哪兩件事?

<details>
<summary>👉 點此看參考答案</summary>

1. L1 中繼資料(name/description)啟動就載；L2 `SKILL.md` 本文在判斷用得到時載；L3 附帶資源(腳本/參考檔)在需要時才載。
2. **不行**。`pptx` 等四個文件技能是 source-available 的**專有授權**，僅供參考，不可任意散布。
3. 範例：`Convert CSV/tabular data into charts (bar, line, pie). Use when the user has a .csv file or asks to visualize/plot data.`
5. (a) 先打開那份 `.md` 自己讀、看它要 agent 做什麼；(b) 確認來源可信，或先請 agent「先摘要再執行」。

</details>

---

## 🔑 重點回顧

- 官方 repo:**github.com/anthropics/skills**(文件類技能為專有授權，非開源)。
- Skills = 資料夾 + `SKILL.md`(`name`/`description` 必填，description 決定觸發)。
- 核心是**漸進式揭露**三層，啟動成本低、可擴充。
- 用法很簡單：**官方技能自動用**；缺的就**找一份 `.md`、貼網址叫 agent 跑**(第 8、9 課);**別替換官方件**。

---

> 想知道**去哪裡找適合你任務的技能 `.md`**? 👉 [第 8 課 找到適合你任務的 Skill .md](08-find-skill-md.md)
>
> 回到 [課程首頁](../README.md) ｜ 複習 [速查表](../CHEATSHEET.md)
