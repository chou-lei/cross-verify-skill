# cross-verify — Claude Code 資訊交叉查證技能

[繁體中文](#繁體中文) · [English](#english)

## 繁體中文

讓 Claude Code 在**安裝開源專案、技能、外掛、MCP 之前**，以及**設計架構**或**採信某個說法**之前，先把資訊查清楚。

AI 聊天工具給的答案和引用只算「線索」。這個技能會要求 Claude Code 逐一打開每個引用來源，確認原文真的這樣寫，再到學術索引（Semantic Scholar、OpenAlex、arXiv）和軟體安全資料庫（deps.dev、OSV）找獨立的佐證，最後照固定的評分表打分並給出建議。是否安裝，最後由你決定。

### 分工
| 角色 | 負責 |
|---|---|
| 你（或你的助理） | 手動操作 Elicit、NotebookLM、ChatGPT Search、Gemini Deep Research、Perplexity 等網頁工具，一次問一題，把答案和連結貼進查證檔。這些服務的條款限制自動化擷取，所以不寫程式自動操作 |
| Claude Code | 打開每個來源核對原文（WebFetch／markitdown／playwright-cli），用 Semantic Scholar、OpenAlex、arXiv、deps.dev、OSV 交叉比對，再打分並建議 |
| 你的安全掃描工具 | 實際安裝時的最後一道關卡（選用） |
| 你 | 做最後決定 |

### 三種模式
- **install**：評估要不要裝某個套件。用 100 分評分表打分：維護者 25、活躍度 15、安全 25、獨立佐證 20、適合度 15，另外有「直接不通過」的條件。
- **architecture**：對照 10 項清單，檢查架構是否完整。
- **claim**：查證單一說法，結果分成「有佐證／被推翻／無法證實」。

### 安裝
```bash
git clone https://github.com/<you>/<repo>.git
mkdir -p ~/.claude/skills
cp -r <repo> ~/.claude/skills/cross-verify   # 或只複製 SKILL.md 和 references/
```
重新開啟 Claude Code 就會出現。可以直接說「用 cross-verify 查證 github.com/OWNER/REPO」。

查證檔預設存在 `~/research/`（不要放在 `~/.claude/` 裡，Claude Code 每次寫入那裡都會要求批准），也可以設定環境變數 `CROSS_VERIFY_DIR` 改到別的地方。原始網頁只用 grep 讀需要的段落，不整份讀，以節省 Token。（不要把這個資料夾加進 `permissions.deny`，因為 deny 的 Read 規則也會擋掉 Grep。）

### 需求
- Claude Code
- `curl`
- 選用：[markitdown](https://github.com/microsoft/markitdown)（靜態網頁轉 Markdown）、[playwright-cli](https://github.com/microsoft/playwright-cli)（需要 JavaScript 才能顯示的網頁）

### 限制
- 不需要 API 金鑰，但免費額度是共用的：Semantic Scholar 常被回 429；OpenAlex 每個 IP 每天只有小額免費預算；arXiv 每 3 秒最多 1 次請求。詳見 `references/sources-and-limits.md`。
- 分數只是把證據整理出來，**不保證安全**。

### 聲明
本專案與 OpenAI、Google、Elicit、Allen Institute for AI（Semantic Scholar）、Microsoft、Perplexity、Anthropic、OpenAlex／OurResearch、arXiv／Cornell 或其他任何公司、機構沒有任何關係，也沒有得到它們的背書。所有商標都屬於各自的擁有者。本專案只說明如何使用這些公開服務，並附上它們的連結。

## English

A Claude Code skill that verifies information **before you install** an open-source repo, skill, plugin, or MCP server, **review an architecture**, or **trust a claim**.

Chatbot answers and their citations are treated as leads only. The skill makes Claude Code open every cited source and confirm it says what was claimed. It then looks for independent agreement in academic indexes (Semantic Scholar, OpenAlex, arXiv) and software security databases (deps.dev, OSV), scores the result with a fixed rubric, and recommends. You make the final call.

### Roles
| Role | Does |
|---|---|
| You (or your assistant) | Use Elicit, NotebookLM, ChatGPT Search, Gemini Deep Research, Perplexity, etc. manually, one question at a time, and paste answers and links into the evidence file. Their terms restrict automated extraction, so nothing here automates them |
| Claude Code | Opens every source (WebFetch / markitdown / playwright-cli), cross-checks with Semantic Scholar, OpenAlex, arXiv, deps.dev, OSV, then scores and recommends |
| Your security scanner | Final gate at install time (optional) |
| You | Final decision |

### Modes
- **install**: a 100-point rubric (maintainer 25, activity 15, security 25, independent agreement 20, fit/cost 15) plus hard fails.
- **architecture**: a 10-item completeness checklist.
- **claim**: supported / contradicted / unverified.

### Install
```bash
git clone https://github.com/<you>/<repo>.git
mkdir -p ~/.claude/skills
cp -r <repo> ~/.claude/skills/cross-verify   # or copy just SKILL.md and references/
```
Restart Claude Code, then ask: "use cross-verify to vet github.com/OWNER/REPO".

Evidence files go to `~/research/` by default (not inside `~/.claude/`, where Claude Code asks for approval on every write); override with `CROSS_VERIFY_DIR`. Raw pages are grepped, not read whole, to save tokens. (Don't put that folder in `permissions.deny`: a Read deny also blocks Grep.)

### Requirements
- Claude Code
- `curl`
- Optional: [markitdown](https://github.com/microsoft/markitdown) for static pages and [playwright-cli](https://github.com/microsoft/playwright-cli) for JS-rendered pages

### Limits
- No API keys are needed, but the free pools are shared: Semantic Scholar often returns 429, OpenAlex gives a small free daily budget per IP, and arXiv allows at most 1 request every 3 seconds. See `references/sources-and-limits.md`.
- A score organizes evidence. It is **not a safety guarantee**.

### Disclaimer
This project is not affiliated with or endorsed by OpenAI, Google, Elicit, the Allen Institute for AI (Semantic Scholar), Microsoft, Perplexity, Anthropic, OpenAlex/OurResearch, arXiv/Cornell, or any other organization. All trademarks belong to their respective owners. This project only documents how to use their public services and links to them.

## License
MIT
