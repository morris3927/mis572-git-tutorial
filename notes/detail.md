# MIS572 助教課：從 Git 到 AI 協作開發（詳細內容）

> 課程大綱見 `syllabus.md`，本檔記錄每一段的細部內容、示範步驟與講法。
>
> **主線：想用 AI 寫 code，先學會管理 AI 做的每一個改動。**

- **時間**：14:10–16:00（含 10 分鐘休息）
- **形式**：實體教室 + Zoom 同步，螢幕分享與投影片
- **對象**：資管系大三、碩一、碩二，少數外系同學
- **示範方式**：投影片講概念；操作一律在 VS Code 的 terminal 下指令，旁邊開 Git Graph 看變化
- **課堂範例**：單頁 HTML 網頁 + GitHub Pages（本 repo 根目錄的 `index.html`），push 之後網站自動更新
- **課前**：上課前一天把投影片與範例上傳到課程網站，讓學生上課時下載
- **學習目標**
  1. 知道版本控制在解決什麼問題，以及 Git 的由來
  2. 分得清 Git、託管平台（GitHub / GitLab）與 GUI 工具
  3. 會用 commit、branch、merge、PR 完成日常開發
  4. 知道發生錯誤時怎麼回復（restore / reset / revert）
  5. 認識主流 coding agent 與其差異，會用 git 管理 AI 的改動

---

## 1. 版本控制與 Git 的由來

### 1.1 開場例子
- 投影片放一個資料夾截圖：
  - `報告.docx`
  - `報告_v2.docx`
  - `報告_v10_3_final.docx`
  - `報告_v10_3_final_last.docx`
  - `報告_v10_3_final_last_真的不改了.docx`
- 拋出四個問題：
  1. 哪一個才是最新版？
  2. v3 跟 v7 差在哪裡？
  3. 組員也各自改了一份，要怎麼合併？
  4. 想救回三天前刪掉的那段，要去哪裡找？
- 結論：**版本控制（Version Control System, VCS）就是解決這些問題的工具**

### 1.2 Git 之前怎麼工作
| 時代 | 代表 | 做法 | 限制 |
|---|---|---|---|
| 人工 | 複製資料夾、email、隨身碟、`diff` / `patch` | 手動改檔名、手動合併 | 容易蓋掉別人的東西，沒有完整歷史 |
| 本地版控 | SCCS（1972）、**RCS**（1982） | 在自己電腦上記錄單一檔案的版本 | 只能單機、一次管一個檔案，無法協作 |
| 集中式 | **CVS**（1990 年前後）、**SVN**（2000） | 一台中央伺服器存所有歷史，大家從伺服器 checkout / commit | 伺服器掛了大家都不能工作；沒網路不能 commit；開 branch、merge 很痛苦 |
| 分散式 | BitKeeper、Monotone、**Git**、Mercurial | 每個人都有完整的 repo 與歷史 | 觀念需要重新學習（今天的重點） |

- 投影片建議畫兩張圖：集中式是「一台伺服器、多個用戶端」；分散式是「每台電腦都有完整 repo，再跟遠端同步」

### 1.3 Git 的誕生
- 2002 年起，Linux 核心開始用商業的分散式版控 **BitKeeper**（免費授權給開源社群）
- 2005 年因為授權爭議，BitKeeper 收回免費授權
- **Linus Torvalds** 參考 BitKeeper 與 **Monotone** 的使用經驗，自己設計 Git：
  - 2005 年 4 月開始寫，幾天內 Git 就能管理自己的原始碼
  - 同年 6 月，Linux 核心正式用 Git 管理
  - 7 月交給 **濱野純（Junio Hamano）** 維護，他到現在仍是 Git 的主要維護者
- 設計目標：**速度快、完全分散、支援大量平行開發（branch 很便宜）、資料完整不可竄改**（每個 commit 用雜湊值識別）
- 後續發展：
  - 2008 年 GitHub 上線，2011 年 GitLab 出現
  - 2018 年微軟收購 GitHub
  - 2020 年 GitHub 預設分支從 `master` 改名為 `main`
  - 現在 Git 是業界標準，AI coding agent 也都建立在 Git 之上（伏筆，接到第 7 章）

---

## 2. Git 生態系與環境準備

### 2.1 Git 能做什麼（Big picture）
- **記錄版本**：每次 commit 等於替整個專案拍一張快照，附上時間、作者、說明
- **差異比對**：任兩個版本之間改了哪幾行，一目了然
- **回復版本**：改壞了可以回到任何一個之前的版本
- 延伸：分散式架構讓每個人都能獨立工作，再把成果合併起來
- 不只能管程式碼，任何文字檔都能管：論文 LaTeX、Markdown 筆記、設定檔

