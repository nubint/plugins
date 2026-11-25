# Nubint plugins

Use [Nubint](https://nubint.ai) — an AI research-writing workspace with a scholarly paper index — from **Codex** and **Claude Code**.

- Verify that every reference in your manuscript is a real paper
- Run peer review, claim-credibility, source-quality and proofreading reviews
- Find and save real papers, and run a cited literature review
- Propose edits to a document open in the Nubint editor — you accept or reject each one

## Install

Paste this into Codex or Claude Code:

    Read https://github.com/nubint/plugins/blob/main/AGENTS.md and install Nubint for this app, then connect my Nubint account.

Or install manually:

| Host | Commands |
| --- | --- |
| Codex | `codex plugin marketplace add nubint/plugins --ref main` then `codex plugin add nubint@nubint-marketplace` |
| Claude Code | `claude plugin marketplace add nubint/plugins` then `claude plugin install nubint@nubint-marketplace` |

Then sign in to Nubint when the app asks (OAuth).

**ChatGPT and Claude (chat apps):** add the connector URL `https://api.nubint.ai/api/v1/mcp` in the app's connector / plugin settings instead.

## Plans

Every tool needs the Starter plan or above — on the Free plan you can connect, and each tool answers with a plan notice. Paper and library search, saving manuscripts, reading and editing your documents and reference verification use no credits. Reviews, AI-writing detection, source quality and literature reviews use credits.

## Contents

```
.agents/plugins/marketplace.json        Codex marketplace
.claude-plugin/marketplace.json         Claude Code marketplace
plugins/nubint/
  .codex-plugin/plugin.json             Codex plugin manifest
  .claude-plugin/plugin.json            Claude Code plugin manifest
  .mcp.json                             remote MCP server (shared)
  skills/                               workflow skills (shared)
AGENTS.md                               installation guide for agents
```
