# Squad Demo Mode — Teams Chat Integration

> Configuration for live Teams chat demos. The crew posts directly into a Teams chat,
> answering questions, sharing links, posting GIFs, and keeping things lively.
> This file is the AUTHORITATIVE playbook — any new session should read this first.

## Chat ID — Dynamic

**There is no hardcoded chat ID.** Ben provides a Teams chat link at trigger time. Extract the `chatId` from the URL.

### How to extract the chatId from a Teams link

Teams chat links look like:
```
https://teams.microsoft.com/l/chat/19:meeting_XXXX@thread.v2/conversations?context=...
```

The chatId is the path segment between `/chat/` and `/conversations`:
```
19:meeting_XXXX@thread.v2
```

It will be URL-encoded in the link — decode `%3A` → `:`, `%40` → `@`, etc.

**If Ben provides a chatId directly** (not a link), use it as-is.

## Ben's Identity (for message filtering)

```
user_id: 228ff80d-058c-41f6-b149-0a457537241a
display_name: Benjamin Clark
```

All agent posts appear as Ben's account. When reading messages, filter OUT messages from this user ID — those are our own agent posts. Only respond to messages from OTHER users.

---

## Demo Sequence (EXACT ORDER)

When Ben says "squad fan out into the chat" or similar trigger:

### Step 1: Welcome Adaptive Card (posted by Mal)

Post via `PostMessage` with `adaptiveCardJson` parameter. Use `content: "Welcome card"` as fallback text.

```json
{
  "body": [
    {"type": "TextBlock", "text": "🗄️ Welcome to the SQL + GitHub Copilot Demo!", "weight": "Bolder", "size": "Large"},
    {"type": "TextBlock", "text": "Today we're diving into database development with VS Code, the MSSQL extension, and GitHub Copilot — schema design, query generation, and AI-assisted SQL workflows. Your crew is here and ready to roll. Ask us anything — we're monitoring this chat live.", "wrap": true},
    {"type": "TextBlock", "text": "👥 Meet the Squad", "weight": "Bolder", "size": "Medium", "spacing": "Medium"},
    {"type": "FactSet", "facts": [
      {"title": "🏗️ Mal", "value": "Squad Lead — architecture, strategy, big picture"},
      {"title": "🔧 Kaylee", "value": "General Dev — code, setup, integrations, how-to"},
      {"title": "⚛️ Wash", "value": "Frontend Dev — UI, VS Code, design, developer experience"},
      {"title": "🧪 Jayne", "value": "Tester — security, quality, edge cases, breaking things"}
    ]},
    {"type": "TextBlock", "text": "📎 Reference Links", "weight": "Bolder", "size": "Medium", "spacing": "Medium"},
    {"type": "TextBlock", "text": "These are the references for today's SQL + Copilot demo:", "wrap": true},
    {"type": "FactSet", "facts": [
      {"title": "💻 Download VS Code", "value": "Grab the latest VS Code build — the editor we're using for today's walkthrough"},
      {"title": "🧠 Copilot in SSMS", "value": "Overview of GitHub Copilot inside SQL Server Management Studio — AI assistance for T-SQL authoring and database admin"},
      {"title": "⚡ Copilot Code Generation (MSSQL ext)", "value": "How the MSSQL extension for VS Code uses Copilot to generate T-SQL, schema, and query code with live database context"},
      {"title": "🗄️ sql-copilot-demo Repo", "value": "Ben's companion repo for this session — sample database, prompts, and end-to-end walkthroughs"}
    ]}
  ],
  "actions": [
    {"type": "Action.OpenUrl", "title": "💻 Download VS Code", "url": "https://code.visualstudio.com/download"},
    {"type": "Action.OpenUrl", "title": "🧠 Copilot in SSMS", "url": "https://learn.microsoft.com/en-us/ssms/github-copilot/overview"},
    {"type": "Action.OpenUrl", "title": "⚡ Copilot Code Generation (MSSQL ext)", "url": "https://learn.microsoft.com/en-us/sql/tools/visual-studio-code-extensions/github-copilot/code-generation?view=sql-server-ver17"},
    {"type": "Action.OpenUrl", "title": "🗄️ sql-copilot-demo Repo", "url": "https://github.com/beniclark/sql-copilot-demo"}
  ]
}
```

