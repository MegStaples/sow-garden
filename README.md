# SOW Garden 🌱

A Claude (Cowork) plugin for managing your professional services SOW pipeline — draft, scope, scan, and report on Statements of Work from one place.

## Skills

| Skill | Trigger phrases |
|---|---|
| **Open dashboard** | "open sow garden", "show my pipeline" |
| **Draft a SOW** | "draft a SOW for Acme", "write a SOW" |
| **Inbox scan** | "scan my inbox", "any SOW messages", "tend the garden" |
| **Status update** | "pipeline status", "where are my SOWs", "how's the pipeline" |
| **Scoping** | "scope this engagement", "scoping questions for [customer]" |

## Quick start

1. Clone this repo
2. Fill in the `{{VARIABLES}}` — see **[SETUP.md](SETUP.md)** for every variable and how to find it
3. In Cowork: **Settings → Plugins → Install from folder**
4. Connect Google Drive, Gmail, and your work chat in **Settings → Connectors**

## Required connections

- **Google Drive** — reads/creates SOW files and pipeline data
- **Gmail** — inbox scan for SOW-related emails
- **Work Chat** (Slack, Teams, 8x8 Work, etc.) — chat scan for pipeline signals

## How it works

```
You ──► Claude (Cowork)
              │
    ┌─────────┼──────────┐
    ▼         ▼          ▼
 Gmail    Google      Work Chat
          Drive
    │         │
    └────►  sow-dashboard-data.json
              (your pipeline state)
```

Claude reads your Gmail + chat for signals, drafts SOWs from your Drive templates, and
maintains a pipeline view from a JSON file you control.

## Adapting this for your org

This plugin was originally built for a PS/implementation team. To adapt it:

- Replace the reference guide with your own delivery playbook
- Update the scoping questions to match your service catalog
- Rename "SOW" to whatever your org calls these documents (MSA, proposal, engagement letter, etc.)
- The Apps Script in SETUP.md can auto-sync your pipeline from any Google Sheet

## License

MIT — fork freely, adapt for your team.
