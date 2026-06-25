# 第 2 課｜內建指令大全

> 上一課：[第 1 課 基礎概念](01-fundamentals.md) ｜ 下一課：[第 3 課 技能指令](03-skills.md)

---

## 🎯 學習目標

1. 認識 15 大類的內建指令，知道「想做某件事該用哪個指令」。
2. 能分辨幾組最容易混淆的指令。
3. 熟練 5 個對話管理的核心指令。

> 📌 標示 `[技能]` 的指令屬於「技能型」，詳細運作見 [第 3 課](03-skills.md)，本課先列出供查閱。

---

## 1️⃣ 對話與工作階段管理

最常用、每天都會碰到的一類。

| 指令 | 用途 |
|---|---|
| `/clear [名稱]` | 開新對話、清空上下文（舊對話仍可 `/resume`）。別名 `/reset`、`/new` |
| `/resume [階段]` | 依 ID／名稱恢復對話，或開啟挑選器。別名 `/continue` |
| `/branch [名稱]` | 從目前這個點「分支」對話，試不同方向又不弄丟原對話 |
| `/fork <指示>` | 派出背景子代理，繼承完整對話去做某件事，完成後把結果送回 |
| `/background [提示]` | 把目前階段轉為背景代理執行，釋放終端機。別名 `/bg` |
| `/rename [名稱]` | 重新命名目前階段並顯示在提示列 |
| `/teleport` | 把「網頁版 Claude Code」的階段拉進本機。別名 `/tp` |
| `/remote-control` | 讓本階段可從 claude.ai 遠端控制。別名 `/rc` |
| `/exit` | 離開（背景階段中會改為「分離」，階段繼續跑）。別名 `/quit` |

---

## 2️⃣ 上下文與記憶體管理

> 「上下文」= Claude 目前記得的對話內容。它有容量上限，太滿會變慢、變貴。

| 指令 | 用途 |
|---|---|
| `/context [all]` | 以彩色格狀圖檢視目前上下文用量，並給優化建議 |
| `/compact [指示]` | 摘要壓縮目前對話以釋出上下文，可指定保留重點 |
| `/memory` | 編輯 `CLAUDE.md` 記憶檔、開關自動記憶、檢視記憶條目 |
| `/rewind` | 把對話和／或程式碼倒回先前某點。別名 `/checkpoint`、`/undo` |
| `/export [檔名]` | 把目前對話匯出成純文字 |

> 🔎 **講師提醒：** `/clear`（清空）≠ `/compact`（摘要壓縮）≠ `/rewind`（倒回某點）。這三個最常被搞混，見本課末「易混淆對照表」。

---

## 3️⃣ 程式碼審查與品質

> 🔰 **非工程師看到這些詞先別怕（會寫程式可跳過這格）：**
> **diff** ＝ 程式碼這次改了哪幾行的對照;**repo（儲存庫）** ＝ 放一個專案所有程式碼的資料夾,通常在 GitHub 上;**PR（Pull Request）** ＝ 把你的修改提出來請人審查、合併的請求。這一節主要給「有程式專案」的人,沒有的可先略過。

| 指令 | 用途 |
|---|---|
| `/code-review [等級] [--fix] [--comment] [目標]` | `[技能]` 審查目前 diff 的錯誤與可簡化處。等級：`low`/`medium`/`high`/`xhigh`/`max`/`ultra`。`--fix` 自動修、`--comment` 貼到 PR、`ultra` 走雲端深度審查 |
| `/simplify [目標]` | `[技能]` 只做整理（重用、簡化、效率、抽象層級），不抓 bug |
| `/security-review` | `[技能]` 分析目前分支待提交的變更，找出安全漏洞 |
| `/review [PR]` | 用同一套引擎審查 GitHub PR（無參數會列出開啟中的 PR） |

> 🔎 **易混淆：** `/review` 針對 **GitHub PR**、`/code-review` 針對 **本機 diff**、`/simplify` **不抓 bug 只清理**。

---

## 4️⃣ 工作流程與執行

| 指令 | 用途 |
|---|---|
| `/plan [描述]` | 直接進入「計畫模式」，可附上要規劃的任務 |
| `/batch <指示>` | `[技能]` 大規模平行改動：研究→拆成 5–30 個單元→每單元一個背景代理（需 git） |
| `/goal [條件\|clear]` | 設定目標，Claude 會跨回合持續做到條件達成為止 |
| `/tasks` | 檢視／管理所有背景執行中的工作。別名 `/bashes` |
| `/debug [描述]` | `[技能]` 開啟除錯記錄並排查問題 |
| `/loop [間隔] [提示]` | `[技能]` 在階段開著時重複跑某提示（省略間隔則自行調步）。別名 `/proactive` |
| `/deep-research <問題>` | `[技能]` 對問題大量平行網搜、交叉查證、產出附引用報告 |

