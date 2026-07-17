# SOW Garden — Setup Guide

This plugin works with Claude (via Cowork) to manage your professional services SOW pipeline.
Before using it, you need to replace the `{{VARIABLE}}` placeholders in the skill files
with values from your own environment.

---

## Step 1 — Fork or clone this repo

```bash
git clone https://github.com/YOUR_USERNAME/sow-garden.git
```

---

## Step 2 — Install the plugin in Cowork

In Claude's Cowork desktop app:
1. Go to **Settings → Plugins**
2. Click **Install from folder**
3. Select the cloned `sow-garden` directory

---

## Step 3 — Connect your tools

SOW Garden requires these Cowork connectors. Connect them in **Settings → Connectors**:

| Connector | Used for |
|---|---|
| **Google Drive** | Reading/creating SOW files and pipeline data |
| **Gmail** | Inbox scan for SOW-related emails |
| **Work Chat** (Slack, Teams, 8x8 Work, etc.) | Chat scan for pipeline signals |

---

## Step 4 — Find your MCP tool prefixes

The inbox-scan skill references your Gmail and Work Chat connectors by their MCP tool prefix.
These are auto-assigned GUIDs when you connect a tool — you need to find yours.

**How to find them:**
1. In Cowork, open a new conversation
2. Type: `what gmail tools do I have available?` — Claude will list tools with names like `mcp__abc123__search_threads`
3. The part before `__search_threads` is your `{{GMAIL_MCP_TOOL_PREFIX}}`
4. Repeat for your work chat connector: `what work chat / Slack tools do I have?`

Then update `skills/inbox-scan/SKILL.md`:
- Replace `{{GMAIL_MCP_TOOL_PREFIX}}` with your value (e.g. `mcp__2cef1217-8448-428a-8b58-7e0b78a9c0ff`)
- Replace `{{WORK_CHAT_MCP_TOOL_PREFIX}}` with your value (e.g. `mcp__8a44cf5b-e031-47e0-8411-43ffb28c7595`)

---

## Step 5 — Set up your Google Drive resources

The drafting and status skills reference specific files and folders by their Google Drive IDs.
You need to create your own versions and plug in the IDs.

### How to find a Google Drive file or folder ID

Open the file or folder in Google Drive. The ID is the long string in the URL:
```
https://drive.google.com/drive/folders/THIS_PART_IS_THE_ID
https://docs.google.com/document/d/THIS_PART_IS_THE_ID/edit
```

### Files/folders to create and configure

| Variable | What it is | Where to use it |
|---|---|---|
| `{{SOW_TEMPLATE_FILE_ID}}` | Your standard SOW template (Google Doc) | `skills/sow-drafting/SKILL.md` |
| `{{SOWS_FOLDER_ID}}` | Google Drive folder where completed SOWs are saved | `skills/sow-drafting/SKILL.md` |
| `{{PSIC_BIBLE_FILE_ID}}` | Your PS reference guide / delivery playbook (Google Doc) | `skills/sow-drafting/SKILL.md`, `skills/scoping/SKILL.md` |
| `{{TEMPLATES_SHEET_ID}}` | Google Sheet with template variations by offering type | `skills/sow-drafting/SKILL.md` |
| `{{CAPABILITY_MATRIX_ID}}` | Integration or capability reference doc | `skills/sow-drafting/SKILL.md` |
| `{{DASHBOARD_DATA_FOLDER_ID}}` | Folder that holds `sow-dashboard-data.json` | `skills/sow-drafting/SKILL.md`, `skills/status-update/SKILL.md` |
| `{{REFERENCE_SOW_OWNER_EMAIL}}` | Email of a colleague whose existing SOWs are good style references | `skills/sow-drafting/SKILL.md` |

### Minimum required files

At minimum you need:
- A **SOWs folder** — create a folder in Drive, get its ID
- A **dashboard data folder** — create a folder, and add a `sow-dashboard-data.json` file (see format below)

Everything else (template, bible, capability matrix) is optional — remove those lines from the skill files if you don't have them.

---

## Step 6 — Set up the pipeline data file

The status-update and dashboard skills read from a JSON file called `sow-dashboard-data.json`
in your `{{DASHBOARD_DATA_FOLDER_ID}}` folder.

### File format

