# MIS572 助教課：從 Git 到 AI 協作開發

> **主線：想用 AI 寫 code，先學會管理 AI 做的每一個改動。**

- **時間**：14:10–16:00（含 10 分鐘休息）
- **形式**：實體教室 + Zoom 同步，螢幕分享與投影片
- **對象**：資管系大三、碩一、碩二，少數外系同學

| 時間 | 段落 |
|---|---|
| 14:10–14:20 | 1. 為什麼需要版本控制 |
| 14:20–14:45 | 2. Git 核心概念與常用指令 |
| 14:45–15:00 | 3. Branch 與 Merge |
| 15:00–15:10 | 休息 |
| 15:10–15:28 | 4. 遠端協作：GitHub 與 PR |
| 15:28–15:35 | 5. 分支策略 |
| 15:35–15:55 | 6. Coding Agent 與 AI 協作 |
| 15:55–16:00 | 7. 總結與 Q&A |

---

## 1. 為什麼需要版本控制（10 分鐘）
- **1.1 開場例子**：用 `報告_v10_3_final_last.docx` 這類檔名，帶出找不到最新版、無法比對、難以合併、救不回舊版等問題。
- **1.2 Git 之前怎麼工作**：從複製資料夾、email 傳檔，到集中式版控 CVS、SVN，以及它們的限制。
- **1.3 Git 的誕生與演進**：2005 年 Linus 為了 Linux 核心寫出 Git，之後 GitHub、GitLab 讓它成為業界標準。

## 2. Git 核心概念與常用指令（25 分鐘）
- **2.1 Git 能做什麼**：先講 big picture，也就是記錄版本、比對差異、回復版本這三件事。
- **2.2 Git ≠ GitHub / GitLab**：Git 是本機的版控工具，GitHub、GitLab 是放 repo 的協作平台，兩者完全不同。
- **2.3 一個檔案的旅程**：說明未追蹤、已修改、已暫存、已 commit（本機）、已 push（雲端）這幾種狀態，並強調 commit 不等於上傳。
- **2.4 常用指令**：用 CLI 示範 `init / status / add / commit / log / diff / restore`，同時看 VS Code Git Graph 的變化。

## 3. Branch 與 Merge（15 分鐘）
- **3.1 Branch 的概念**：branch 是指向 commit 的指標，讓你在不影響主線的情況下開發。
- **3.2 建立與合併**：示範開 branch、commit、merge，並用 Git Graph 看分岔與合併。
- **3.3 Merge conflict**：示範衝突怎麼發生，以及怎麼在 VS Code 裡解決。

## 4. 遠端協作：GitHub 與 PR（18 分鐘）
- **4.1 遠端操作**：`clone / pull / push`，接續 2.3 講的本機與雲端。
- **4.2 Pull Request**：示範開 PR、review、修改、merge 的完整流程。
- **4.3 Code review 的價值**：為什麼要 review，並埋下伏筆：之後 AI 交給你的也是 PR。

## 5. 分支策略（7 分鐘）
- **5.1 四種常見流派**：GitHub Flow、Git Flow、環境分支（dev / staging / prod）、Trunk-based 的差異。
- **5.2 該選哪一種**：釐清 main/dev/prod 不等於 Git Flow，並建議課堂專案用 GitHub Flow。

## 6. Coding Agent 與 AI 協作（20 分鐘）
- **6.1 Coding agent 是什麼**：能讀專案、跑指令、改多個檔案的 AI，所以更需要 git 當安全網。
- **6.2 工具分類**：分成 IDE 整合、終端機 CLI（含 OpenCode）、雲端非同步三類，並比較開源與閉源。
- **6.3 AI 協作守則**：先 commit、一個任務開一個 branch、一定要看 diff、把任務切小、管好 secrets。
- **6.4 Live demo**：用 agent 在 branch 上完成一個小功能，最後發成 PR。

## 7. 總結與 Q&A（5 分鐘）
- **7.1 回顧**：Git 管版本，平台管協作，AI 時代的 git 是安全網。
- **7.2 作業與資源**：作業待與老師確認，另外提供 Learn Git Branching、Pro Git 等延伸資源。
