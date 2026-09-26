# MIS572 助教課：從 Git 到 AI 協作開發（詳細內容）

> 課程大綱見 `syllabus.md`，本檔記錄每一段的細部內容與講法。

> **主線：想用 AI 寫 code，先學會管理 AI 做的每一個改動。**

- **時間**：14:10–16:00，共 110 分鐘（含 10 分鐘休息）
- **對象**：資管系大三、碩一、碩二，少數外系同學
- **形式**：實體教室 + Zoom 同步，以螢幕分享與投影片為主，助教示範、學生可選擇跟著做
- **學習目標**
  1. 理解版本控制的用途，分得清 Git 跟 GitHub / GitLab
  2. 看得懂 commit、branch、merge、PR 的流程
  3. 知道常見的分支策略，以及自己的專案該用哪一種
  4. 認識主要的 coding agent，掌握用 git 管理 AI 改動的協作方式

---

## 時間總覽

| 時間 | 段落 | 形式 |
|---|---|---|
| 14:10–14:20 | 一、為什麼需要版本控制 + 版控的歷史 | 投影片 |
| 14:20–14:45 | 二、Git 核心概念與常用指令 | 投影片 + CLI + VS Code Git Graph |
| 14:45–15:00 | 三、Branch 與 Merge | CLI + Git Graph 示範 + 互動提問 |
| 15:00–15:10 | 休息 | |
| 15:10–15:28 | 四、遠端協作：GitHub 與 PR | GitHub 網頁示範 |
| 15:28–15:35 | 五、分支策略 | 投影片 |
| 15:35–15:55 | 六、Coding Agent 與 AI 協作 | 投影片 + live demo |
| 15:55–16:00 | 七、總結、作業、Q&A | |

Git 相關內容（一到五）約 75 分鐘，Coding Agent 約 20 分鐘。

---

## 一、為什麼需要版本控制 + 版控的歷史（14:10–14:20，10 分鐘）

### 1.1 開場例子（約 5 分鐘）

- **開場例子**：一個資料夾裡有
  - `報告.docx`
  - `報告_v2.docx`
  - `報告_v10_3_final.docx`
  - `報告_v10_3_final_last.docx`
  - `報告_v10_3_final_last_真的不改了.docx`
- 拋出問題：
  - 哪一個才是最新版？
  - v3 跟 v7 差在哪裡？
  - 組員也各自改了一份，要怎麼合併？
  - 想救回三天前刪掉的那段，要去哪裡找？
- **帶出結論**：版本控制就是解決這些問題的工具

### 1.2 在 Git 之前，大家怎麼工作？（約 5 分鐘）
- **手動時代**：複製資料夾、檔名加日期、用 email 或隨身碟傳檔案、用 diff / patch 交換修改
- **本地版控**：RCS（1982）只能管單一檔案、單一台電腦
- **集中式版控**：CVS（1990）、SVN（2000）有一台中央伺服器，大家從同一個地方 checkout / commit
  - 缺點：伺服器掛了大家都不能工作、沒網路就不能 commit、開 branch 很麻煩
- **分散式版控與 Git 誕生**：Linux 核心原本用商業工具 BitKeeper，2005 年授權被收回，Linus Torvalds 花大約兩週寫出 Git
  - 設計目標：速度快、完全分散、方便大量平行開發（branch 很便宜）
- **平台時代**：GitHub（2008）、GitLab（2011）讓 Git 變成協作平台；2018 年微軟收購 GitHub
- **現在**：Git 是業界標準，AI 工具也建立在 Git 之上（伏筆，接到第六段）
- 預告今天的路線：Git 基礎 → 團隊協作 → AI 協作

## 二、Git 核心概念與常用指令（14:20–14:45，25 分鐘）

講解順序：先講 big picture，再釐清 Git 跟 GitHub 的差別，接著講檔案的狀態，最後才進指令。