---

## 5️⃣ 專案與資料夾設定

| 指令 | 用途 |
|---|---|
| `/init` | 為專案建立 `CLAUDE.md` 指南（設 `CLAUDE_CODE_NEW_INIT=1` 進互動流程） |
| `/add-dir <路徑>` | 本階段額外加入一個可存取的工作目錄 |
| `/cd <路徑>` | 把本階段切換到新的工作目錄（保留快取） |

---

## 6️⃣ 建置與執行

| 指令 | 用途 |
|---|---|
| `/run` | `[技能]` 實際啟動並操作你的 App，看變更在「跑起來的程式」裡是否生效 |
| `/verify` | `[技能]` 建置＋執行＋觀察結果來確認變更正確，而非只靠測試 |
| `/run-skill-generator` | `[技能]` 教 `/run` 與 `/verify` 如何從乾淨環境建置、啟動、操作你的 App |

---

## 7️⃣ 模型與運算強度

| 指令 | 用途 |
|---|---|
| `/model [模型]` | 切換 AI 模型並存為新階段預設（可用左右鍵調 effort） |
| `/effort [等級\|auto]` | 設定運算強度：`low`/`medium`/`high`/`xhigh`/`max`/`ultracode` |
| `/advisor [模型\|off]` | 開關「顧問」工具：在關鍵時刻諮詢第二個模型 |
| `/fast [on\|off]` | 切換快速模式（Opus 加速輸出，非降級成小模型） |

> 🔎 **易混淆：** `/model` 是「換哪個模型」，`/effort` 是「同一模型思考多深」，兩者獨立。

---

## 8️⃣ 說明與資訊

| 指令 | 用途 |
|---|---|
| `/help` | 顯示說明與可用指令 |
| `/status` | 開啟設定介面（狀態頁）：版本、模型、帳號、連線 |
| `/usage` | 顯示花費、方案用量與活動統計。別名 `/cost`、`/stats` |
| `/doctor` | 診斷安裝與設定（按 `f` 讓 Claude 嘗試修復） |
| `/feedback [內容]` | 回報問題或分享對話。別名 `/bug`、`/share` |
| `/release-notes` | 用版本挑選器看更新說明 |

> 💰 **關於費用,在職新手先懂這幾件事：**
> 1. 先用 `/usage` 看自己的方案與目前用量（Code 分頁功能需 **Pro／Max／Team／Enterprise** 方案）。
> 2. **越強越貴**：用 Opus、把 `/effort` 調高、`--fix`、`/code-review ultra`,以及雲端指令（`/ultraplan`、`/autofix-pr`、`/schedule`）都比較耗用量。
> 3. 額度用完時,`/usage-credits` 可設定是否繼續使用（可能額外計費）。建議先觀察幾天用量,再決定怎麼用。

---

## 9️⃣ 設定與偏好

| 指令 | 用途 |
|---|---|
| `/config [key=value]` | 開啟設定介面（主題、模型、輸出風格等）；新版可直接傳鍵值對。別名 `/settings` |
| `/theme` | 變更色彩主題（含 auto、無障礙、自訂主題） |
| `/tui [default\|fullscreen]` | 設定終端機 UI 渲染器（`fullscreen` 無閃爍） |
| `/color [顏色\|default]` | 設定本階段提示列顏色 |
| `/keybindings` | 開啟鍵盤快捷鍵設定檔 |
| `/terminal-setup` | 設定 Shift+Enter 等終端快捷鍵（僅特定終端機顯示） |
| `/statusline` | 設定狀態列 |
| `/focus`、`/scroll-speed` | 專注檢視 ／ 滾動速度（僅 fullscreen 渲染） |

---

## 🔟 帳號與驗證

| 指令 | 用途 |
|---|---|
| `/login` ／ `/logout` | 登入 ／ 登出 Anthropic 帳號 |
| `/upgrade` | 開啟升級頁（僅 Pro／Max 顯示） |
| `/usage-credits` | 設定用量額度，額度用完仍能繼續（舊名 `/extra-usage`） |

---

## 1️⃣1️⃣ 程式碼檢視與編輯

| 指令 | 用途 |
|---|---|
| `/diff` | 開互動式 diff 檢視器（看未提交變更與每回合 diff） |
| `/copy [N]` | 複製最後一則回覆（`N` 指定倒數第幾則；有程式碼時可挑區塊） |

---

## 1️⃣2️⃣ 開發工具（多為互動面板）