### 2.2 Git 與周邊工具
**Git 是核心，其他工具都建立在它之上。**

| 類型 | 代表 | 做什麼 | 比喻 |
|---|---|---|---|
| **版控工具** | Git | 在自己電腦上記錄版本 | 相機 |
| **託管平台** | GitHub、GitLab、Bitbucket、Gitea | 放 repo 的雲端空間，外加 PR、Issue、CI/CD 等協作功能 | 雲端相簿 |
| **GUI 用戶端** | Sourcetree、GitHub Desktop、GitKraken、VS Code | 用圖形介面操作 Git，背後還是在下 Git 指令 | 相機的觸控螢幕 |

- 不用 GitHub 也可以用 Git；Sourcetree 也不是 GitHub 的替代品，兩者是不同類的東西
- 託管平台補充：
  - GitHub：最大的開源社群，現在屬於微軟
  - GitLab：可以架在公司內部（self-hosted），很多企業會這樣用，內建 CI/CD
  - Bitbucket：Atlassian 旗下，常跟 Jira 一起用
- **今天的選擇**：Git（CLI）+ VS Code + GitHub

### 2.3 安裝 Git
| 系統 | 安裝方式 |
|---|---|
| **Windows** | 到 git-scm.com 下載 Git for Windows（內含 Git Bash 與 Git Credential Manager），或 `winget install --id Git.Git -e` |
| **macOS** | `xcode-select --install`（安裝 Apple 開發者工具時會附帶 Git），或用 Homebrew：`brew install git` |
| **Linux** | Ubuntu / Debian：`sudo apt install git`；Fedora：`sudo dnf install git` |

- 安裝完確認：`git --version`
- 提醒 Windows 同學：安裝過程的選項大多用預設值就好；預設編輯器可以改選 VS Code

### 2.4 VS Code 與 Git Graph
- VS Code 內建 Source Control 面板（左側分岔圖示），可以看改動、stage、commit、push
- 安裝擴充套件 **Git Graph**（作者 mhutchie），用圖形方式看 commit 歷史與 branch
  - 開啟方式：Source Control 面板上方的 Git Graph 按鈕，或 Command Palette 輸入 `Git Graph: View Git Graph`
- 示範畫面配置：左邊編輯器、下方 terminal、右邊 Git Graph。**每下一個指令就看一次 Git Graph 的變化**

### 2.5 初始設定
**必做：設定身分**（每個 commit 都會記錄作者）
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"   # 建議跟 GitHub 帳號用同一個 email
```

**建議：**
```bash
git config --global init.defaultBranch main        # 新 repo 預設分支叫 main
git config --global core.editor "code --wait"      # 用 VS Code 當預設編輯器
git config --list                                  # 檢查設定
```

**連到 GitHub 的認證**（GitHub 從 2021 年起不能用帳號密碼 push）

| 方式 | 做法 | 適合 |
|---|---|---|
| **HTTPS + 瀏覽器登入**（推薦） | Windows 內建 Git Credential Manager，第一次 push 會跳瀏覽器登入；macOS / Linux 可用 GitHub CLI 執行 `gh auth login` 完成設定 | 初學者、今天的課 |
| **SSH key** | 產生一組金鑰，把公鑰放到 GitHub，之後用 SSH 網址連線（步驟見下方） | 常用 Git 的人，設定一次之後很方便 |

**SSH key 設定步驟（投影片要放）：**
1. 產生金鑰（一路按 Enter 用預設值即可，也可以設定密碼）：
   ```bash
   ssh-keygen -t ed25519 -C "you@example.com"
   ```
   會產生兩個檔案：私鑰 `~/.ssh/id_ed25519`（**絕對不能給別人**）和公鑰 `~/.ssh/id_ed25519.pub`
2. 複製公鑰內容：
   ```bash
   cat ~/.ssh/id_ed25519.pub          # macOS / Linux / Git Bash
   ```
3. GitHub 右上角頭像 → Settings → SSH and GPG keys → New SSH key → 貼上公鑰
4. 測試連線，看到 `Hi <帳號>! You've successfully authenticated` 就成功：
   ```bash
   ssh -T git@github.com
   ```
5. 之後 clone 改用 SSH 網址：`git@github.com:<帳號>/<repo>.git`；已經用 HTTPS clone 的 repo 可以改遠端網址：
   ```bash
   git remote set-url origin git@github.com:<帳號>/<repo>.git
   ```


