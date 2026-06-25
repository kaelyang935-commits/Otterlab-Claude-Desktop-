# 第 4 課｜自訂你自己的指令

> 上一課：[第 3 課 技能指令](03-skills.md) ｜ 下一課：[第 5 課 執行環境差異](05-environments.md)

---

## 🎯 學習目標

1. 知道自訂指令的檔案要放哪裡。
2. 看懂並會寫一個指令的 `.md` 檔（含 frontmatter）。
3. 會用參數（`$ARGUMENTS`、`$1`）與動態注入（`` !`指令` ``）。
4. **動手做出你的第一個 `/commit` 指令。**

---

## 1. 檔案放哪裡？

自訂指令就是一個 Markdown 檔，依「適用範圍」放在不同位置：

| 範圍 | 路徑 | 適用 |
|---|---|---|
| **專案** | `.claude/commands/` 或 `.claude/skills/<名稱>/` | 只在這個專案可用 |
| **個人** | `~/.claude/commands/` 或 `~/.claude/skills/<名稱>/` | 你的所有專案都可用 |
| **外掛** | `<外掛>/skills/<名稱>/` | 以 `/外掛名:名稱` 呼叫 |

> 兩種格式都能用：傳統的 `commands/xxx.md`，與較新的 `skills/xxx/SKILL.md`。新格式（Skill）功能較完整，**官方建議優先用 Skill 格式**。

---

## 2. 最簡單的做法（30 秒做出一個指令）

在專案裡建立 `.claude/commands/refactor.md`，內容直接寫指示：

```markdown
請把選取的程式碼重構，著重可讀性與可維護性，遵守 clean code 原則。
```

**完成！** 檔名 `refactor`（去掉 `.md`）就成了指令 `/refactor`。

---

## 3. 進階格式：Skill（含設定）

建立 `.claude/skills/commit/SKILL.md`：

```markdown
---
description: 暫存並提交目前變更
argument-hint: [訊息]
allowed-tools: Bash(git add *) Bash(git commit *) Bash(git status *)
disable-model-invocation: true
---

## 目前狀態
!`git status --short`

## 變更內容
!`git diff HEAD`

## 你的任務
依上面的變更，建立一個語意清楚的 git commit，
說明「改了什麼、為什麼改」。
```

呼叫：`/commit`

---

## 4. Frontmatter 設定欄位（`---` 之間那段）

所有欄位皆為選填：

| 欄位 | 作用 |
|---|---|
| `name` | 顯示名稱（預設用資料夾名） |
| `description` | 這個指令做什麼、何時用。Claude 會用它判斷要不要自動套用 |
| `when_to_use` | 補充「何時使用」的說明 |
| `argument-hint` | 自動完成時顯示的參數提示，如 `[issue-number]` |
| `arguments` | 宣告具名參數，供 `$name` 代入 |
| `disable-model-invocation` | 設 `true` = 只能手動 `/` 觸發，Claude 不會自動用 |
| `user-invocable` | 設 `false` = 從 `/` 選單隱藏（純背景知識用） |
| `allowed-tools` | 此指令執行時「免詢問」可用的工具 |
| `disallowed-tools` | 此指令執行時禁用的工具 |
| `model` | 此指令使用的模型（同 `/model` 的值，或 `inherit`） |
| `effort` | 運算強度：`low`/`medium`/`high`/`xhigh`/`max` |
| `paths` | 用 glob 限制此指令只在特定檔案情境啟用 |

---

## 5. 參數：把使用者輸入帶進指令

| 變數 | 意義 |
|---|---|
| `$ARGUMENTS` | 使用者輸入的「全部」參數 |
| `$0`、`$1` … | 第 1 個、第 2 個…參數（0 起算） |
| `$ARGUMENTS[0]` | 同 `$0`，依索引取參數 |
| `$name` | 在 frontmatter 用 `arguments:` 宣告的具名參數 |
| `${CLAUDE_SESSION_ID}` | 目前階段 ID |
| `${CLAUDE_SKILL_DIR}` | 此 SKILL.md 所在資料夾 |

**範例** — `.claude/commands/fix-issue.md`：

```markdown
---
argument-hint: [issue-number] [priority]
description: 修復一個 GitHub issue
---

修復 GitHub issue #$0，優先級 $1。
請先看 issue 描述，再實作必要的修改。
```

呼叫：`/fix-issue 123 high`
→ `$0` = `123`，`$1` = `high`

---

## 6. 動態注入：先跑指令，再把結果塞進去

`` !`<指令>` `` 語法會在「送給 Claude 之前」先執行 shell，並把**輸出**插入指令內容。

**單行：**
```markdown
目前狀態：!`git status`
最近變更：!`git diff HEAD~1`
```

**多行（用 ```! 圍欄）：**
````markdown
## 環境資訊
```!
node --version
git status --short
```
````

> ⚠️ 重點：這是「前處理」，**不是** Claude 去執行。輸出只插入一次，且不會被再次掃描。

---

## 7. 🛠️ 動手做：你的第一個 `/standup` 指令

> 目標：做一個指令，自動整理今天的 git 進度，產生「每日站會報告」。

**步驟 1：** 建立檔案 `.claude/commands/standup.md`

**步驟 2：** 貼上以下內容
```markdown
---
description: 產生今日的每日站會（standup）進度報告
allowed-tools: Bash(git log *) Bash(git diff *)
---

## 今天的提交紀錄
!`git log --since="06:00" --oneline --author="$(git config user.name)"`

## 你的任務
根據上面的提交，用三點條列產生一份站會報告：
1. 昨天／今天完成了什麼
2. 今天接下來要做什麼
3. 有沒有卡住的地方（blocker）
用繁體中文、精簡、適合在會議口頭講。
```

**步驟 3：** 在輸入框執行 `/standup`，看它幫你生出報告。

---

## 📝 課堂練習

1. **改寫：** 把上面的 `/standup` 改成「英文輸出」並「只列今天的 commit」。
2. **參數練習：** 寫一個 `/translate` 指令，讓 `/translate 日文 你好嗎` 能把第二個參數翻成第一個參數指定的語言。（提示：用 `$0`、`$ARGUMENTS`）
3. **思考題：** 什麼情況下你會把 `disable-model-invocation` 設成 `true`？
4. **團隊題：** 你想讓「整個團隊」都能用某指令，檔案該放專案的 `.claude/` 還是個人的 `~/.claude/`？

<details>
<summary>👉 點此看參考答案</summary>

3. 當這是一個「破壞性或需謹慎執行」的流程（如部署、刪檔），你希望它**只在你手動輸入 `/` 時才跑**，而不要被 Claude 自動判斷觸發時，就設 `true`。
4. 放 **專案的 `.claude/commands/`** 並隨專案進版控（git），團隊成員 clone 後就都有了。個人的 `~/.claude/` 只有你自己的機器有。

</details>

---

## 🔑 重點回顧

- 自訂指令 = 一個 `.md`／`SKILL.md` 檔；檔名／資料夾名就是指令名。
- frontmatter 控制行為（`description`、`allowed-tools`、`disable-model-invocation`…）。
- `$ARGUMENTS`／`$0` 取參數；`` !`指令` `` 先跑 shell 再注入結果。

---

> 下一課看看不同環境（桌面／終端／網頁）的差別 👉 [第 5 課 執行環境差異](05-environments.md)
