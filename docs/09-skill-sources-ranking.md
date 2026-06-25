# 第 9 課（參考）｜Skill 來源榜:Top 10 GitHub 來源與「用 agent 調用」法

> 回到：[課程首頁](../README.md) ｜ 延伸自：[第 7 課 官方 Skills](07-anthropic-skills.md)、[第 8 課 上傳技能](08-upload-skill-on-claude-app.md)

> 本課給學員一份「去哪裡找技能、怎麼用 agent 一鍵調用」的速查榜。
> ⭐ 星數為 **2026-06-25 透過 GitHub API 實測**,僅供參考,**會隨時間變動**;請以當下實際數字與來源可信度為準。

---

## 🎯 學習目標

1. 知道技能的主要來源在哪。
2. 分辨**「可安裝的市集」**與**「精選清單」**兩種來源(用法不同)。
3. 學會用 `/plugin` 與 `/plugin marketplace add` **一鍵調用**技能。

---

## 1. 先懂兩種來源類型(關鍵!)

| 類型 | 是什麼 | 怎麼用 |
|---|---|---|
| 🛒 **市集(Marketplace)** | repo 內含 `.claude-plugin/marketplace.json`,可被 Claude Code 直接安裝 | `/plugin marketplace add <owner/repo>` → `/plugin` 安裝 |
| 📒 **精選清單(Awesome List)** | 一份「連結目錄」,列出散落各處的技能 | 開網頁瀏覽 → 找到想要的 → 下載資料夾放進 `~/.claude/skills/`,或請 agent 幫你抓 |

> 一句話:**市集 = 一鍵裝;清單 = 逛街找,再自己拿。**

---

## 2. 🏆 Top 10 GitHub Skill 來源榜（依星數）