```json
{
  "updated": "2026-07-17",
  "sows": [
    {
      "customer": "Acme Corp",
      "stage": "drafting",
      "priority": "urgent",
      "next_action": "Send revised pricing by Friday",
      "open_scoping": false,
      "notes": "Legal review pending"
    },
    {
      "customer": "Globex",
      "stage": "scoping",
      "priority": "normal",
      "next_action": "Schedule discovery call",
      "open_scoping": true,
      "notes": "Need to confirm integration requirements"
    }
  ]
}
```

**Stage values:** `lead`, `scoping`, `drafting`, `review`, `signed`, `delivered`
**Priority values:** `urgent`, `normal`, `low`

You can maintain this file manually, or set up a Google Apps Script to auto-update it
(see the optional Apps Script section below).

---

## Step 7 — Update plugin.json

In `.claude-plugin/plugin.json`, replace `{{YOUR_NAME}}` with your name.

---

## Optional — Google Apps Script automation

If you want `sow-dashboard-data.json` to be auto-updated from a Google Sheet:

1. Create a Google Sheet with columns: Customer, Stage, Priority, Next Action, Open Scoping, Notes
2. Open **Extensions → Apps Script** in the sheet
3. Paste the script below, fill in your folder ID, and set a time-based trigger to run it daily

```javascript
function exportSOWDashboard() {
  const DASHBOARD_FOLDER_ID = 'YOUR_DASHBOARD_DATA_FOLDER_ID'; // replace this
  
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const rows = sheet.getDataRange().getValues();
  const headers = rows[0].map(h => h.toString().toLowerCase().replace(/ /g, '_'));
  
  const sows = rows.slice(1)
    .filter(row => row[0]) // skip empty rows
    .map(row => {
      const obj = {};
      headers.forEach((h, i) => obj[h] = row[i]);
      return obj;
    });
  
  const data = {
    updated: new Date().toISOString().split('T')[0],
    sows: sows
  };
  
  const folder = DriveApp.getFolderById(DASHBOARD_FOLDER_ID);
  const fileName = 'sow-dashboard-data.json';
  const files = folder.getFilesByName(fileName);
  
  if (files.hasNext()) {
    files.next().setContent(JSON.stringify(data, null, 2));
  } else {
    folder.createFile(fileName, JSON.stringify(data, null, 2), MimeType.PLAIN_TEXT);
  }
  
  Logger.log('Dashboard updated: ' + sows.length + ' SOWs');
}
```

4. In Apps Script: **Triggers → Add trigger → exportSOWDashboard → Time-driven → Day timer**

---

## Variables quick-reference

| Variable | File(s) |
|---|---|
| `{{YOUR_NAME}}` | `.claude-plugin/plugin.json` |
| `{{GMAIL_MCP_TOOL_PREFIX}}` | `skills/inbox-scan/SKILL.md` |
| `{{WORK_CHAT_MCP_TOOL_PREFIX}}` | `skills/inbox-scan/SKILL.md` |
| `{{SOW_TEMPLATE_FILE_ID}}` | `skills/sow-drafting/SKILL.md` |
| `{{SOWS_FOLDER_ID}}` | `skills/sow-drafting/SKILL.md` |
| `{{PSIC_BIBLE_FILE_ID}}` | `skills/sow-drafting/SKILL.md`, `skills/scoping/SKILL.md` |
| `{{TEMPLATES_SHEET_ID}}` | `skills/sow-drafting/SKILL.md` |
| `{{CAPABILITY_MATRIX_ID}}` | `skills/sow-drafting/SKILL.md` |
| `{{REFERENCE_SOW_OWNER_EMAIL}}` | `skills/sow-drafting/SKILL.md` |
| `{{DASHBOARD_DATA_FOLDER_ID}}` | `skills/sow-drafting/SKILL.md`, `skills/status-update/SKILL.md` |

---

## Troubleshooting

**"Can't find my MCP tool prefix"** — In Cowork, ask Claude: *"list all your available tools"* and look for tool names starting with `mcp__`. The GUID portion is your prefix.

**"Dashboard shows no data"** — Make sure `sow-dashboard-data.json` exists in the right folder and is valid JSON. You can validate it at [jsonlint.com](https://jsonlint.com).

**"Drive file not found"** — Make sure the file is shared with the Google account you connected to Cowork (not just in your personal Drive if using a different org account).

**"Skill not triggering"** — After editing any SKILL.md, restart Cowork or re-install the plugin from Settings → Plugins.
