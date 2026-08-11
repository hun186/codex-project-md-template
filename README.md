# Codex 專案 Markdown 記憶範本

這份範本把既有的 Agent Markdown 機制整理成可直接移植到新 Git 專案的通用版本。它保留「長期專案記憶」的優點，但避免每次任務都讀完所有歷史、把目前架構寫成時間流水帳，或讓每個任務都修改所有文件。

## 使用方式

1. 把 `template/` 裡的所有內容複製到新專案根目錄；複製時要包含隱藏目錄 `.codex/`。
2. 從新專案根目錄啟動 Codex。
3. 將 `CODEX_BOOTSTRAP.md` 內的「首次初始化提示詞」交給 Codex。
4. 檢查 Codex 對專案目的、入口、測試指令、架構與介面的盤點結果。
5. 修正任何誤解後，將這批文件作為專案記憶基線提交到 Git。

完成初始化後，`AGENTS.md` 會保持精簡，Codex 會依任務種類載入 `.codex/` 下的相關文件，並只在持久資訊真的改變時更新它們。

首次初始化另包含一個保守的 README 閘門：Codex 會先判定根目錄 `README.md` 是否可沿用、需要局部補強，或只是缺少／占位文件。只有後兩種狀態才修改，且不會用通用範本覆蓋仍有價值的原作者內容。

Windows 可用：

```powershell
robocopy .\template D:\AIProjects\your-new-project /E
```

macOS／Linux 可用：

```bash
cp -a template/. /path/to/your-new-project/
```

兩種方式都會複製 `.codex/`；若目標專案已有同名文件，先比較內容，不要直接覆蓋既有專案規則。

## 範本結構

```text
template/
├── AGENTS.md
├── CODEX_BOOTSTRAP.md
└── .codex/
    ├── project.md
    ├── workflows.md
    ├── architecture.md
    ├── contracts.md
    ├── memory.md
    ├── decisions.md
    ├── known_issues.md
    ├── backlog.md
    ├── archive/
    │   └── README.md
    └── templates/
        ├── module-AGENTS.template.md
        ├── project-README.template.md
        ├── decision-entry.template.md
        └── memory-entry.template.md
```

## 舊機制與新機制的對照

| 舊檔案／行為 | 新位置／行為 | 調整目的 |
| --- | --- | --- |
| 根目錄 `AGENTS.md` 同時放通用規則與大量專案細節 | `AGENTS.md` 只保留高價值規則、快速摘要與文件路由 | 自動載入內容維持短小 |
| 每次強制讀取所有 `.codex/*.md` | 只固定讀 `project.md`、`memory.md`，其餘按任務載入 | 降低 token 與錯誤上下文 |
| `.codex/memory.md` 保存完整工作流水帳 | 只保留目前焦點與最近 10 筆有價值成果 | 讓近期脈絡可快速讀取 |
| `.codex/architecture.md` 持續追加日期紀錄 | 重寫為目前有效的架構模型 | 避免讀到已失效架構 |
| `.codex/api_contract.md` 假設所有專案都有 HTTP API | `.codex/contracts.md` 同時涵蓋 API、CLI、資料格式、事件與整合介面 | 適用更多專案類型 |
| 每個改動都追加 decision | 只有真正影響長期維護的取捨才建立決策紀錄 | 降低決策噪音 |
| Agent 自動加入大量「未來建議」 | backlog 只收已接受的延後工作或完成正確性所必需的後續 | 避免待辦清單失控 |
| 濃縮前反覆保存整檔快照 | 只把移出的有效舊條目歸檔，細節優先由 Git 追溯 | 減少重複內容 |
| 新專案缺少 README，或 README 只是產生器占位文字 | 初始化時先分類，再保留、局部補強或依證據建立 | 補齊專案入口，同時避免覆寫與杜撰 |

## 設計原則

- 程式碼、設定、測試與可重現行為是事實來源；Markdown 文件若過時，應在查證後更新。
- `AGENTS.md` 是規則與路由，不是專案百科全書。
- `project.md`、`architecture.md`、`contracts.md` 與 `workflows.md` 描述「現在」；歷史理由放 `decisions.md`，近期成果放 `memory.md`。
- 不確定資訊一律標為「待確認」，不得把檔名或目錄名稱猜成事實。
- 只有能在未來任務減少誤解的資訊才值得保存；命令全文輸出、聊天逐字稿與一次性失敗通常不值得保存。
- 同一條規則應放在最接近其作用範圍的位置。大型 repo 只有在子模組真的有不同命令、限制或邊界時才建立巢狀 `AGENTS.md`。
- README 的初始化採證據優先與最小修改原則；不因格式偏好重寫既有內容，也不猜測尚未存在的命令、功能或授權。

## 官方行為依據

Codex 會從專案根目錄走到目前工作目錄，依序載入適用的 `AGENTS.md`；較接近工作目錄的指引具有較高優先性。官方也建議保持 `AGENTS.md` 精簡，並把架構、規劃或審查等細節放到專門文件：

- [Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- [Codex best practices](https://learn.chatgpt.com/guides/best-practices)

## 建議的第一次 Git 提交

初始化並人工確認後，可使用類似下列提交訊息：

```text
docs: initialize Codex project context
```

範本不會替你決定 Git 操作，也不會要求 Codex 自動提交；是否提交與何時提交仍由專案擁有者決定。
