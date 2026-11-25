---
name: nubint-onboarding
description: Connect to Nubint and introduce what it does. Use when the user installs or first uses the Nubint plugin, asks what Nubint can do, or asks to check that the connection works. Do not use for an actual review, edit, search or literature-review request — use the matching workflow skill.
---

# Start with Nubint

Use only this plugin's `nubint` MCP connection. Authenticate through the host's supported connection flow (OAuth in the browser). Never ask the user to paste a password, token or API key into the conversation. If a tool you expect is missing from discovery, say so; do not invent a replacement call.

## Check the connection

1. Call `get_credits`. It costs nothing and confirms both the login and the plan: it returns `plan`, `creditsRemaining`, `creditsPerMonth`.
2. If it fails with an authentication error, guide the user through the host's Connect / Authorize step for this plugin and try again once they finish. Do not loop the login.

Do not create a document or review just to test the connection.

## Introduce Nubint (in the user's language)

Nubint is an AI research-writing workspace with a scholarly paper index (OpenAlex + KCI). Through this plugin you can:

- **Find and save papers** — `search_papers` (scholarly index, real papers only), `search_library` and `save_to_library` (the user's Nubint library). No credits.
- **Check a manuscript** — verify that every reference is a real, findable work (`run_manuscript_check` with `checks: ["reference_verification"]`, no credits); detect AI-written text and weak sources (credits).
- **Review a manuscript** — save it as a Nubint document (`save_manuscript`) and run Nubint's reviews: peer review, claim credibility, source quality, proofreading, tone, source overlap, AI detection (`start_review`, credits). See the nubint-manuscript-review skill.
- **Edit a document side by side** — the user keeps the document open in the Nubint editor; you read it (`read_document`) and propose edits (`edit_document`) that appear there as suggestions to accept or reject. See the nubint-document-edit skill.
- **Literature review** — a cited review written from the index (`start_literature_review`, credits). See the nubint-literature skill.

Plans: every tool needs the Starter plan or above. On the Free plan every tool, `get_credits` included, answers with a plan notice — that still shows the connection works; relay it as it is and suggest upgrading. Searching, saving, listing, reading and editing documents and reference verification use no credits.

State that nothing was created and no credits were spent by this introduction. Offer two or three example requests that fit what the user is working on (for example their open `.tex` or `.md` manuscript, or a research question).