---

## 3. 建立 Repo 與基本操作

### 3.1 課堂範例介紹
- 範例：就是本 repo `mis572-git-tutorial` 根目錄的 `index.html`，一個很陽春的課程頁（標題加幾個 row / col 區塊），上課時直接修改它
- 直接在 main 上示範，最後幾個 commit 就是課堂上對 `index.html` 的修改
- 放在 GitHub 上，開啟 **GitHub Pages**：repo 的 Settings → Pages → Source 選 `Deploy from a branch`，branch 選 `main`、資料夾選 `/ (root)`
- 網址：`https://morris3927.github.io/mis572-git-tutorial/`
- 先展示網站，讓大家知道等一下每次 push 後，網站大約一分鐘內就會更新
- 注意：免費帳號的 GitHub Pages 需要 **public repo**

### 3.2 建立 repo 的兩種情境

**情境一：從零開始（先在 GitHub 開 repo，再 clone）**
1. GitHub 右上角 `+` → New repository → 填名稱、選 Public、勾選 Add a README
2. 複製 repo 網址
3. 在 terminal：
   ```bash
   git clone https://github.com/<帳號>/<repo>.git
   cd <repo>
   code .
   ```
- 優點：遠端已經設定好，最不容易出錯

**情境二：現有專案接上 git**
1. 在 GitHub 開一個**空的** repo（不要勾 README，避免跟本機歷史衝突）
2. 在專案資料夾：
   ```bash
   git init
   # 先建立 .gitignore（見 3.5），再進行第一次 commit
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/<帳號>/<repo>.git
   git push -u origin main
   ```
- 說明 `origin` 只是遠端的預設名稱；`-u` 會設定追蹤關係，之後直接 `git push` 就好
- Git Graph 對照：`init` 後沒有節點 → 第一次 commit 出現第一個節點 → push 後出現 `origin/main` 標籤

### 3.3 一個檔案的旅程

> 初學者最容易卡住的地方，值得多花一點時間。（可以分享自己第一次學 git 卡在這裡的經驗）

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

| 狀態 | 意思 | `git status` 會看到 | VS Code 標記 |
|---|---|---|---|
| **未追蹤（untracked）** | 新檔案，Git 還不認識它 | `Untracked files` | U |
| **已修改（modified）** | Git 認識的檔案，有新的改動還沒 add | `Changes not staged for commit` | M |
| **已暫存（staged）** | 已經 add，下次 commit 會包含它 | `Changes to be committed` | A（新檔）/ M |
| **已提交（committed）** | 存進**本機**的歷史紀錄 | `nothing to commit, working tree clean` | 標記消失 |
| **已推送（pushed）** | 同步到**雲端** | `Your branch is up to date with 'origin/main'` | 同步按鈕沒有數字 |

**三個常見誤會，要特別講清楚：**
1. **commit ≠ 上傳**：commit 只存在自己的電腦，要 push 之後 GitHub 上才看得到
2. **新檔案不會自動被追蹤**：建立新檔案後沒有 `git add`，commit 就不會包含它
3. **改完不 add 就 commit，改動不會進去**：staging 決定這次 commit 要放哪些東西

- 比喻：staging 就像拍團體照前先喊「要入鏡的人站過來」，commit 是按下快門，push 是把照片上傳到雲端相簿
- 養成習慣：**不確定現在是什麼狀態，就打 `git status`**

### 3.4 基本操作：CLI 與 VS Code 對照
| 指令 | 作用 | VS Code GUI 做法 |
|---|---|---|
| `git status` | 看目前狀態 | Source Control 面板上的檔案清單 |
| `git add <file>` / `git add .` | 放進 staging | 檔案旁邊的 `+` |
| `git diff` | 看尚未 stage 的改動 | 點檔案，出現左右對照 |
| `git diff --staged` | 看已經 stage 的改動 | 點 Staged Changes 裡的檔案 |
| `git commit -m "訊息"` | 存成一個版本 | 上方輸入訊息 → Commit |
| `git push` | 推到遠端 | Sync Changes / Push |
| `git pull` | 抓遠端更新 | Sync Changes / Pull |

**示範流程（課堂範例網頁）：**
1. 修改 `index.html` 的標題文字 → `git status` 看到 M
2. `git diff` 看改了哪一行，同時在 VS Code 點檔案看左右對照
3. 把 `#row1` 的 `background` 從 `transparent` 改成 `lightblue` → 同一個檔案多了一處改動
4. `git add index.html` → `git status` 看到檔案進了 staging
5. `git commit -m "Update title and row1 background"` → Git Graph 多一個節點，`main` 超前 `origin/main`
6. `git push` → `origin/main` 跟上 → 打開網站，等一分鐘後重新整理看到更新
7. 改 `#row2` 的背景色，這次全部用 VS Code 的按鈕完成，讓大家看到 GUI 跟 CLI 做的是同一件事