| # | 來源 | ⭐ 星數 | 類型 | 一句話介紹 |
|---|---|---|---|---|
| 1 | [anthropics/skills](https://github.com/anthropics/skills) | 155,081 | 🛒 官方市集 | Anthropic 官方 Agent Skills,最權威的參考來源 |
| 2 | [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) | 65,874 | 📒 清單 | 1000+ 可用技能與外掛的精選大全 |
| 3 | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) | 41,677 | 🛒 市集 | 1,600+ 可安裝技能,附 CLI 安裝器與套組 |
| 4 | [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) | 31,097 | 🛒 官方市集 | Anthropic 官方精選外掛目錄(Claude Code 內建) |
| 5 | [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | 26,422 | 📒 清單 | 1000+ 來自 Anthropic、Vercel、Stripe 等官方團隊的技能 |
| 6 | [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) | 19,033 | 🛒 市集 | 330+ 技能、30+ agents、70+ 自訂指令 |
| 7 | [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | 13,721 | 📒 清單 | 聚焦 Claude Code 工作流的精選清單 |
| 8 | [BehiSecc/awesome-claude-skills](https://github.com/BehiSecc/awesome-claude-skills) | 9,625 | 📒 清單 | 簡潔的 Claude Skills 精選清單 |
| 9 | [jeremylongshore/claude-code-plugins-plus-skills](https://github.com/jeremylongshore/claude-code-plugins-plus-skills) | 2,429 | 🛒 市集 | 425 外掛、2,810 技能,附 `ccpi` 套件管理器 |
| 10 | [ComposioHQ/awesome-claude-plugins](https://github.com/ComposioHQ/awesome-claude-plugins) | 1,771 | 📒 清單 | 擴充 Claude Code 的外掛精選清單 |

> ⭐ **特別收錄(官方,星數雖低但重要):** [anthropics/claude-plugins-community](https://github.com/anthropics/claude-plugins-community)（214 ⭐）——官方維護的**社群外掛市集**(唯讀),適合找經過官方收錄的社群外掛。

---

## 3. 各來源怎麼「用 agent 調用」

> **三種調用法,由易到難 —— 新手從 🟢 開始就好:**
> 🟢 給 URL 叫它讀(臨時、零安裝) < 🟡 放進 skills 目錄(自己常用) < 🔵 裝市集(整批、長期)

### 🟢 最簡單(新手首選):直接給 agent 一個 GitHub URL

**不必安裝任何東西。** 把某個技能的 `.md`(通常是 `SKILL.md`)網址貼給 agent,叫它讀取並照做就行。

**做法:**
1. 在 GitHub 開啟那個 `.md`,複製網址(點頁面上的 **Raw** 鈕拿到更乾淨的 raw 連結)。
2. 在 Claude Desktop / Claude Code 對它說(複製下面這句、把網址換成你要的):
   > 讀取這個網址的內容,並依照裡面的說明完成任務:
   > `https://raw.githubusercontent.com/anthropics/skills/main/skills/pdf/SKILL.md`
3. agent 會用內建的網頁讀取能力抓下這份說明,然後照著做。

**適合:** 只想用一次、不想安裝、想先試試某個技能。
**限制:** 只能讀**公開** repo(私有的要另外授權);github.com 的 blob 網址通常也讀得到,但 raw 連結最穩。

> ⚠️ **安全(務必提醒學員):** 這等於讓「遠端文件」直接指揮你的 agent(它可能據此跑指令、改檔案)。**只指向你信任的網址**,最好**先自己讀過那份 `.md`** 再叫它執行。

> 📖 **完整圖文步驟 + 示範對話** 👉 [第 10 課:給 agent 一個 URL 一鍵執行](10-run-skill-from-url.md)

### 🟡 放進 skills 目錄(自己常用)

把技能資料夾(含 `SKILL.md`)放到 `~/.claude/skills/<名稱>/`(個人)或專案 `.claude/skills/<名稱>/`,再 `/reload-skills`;之後打 `/<名稱>` 就能用。

### 🔵 市集類(整批、長期:一鍵安裝)

通用三步驟(在 Claude Code / Claude Desktop 的 Cowork 模式輸入):

```
/plugin marketplace add <owner/repo>   ← 加入市集
/plugin                                ← 開選單瀏覽並安裝想要的技能/外掛
/skills                                ← 確認技能已可用
```

各市集的指令:

| 來源 | 調用指令 |
|---|---|
| 官方技能 | `/plugin marketplace add anthropics/skills` |
| 官方外掛(內建) | 直接 `/plugin` 瀏覽即可;或 `/plugin marketplace add anthropics/claude-plugins-official` |
| 官方社群市集 | `/plugin marketplace add anthropics/claude-plugins-community` |
| antigravity（1,600+） | `/plugin marketplace add sickn33/antigravity-awesome-skills` |
| alirezarezvani（330+） | `/plugin marketplace add alirezarezvani/claude-skills` |
| jeremylongshore（2,810） | `/plugin marketplace add jeremylongshore/claude-code-plugins-plus-skills` |

> 💡 安裝後,個別外掛技能用 `/外掛名:技能名` 呼叫(見 [第 3 課](03-skills.md))。

### 📒 清單類(瀏覽後取用)

清單不能直接 `add`。三種取用方式:
1. **瀏覽**:打開該 repo 的 README,找到想要的技能,點進它指向的來源 repo。
2. **若它指向一個市集** → 用上面的 `/plugin marketplace add` 裝。
3. **若它只是一個技能資料夾** → 下載該資料夾,放進 `~/.claude/skills/<名稱>/`,再 `/reload-skills`。
   - 或直接請 agent 幫忙:「幫我從 `<repo 連結>` 取得 X 技能並放到我的 skills 目錄」。

---

## 4. 📂 技能目錄網站(非 GitHub,適合用瀏覽器逛)

| 網站 | 特色 |
|---|---|
| [claudemarketplaces.com](https://claudemarketplaces.com/) | 社群最大目錄之一,每日從 GitHub 更新技能/外掛/MCP |
| [awesomeclaude.ai](https://awesomeclaude.ai/awesome-claude-skills) | Claude Agent Skills 目錄站 |
| [aitmpl.com](https://www.aitmpl.com/plugins/) | 外掛與市集合集 |
| tonsofskills.com | 對應 `jeremylongshore` 那個市集,附 `ccpi` CLI |

> ⚠️ 目錄網站只是「導覽」,實際安裝仍要回到可信的 repo/市集。**先確認來源可信再裝。**

---

## 5. 🔥 熱門個別技能(範例,供參考)

據公開外掛目錄統計(2026 年中,**數字會變動**),這幾個是常被安裝的明星技能:

| 技能 | 用途 |
|---|---|
| **Frontend Design** | 前端/UI 設計,產生美觀介面 |
| **Superpowers** | 一整套強化 Claude Code 的工作流外掛 |
| **Context7** | 即時抓取最新的套件/框架官方文件 |

> 想裝的話:先用 `/plugin` 在官方或上述市集裡搜尋同名技能再安裝。

---

## 6. ⚠️ 安全提醒(務必教給學員)

> **市集與技能可包含會被執行的程式碼與腳本。加入第三方市集 = 信任該來源。**

- **只做加法,別替換官方** —— 這些來源是用來「額外新增」技能;**不要拿來覆蓋或修改官方內建技能**。官方 Skills 會隨 Claude Desktop 更新,並配合模型能力、harness、loop 工程(原因詳見 [第 7 課](07-anthropic-skills.md))。
- **最安全**:`anthropics/*` 三個官方來源。
- 第三方市集/清單:**安裝前先看該技能的 `SKILL.md` 與 `scripts/`**,確認它做什麼。
- 用 `allowed-tools` 限制技能可用工具;用 `claude plugin validate` 檢查結構。
- 星數高 ≠ 安全;只是熱門。仍要自行判斷來源可信度。

---

## 📝 課堂練習

1. **分類題:** `anthropics/skills` 和 `ComposioHQ/awesome-claude-skills`,哪個能直接 `/plugin marketplace add`?為什麼?
2. **實作:** 執行 `/plugin marketplace add anthropics/claude-plugins-community`,再 `/plugin` 看看有哪些可裝。
3. **思考題:** 你在一份「清單」裡看到想要的技能,但它不是市集,要怎麼把它弄進 Claude Code?
4. **安全題:** 一個有 4 萬星的第三方市集,可以閉著眼睛裝嗎?說明你的判斷。

<details>
<summary>👉 點此看參考答案</summary>

1. `anthropics/skills`(它含 `.claude-plugin/marketplace.json`,是市集);`ComposioHQ/...` 是精選清單,只能瀏覽。
3. 下載該技能資料夾放進 `~/.claude/skills/<名稱>/` 後 `/reload-skills`;或請 agent 幫你抓。
4. 不行。星數代表熱門,不代表安全。仍要看它的技能/腳本內容、確認來源可信,再決定。

</details>

---

## 🔑 重點回顧

- 兩種來源:**🛒 市集(一鍵 add)** vs **📒 清單(逛街找)**。
- 最權威:`anthropics/skills`(15 萬星)、官方外掛市集、官方社群市集。
- 調用三步:`/plugin marketplace add <owner/repo>` → `/plugin` → `/skills`。
- **安裝前先審查來源與腳本**;官方來源最安全。

---

> 回到 [課程首頁](../README.md) ｜ [第 7 課 官方 Skills](07-anthropic-skills.md) ｜ [速查表](../CHEATSHEET.md)
