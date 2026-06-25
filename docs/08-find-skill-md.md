# 第 8 課｜找到適合你「當下任務」的 GitHub Skill `.md`

> 上一課：[第 7 課 官方 Skills](07-anthropic-skills.md) ｜ 下一課：[第 9 課 給 agent 一個 URL 一鍵執行](09-run-skill-from-url.md)

---

## 🎯 學習目標

1. 建立正確心法:**官方的先用;缺了,再找一份 `.md` 補**。
2. 知道**去哪裡找**技能。
3. 學會**挑到「對的」那一個**。

---

## 1. 心法:先官方,缺的再找一份 `.md`

- **日常任務** → 直接用**官方內建技能**(Word／PPT／Excel／PDF、程式碼審查…)。它們最穩、會隨 Claude Desktop 一起更新(見 [第 7 課](07-anthropic-skills.md))。
- **官方沒有、又很具體的任務**(例:某個小眾格式轉換、特定流程) → 去 GitHub **找一份專門做這件事的 skill `.md`**,用 [第 9 課](09-run-skill-from-url.md) 的方法叫 agent 讀它來做。
- **不需要安裝、不需要外掛、不需要打包 `.zip`** —— 找到那份 `.md`、拿到網址就行。

> 🧠 一句話:**官方的先用;缺的,找一份 `.md` 補。**

---

## 2. 怎麼挑「適合當下任務」的那一個(3 步)

1. **講清楚你的任務**:用幾個關鍵字描述。
   - 例:`PDF 合併`、`逐字稿轉行動項目`、`Mermaid 流程圖`、`Excel 樞紐分析`
2. **到來源用關鍵字找**(下面的來源榜),或直接在 GitHub 搜尋框打 `skill <你的關鍵字>`。
3. **打開它的 `SKILL.md`,看開頭的 `description`**:它寫的「做什麼、何時用」對得上你的任務嗎?
   - ✅ 對得上 → 用它(進 [第 9 課](09-run-skill-from-url.md))。
   - ❌ 對不上 → 換一個。

> 💡 `description` 就是技能的「自我介紹」,挑技能**先看它**,比看名字準。

---

## 3. 📚 去哪裡找:熱門來源榜

> ⭐ 星數為 **2026-06-25 透過 GitHub API 實測**,僅供參考,**會變動**。優先看 `anthropics/*` 官方來源最安心。

| # | 來源 | ⭐ | 適合找什麼 |
|---|---|---|---|
| 1 | [anthropics/skills](https://github.com/anthropics/skills) | 155,081 | 官方技能的 `.md`(最權威的參考實作) |
| 2 | [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) | 65,874 | 1000+ 技能彙整,依用途分類好找 |
| 3 | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) | 41,677 | 1,600+ 技能大庫,主題最廣 |
| 4 | [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | 26,422 | 來自各大廠團隊的技能 |
| 5 | [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) | 19,033 | 工程／行銷／產品等職能技能 |
| 6 | [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | 13,721 | 精選、聚焦實用工作流 |
| 7 | [BehiSecc/awesome-claude-skills](https://github.com/BehiSecc/awesome-claude-skills) | 9,625 | 簡潔的精選清單 |
| 8 | [karanb192/awesome-claude-skills](https://github.com/karanb192/awesome-claude-skills) | 402 | 50+ 嚴選、標註驗證過的技能 |

> 💡 想用瀏覽器逛、找靈感,也可看目錄站如 [claudemarketplaces.com](https://claudemarketplaces.com/)、[awesomeclaude.ai](https://awesomeclaude.ai/awesome-claude-skills);**用前仍要確認來源可信**。

---

## 4. 找到之後怎麼用?

1. 打開那份技能的 `SKILL.md`。
2. 點頁面的 **Raw** 鈕,複製 raw 連結(例:`https://raw.githubusercontent.com/<owner>/<repo>/main/skills/<名稱>/SKILL.md`)。
3. 進 [第 9 課](09-run-skill-from-url.md):把網址貼給 agent,說「**讀這個並照做**」+ 你的需求。

> 就這樣,零安裝。要重複用,再把那份 `.md` 存進 `~/.claude/skills/<名稱>/`(見 [第 9 課](09-run-skill-from-url.md))。

---

## 5. ⚠️ 安全

- **官方 `anthropics/*` 最安全。**
- 第三方來源:**先打開 `.md` 自己讀**(它要 agent 做什麼?),或先請 agent「先摘要再執行」。
- **星數高 ≠ 安全**,只是熱門;仍要自行判斷來源可信度。

---

## 📝 課堂練習

1. **挑技能:** 想一個你工作上常做、但官方沒有的小任務,寫出 3 個搜尋關鍵字。
2. **看描述:** 到來源榜第 1 個 repo,找一個技能,讀它的 `description`,判斷它適合什麼任務。
3. **心法題:** 「做一份 PPT」和「把某個冷門格式轉檔」,各該用官方技能還是去找一份 `.md`?
4. **安全題:** 你找到一個剛好符合的第三方技能,叫 agent 用它之前先做什麼?

<details>
<summary>👉 點此看參考答案</summary>

3. 「做 PPT」用**官方** pptx 技能(內建、最穩);「冷門格式轉檔」官方多半沒有,去 GitHub **找一份 `.md`**。
4. 先**打開那份 `.md` 自己讀**或請 agent「先摘要再執行」,並確認來源可信。

</details>

---

## 🔑 重點回顧

- 心法:**官方的先用;缺的,找一份 `.md` 補**。
- 挑技能 3 步:講清任務 → 到來源用關鍵字找 → **看 `description` 對不對得上**。
- 找到後拿 **Raw** 連結 → 進 [第 9 課](09-run-skill-from-url.md) 叫 agent 執行。
- 官方 `anthropics/*` 最安全;第三方先讀過再用。

---

> 找到了嗎?下一課教你**一鍵叫 agent 執行它** 👉 [第 9 課 給 agent 一個 URL 一鍵執行](09-run-skill-from-url.md)