**好的 commit message：**
- 說明「做了什麼、為什麼」，第一行簡短（50 字元內）
- 不好的例子：`update`、`fix`、`asdf`、`改一下`
- 好的例子：`Fix broken link in navigation bar`、`新增聯絡資訊區塊`
- 可以提 Conventional Commits 格式：`feat: ...`、`fix: ...`、`docs: ...`

### 3.5 .gitignore
- 用途：告訴 Git 哪些檔案**永遠不要追蹤**
- 常見要排除的東西：

  ```gitignore
  # 系統產生的檔案
  .DS_Store
  Thumbs.db

  # 套件與建置產物
  node_modules/
  __pycache__/
  dist/

  # 機密資訊
  .env
  *.pem

  # 編輯器設定
  .vscode/
  .idea/
  ```
- 示範：建立 `.env` 放一個假的 API key → `git status` 看到它 → 加進 `.gitignore` → 再 `git status`，它消失了
- **重點提醒**：
  - `.gitignore` 最好在第一次 commit 前就建好
  - 已經被追蹤的檔案，加進 `.gitignore` 也沒用，要先 `git rm --cached <file>`
  - API key 一旦 push 到 public repo，就算事後刪掉也要當作已外洩，立刻作廢重發
- 資源：GitHub 的 gitignore 範本（github.com/github/gitignore），或建 repo 時直接選範本

---

## 4. 查看歷史與回復版本

### 4.1 查看歷史
```bash
git log                    # 完整歷史
git log --oneline          # 一行一個 commit
git log --oneline --graph --all   # 用文字畫出分支圖
git show <commit>          # 看某個 commit 改了什麼
```
- 對照 Git Graph：每個節點就是一個 commit，點下去可以看改了哪些檔案
- 說明 commit hash（例如 `3e87281`）是每個版本的身分證字號

### 4.2 還沒 commit 的改動
| 情況 | 指令 | VS Code |
|---|---|---|
| 改壞了，想放棄修改 | `git restore <file>` | 檔案旁邊的 Discard Changes（↶） |
| add 錯了，想移出 staging | `git restore --staged <file>` | Staged Changes 裡的 `−` |

- 示範：把網頁改壞 → `git restore index.html` → 回到上一次 commit 的樣子
- 提醒：restore 放棄的修改**救不回來**，因為它從來沒被 commit 過

### 4.3 已經 commit 的改動

**`git reset`：移動 branch 指標，改寫歷史**
| 模式 | commit 紀錄 | staging | 檔案內容 |
|---|---|---|---|
| `--soft` | 退回 | 保留 | 保留 |
| `--mixed`（預設） | 退回 | 清空 | 保留 |
| `--hard` | 退回 | 清空 | **一起退回**（改動會消失） |

```bash
git reset --soft HEAD~1    # 取消上一個 commit，改動留在 staging（常用：commit 訊息打錯、少加檔案）
git reset --hard HEAD~1    # 整個退回上一版，小心使用
```

**`git revert`：新增一個反向 commit，不改寫歷史**
```bash
git revert <commit>        # 產生一個新 commit，把指定 commit 的改動抵銷掉
```

**怎麼選：**
| 情況 | 用什麼 | 原因 |
|---|---|---|
| 還沒 push，只在自己電腦 | `reset` | 改寫歷史沒關係，沒人受影響 |
| 已經 push，別人可能拉過了 | `revert` | 不改寫歷史，大家的紀錄保持一致 |

- 示範情境：push 了一個把網站改壞的 commit → `git revert` → push → 網站恢復，而且歷史上看得到「壞掉 → 修回來」的紀錄
- **救命指令 `git reflog`**：記錄 HEAD 移動過的每一步。就算 `reset --hard` 退過頭，也能找回原本的 commit：
  ```bash
  git reflog                 # 找到要回去的那一步
  git reset --hard <hash>
  ```
- 補充：`git commit --amend` 可以修改最後一個 commit（訊息或內容），同樣只適合還沒 push 的情況

---

## 5. Branch 與 Merge

### 5.1 Branch 的概念
- branch 只是一個**指向某個 commit 的指標**，建立幾乎不花成本
- `HEAD` 表示「你現在在哪裡」
- 用途：在不影響 main 的情況下開發新功能或做實驗

