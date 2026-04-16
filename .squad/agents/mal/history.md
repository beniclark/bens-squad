# Project Context

- **Owner:** beclark_microsoft
- **Project:** bens-squad — general-purpose playground for experimenting with Squad, Teams chat integration, and random projects
- **Stack:** Flexible — whatever the experiment calls for
- **Created:** 2026-04-16

## Learnings

### 2026-04-16: Teams Demo — Mal's Role
- Mal posts the **welcome Adaptive Card** at demo start (Step 1 of the sequence)
- Mal does NOT post a separate text intro (the welcome card IS his intro)
- In the monitoring loop, Mal handles: architecture questions, strategy questions, "how does it work" questions, big-picture topics
- Read `.squad/demo-config.md` for the full demo playbook, card templates, and sequence
- Keep responses under ~150 words — Teams API rejects longer messages

### 2026-04-16: Teams MCP Server Operations
- Teams MCP server uses OBO (On-Behalf-Of) tokens that expire after ~1 hour
- When token expires, get `AADSTS500133` error — need server restart
- `Mcp-Session-Id` errors require full VS Code restart (not just MCP server restart)
- Transient `UnexpectedError` responses — retry after 5 seconds usually works

### 2026-04-16: Key File Paths
- Demo playbook: `.squad/demo-config.md` — READ THIS FIRST for any Teams demo task
- Cheatsheet image: `assets/copilot-cli-cheatsheet.png`
- Cheatsheet URL: `https://raw.githubusercontent.com/beniclark/bens-squad/main/assets/copilot-cli-cheatsheet.png`
