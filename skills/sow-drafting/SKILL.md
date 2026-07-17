---
name: sow-drafting
description: >
  Draft a new Statement of Work (SOW) for a customer. Use when the user says "draft a SOW",
  "write a SOW", "create a statement of work", "generate SOW", "new SOW for [customer]",
  "SOW draft", "help me write a SOW", or "start a SOW".
---

Draft a new SOW using your templates and reference materials.

## Key resources

| Variable | Description | Value |
|---|---|---|
| Master template | Your standard SOW template file | `{{SOW_TEMPLATE_FILE_ID}}` |
| SOWs folder | Folder where completed SOWs are saved | `{{SOWS_FOLDER_ID}}` |
| Reference guide | PS delivery playbook / bible | `{{PSIC_BIBLE_FILE_ID}}` |
| Templates sheet | Google Sheet with template variations | `{{TEMPLATES_SHEET_ID}}` |
| Capability matrix | Integration or capability reference | `{{CAPABILITY_MATRIX_ID}}` |
| Reference SOW owner | Email of colleague with good example SOWs | `{{REFERENCE_SOW_OWNER_EMAIL}}` |
| Dashboard data folder | Folder where pipeline JSON lives | `{{DASHBOARD_DATA_FOLDER_ID}}` |

> **Setup note:** Replace all `{{...}}` values above with your own Google Drive file/folder IDs
> and email addresses. See SETUP.md for step-by-step instructions.

## Tools

Use Google Drive MCP tools to read and create files.

## Process

1. **Gather requirements** — Ask for: customer name, offering type (T&M / fixed price / managed service),
   rough scope, estimated hours/days, and target start date. If the user already provided these, skip to drafting.

2. **Read reference material** — Read the PS reference guide (`{{PSIC_BIBLE_FILE_ID}}`) to inform
   pricing, scope descriptions, and service definitions.

3. **Find reference SOWs** — Search `{{REFERENCE_SOW_OWNER_EMAIL}}`'s Drive for existing SOWs
   matching the offering type. Pull up to 2 examples (max 5000 chars each) for stylistic reference.

4. **Draft the SOW** — Produce a complete SOW with: executive summary, scope of work, deliverables,
   timeline, pricing table, assumptions, out-of-scope items, and acceptance criteria.
   Match the style from the reference examples.

5. **Offer to save** — Ask if the user wants the SOW saved as a Google Doc in the SOWs folder
   (`{{SOWS_FOLDER_ID}}`). If yes, use the Drive create_file tool.

Keep the tone professional and concise. Flag any gaps in requirements (use the `/scoping` skill if needed).
