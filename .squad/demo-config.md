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
    {"type": "TextBlock", "text": "🚀 Welcome to the Copilot CLI Demo!", "weight": "Bolder", "size": "Large"},
    {"type": "TextBlock", "text": "Your crew is here and ready to roll. Ask us anything — we're monitoring this chat live.", "wrap": true},
    {"type": "TextBlock", "text": "👥 Meet the Squad", "weight": "Bolder", "size": "Medium", "spacing": "Medium"},
    {"type": "FactSet", "facts": [
      {"title": "🏗️ Mal", "value": "Squad Lead — architecture, strategy, big picture"},
      {"title": "🔧 Kaylee", "value": "General Dev — code, setup, integrations, how-to"},
      {"title": "⚛️ Wash", "value": "Frontend Dev — UI, VS Code, design, developer experience"},
      {"title": "🧪 Jayne", "value": "Tester — security, quality, edge cases, breaking things"}
    ]},
    {"type": "TextBlock", "text": "📎 Useful Links", "weight": "Bolder", "size": "Medium", "spacing": "Medium"},
    {"type": "TextBlock", "text": "These will be relevant throughout today's demo:", "wrap": true},
    {"type": "FactSet", "facts": [
      {"title": "🚀 Copilot CLI", "value": "Product page — features, pricing, getting started"},
      {"title": "💻 CLI Repo", "value": "Open-source repo with issues, discussions, and releases"},
      {"title": "📝 Customize Your Repo", "value": "Guide to instruction files, custom agents, and repo-level config"},
      {"title": "⭐ Awesome Copilot", "value": "Curated list of Copilot extensions, tools, and resources"},
      {"title": "📰 Multi-Model Blog", "value": "How CLI combines model families for a second opinion"},
      {"title": "🤝 Squad Docs", "value": "Official Squad framework documentation and setup guide"},
      {"title": "🔧 Squad Repo", "value": "Source code for the Squad custom agent framework"}
    ]}
  ],
  "actions": [
    {"type": "Action.OpenUrl", "title": "🚀 Copilot CLI Product Page", "url": "https://github.com/features/copilot/cli/"},
    {"type": "Action.OpenUrl", "title": "💻 Copilot CLI Repo", "url": "https://github.com/github/copilot-cli"},
    {"type": "Action.OpenUrl", "title": "📝 Customize Your Repo Guide", "url": "https://github.com/microsoftnorman/customize-your-repo-with-github-copilot"},
    {"type": "Action.OpenUrl", "title": "⭐ Awesome Copilot", "url": "https://github.com/github/awesome-copilot"},
    {"type": "Action.OpenUrl", "title": "📰 Multi-Model Blog Post", "url": "https://github.blog/ai-and-ml/github-copilot/github-copilot-cli-combines-model-families-for-a-second-opinion/"},
    {"type": "Action.OpenUrl", "title": "🤝 Squad Docs", "url": "https://bradygaster.github.io/squad/"},
    {"type": "Action.OpenUrl", "title": "🔧 Squad Repo", "url": "https://github.com/bradygaster/squad"}
  ]
}
```

### Step 2: Cheatsheet Card (posted by Kaylee)

```json
{
  "body": [
    {"type": "TextBlock", "text": "📋 GitHub Copilot CLI Cheatsheet", "weight": "Bolder", "size": "Medium"},
    {"type": "TextBlock", "text": "By @pvergadia / thecloudgirl.dev", "isSubtle": true, "size": "Small"},
    {"type": "Image", "url": "https://raw.githubusercontent.com/beniclark/bens-squad/main/assets/copilot-cli-cheatsheet.png", "size": "Stretch"}
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

### Step 5: Enter Monitoring Loop

After all intros are posted, enter the autonomous monitoring loop (see below).

---

## Link Reminder Card (Post Every 10-15 Minutes)

Rotate which agent posts the reminder. Use this Adaptive Card:

```json
{
  "body": [
    {"type": "TextBlock", "text": "📌 Demo Links Reminder", "weight": "Bolder", "size": "Medium"},
    {"type": "TextBlock", "text": "These are relevant to what we're covering today — grab them while you're here!", "wrap": true, "spacing": "Small"},
    {"type": "FactSet", "facts": [
      {"title": "🚀 Copilot CLI", "value": "Product page — features, pricing, getting started"},
      {"title": "💻 CLI Repo", "value": "Open-source repo with issues, discussions, and releases"},
      {"title": "📝 Customize Your Repo", "value": "Guide to instruction files, custom agents, and repo-level config"},
      {"title": "⭐ Awesome Copilot", "value": "Curated list of Copilot extensions, tools, and resources"},
      {"title": "📰 Multi-Model Blog", "value": "How CLI combines model families for a second opinion"},
      {"title": "🤝 Squad Docs", "value": "Official Squad framework documentation and setup guide"},
      {"title": "🔧 Squad Repo", "value": "Source code for the Squad custom agent framework"}
    ]}
  ],
  "actions": [
    {"type": "Action.OpenUrl", "title": "🚀 Copilot CLI Product Page", "url": "https://github.com/features/copilot/cli/"},
    {"type": "Action.OpenUrl", "title": "💻 Copilot CLI Repo", "url": "https://github.com/github/copilot-cli"},
    {"type": "Action.OpenUrl", "title": "📝 Customize Your Repo Guide", "url": "https://github.com/microsoftnorman/customize-your-repo-with-github-copilot"},
    {"type": "Action.OpenUrl", "title": "⭐ Awesome Copilot", "url": "https://github.com/github/awesome-copilot"},
    {"type": "Action.OpenUrl", "title": "📰 Multi-Model Blog Post", "url": "https://github.blog/ai-and-ml/github-copilot/github-copilot-cli-combines-model-families-for-a-second-opinion/"},
    {"type": "Action.OpenUrl", "title": "🤝 Squad Docs", "url": "https://bradygaster.github.io/squad/"},
    {"type": "Action.OpenUrl", "title": "🔧 Squad Repo", "url": "https://github.com/bradygaster/squad"}
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
| General | `https://media.giphy.com/media/xT0xeJpnrWC3XWblEk/giphy.gif` | Mind blown |
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
6. **Check** if it's time for a link reminder (every 10-15 minutes)
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