```bash
git branch                     # 列出所有 branch
git switch -c feature/footer   # 建立並切換到新 branch
git switch main                # 切回 main
git branch -d feature/footer   # 刪除已合併的 branch
```
- 補充：舊教學常用 `git checkout`，新版 Git 把它拆成 `switch`（切 branch）和 `restore`（還原檔案），比較不容易混淆
- 示範：在 `feature/footer` 替網頁加上頁尾並 commit → 切回 main，頁尾不見了 → 再切過去，頁尾又出現。同時看 Git Graph 分出一條線
- 輔助工具：Learn Git Branching（learngitbranching.js.org）可以視覺化說明

### 5.2 正常的 merge
```bash
git switch main
git merge feature/footer
```

| 類型 | 發生時機 | Git Graph 上的樣子 |
|---|---|---|
| **Fast-forward** | main 在開 branch 之後沒有新的 commit | 指標直接往前移，還是一直線 |
| **Three-way merge** | main 和 branch 各自都有新的 commit | 產生一個有兩個 parent 的 merge commit，兩條線匯合 |

- 示範兩次：
  1. 直接 merge `feature/footer` → fast-forward
  2. 開 `feature/nav`，同時在 main 改另一個地方 → merge → 出現 merge commit
- 補充：`git merge --no-ff` 可以強制產生 merge commit，保留「這裡曾經有一個 branch」的紀錄

### 5.3 Merge conflict
- 發生原因：兩個 branch **改到同一個檔案的同一個地方**，Git 不知道要用哪個版本
- 示範：main 和 `feature/row1-color` 都改了 `#row1` 的背景色（一個 `lightblue`、一個 `lightpink`）→ merge → 出現 conflict
- conflict 標記：
  ```
  <<<<<<< HEAD
    background: lightblue;
  =======
    background: lightpink;
  >>>>>>> feature/row1-color
  ```
- 解決步驟：
  1. `git status` 看哪些檔案有衝突
  2. 在 VS Code 選 Accept Current / Accept Incoming / Accept Both，或用 Merge Editor 手動編輯
  3. 刪掉所有衝突標記，確認內容正確
  4. `git add <file>` → `git commit`
- 反悔：`git merge --abort` 回到 merge 之前
- **互動**：示範前先讓大家猜「這樣 merge 會不會衝突？」，現場舉手、Zoom 同學打在聊天室
- 講法：conflict 不是錯誤，只是 Git 在問你「這兩個改動要留哪一個？」

### 5.4 Rebase
- 作用：把自己 branch 上的 commit「搬到」目標 branch 的最新位置，讓歷史變成一直線

```bash
git switch feature/nav
git rebase main
```

| | Merge | Rebase |
|---|---|---|
| 歷史 | 保留真實的分岔與合併 | 整理成一直線，比較乾淨 |
| 會不會改寫歷史 | 不會 | **會**（commit hash 會變） |
| 適合 | 合併到共用的 branch | 在自己的 branch 上跟上 main 的進度 |

- 用 Git Graph 對照 rebase 前後的樣子
- **黃金守則：已經 push 給別人用的 branch，不要 rebase**
- 只提一句：`git rebase -i` 可以合併、修改、重排 commit；`git cherry-pick`、`git stash` 以後會用到

---

## 休息（15:00 前後，10 分鐘）

---

## 6. 團隊協作：PR 與分支策略

### 6.1 Pull Request
- PR 的意思是：「我在 branch 上改好了，請大家看看能不能合進 main」
- PR 是**託管平台的功能**，不是 Git 本身的功能；GitLab 叫 Merge Request（MR）

**示範流程：**
1. 開 branch、修改網頁、commit：
   ```bash
   git switch -c feature/contact
   # 修改 index.html
   git commit -am "Add contact section"
   git push -u origin feature/contact
   ```
2. GitHub 上會出現 **Compare & pull request** 按鈕 → 填標題與描述（改了什麼、為什麼、怎麼測試）
3. **Review**：
   - Files changed 分頁逐行留言
   - 用 Suggest changes 直接提出修改建議
   - Approve / Request changes
4. 根據 review 修改 → 再 commit、push，PR 會自動更新
5. **Merge** 的三種選項：
   - Create a merge commit：保留所有 commit 和合併紀錄
   - Squash and merge：把所有 commit 壓成一個
   - Rebase and merge：把 commit 接在 main 後面成一直線
6. Merge 後刪除 branch → 本機 `git switch main` + `git pull` → **網站自動更新**