### 2.1 Big picture：Git 能幫你做什麼（約 4 分鐘）
- **Git 是版本控制工具**，核心功能有三個：
  - **記錄版本**：每次 commit 等於替整個專案拍一張快照（snapshot），附上時間、作者、說明
  - **差異比對**：任兩個版本之間改了哪幾行，一目了然
  - **回復版本**：改壞了可以回到任何一個之前的版本
- 延伸：分散式（每個人手上都有完整歷史）、支援多人同時開發
- 不只能管程式碼，任何文字檔都能管（論文 LaTeX、Markdown 筆記、設定檔）

### 2.2 那大家常聽到的 GitHub、GitLab 又是什麼？（約 4 分鐘）
- **其實是完全不同的東西**
| | 是什麼 | 比喻 |
|---|---|---|
| **Git** | 裝在自己電腦上的版控工具 | 相機 |
| **GitHub / GitLab / Bitbucket** | 放 repo 的雲端平台，外加 PR、Issue、CI/CD 等協作功能 | 雲端相簿 |

- 不用 GitHub 也可以用 Git；GitHub 則是建立在 Git 之上的服務
- GitLab 可以架在公司內部（self-hosted），很多企業會這樣用

### 2.3 一個檔案的旅程：它現在在哪裡？（約 7 分鐘）

> 初學者最容易卡住的地方，值得多花一點時間。（可以分享自己第一次學 git 在這裡卡很久的經驗）

```
【本機電腦】
  Working Directory ──add──> Staging Area ──commit──> Local Repo
  (未追蹤 / 已修改)          (準備提交)               (本機歷史)
                                                          │  ▲
                                                     push │  │ pull
                                                          ▼  │
【雲端】                                              Remote Repo
                                                    (GitHub / GitLab)
```

| 狀態 | 意思 | `git status` 會看到 |
|---|---|---|
| **未追蹤（untracked）** | 新檔案，Git 還不認識它 | `Untracked files` |
| **已修改（modified）** | Git 認識的檔案，有新的改動還沒 add | `Changes not staged for commit` |
| **已暫存（staged）** | 已經 add，下次 commit 會包含它 | `Changes to be committed` |
| **已提交（committed）** | 存進**本機**的歷史紀錄 | `nothing to commit, working tree clean` |
| **已推送（pushed）** | 同步到**雲端**的 remote repo | `Your branch is up to date with 'origin/main'` |

- **三個常見誤會，要特別講清楚**：
  1. **commit ≠ 上傳**：commit 只存在自己的電腦，要 push 之後 GitHub 上才看得到
  2. **新檔案不會自動被追蹤**：建立新檔案後沒有 `git add`，commit 就不會包含它
  3. **改完不 add 就 commit，改動不會進去**：staging 決定這次 commit 要放哪些東西
- 比喻：staging 就像拍團體照前先喊「要入鏡的人站過來」，commit 是按下快門，push 是把照片上傳到雲端相簿
- 教大家養成習慣：**不確定現在狀態時，就打 `git status`**

### 2.4 常用指令：CLI + VS Code Git Graph 對照（約 10 分鐘）

每下一個指令，就切到 VS Code 看 Source Control 面板和 Git Graph 的變化，讓同學把指令跟畫面連起來。

| 指令 | 作用 | 在 VS Code 看到的變化 |
|---|---|---|
| `git init` | 建立 repo | 出現 Source Control 面板 |
| `git status` | 看目前狀態（最常用） | 對照面板上的 U / M / A 標記 |
| `git add <file>` | 放進 staging | 檔案移到 Staged Changes |
| `git commit -m "訊息"` | 存成一個版本 | Git Graph 多一個節點 |
| `git log --oneline` | 看歷史 | 跟 Git Graph 的節點一一對應 |
| `git diff` | 看改了什麼 | 點檔案出現左右對照的 diff |
| `git restore <file>` | 放棄尚未 commit 的修改 | 改動消失，回到上一版 |