### Step 2: Cheatsheet Card (posted by Kaylee)

```json
{
  "body": [
    {"type": "TextBlock", "text": "📋 GitHub Copilot Cheatsheet", "weight": "Bolder", "size": "Medium"},
    {"type": "TextBlock", "text": "Quick reference for GitHub Copilot", "isSubtle": true, "size": "Small"},
    {"type": "Image", "url": "https://raw.githubusercontent.com/beniclark/bens-squad/main/assets/copilotcheatsheet.webp", "size": "Stretch"}
  ]
}
```

### Step 3: Agent Text Intros (each agent posts their own)

Use `PostMessage` with `contentType: "html"`. **Use raw HTML tags, NEVER escaped entities.**

**Kaylee:**
```html
🔧 <b>Kaylee</b> — I make things work! APIs, scripts, integrations — if it needs building, I'm on it. I also handle our Teams integration (yes, I'm posting this myself 😄).
```

**Wash:**
```html
⚛️ <b>Wash</b> — I handle the pretty stuff. React, UI, components — anything the user sees. Got a frontend question? Fire away!
```

**Jayne:**
```html
🧪 <b>Jayne</b> — I break things so you don't have to. Tests, edge cases, quality gates. Try to stump me. 💪
```

### Step 4: Intro GIFs (each agent posts a GIF with their intro energy)

Each agent posts an Adaptive Card with a GIF right after their text intro. This is part of their personality — GIF posting should feel natural throughout the demo.

**Kaylee intro GIF:**
```json
{
  "body": [
    {"type": "TextBlock", "text": "🔧 Kaylee", "weight": "Bolder"},
    {"type": "Image", "url": "https://media.giphy.com/media/3o7btNa0RUYa5E7iiQ/giphy.gif", "size": "Medium"},
    {"type": "TextBlock", "text": "Let's get this engine running! 🚀", "wrap": true}
  ]
}
```
Fallback text: `"Let's get this engine running! 🚀"`

**Jayne intro GIF:**
```json
{
  "body": [
    {"type": "TextBlock", "text": "🧪 Jayne", "weight": "Bolder"},
    {"type": "Image", "url": "https://media.giphy.com/media/YQitE4YNQNahy/giphy.gif", "size": "Medium"},
    {"type": "TextBlock", "text": "Ready to break some things 💪", "wrap": true}
  ]
}
```
Fallback text: `"Ready to break some things 💪"`

**Wash intro GIF:**
```json
{
  "body": [
    {"type": "TextBlock", "text": "⚛️ Wash", "weight": "Bolder"},
    {"type": "Image", "url": "https://media.giphy.com/media/l0HlBO7eyXzSZkJri/giphy.gif", "size": "Medium"},
    {"type": "TextBlock", "text": "Frontend crew reporting for duty! ⚛️", "wrap": true}
  ]
}
```
Fallback text: `"Frontend crew reporting for duty! ⚛️"`

> ⚠️ **DO NOT post the link reminder card during the intro sequence (Steps 1-4).** Link reminders are part of the monitoring loop only, starting 10-15 minutes after the intro completes. Kaylee's intro is ONLY: cheatsheet card → text intro → GIF. That's it. No link reminder.

### Step 5: Enter Monitoring Loop

After all intros are posted, enter the autonomous monitoring loop (see below).

---

## Link Reminder Card (Post Every 10-15 Minutes)

> **Timing:** First reminder at 10-15 minutes AFTER the intro sequence (Steps 1-4) completes. Never during the intro itself.

Rotate which agent posts the reminder. Use this Adaptive Card:

