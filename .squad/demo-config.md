# Squad Demo Mode

> Configuration for live Teams chat demos. The crew posts directly into a Teams chat,
> answering questions, sharing links, posting GIFs, and having fun.

## Target Chat

```
chatId: 19:meeting_YmJiOWIyNTgtODIyOC00OTg2LWI2MzEtNjdmMGIwMTE3OWM4@thread.v2
```

## Welcome Card (Post at Session Start)

Adaptive Card JSON — update the links before posting:

```json
{
  "body": [
    {"type": "TextBlock", "text": "🚀 Welcome to the Copilot CLI Demo!", "weight": "Bolder", "size": "Large"},
    {"type": "TextBlock", "text": "We're an AI dev team built on GitHub Copilot. Ask us anything — we'll answer right here in the chat!", "wrap": true},
    {"type": "FactSet", "facts": [
      {"title": "🏗️ Mal", "value": "Lead — architecture, decisions, code review"},
      {"title": "🔧 Kaylee", "value": "Dev — code, APIs, integrations"},
      {"title": "⚛️ Wash", "value": "Frontend — UI, components, design"},
      {"title": "🧪 Jayne", "value": "Tester — quality, edge cases, breaking things"}
    ]},
    {"type": "TextBlock", "text": "📎 These links will be relevant throughout the demo — grab them now:", "weight": "Bolder", "separator": true}
  ],
  "actions": [
    {"type": "Action.OpenUrl", "title": "🚀 Copilot CLI", "url": "https://github.com/features/copilot/cli/"},
    {"type": "Action.OpenUrl", "title": "📦 Copilot CLI Repo", "url": "https://github.com/github/copilot-cli"},
    {"type": "Action.OpenUrl", "title": "📝 Blog: Model Families in CLI", "url": "https://github.blog/ai-and-ml/github-copilot/github-copilot-cli-combines-model-families-for-a-second-opinion/"},
    {"type": "Action.OpenUrl", "title": "🎨 Customize Your Repo", "url": "https://github.com/microsoftnorman/customize-your-repo-with-github-copilot"},
    {"type": "Action.OpenUrl", "title": "⭐ Awesome Copilot", "url": "https://github.com/github/awesome-copilot"},
    {"type": "Action.OpenUrl", "title": "🤖 Squad Docs", "url": "https://bradygaster.github.io/squad/"},
    {"type": "Action.OpenUrl", "title": "💻 Squad Repo", "url": "https://github.com/bradygaster/squad"}
  ]
}
```

## Agent Intro Messages

Each agent posts a short intro after the welcome card.

### Mal (Lead)
```
🏗️ **Mal** — I keep the ship pointed in the right direction. Architecture, scope, code review — that's my lane. Ask me about how we're built or what decisions we've made.
```

### Kaylee (General Dev)
```
🔧 **Kaylee** — I make things work! APIs, scripts, integrations — if it needs building, I'm on it. I also handle our Teams integration (yes, I'm posting this myself 😄).
```

### Wash (Frontend Dev)
```
⚛️ **Wash** — I handle the pretty stuff. React, UI, components — anything the user sees. Got a frontend question? Fire away!
```

### Jayne (Tester)
```
🧪 **Jayne** — I break things so you don't have to. Tests, edge cases, quality gates. Try to stump me. 💪
```

## Fun GIF URLs (Verified Working in Adaptive Cards)

| Occasion | URL |
|----------|-----|
| Celebration | `https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExcDdkMnJ5OGtiZHN0OXM0Y2Zkcmg2N3VhMWtjamVjbHo3bHRsb3k0MSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3o7abKhOpu0NwenH3O/giphy.gif` |
| Mind blown | `https://media.giphy.com/media/xT0xeJpnrWC3XWblEk/giphy.gif` |
| Thumbs up | `https://media.giphy.com/media/111ebonMs90YLu/giphy.gif` |
| Ship it | `https://media.giphy.com/media/143vPc6b08locw/giphy.gif` |

### GIF Card Template
```json
{
  "body": [
    {"type": "TextBlock", "text": "🔧 **Kaylee**", "weight": "Bolder"},
    {"type": "Image", "url": "{gif_url}", "size": "Medium"},
    {"type": "TextBlock", "text": "{caption}", "wrap": true}
  ]
}
```

## Autonomous Monitoring Mode

When Ben says **"monitor the chat"** (or includes it in his initial prompt), the coordinator enters a polling loop:

1. **Read** all new messages from the demo chat (via `ListChatMessages`)
2. **Filter** — skip messages from Ben's own account (those are agent posts), skip system messages
3. **Identify** questions or topics that need a response
4. **Route** to the right agent based on topic:
   - Technical / code → 🔧 Kaylee
   - Architecture / strategy / "how does it work" → 🏗️ Mal
   - UI / frontend / design → ⚛️ Wash
   - Testing / quality / "how do you test" → 🧪 Jayne
   - Fun / banter / general → whoever fits best (rotate)
5. **Post** the agent's response to the demo chat
6. **Sleep 30 seconds**
7. **Repeat**

### Stopping
- Ben posts **"stop monitoring"** or **"monitoring off"** in the Teams chat
- The coordinator sees it in the next cycle and exits the loop

### Important
- While monitoring, Ben **cannot type other commands** to the CLI — the turn is active
- That's fine for a demo — Ben is presenting, not typing
- The monitoring loop tracks seen message IDs to avoid double-responding
- All agent responses still appear as Ben's account, signed with emoji + agent name

## How to Trigger

Triggers are natural language — the coordinator matches intent, not exact phrases.

| Intent | Examples | What happens |
|--------|----------|-------------|
| Enter the chat | "Squad, join the chat", "Crew, enter the meeting", "Post the welcome" | Posts welcome card + agent intros |
| Monitor the chat | "Start monitoring", "Watch for questions", "Keep an eye on the chat" | Enters 30-second polling loop |
| Answer a question | "Kaylee, answer this: {question}", "Team, someone asked: {question}" | Routes to best agent, posts reply |
| Post a GIF | "Post a GIF about {topic}", "Kaylee, send a celebration GIF" | Agent posts Adaptive Card with GIF |
| Wrap up | "Wrap it up", "Say goodbye", "Crew, sign off" | Closing messages from the crew |
| Stop monitoring | "Stop monitoring", "Monitoring off" (in Teams chat or CLI) | Exits the polling loop |

## Important Notes

- ⚠️ All messages appear as **Ben's account** — agents sign with emoji + name
- ⚠️ **DO NOT test-post** in the demo chat before the demo
- Use `SendMessageToSelf` for testing (Notes to Self chat)
- Adaptive Cards support `Action.OpenUrl` only (no submit buttons)
- HTML supports: `<b>`, `<i>`, `<a>`, `<ul>/<li>`, `<pre>`, `<blockquote>`
