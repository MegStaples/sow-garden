---
name: scoping
description: >
  Guide the user through scoping questions for a SOW or customer engagement. Use when the user says
  "scope this", "help me scope", "scoping questions", "what do I need to scope",
  "I need to scope [customer]", "let's scope this out", "run scoping", or
  "what questions should I ask".
---

Guide the user through structured scoping for a professional services engagement.

## Tools

Optionally read the reference guide (`{{PSIC_BIBLE_FILE_ID}}`) from Google Drive for
service-specific scoping requirements.

> **Setup note:** `{{PSIC_BIBLE_FILE_ID}}` is the Google Drive file ID of your scoping
> reference document (e.g. a PS bible, delivery playbook, or capability guide).
> See SETUP.md for how to find a file's ID.

## Process

1. **Identify the engagement** — Ask which customer and offering type if not already stated.
   Common types: T&M implementation, managed service, fixed-price project, training.

2. **Run through scoping questions** — Cover these areas in a conversational flow
   (go section by section, not all at once):

   - **Customer environment**: Current stack, number of users/sites, regions, existing product footprint
   - **Integration requirements**: CRM, ticketing, custom integrations needed
   - **Timeline**: Go-live target, any hard deadlines, executive sponsors
   - **Complexity signals**: Multiple phases? Custom dev? Third-party dependencies?
   - **Commercial**: T&M vs fixed price preference, budget range, who signs off
   - **Risks / open items**: Known unknowns, blockers, anything unusual

3. **Summarize the scope** — After gathering answers, produce a concise scope summary with:
   - Recommended offering type and rough effort estimate
   - Key deliverables
   - Assumptions and out-of-scope items
   - Open questions still needing answers

4. **Next steps** — Offer to feed the summary into a SOW draft (trigger the `sow-drafting` skill)
   or save it as a note in Drive.

Be conversational — the user may be mid-customer-call or preparing for one. Keep questions short and scannable.