- 示範流程：建新檔（U）→ add（A）→ commit → 改檔（M）→ diff → commit → 看 Git Graph 長出一串節點 → 故意改壞 → restore
- 順帶介紹好的 commit message：說明「做了什麼、為什麼」，不要寫 `update`、`fix`、`asdf`
- Git Graph：可用 VS Code 擴充套件 **Git Graph**，或 VS Code 內建的 Source Control Graph

## 三、Branch 與 Merge（14:45–15:00，15 分鐘）

### 3.1 Branch 的概念
- branch 就是一個指向某個 commit 的**指標**，建立的成本很低
- 用途：在不影響主線的情況下開發新功能、做實驗
- 搭配 Git Graph 看 branch 分岔、合併的樣子；也可以用 **Learn Git Branching**（learngitbranching.js.org）輔助說明

### 3.2 示範
```bash
git switch -c feature/login   # 建立並切換到新 branch
# ... 修改、commit ...
git switch main
git merge feature/login
```

### 3.3 Merge conflict
- 示範：兩個 branch 改到同一行 → merge → 出現 conflict
- 解讀 conflict 標記：
  ```
  <<<<<<< HEAD
  main 上的版本
  =======
  feature 上的版本
  >>>>>>> feature/login
  ```
- 用 VS Code 的 conflict 介面解決（Accept Current / Incoming / Both）
- **互動**：示範前先讓大家猜「這樣 merge 會不會衝突？」，現場舉手，Zoom 的同學打在聊天室

### 3.4 只提一句，不展開
- `rebase`、`cherry-pick`、`stash`、`reset`：以後會遇到，今天先知道有這些東西就好

## 休息（15:00–15:10，10 分鐘）

## 四、遠端協作：GitHub 與 PR（15:10–15:28，18 分鐘）

### 4.1 遠端操作（接續 2.3 的「本機 vs 雲端」概念）
```bash
git clone <url>     # 把遠端 repo 複製下來
git pull            # 抓遠端的更新
git push            # 推上自己的 commit
```
- 登入建議用 `gh auth login` 或 GitHub Desktop（GitHub 已經不能用密碼 push）

### 4.2 Pull Request
- PR 是什麼：「我改好了，請幫我看看能不能合進 main」
- PR 是平台提供的功能，不是 Git 本身的功能（GitLab 叫 Merge Request）
- 示範完整流程：
  1. 開 branch、commit、push
  2. 在 GitHub 上開 PR，寫清楚描述
  3. Review：逐行留言、Request changes / Approve
  4. 修改後再 push，PR 會自動更新
  5. Merge，刪除 branch
- 簡單提到：Issue 串 PR（`Closes #12`）、CI 自動檢查

### 4.3 為什麼要 code review
- 抓 bug、分享知識、維持程式碼品質
- **伏筆**：之後 AI 交給你的也是 PR，到時候你就是 reviewer

## 五、分支策略（15:28–15:35，7 分鐘）

| 策略 | 分支結構 | 重點 | 適合 |
|---|---|---|---|
| **GitHub Flow** | main + 短期 feature branch | 所有改動都走 PR | 課堂專案、小團隊、持續部署 |
| **Git Flow** | main、develop、feature、release、hotfix | 管理版本發布 | 有固定發版週期的產品 |
| **環境分支** | dev → staging → prod | 一個 branch 對應一個部署環境 | 需要分階段上線的系統 |
| **Trunk-based** | 大家頻繁合回 main，搭配 feature flag | 減少長期分支和大型衝突 | 大型團隊、成熟的 CI/CD |

- **釐清常見誤解**：「main / dev / prod」是環境分支，跟 Git Flow 不一樣
- **結論**：課堂專案用 GitHub Flow 就夠了

## 六、Coding Agent 與 AI 協作（15:35–15:55，20 分鐘）

### 6.1 Coding agent 是什麼
- 不只是補完程式碼：能讀整個專案、執行指令、修改多個檔案、跑測試
- 正因為一次可能改很多檔案，**git 變成必備的安全網**

