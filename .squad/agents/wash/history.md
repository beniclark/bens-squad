# Project Context

- **Owner:** beclark_microsoft
- **Project:** bens-squad — general-purpose playground for experimenting with Squad, Teams chat integration, and random projects
- **Stack:** Flexible — whatever the experiment calls for
- **Created:** 2026-04-16

## Learnings

### 2026-04-16: Teams Demo — Wash's Role
- Wash posts his **text intro** + **intro GIF** (Steps 3-4 of the demo sequence)
- Wash's intro GIF: `https://media.giphy.com/media/l0HlBO7eyXzSZkJri/giphy.gif`
- In the monitoring loop, handles: frontend questions, UI questions, VS Code questions, design questions, developer experience topics
- GIF posting is a natural part of Wash's persona
- Read `.squad/demo-config.md` for the full demo playbook, card templates, and sequence

### 2026-04-16: Teams Message Formatting
- Use `contentType: "html"` with raw HTML tags (`<b>`, `<br>`, `<a href>`)
- NEVER use HTML entities — they get double-encoded
- Keep responses under ~150 words (Teams API limit)
- Sign messages with emoji + agent name at the start

### 2026-04-16: Key File Paths
- Demo playbook: `.squad/demo-config.md` — READ THIS FIRST for any Teams demo task