**補充：**
- Issue 串 PR：描述裡寫 `Closes #12`，merge 後 Issue 會自動關閉
- Branch protection：設定 main 不能直接 push，一定要經過 PR 與 review
- CI 檢查：PR 可以自動跑測試，沒通過就不能 merge
- **伏筆**：之後 AI 交給你的也是 PR，到時候你就是 reviewer

### 6.2 分支策略
| 策略 | 分支結構 | 重點 | 適合 |
|---|---|---|---|
| **GitHub Flow** | main + 短期 feature branch | 所有改動都走 PR，main 隨時可以部署 | 課堂專案、小團隊、網站 |
| **Git Flow** | main、develop、feature、release、hotfix | 管理版本發布 | 有固定發版週期的產品（App、套件） |
| **環境分支** | dev → staging → prod | 一個 branch 對應一個部署環境 | 需要分階段上線驗證的系統 |
| **Trunk-based** | 大家頻繁合回 main，搭配 feature flag | 減少長期分支和大型衝突 | 大型團隊、成熟的 CI/CD |

- 投影片建議每種策略配一張 Git Graph 風格的示意圖

### 6.3 該選哪一種
- **釐清常見誤解**：「main / dev / prod」是**環境分支**，重點在部署；Git Flow 的 main / develop 重點在**發版**。兩者不同，但實務上常混用
- **建議**：課堂專案、小組作業用 **GitHub Flow** 就夠了，今天示範的就是這個流程
- 團隊越大、發版越正式，才需要更複雜的策略

---

## 7. Coding Agent 與 AI 協作

> 工具更新非常快（例如 Windsurf 在 2026 年 6 月改名為 Devin Desktop、Gemini CLI 在 2026 年 6 月停止服務一般使用者），**上課前一週請再確認一次各家名稱、功能與價格**。本章重點放在分類與觀念，工具換了還能用。

### 7.1 從補完到 agent
| 階段 | 代表 | AI 做什麼 | 人做什麼 |
|---|---|---|---|
| 自動補完 | 早期 GitHub Copilot（2021） | 猜你下一行要打什麼 | 還是自己寫 |
| 對話問答 | ChatGPT、Claude | 回答問題、生成程式碼片段 | 複製、貼上、除錯 |
| Coding agent | Claude Code、Codex、Cursor | 自己讀專案、跑指令、改多個檔案、跑測試 | 下指令、review 結果 |
| 多 agent / 背景執行 | Antigravity、雲端 agent | 多個 agent 平行處理不同任務，做完交回 PR | 分派任務、審查、決策 |

- 重點：**agent 一次會改很多檔案，所以 git 變成必備的安全網**，這就是今天先教 git 的原因

### 7.2 Agent = 模型 + Harness
- **模型（Model）**：大腦，負責理解與生成，例如 Claude、GPT、Gemini
- **Harness**：包在模型外面的外殼，讓模型能真的做事：
  | 元件 | 作用 |
  |---|---|
  | 工具 | 讀檔、寫檔、執行終端機指令、搜尋、上網 |
  | Agent loop | 想 → 用工具 → 看結果 → 再想，直到任務完成 |
  | Context 管理 | 決定要把哪些檔案、對話放進模型的記憶範圍，太長時壓縮 |
  | 權限與沙盒 | 哪些動作要先問使用者、哪些禁止 |
  | 記憶與規則 | 讀取 `AGENTS.md` 這類專案說明、跨對話記住資訊 |
- 比喻：模型是引擎，harness 是車身、方向盤和煞車。同一顆引擎裝在不同車上，開起來感覺完全不同
- **同一個模型換不同 harness，表現也會不同**，所以選工具不能只看用哪個模型
- 伏筆：**下週主題就是 harness 與 Hermes Agent**，今天先有概念就好

### 7.3 廠商自家的 agent
**特色：模型與 harness 由同一家公司一起調校，開箱即用，通常採訂閱制。**

| 工具 | 廠商 | 介面 | 特色 |
|---|---|---|---|
| **Claude Code** | Anthropic | CLI、IDE 擴充套件、桌面版、網頁版 | 終端機原生，擅長大型專案與長任務，支援 subagent 與 hooks |
| **OpenAI Codex** | OpenAI | CLI（開源）、IDE 擴充套件、桌面 app、雲端 | 用 ChatGPT 帳號登入；內建 `/review` 可以直接檢查改動（今天 demo 用它） |
| **Google Antigravity** | Google | 桌面 app、CLI（`agy`）、IDE | agent 優先的平台：Editor view 像一般 IDE，Manager view 同時管理多個 agent；內建瀏覽器讓 agent 測前端；個人免費 |
| **GitHub Copilot** | GitHub / 微軟 | 編輯器擴充套件、CLI、雲端 agent | 跟 GitHub 整合最深，可以把 Issue 指派給 Copilot，做完交回 PR；可切換多家模型 |
| **Cursor** | Anysphere | 以 VS Code 為基礎的 IDE | 編輯器與 agent 整合最緊密，可切換多家模型 |