### 6.2 工具分類
| 類型 | 代表工具 | 特色 |
|---|---|---|
| **IDE 整合** | Cursor、GitHub Copilot、Windsurf | 在編輯器裡邊看邊改 |
| **終端機 CLI** | Claude Code、OpenAI Codex CLI、Gemini CLI、**OpenCode** | 直接在 repo 裡讀檔、跑指令、改檔 |
| **雲端非同步** | Copilot coding agent、Codex cloud、Claude Code on the web | 丟任務出去，做完交回一個 PR |

- **OpenCode**：開源的終端機 coding agent，不綁定特定模型廠商，可以接 Claude、GPT、Gemini，也能接本地模型
  - 可以帶出另一個比較角度：**閉源 vs 開源、綁定自家模型 vs 自由切換模型**
- 補一句：工具更新很快，知道怎麼分類比記住產品名稱重要
- （上課前再確認一次各家最新的名稱、功能與價格）

### 6.3 AI 協作守則
1. **讓 agent 動手前先 commit**：做壞了隨時可以退回
2. **一個任務開一個 branch**：不要讓 agent 直接改 main
3. **一定要自己看 diff**：沒看就合，等於讓陌生人直接 push 到你的專案
4. **任務切小**：小任務比較好 review，也比較好撤銷
5. **寫專案說明檔**（`CLAUDE.md`、`AGENTS.md` 等）：把專案規則告訴 agent
6. **管好 `.gitignore` 和 secrets**：API key 不能進 repo，也要注意 agent 會不會不小心把它加進去
7. 進階：`git worktree` 可以讓多個 agent 同時處理不同任務

### 6.4 Live demo
1. 開一個新 branch
2. 請 agent 加一個小功能
3. `git diff` 檢查改了什麼
4. 挑一處不滿意的地方請它修改
5. commit → push → 開 PR
- 整個流程就是把前半段教的東西再走一次

## 七、總結、作業、Q&A（15:55–16:00，5 分鐘）

### 回顧
- Git 是版控工具，GitHub / GitLab 是協作平台
- commit 存版本，branch 做隔離，PR 做審查
- 用 AI 寫 code 時，git 是讓你敢放手的安全網

### 作業（待與老師確認）
> 用任一 coding agent，在自己的 branch 完成一個小功能，對課程 repo 發一個 PR，並在 PR 描述中說明：你 review 時發現了什麼、改了哪些地方。

### 延伸資源
- Learn Git Branching：https://learngitbranching.js.org
- Pro Git（免費電子書，有中文版）：https://git-scm.com/book/zh-tw/v2
- GitHub Skills：https://skills.github.com

---

## 課前準備

### 寄給學生（課前 3–5 天）
- [ ] 安裝 Git，確認 `git --version` 有結果
- [ ] 註冊 GitHub 帳號
- [ ] 設定身分：`git config --global user.name "..."`、`git config --global user.email "..."`
- [ ] 安裝 VS Code，以及 **Git Graph** 擴充套件
- [ ] （選填）簡短問卷：用過 git 嗎？用過哪些 AI 寫程式工具？

### 助教自己準備
- [ ] 投影片
- [ ] 示範用 repo，先埋好會衝突的兩個 branch
- [ ] VS Code 裝好 Git Graph，確認分享畫面時看得清楚
- [ ] demo 用的 coding agent 先登入、測過一輪
- [ ] 終端機與 VS Code 字體放大（Zoom 畫質會壓縮，建議 20pt 以上）
- [ ] 終端機用高對比配色
- [ ] 關掉通知，避免分享畫面時跳出訊息

### 混合授課注意事項
- 請一位同學或共同主持人幫忙看 Zoom 聊天室
- 提問時兩邊都要照顧到：現場舉手、Zoom 打字或用 reaction
- 講話時記得對著麥克風，重複現場同學的提問，讓線上同學聽得到
- 確認是否錄影，方便課後複習
- live demo 準備錄好的備用影片或截圖，以防網路或 API 出狀況