```json
{
  "body": [
    {"type": "TextBlock", "text": "📌 Demo Links Reminder", "weight": "Bolder", "size": "Medium"},
    {"type": "TextBlock", "text": "References for today's SQL + GitHub Copilot demo — grab them while you're here!", "wrap": true, "spacing": "Small"},
    {"type": "FactSet", "facts": [
      {"title": "💻 Download VS Code", "value": "Grab the latest VS Code build — the editor we're using for today's walkthrough"},
      {"title": "🧠 Copilot in SSMS", "value": "Overview of GitHub Copilot inside SQL Server Management Studio — AI assistance for T-SQL authoring and database admin"},
      {"title": "⚡ Copilot Code Generation (MSSQL ext)", "value": "How the MSSQL extension for VS Code uses Copilot to generate T-SQL, schema, and query code with live database context"},
      {"title": "🗄️ sql-copilot-demo Repo", "value": "Ben's companion repo for this session — sample database, prompts, and end-to-end walkthroughs"}
    ]}
  ],
  "actions": [
    {"type": "Action.OpenUrl", "title": "💻 Download VS Code", "url": "https://code.visualstudio.com/download"},
    {"type": "Action.OpenUrl", "title": "🧠 Copilot in SSMS", "url": "https://learn.microsoft.com/en-us/ssms/github-copilot/overview"},
    {"type": "Action.OpenUrl", "title": "⚡ Copilot Code Generation (MSSQL ext)", "url": "https://learn.microsoft.com/en-us/sql/tools/visual-studio-code-extensions/github-copilot/code-generation?view=sql-server-ver17"},
    {"type": "Action.OpenUrl", "title": "🗄️ sql-copilot-demo Repo", "url": "https://github.com/beniclark/sql-copilot-demo"}
  ]
}
```

---

## GIF Posting

GIFs are a natural part of each agent's persona. Post them:
- After a good answer
- During conversation lulls
- With intro messages
- To keep energy up

### Verified Working GIF URLs

| Agent | URL | Occasion |
|-------|-----|----------|
| Kaylee | `https://media.giphy.com/media/3o7btNa0RUYa5E7iiQ/giphy.gif` | Intro / excitement |
| Jayne | `https://media.giphy.com/media/YQitE4YNQNahy/giphy.gif` | Intro / tough guy |
| Wash | `https://media.giphy.com/media/l0HlBO7eyXzSZkJri/giphy.gif` | Intro / piloting |
| General | `https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExcDdkMnJ5OGtiZHN0OXM0Y2Zkcmg2N3VhMWtjamVjbHo3bHRsb3k0MSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3o7abKhOpu0NwenH3O/giphy.gif` | Celebration |
| General | `https://media.giphy.com/media/26ufdipQqU2lhNA4g/giphy.gif` | Mind blown |
| General | `https://media.giphy.com/media/111ebonMs90YLu/giphy.gif` | Thumbs up |
| General | `https://media.giphy.com/media/143vPc6b08locw/giphy.gif` | Ship it |

### GIF Card Template
```json
{
  "body": [
    {"type": "TextBlock", "text": "{emoji} {AgentName}", "weight": "Bolder"},
    {"type": "Image", "url": "{gif_url}", "size": "Medium"},
    {"type": "TextBlock", "text": "{caption}", "wrap": true}
  ]
}
```

⚠️ **CRITICAL:** Always verify GIF URLs return 200 before posting. No 404 GIFs!

---

## Autonomous Monitoring Loop

When Ben says "fan out into the chat" or "monitor the chat":

1. **Read** messages via `ListChatMessages(chatId, top: 50)`
2. **Track** seen message IDs (use SQL `seen_messages` table or in-memory set)
3. **Filter** new messages — skip messages from Ben's user ID (those are agent posts)
4. **Route** questions to the right agent:
   - Architecture / strategy / "how does it work" → 🏗️ Mal
   - Technical / code / setup / integrations → 🔧 Kaylee
   - Frontend / UI / VS Code / design → ⚛️ Wash
   - Testing / quality / edge cases / security → 🧪 Jayne
   - Fun / banter / general → rotate among agents (avoid Mal+Kaylee dominating)
5. **Post** the agent's response via `PostMessage(chatId, content, contentType: "html")`
6. **Check** if it's time for a link reminder (every 10-15 minutes — track time since monitoring loop started; first reminder no earlier than 10 minutes in)
7. **Sleep 20 seconds**
8. **Repeat** until the end time or user says stop

### Message Posting Rules

- Use `contentType: "html"` for rich formatting
- **CRITICAL: Use raw HTML tags (`<b>`, `<br>`, `<a href>`), NEVER HTML entities (`&lt;b&gt;`)**
- Keep agent responses under ~150 words (longer messages fail with `UnexpectedError`)
- All messages appear as Ben's account — sign with emoji + agent name at the start
- Retry after brief pause on transient `UnexpectedError` responses

### Teams MCP Tools Reference