- **Gemini CLI 的變化**：Google 原本的開源 Gemini CLI，已在 2026 年 6 月 18 日停止服務一般使用者，改由閉源的 **Antigravity CLI** 取代。可以當成例子：廠商工具的方向由廠商決定，隨時可能改名、合併或收掉
- 其他可以提一句：AWS 的 Kiro（先寫規格再實作）、Cognition 的 Devin Desktop（原 Windsurf）

### 7.4 開源 harness，自己接模型
**特色：harness 開源，模型自己選；可以換模型、接本地模型、自己客製，代價是要自己設定。**

| 工具 | 介面 | 特色 |
|---|---|---|
| **OpenCode** | CLI、桌面版、IDE 擴充套件 | 可接 75 家以上的模型供應商，也能用 GitHub Copilot、ChatGPT 帳號登入；有 Plan / Build 兩種模式；`/undo` 背後就是用 git 管理改動 |
| **Cline** | VS Code 擴充套件 | 每個動作都要使用者同意，適合想看清楚 AI 在做什麼的人 |
| **Aider** | CLI | 跟 git 整合很深，每次修改會自動 commit |
| **Hermes Agent**（Nous Research） | CLI、Telegram / Discord / Slack 等通訊軟體 | MIT 授權，不限定寫程式的通用 agent harness，有技能系統與長期記憶（**下週主題**） |

- 開源的好處：看得到原始碼、自由換模型、資料不必經過工具廠商、可以接公司內部或本地模型（例如用 Ollama）
- 缺點：要自己準備 API key 或模型，設定與除錯比較花時間
- **兩條路的比較：**
  | | 廠商自家 agent | 開源 harness |
  |---|---|---|
  | 模型 | 綁定自家（Copilot、Cursor 除外） | 自由選擇 |
  | 上手 | 登入就能用 | 要自己設定模型 |
  | 費用 | 訂閱制 | 工具免費，模型按用量付費或用本地模型 |
  | 客製 | 有限（設定檔、擴充） | 可以改原始碼、自己加工具 |
  | 風險 | 廠商改方向就得跟著改 | 社群維護，品質不一 |

### 7.5 CLI、GUI 與雲端的差異
| 面向 | CLI | GUI（IDE / 桌面 app） | 雲端 agent |
|---|---|---|---|
| 代表 | Claude Code、Codex CLI、Antigravity CLI、OpenCode | Cursor、Copilot、Antigravity、Cline | Codex cloud、Copilot 雲端 agent、Claude Code 網頁版、Devin |
| 上手難度 | 需要熟悉終端機 | 比較直覺 | 在網頁上交代任務 |
| 看改動 | 終端機 diff，搭配 `git diff`、VS Code | 內建視覺化 diff，逐段接受或拒絕 | 在 PR 上看 diff |
| 自動化 | 容易寫進腳本、CI、排程 | 比較難 | 可以從 Issue 觸發 |
| 遠端環境 | SSH 連到伺服器也能用 | 通常需要本機桌面 | 不佔用自己的電腦 |
| 多 agent | 開多個終端機，搭配 `git worktree` | Antigravity Manager 這類管理介面 | 同時丟多個任務 |

- **趨勢**：界線越來越模糊，大廠多半同時提供 CLI、IDE、桌面版和雲端
- 雲端 agent 跟第 6 章的連結：做完交回一個 **PR**，你就是 reviewer
- 講法：不用急著選一個，先挑一個試用；核心概念相通，換工具的成本不高

### 7.6 AI 協作守則
1. **讓 agent 動手前先 commit**：做壞了隨時可以退回
2. **一個任務開一個 branch**：不要讓 agent 直接改 main
3. **一定要自己看 diff**：沒看就合，等於讓陌生人直接 push 到你的專案
4. **任務切小**：小任務比較好 review，也比較好撤銷
5. **寫專案說明檔**：`AGENTS.md` 是多數工具都支援的共通格式（Claude Code 用 `CLAUDE.md`），寫下專案結構、指令、程式風格與禁止事項
6. **注意權限設定**：一開始讓 agent 改檔、跑指令前先問你，熟悉之後再放寬
7. **管好 `.gitignore` 和 secrets**：API key 不能進 repo，也要注意 agent 會不會不小心把它加進去
8. **進階**：`git worktree` 可以讓多個 agent 在不同資料夾同時處理不同 branch，互不干擾
9. **你還是負責人**：agent 寫的程式出問題，責任還是在 merge 的人身上

