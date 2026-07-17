---
name: status-update
description: >
  Generate a pipeline status report from the SOW Garden. Use when the user says
  "status update", "pipeline report", "SOW status", "what's the status of my SOWs",
  "pipeline summary", "give me a status", "weekly update", "where are my SOWs",
  "what's in flight", or "how's the pipeline looking".
---

Generate a crisp SOW pipeline status report.

## Tools

Use Google Drive MCP to read `sow-dashboard-data.json` from folder `{{DASHBOARD_DATA_FOLDER_ID}}`.

> **Setup note:** `{{DASHBOARD_DATA_FOLDER_ID}}` is the Google Drive folder ID where your
> pipeline JSON file lives. See SETUP.md for details.

## Process

1. Read `sow-dashboard-data.json` from your SOW dashboard data folder.
2. Parse the pipeline entries.
3. Produce the status report below.

## Report format

**SOW Pipeline — [today's date]**

**At a glance:** X total | Y urgent | Z scoping | W drafting

Then for each SOW, one line:
`[Customer] — [stage] — [next action / blocker if any]`

Flag urgent items with ⚠️. Flag items with open scoping questions with ❓.

After the list, add a **"Focus for this week"** section (2–3 bullets) highlighting
the highest-priority items to act on today.

Keep the whole report under 30 lines. If asked for more detail on a specific SOW, expand that entry only.