| 指令 | 用途 |
|---|---|
| `/permissions` | 管理工具權限的 allow／ask／deny 規則。別名 `/allowed-tools` |
| `/agents` | 管理子代理（subagents）設定 |
| `/mcp [reconnect\|enable\|disable …]` | 管理 MCP 伺服器連線與 OAuth |
| `/hooks` | 檢視／設定各工具事件的 hooks |
| `/skills` | 列出可用技能（按 `t` 依 token 排序、`Space` 隱藏） |
| `/plugin [子指令]` | 管理外掛（`list`／`install`／`enable`／`disable`） |
| `/reload-plugins [--force]` | 重新載入外掛而不重啟 |
| `/reload-skills` | 重新掃描技能／指令目錄，讓新加的即時可用 |

---

## 1️⃣3️⃣ IDE 與整合

| 指令 | 用途 |
|---|---|
| `/ide` | 管理 IDE 整合並顯示狀態 |
| `/chrome` | 設定 Claude in Chrome |
| `/install-github-app` | 為儲存庫安裝 Claude GitHub App |
| `/install-slack-app` | 安裝 Claude Slack App |

---

## 1️⃣4️⃣ 雲端與遠端服務

| 指令 | 用途 |
|---|---|
| `/schedule [描述]` | 在雲端建立／管理排程「routines」。別名 `/routines` |
| `/autofix-pr [提示]` | 派雲端階段盯著 PR，CI 失敗或有評論時自動推修正 |
| `/web-setup` | 用本機 `gh` 憑證把 GitHub 連到網頁版 Claude Code |
| `/remote-env` | 選擇雲端代理的預設環境 |
| `/ultraplan <提示>` | 在雲端起草計畫→瀏覽器審閱→遠端執行或送回終端機 |
| `/ultrareview [PR]` | 雲端多代理深度審查（建議改用 `/code-review ultra`） |
| `/desktop` | 把目前階段接續到 Claude Code 桌面 App。別名 `/app` |

---

## 1️⃣5️⃣ 其他實用工具

| 指令 | 用途 |
|---|---|
| `/btw <問題>` | 問個「不寫進對話歷史」的小問題，避免汙染上下文 |
| `/recap` | 即時產生目前階段的一句話摘要 |
| `/insights` | 分析使用階段，產出專案領域、互動模式、卡關點報告 |
| `/team-onboarding` | 由近 30 天使用紀錄產生團隊上手指南 |
| `/powerup` | 用互動小課程＋動畫示範認識功能（★ 教學現場很好用） |
| `/mobile` | 顯示下載 Claude App 的 QR code。別名 `/ios`、`/android` |
| `/privacy-settings`、`/heapdump`、`/radio`、`/stickers`、`/passes` | 隱私設定／記憶體快照／lo-fi 電台／貼紙／邀請好友 |

---

## ⚠️ 已移除／已棄用

| 指令 | 狀態 | 改用 |
|---|---|---|
| `/vim` | 已移除 | `/config` → Editor mode |
| `/pr-comments` | 已移除 | 直接請 Claude 看 PR 評論 |

---

## 🧩 易混淆指令對照表（考試重點）

| 容易混 | 差別 |
|---|---|
| `/clear` vs `/compact` vs `/rewind` | 清空全部 ／ 摘要壓縮 ／ 倒回某一點 |
| `/review` vs `/code-review` | GitHub PR ／ 本機 diff |
| `/model` vs `/effort` | 換模型 ／ 同模型思考多深 |
| `/branch` vs `/fork` | 我自己換方向試 ／ 派背景代理幫我做 |
| `/usage` vs `/usage-credits` | 查用量花費 ／ 設定額度上限 |

---

## 📝 課堂練習

1. **查表練習：** 我想「看看這次對話花了多少錢」，該用哪個指令？「對話太長想壓縮」呢？
2. **情境題：** 你改壞了程式碼，想回到 10 分鐘前的狀態，該用 `/clear` 還是 `/rewind`？為什麼？
3. **分類練習：** 把下列指令分到正確的類別——`/diff`、`/login`、`/compact`、`/security-review`、`/theme`。
4. **實作：** 執行 `/context`，截圖目前的上下文用量；再執行 `/compact`，比較前後差異。

<details>
<summary>👉 點此看參考答案</summary>

1. 花費 → `/usage`；壓縮 → `/compact`。
2. 用 `/rewind`。`/clear` 是「清空開新對話」，會丟掉內容；`/rewind` 才是「倒回先前某一點」（含程式碼）。
3. `/diff`→程式碼檢視；`/login`→帳號；`/compact`→上下文；`/security-review`→程式碼審查；`/theme`→設定偏好。

</details>

---

> 下一課我們深入「技能型指令」如何幫你跑一整套工作流 👉 [第 3 課 技能指令](03-skills.md)