### 7.7 Live demo（Codex）
**事前準備：**
```bash
# 安裝（擇一）
curl -fsSL https://chatgpt.com/codex/install.sh | sh
npm install -g @openai/codex
```
- 用 ChatGPT 帳號登入，並先完整跑過一次流程

**示範流程：**
1. 開 branch：`git switch -c feature/ai-dark-mode`
2. 在 repo 裡執行 `codex` 啟動
3. `/init` 產生 `AGENTS.md`，打開看它寫了什麼（專案說明、規則），說明這個檔案也要 commit
4. `/permissions` 展示權限設定：改檔、跑指令要不要先問
5. 交代任務：「幫這個網頁加上深色模式切換按鈕」
6. 看它讀了哪些檔案、做了哪些修改
7. `/review` 讓 Codex 檢查這次的改動，再用 `git diff` 或 VS Code 的 diff 畫面自己看一次
8. 挑一處不滿意的地方請它修改，例如按鈕位置或配色
9. commit → push → 在 GitHub 開 PR
10. Merge → 網站自動更新，大家打開網址看到深色模式

- 講法：整個流程就是把前面教的 branch、diff、commit、PR 再走一次，**差別只是改 code 的人變成 AI**
- 備案：事先錄好 demo 影片或截圖，以防網路或 API 出狀況

---

## 8. 總結與 Q&A

### 8.1 回顧
- Git 管版本，GitHub / GitLab 管協作，GUI 工具只是操作介面
- commit 存版本，branch 做隔離，merge / PR 做整合
- 改壞了：沒 commit 用 `restore`，沒 push 用 `reset`，已 push 用 `revert`
- Agent = 模型 + harness；廠商自家 agent 開箱即用，開源 harness 可以自己接模型
- AI 時代，git 是讓你敢放手給 agent 改的安全網

### 8.2 延伸資源
- Learn Git Branching：https://learngitbranching.js.org
- Pro Git（免費電子書，有中文版）：https://git-scm.com/book/zh-tw/v2
- GitHub Skills：https://skills.github.com
- gitignore 範本：https://github.com/github/gitignore
- GitHub Education 學生方案：https://education.github.com

### 8.3 下週預告
- 主題：**Harness 與 Hermes Agent**
- 接續 7.2：深入看 agent 的外殼怎麼設計，以及開源的 Hermes Agent 怎麼運作

### 8.4 指令速查（可做成最後一頁投影片）
| 用途 | 指令 |
|---|---|
| 建立 / 下載 | `git init`、`git clone <url>` |
| 日常 | `git status`、`git add`、`git diff`、`git commit -m`、`git push`、`git pull` |
| 歷史 | `git log --oneline --graph`、`git show` |
| 回復 | `git restore`、`git reset`、`git revert`、`git reflog` |
| 分支 | `git switch -c`、`git merge`、`git rebase`、`git branch -d` |

---

## 課前準備

### 課前一天
- [ ] 把投影片與課堂範例上傳到課程網站，讓學生上課時下載

### 助教自己準備
- [ ] 投影片
- [ ] 本 repo 開好 GitHub Pages（Settings → Pages → main / root），確認網址能開
- [ ] 預演前記下當時的 commit hash，預演完用 `git reset --hard <hash>` + `git push --force` 回到乾淨狀態
- [ ] 預先準備好會衝突的 branch，以及要用來 revert 的「改壞」commit
- [ ] Codex 先安裝、登入，完整跑過一次 7.7 的流程
- [ ] VS Code 版面：編輯器、terminal、Git Graph 同時可見
- [ ] 終端機與 VS Code 字體放大（Zoom 畫質會壓縮，建議 20pt 以上），使用高對比配色
- [ ] 關掉通知，避免分享畫面時跳出訊息
- [ ] 上課前一週再確認一次第 7 章的工具資訊

### 混合授課注意事項
- 請一位同學或共同主持人幫忙看 Zoom 聊天室
- 提問時兩邊都要照顧到：現場舉手、Zoom 打字或用 reaction
- 講話對著麥克風，並重複現場同學的提問，讓線上同學聽得到
- 確認是否錄影，方便課後複習
- live demo 準備錄好的備用影片或截圖

---

## 待確認事項
- [ ] 投影片由誰製作
