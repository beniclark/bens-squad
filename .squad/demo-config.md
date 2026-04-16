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
    {"type": "TextBlock", "text": "🚀 Welcome! Meet the Squad", "weight": "Bolder", "size": "Large"},
    {"type": "TextBlock", "text": "We're an AI dev team built on GitHub Copilot. Ask us anything — we'll answer right here in the chat!", "wrap": true},
    {"type": "FactSet", "facts": [
      {"title": "🏗️ Mal", "value": "Lead — architecture, decisions, code review"},
      {"title": "🔧 Kaylee", "value": "Dev — code, APIs, integrations"},
      {"title": "⚛️ Wash", "value": "Frontend — UI, components, design"},
      {"title": "🧪 Jayne", "value": "Tester — quality, edge cases, breaking things"}
    ]},
    {"type": "TextBlock", "text": "📎 Grab these links:", "weight": "Bolder", "separator": true}
  ],
  "actions": [
    {"type": "Action.OpenUrl", "title": "GitHub Copilot", "url": "https://github.com/features/copilot"},
    {"type": "Action.OpenUrl", "title": "Copilot Docs", "url": "https://docs.github.com/copilot"},
    {"type": "Action.OpenUrl", "title": "Squad Repo", "url": "https://github.com/bradygaster/squad"}
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

## How to Trigger

| Say this | What happens |
|----------|-------------|
| "Post the welcome card" | Posts the welcome Adaptive Card to the demo chat |
| "Crew, introduce yourselves" | Each agent posts their intro message |
| "Kaylee, answer this: {question}" | Kaylee reads the question and posts a reply |
| "Post a GIF about {topic}" | Agent posts an Adaptive Card with a relevant GIF |
| "Team, someone asked: {question}" | Routes to the best agent, they post the answer |
| "Wrap it up" | Fun closing messages from the crew |

## Important Notes

- ⚠️ All messages appear as **Ben's account** — agents sign with emoji + name
- ⚠️ **DO NOT test-post** in the demo chat before the demo
- Use `SendMessageToSelf` for testing (Notes to Self chat)
- Adaptive Cards support `Action.OpenUrl` only (no submit buttons)
- HTML supports: `<b>`, `<i>`, `<a>`, `<ul>/<li>`, `<pre>`, `<blockquote>`
