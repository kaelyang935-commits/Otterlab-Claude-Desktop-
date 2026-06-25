# 動手做｜在 Claude Desktop（App）上傳你的第一個技能

> 回到：[課程首頁](../README.md) ｜ 延伸自：[第 7 課 Anthropic 官方 Skills](07-anthropic-skills.md)

> 本篇教的是在 **Claude App（聊天介面，桌面版與網頁版相同）** 上傳自訂技能的 `.zip`。
> 桌面版與網頁版（claude.ai）介面一致,步驟完全相同。
> （若你用的是 Claude Code／Cowork 模式,不必上傳,直接把資料夾放進 `~/.claude/skills/` 即可,見 [第 7 課](07-anthropic-skills.md)。）

---

## 🎯 你會做到

把一個自己寫的技能資料夾,打包成 `.zip` 並上傳到 Claude Desktop,讓 Claude 在對話中自動使用它。

---

## ✅ 前置條件（很重要,沒做會看不到 Skills）

1. **方案:** 免費、Pro、Max、Team、Enterprise **皆可**。
2. **必須先開啟「程式執行」**:
   - **設定（Settings）→ Capabilities（功能）→ 開啟「Code execution and file creation（程式執行與檔案建立）」**
   - ⚠️ 沒開這個,**Skills 區塊根本不會出現**。
   - Team／Enterprise 帳號:可能要由管理員在 **Organization settings（組織設定）→ Skills** 同時開啟「Code execution and file creation」與「Skills」。

> 開啟之後,內建的 **Excel／Word／PowerPoint／PDF** 技能會自動可用,Claude 需要時會自己使用。

---

## 步驟 1️⃣：準備技能資料夾

建立一個資料夾(名稱就是技能名,用小寫與連字號),裡面放一個 **`SKILL.md`**(大寫,這是規格標準)。

範例:做一個把逐字稿整理成行動項目的技能。

```
meeting-notes/
└── SKILL.md
```

`SKILL.md` 內容:

```markdown
---
name: meeting-notes
description: 把會議逐字稿整理成決議與行動項目（含負責人與期限）。當使用者貼上會議記錄、逐字稿,或要求整理會議重點時使用。
---

# 會議記錄整理

## 任務
把使用者提供的逐字稿整理成:
1. 一句話會議摘要
2. 決議事項(條列)
3. 行動項目表格:| 事項 | 負責人 | 期限 |

## 規則
- 用繁體中文
- 找不到負責人或期限就標「待確認」
```

**欄位重點:**
- `name`:≤ 64 字,小寫英數與連字號,不可用 `claude`／`anthropic`。
- `description`:**決定 Claude 何時自動使用它**,務必寫清楚「做什麼 + 何時用 + 觸發關鍵字」。請寫精簡(App 上傳介面對長度限制較嚴;若是給 API／Claude Code 用的規格則可到 1024 字)。

> 進階:可在資料夾裡再放 `scripts/`、`REFERENCE.md` 等附帶資源,Claude 需要時才會讀取(漸進式揭露,見 [第 7 課](07-anthropic-skills.md)）。

---

## 步驟 2️⃣：壓成 `.zip`（最容易出錯的一步）

**關鍵原則:壓縮「資料夾本身」,讓資料夾是 zip 的根,不要多包一層、也不要只壓裡面的檔案。**

✅ 正確結構:
```
meeting-notes.zip
└── meeting-notes/
    └── SKILL.md
```

❌ 常見錯誤:
- 只把 `SKILL.md` 壓進去(缺了外層資料夾)
- 多包一層 `我的技能/meeting-notes/SKILL.md`

**怎麼壓:**
- **macOS:** 在 `meeting-notes` 資料夾上**按右鍵 → 壓縮「meeting-notes」**,會得到 `meeting-notes.zip`。
- **Windows:** 在資料夾上右鍵 → **傳送到 → 壓縮的 (zipped) 資料夾**。
- **命令列(最保險):**
  ```bash
  zip -r meeting-notes.zip meeting-notes
  ```

---

## 步驟 3️⃣：上傳到 Claude

1. 開啟 Claude Desktop(或 claude.ai)
2. 進入 **Customize（自訂）→ Skills（技能）**
3. 點 **「+」** 按鈕 → **「+ Create skill（建立技能）」**
4. 選擇 **「Upload a skill（上傳技能）」**
5. 選你的 `meeting-notes.zip`
6. 上傳成功後,該技能會出現在清單中

> 自訂技能預設**只屬於你個人**,除非你的組織開啟了共用。

---

## 步驟 4️⃣：開啟並測試

1. 在 **Customize → Skills** 清單中,確認該技能的開關是**開啟**的。
2. 開一段新對話,輸入一個「應該會觸發它」的訊息,例如:
   > 「幫我把這段會議逐字稿整理成行動項目:……(貼上一段逐字稿)」
3. 觀察 Claude 是否套用了你在 `SKILL.md` 寫的格式(摘要 → 決議 → 行動項目表格)。

---

## 🧰 疑難排解

| 症狀 | 可能原因與解法 |
|---|---|
| 設定裡**找不到 Skills** | 沒開啟「Code execution and file creation」;Team/Enterprise 可能要管理員先開 |
| 上傳**失敗／格式錯誤** | zip 結構不對(見步驟 2);或缺 `SKILL.md`;或 frontmatter 的 `name`／`description` 沒寫 |
| 技能**沒被觸發** | `description` 太模糊。把「觸發情境與關鍵字」寫具體,例如明列「逐字稿、會議記錄、行動項目」 |
| 技能名稱被拒 | `name` 含大寫/空白/保留字(claude、anthropic),或超過 64 字 |
| `SKILL.md` 讀不到 | 檔名大小寫(用大寫 `SKILL.md` 最保險)或不在資料夾根層 |

---

## 🔒 安全提醒

- 技能可包含**會被執行的腳本**。只上傳**你自己寫的**或**可信來源**的技能。
- 上傳他人提供的 `.zip` 前,先解壓檢查 `SKILL.md` 與任何 `scripts/` 內容。

---

## 🔑 重點回顧

1. 先 **設定 → Capabilities → 開啟 Code execution**,Skills 才會出現。
2. 技能 = 資料夾 + `SKILL.md`(`name`、`description` 必填)。
3. **壓縮資料夾本身**(資料夾為 zip 根,別多包一層)。
4. **Customize → Skills → + → Create skill → Upload a skill → 選 `.zip`**。
5. 在清單開啟,用會觸發的 prompt 測試。

---

> 回到 [課程首頁](../README.md) ｜ 複習 [第 7 課 官方 Skills](07-anthropic-skills.md) ｜ [速查表](../CHEATSHEET.md)