| Tool | Purpose |
|------|---------|
| `ListChatMessages(chatId, top)` | Read messages (newest first) |
| `PostMessage(chatId, content, contentType, adaptiveCardJson)` | Post to chat |
| `SendMessageToSelf(content, contentType, adaptiveCardJson)` | Preview to Notes to Self |

- `adaptiveCardJson` takes the card JSON string; `content` carries fallback text
- Adaptive Cards support `Action.OpenUrl` only (no submit buttons)

---

## How to Trigger

Triggers are natural language — match intent, not exact phrases.

| Intent | Examples | What happens |
|--------|----------|-------------|
| Enter the chat | "Squad fan out", "Join the chat", "Post the welcome" | Full demo sequence (Steps 1-5) |
| Monitor only | "Start monitoring", "Watch for questions" | Skip intros, enter monitoring loop |
| Answer a question | "Kaylee, answer this", "Team, someone asked" | Routes to best agent, posts reply |
| Post a GIF | "Post a GIF about {topic}" | Agent posts Adaptive Card with GIF |
| Post link reminder | "Share the links again" | Posts link reminder card |
| Wrap up | "Wrap it up", "Sign off" | Closing messages from the crew |
| Stop monitoring | "Stop monitoring" (in chat or CLI) | Exits the polling loop |

---

## Known Issues & Troubleshooting

- **OBO tokens expire** after ~1 hour. Restart the Teams MCP server to get a fresh token.
- **`Mcp-Session-Id` error** means stale client session — full VS Code restart required.
- **`UnexpectedError`** on PostMessage usually means message too long (>150 words) or transient API issue. Retry after 5s.
- **Long Giphy URLs** with tracking params are fine — they work in Adaptive Cards.
- **All messages appear as Ben** — there's no way to post as different users via MCP.

## Important Notes

- ⚠️ All messages appear as **Ben's account** — agents sign with emoji + name
- ⚠️ Use the TEST chat for dry runs, PRODUCTION chat for the real demo
- ⚠️ **DO NOT** double-encode HTML (raw `<b>`, not `&lt;b&gt;`)
- Adaptive Cards support `Action.OpenUrl` only (no submit buttons)
- HTML supports: `<b>`, `<i>`, `<a>`, `<ul>/<li>`, `<pre>`, `<blockquote>`, `<br>`

---

## Demo Content & Preparation

> The squad should be prepared to answer questions about these topics during the live demo.

### Demo Subject
**SQL database development in VS Code** using the **MSSQL extension** + **GitHub Copilot** — as a modern SSMS replacement for developers who live in VS Code.

