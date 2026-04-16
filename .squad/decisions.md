# Squad Decisions

## Active Decisions

### 2026-04-16T01:06Z: Demo prep — Squad in Teams chat
**By:** beclark_microsoft (via Copilot)
**What:** The team's immediate focus is preparing for a customer demo tomorrow. The squad will be present in a Teams meeting chat, answering questions, posting links, sharing GIFs, and making jokes. Target chat: 19:meeting_YmJiOWIyNTgtODIyOC00OTg2LWI2MzEtNjdmMGIwMTE3OWM4@thread.v2
**Why:** User request — demo for a customer on 2026-04-16/17

### 2026-04-16: Teams Chat Demo Architecture
**By:** Ben (via Copilot sessions)
**What:** The squad operates in a Teams chat during live demos. Agents post messages via the Teams MCP server (`TeamsServer`). All messages appear as Ben's account — agents sign with emoji + name. The coordinator runs a 20-second polling loop reading new messages, routing to the right agent, and posting responses.
**Why:** Customer demo format — interactive, shows squad capabilities live.

### 2026-04-16: Welcome Card Format — FactSet with Link Descriptions
**By:** Ben
**What:** The welcome card uses Adaptive Cards with a `FactSet` for both squad member introductions AND link descriptions. Each link has a brief description of what it is (e.g., "Product page — features, pricing, getting started"). Links are also in `Action.OpenUrl` buttons at the bottom.
**Why:** Ben requested descriptions so attendees understand what each link is about before clicking.

### 2026-04-16: Link Reminder Card — Every 10-15 Minutes
**By:** Ben
**What:** During monitoring, post a link reminder card every 10-15 minutes. Uses same FactSet + Action.OpenUrl format as the welcome card. Rotate which agent posts the reminder.
**Why:** Keeps links visible for latecomers and people who missed the initial post.

### 2026-04-16: GIF Posting as Part of Agent Personas
**By:** Ben
**What:** GIF posting is a natural part of each agent's personality. Agents post GIFs with their intro messages, after good answers, during lulls, and to keep energy up. All GIF URLs must be verified (no 404s). Use Adaptive Cards with `Image` type for GIFs.
**Why:** Ben wanted more personality and visual engagement in the chat.

### 2026-04-16: Agent Intro Sequence — Text Then GIF
**By:** Ben
**What:** After the welcome card and cheatsheet, each agent (Kaylee, Wash, Jayne — NOT Mal, who posted the welcome) introduces themselves with a brief HTML message, then follows up with a GIF Adaptive Card.
**Why:** Creates a lively "squad entering the room" feel.

### 2026-04-16: HTML Formatting — Raw Tags Only
**By:** Coordinator (learned from bugs)
**What:** ALWAYS use raw HTML tags (`<b>`, `<br>`, `<a href>`) in Teams messages. NEVER use HTML entities (`&lt;b&gt;`). Previous sessions had double-encoding bugs that showed literal tag text instead of formatting.
**Why:** Teams API interprets content as HTML when `contentType: "html"` — entities get double-escaped.

### 2026-04-16: Message Length Limit — ~150 Words
**By:** Coordinator (learned from errors)
**What:** Keep all agent responses under ~150 words. Longer messages fail with `UnexpectedError` from the Teams API.
**Why:** Discovered empirically during testing — long messages reliably fail.

### 2026-04-16: Ben's User ID for Message Filtering
**By:** Coordinator
**What:** Ben's Teams user ID is `228ff80d-058c-41f6-b149-0a457537241a`. When reading chat messages, filter OUT messages from this ID — those are our own agent posts. Only respond to messages from OTHER users.
**Why:** All agent posts appear as Ben's account via the MCP server.

### 2026-04-16: Demo Topics to Expect
**By:** Ben
**What:** The demo covers: GitHub Copilot CLI, Squad framework, instruction files (.github/copilot-instructions.md), agent mode, MCP servers, custom agents. Agents should be ready for questions on these topics.
**Why:** FSI customer demo context.

## Governance

- All meaningful changes require team consensus
- Document architectural decisions here
- Keep history focused on work, decisions focused on direction
