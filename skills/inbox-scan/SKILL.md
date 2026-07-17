---
name: inbox-scan
description: >
  Scan my inbox and work chat for SOW-related messages, customer follow-ups, and
  pipeline signals. Use when the user says "scan my inbox", "check for SOW emails",
  "what's new about my SOWs", "any SOW messages", "inbox scan", "check messages",
  "what's come in", "any updates from customers", or "tend the garden".
---

Scan Gmail and work chat for SOW pipeline signals and surface actionable items.

## Tools

- Gmail: `{{GMAIL_MCP_TOOL_PREFIX}}__search_threads`
- Work Chat: `{{WORK_CHAT_MCP_TOOL_PREFIX}}__get_unread_conversations`

> **Setup note:** Replace `{{GMAIL_MCP_TOOL_PREFIX}}` and `{{WORK_CHAT_MCP_TOOL_PREFIX}}`
> with the actual MCP tool prefixes shown in your Cowork connector settings.
> See SETUP.md for how to find these.

## Process

Run both searches in parallel:

**Gmail** — Search for threads matching:
`(SOW OR "statement of work" OR "scoping" OR "proposal" OR "professional services") newer_than:7d`

**Work Chat** — Fetch unread conversations and filter for any mentioning SOW, scoping,
proposals, or customer project names from the pipeline.

## Output format

Group results by urgency:

1. **Needs action** — direct requests, approvals needed, deadlines mentioned
2. **FYI / updates** — status changes, confirmations, informational threads
3. **Noise** — auto-notifications, newsletters (list count only, don't enumerate)

For each actionable item show: source (email/chat), sender, subject/preview, and a one-line suggested next step.

Keep output concise. If nothing relevant found, say so plainly.