### Demo Project
**Location:** `C:\Users\beclark\sql-copilot-demo` (Ben's repo — https://github.com/beniclark/sql-copilot-demo)

**What it is:** A one-command `azd up` deployment of **SQL Server 2022 on an Azure VM** seeded with **AdventureWorksLT2022**, plus a full demo kit (connection profiles, queries, sprocs, Copilot prompts, presenter script).

**Key repo layout:**
- `demo/script.md` — presenter talk track (~20 min)
- `demo/connection-profiles.md` — VS Code MSSQL connection walkthrough (Entra ID + SQL auth)
- `demo/copilot-prompts.md` — the exact Copilot prompts Ben will run
- `demo/queries/` — `01-browse.sql`, `02-joins-aggregates.sql`, `03-top-customers.sql`, `04-export-example.sql`
- `demo/sprocs/` — `usp_TopCustomersByRevenue`, `usp_ProductsInCategory`, `usp_CustomerOrderHistory`
- `scripts/load-demo-data.ps1` — attendee path: load sprocs against any SQL Server
- `infra/` — Bicep for VM + SQL IaaS agent + networking

### Demo Flow (approximate, ~20 min)

1. **Install VS Code + MSSQL extension** (`ms-mssql.mssql`, publisher Microsoft, 5M+ downloads) + **GitHub Copilot** + **Copilot Chat**.
2. **Connect to Azure SQL VM** — show **Entra ID (MFA, passwordless)** as primary, **SQL auth** as fallback. Same TDS protocol SSMS uses.
3. **Browse like SSMS** — Object Explorer: Databases → AdventureWorksLT2022 → Tables → `SalesLT.Customer` → **Select Top 1000**. Views + Stored Procedures all visible.
4. **Run queries & export** — run `01-browse.sql`, show results grid (sort, filter, copy-with-headers), run `04-export-example.sql`, **Save as CSV**.
5. **GitHub Copilot — the big reveal** (~7 min):
   - **NL → SQL** via Copilot Chat with `#mssql` context (grounds on the live connection schema):
     - *"Top 10 customers by lifetime order total — full name, company, total orders, total spend."*
     - *"Monthly revenue for 2008 broken down by product category."*
     - *"Products that have never been ordered."*
   - **Inline completion** — type `-- Top 5 products by quantity sold, including product name and category` → Tab to accept.
   - **Explain existing SQL** — select a query → `@workspace /explain`.
   - **Fix-it** — paste broken query (missing `GROUP BY`, `LEFT JOIN` downgraded to `INNER JOIN` by WHERE on right-side table) → Copilot diagnoses + fixes.
   - **Generate DDL + test data** — `CREATE TABLE SalesLT.ProductReview` + 10 INSERTs.

### Key talking points the squad should have ready

- **`#mssql` chat participant** is what makes Copilot grounded in the connected schema — no manual schema paste.
- **Trust boundary:** MSSQL extension uses the same TDS protocol as SSMS — not a wrapper or transpiler. Works against SQL Server, Azure SQL DB, Managed Instance, Synapse.
- **Three auth modes**: Entra ID (MFA), SQL Login, Windows/Integrated. `trustServerCertificate: true` is fine for this demo's self-signed cert, NOT prod.
- **Copilot fix-it pattern**: Copilot frequently catches the "`LEFT JOIN` + `WHERE` on right-side table silently becomes `INNER JOIN`" bug — a classic T-SQL gotcha. Move the filter into the `ON`.
- **Cross-platform:** Windows, macOS, Linux — same experience.
- **Query plans:** right-click editor tab → *Explain Query Plan* shows visual plan.
- **T-SQL debug / stepping:** available via **SQL Database Projects** extension (`ms-mssql.sql-database-projects-vscode`).
- **Source control for sprocs:** SQL Database Projects + git — real diffs on `.sql` files.
- **Copilot & private data:** Copilot Chat for Business does not retain prompts. Copilot Enterprise supports self-hosted models for fully offline scenarios.

### Question Routing for Demo Q&A

- **"What is the MSSQL extension?"** → Kaylee: Microsoft's official SQL Server client for VS Code (`ms-mssql.mssql`). Connection mgr, Object Explorer, query editor, results grid, CSV/JSON/Excel export.
- **"How does Copilot know my schema?"** → Kaylee: The `#mssql` chat participant feeds the active connection's schema into Copilot context. No manual paste.
- **"Can I debug T-SQL / step through a sproc?"** → Kaylee: Yes, via the **SQL Database Projects** extension.
- **"Does Copilot work on Azure SQL DB / Managed Instance / Synapse?"** → Kaylee: Yes — anything TDS-compatible.
- **"How do I authenticate — Entra ID vs SQL Login?"** → Kaylee: Entra ID is passwordless + MFA through your browser, no password stored; SQL Login is the fallback for air-gapped or legacy.
- **"What about query plans?"** → Jayne: Right-click the editor tab → *Explain Query Plan*.
- **"Can Copilot fix broken SQL?"** → Jayne: Yes — paste it into chat with `#mssql Fix this query:`. It catches missing GROUP BYs, bad joins, and more.
- **"Source control for stored procs?"** → Mal: SQL Database Projects + git — `.sql` files diff cleanly.
- **"Will my SSMS snippets / keybinds work?"** → Kaylee: Copy snippets into VS Code User Snippets; same T-SQL syntax. Ctrl+Shift+E runs queries.
- **"Is my data sent to Copilot?"** → Mal: Copilot Chat for Business doesn't retain prompts. For fully offline, Copilot Enterprise supports private models.
- **"How do I remediate a slow/broken query with Copilot?"** → Jayne: Select the query → Copilot Chat → `#mssql Explain this query's performance and suggest an index or rewrite`. Combine with *Explain Query Plan*.
- **General SQL / T-SQL / schema questions** → Kaylee (technical), Mal (architectural)
- **Testing / query correctness / query plans** → Jayne
- **UI / results grid / VS Code ergonomics** → Wash
