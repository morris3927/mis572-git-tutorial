# MIS572 助教課：從 Git 到 AI 協作開發

> **主線：想用 AI 寫 code，先學會管理 AI 做的每一個改動。**

- **時間**：14:10–16:00（含 10 分鐘休息）
- **形式**：實體教室 + Zoom 同步，螢幕分享與投影片
- **對象**：資管系大三、碩一、碩二，少數外系同學
- **示範方式**：投影片講概念，操作一律在 VS Code 的 terminal 下指令，同時看 Git Graph 的變化
- **課堂範例**：整堂課用同一個單頁 HTML 網頁示範（repo：`mis572-test`），接上 GitHub Pages，push 之後網站就會自動更新
- **課前**：上課前一天把投影片與範例上傳到課程網站，讓學生上課時下載

---

## 1. 版本控制與 Git 的由來
- **1.1 開場例子**：用 `報告_v10_3_final_last.docx` 這類檔名，帶出找不到最新版、無法比對、難以合併、救不回舊版等問題。
- **1.2 Git 之前怎麼工作**：從人工複製檔案，一路介紹 RCS（單機、單檔）、CVS（集中式）、SVN（集中式的改良版），以及它們的限制。
- **1.3 Git 的誕生**：2005 年 BitKeeper 收回 Linux 核心的免費授權，Linus Torvalds 參考 BitKeeper 與 Monotone 的使用經驗，設計出 Git。

## 2. Git 生態系與環境準備
- **2.1 Git 能做什麼**：先講 big picture，也就是記錄版本、比對差異、回復版本這三件事。
- **2.2 Git 與周邊工具**：把工具分成兩類，一類是**託管平台**（GitHub、GitLab、Bitbucket），另一類是 **GUI 用戶端**（Sourcetree、GitHub Desktop、VS Code），兩類都建立在 Git 之上。
- **2.3 安裝 Git**：分別介紹 Windows、macOS、Linux 的安裝方式，裝完用 `git --version` 確認。
- **2.4 VS Code 與 Git Graph**：安裝 Git Graph 擴充套件，之後的操作都搭配它看 commit 與 branch 的變化。
- **2.5 初始設定**：設定 `user.name` / `user.email`，再說明連到 GitHub 的兩種認證方式：HTTPS 搭配瀏覽器登入，以及 SSH key（投影片附上設定步驟）。

## 3. 建立 Repo 與基本操作
- **3.1 課堂範例介紹**：展示單頁 HTML 與它的 GitHub Pages 網址，讓大家看到 push 之後網站會跟著更新。
- **3.2 建立 repo 的兩種情境**：從零開始，先在 GitHub 開 repo 再 `git clone`；或替現有專案 `git init`，再用 `remote add`、`push -u` 連到 GitHub。
- **3.3 一個檔案的旅程**：說明未追蹤、已修改、已暫存、已 commit（本機）、已 push（雲端）這幾種狀態，並強調 commit 不等於上傳。
- **3.4 基本操作**：`status / add / diff / commit / push`，每個指令都對照 VS Code GUI 的做法，最後打開網站看更新結果。
- **3.5 .gitignore**：說明哪些檔案不該進 repo，例如 `.DS_Store`、`node_modules`、`.env` 裡的 API key。

## 4. 查看歷史與回復版本
- **4.1 查看歷史**：用 `git log` 對照 Git Graph 上的節點，看每個 commit 的內容。
- **4.2 還沒 commit 的改動**：用 `git restore` 放棄修改，或把檔案移出暫存區。
- **4.3 已經 commit 的改動**：`reset` 會改寫歷史，適合只在本機；`revert` 會新增一個反向 commit，適合已經 push 的情況。說明這兩個指令各自的使用情境。

## 5. Branch 與 Merge
- **5.1 Branch 的概念**：branch 是指向 commit 的指標。示範用 `git switch -c` 開新 branch 修改網頁，不影響 main。
- **5.2 正常的 merge**：示範 fast-forward 與一般 merge，並用 Git Graph 看分岔與合併的樣子。
- **5.3 Merge conflict**：示範兩個 branch 改到同一行時怎麼衝突，以及怎麼在 VS Code 裡解決。
- **5.4 Rebase**：跟 merge 比較，說明它能把歷史整理成一直線。適合用在自己的 branch 上，已經 push 給別人的歷史不要 rebase。

## 6. 團隊協作：PR 與分支策略
- **6.1 Pull Request**：示範在 GitHub 開 PR、review、修改、merge 的完整流程，merge 之後網站自動更新。
- **6.2 分支策略**：介紹 GitHub Flow、Git Flow、環境分支（dev / staging / prod）、Trunk-based 的差異。
- **6.3 該選哪一種**：釐清 main/dev/prod 不等於 Git Flow，並建議課堂專案用 GitHub Flow。

## 7. Coding Agent 與 AI 協作
- **7.1 從補完到 agent**：AI 寫程式從自動補完、對話問答，一路進化到能自己讀專案、跑指令、改檔案的 agent，現在更能多個 agent 平行、在背景工作。
- **7.2 Agent = 模型 + Harness**：模型是大腦，harness 是讓它能讀檔、跑指令、管權限與記憶的外殼；同一個模型換不同 harness，表現也會不同（下週主題的伏筆）。
- **7.3 廠商自家的 agent**：Claude Code、OpenAI Codex、Google Antigravity（Gemini CLI 已由 Antigravity CLI 取代）、GitHub Copilot、Cursor，模型與 harness 一起調校，開箱即用。
- **7.4 開源 harness，自己接模型**：OpenCode、Cline、Aider、Hermes Agent，可以換模型、接本地模型、自己客製，代價是要自己設定。
- **7.5 CLI、GUI 與雲端的差異**：比較上手難度、看 diff 的方式、自動化與遠端使用、多 agent 管理，以及丟出任務、交回 PR 的雲端 agent。
- **7.6 AI 協作守則**：先 commit、一個任務開一個 branch、一定要看 diff、把任務切小、用 `AGENTS.md` 寫專案規則、管好 secrets。
- **7.7 Live demo（Codex）**：用 Codex 在 branch 上修改課堂範例網頁，用 `/review` 與 `git diff` 檢查、commit、發 PR，merge 後網站自動更新。

## 8. 總結與 Q&A
- **8.1 回顧**：Git 管版本，平台管協作，AI 時代的 git 是安全網。
- **8.2 延伸資源**：Learn Git Branching、Pro Git 等，讓想繼續學的同學課後自己練習。
- **8.3 下週預告**：Harness 與 Hermes Agent，深入看 agent 的外殼怎麼設計。
