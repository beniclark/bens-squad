# Project Context

- **Owner:** beclark_microsoft
- **Project:** bens-squad — general-purpose playground for experimenting with Squad, Teams chat integration, and random projects
- **Stack:** Flexible — whatever the experiment calls for
- **Created:** 2026-04-16

## Learnings

### 2026-04-16: Teams Demo — Kaylee's Role
- Kaylee is the primary Teams integration agent
- Posts the **cheatsheet Adaptive Card** (Step 2 of the demo sequence)
- Posts her **text intro** + **intro GIF** (Step 3-4)
- In the monitoring loop, handles: technical questions, code questions, setup/integration questions
- Also handles posting **link reminder cards** every 10-15 minutes (rotate with other agents)
- Read `.squad/demo-config.md` for the full demo playbook, card templates, and sequence

### 2026-04-16: Teams MCP Tools
- `ListChatMessages(chatId, top)` — reads messages newest first; `top` limits count
- `PostMessage(chatId, content, contentType, adaptiveCardJson)` — posts to chat
- `SendMessageToSelf(content, contentType, adaptiveCardJson)` — preview to Notes to Self
- Use `contentType: "html"` for rich formatting with raw HTML tags (`<b>`, `<br>`, `<a href>`)
- **NEVER** use HTML entities (`&lt;b&gt;`) — they get double-encoded
- `adaptiveCardJson` takes the card JSON string; `content` carries fallback text alongside the card

### 2026-04-16: GIF Posting
- GIFs are part of Kaylee's persona — post them naturally
- Use Adaptive Cards with `{"type": "Image", "url": "...", "size": "Medium"}`
- Kaylee's intro GIF: `https://media.giphy.com/media/3o7btNa0RUYa5E7iiQ/giphy.gif`
- Always verify GIF URLs before posting (no 404s)
- Keep responses under ~150 words — longer messages fail with `UnexpectedError`

### 2026-04-16: Teams MCP Server Troubleshooting
- OBO tokens expire after ~1 hour → server restart needed
- `Mcp-Session-Id` errors → full VS Code restart required
- Transient `UnexpectedError` → retry after 5 seconds
- Ben's user ID for filtering: `228ff80d-058c-41f6-b149-0a457537241a`

### 2026-04-16: Key File Paths
- Demo playbook: `.squad/demo-config.md` — READ THIS FIRST for any Teams demo task
- Cheatsheet image: `assets/copilot-cli-cheatsheet.png`
- Cheatsheet URL: `https://raw.githubusercontent.com/beniclark/bens-squad/main/assets/copilot-cli-cheatsheet.png`
