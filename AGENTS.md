# Nubint for Codex and Claude Code — installation guide

This page is for an AI agent. Install only when the user explicitly asks to install or set up Nubint. If the user asks to read, explain or review this page, answer that request without changing configuration. Follow the host's permissions and the user's instructions throughout.

## What is distributed

- Repository: https://github.com/nubint/plugins.git (ref: `main`)
- Marketplace: `nubint-marketplace` · plugin: `nubint` · qualified id: `nubint@nubint-marketplace`
- Codex manifests: `.agents/plugins/marketplace.json`, `plugins/nubint/.codex-plugin/plugin.json`
- Claude Code manifests: `.claude-plugin/marketplace.json`, `plugins/nubint/.claude-plugin/plugin.json`
- Shared by both: `plugins/nubint/.mcp.json` (remote MCP server `nubint` at `https://api.nubint.ai/api/v1/mcp`) and four skills in `plugins/nubint/skills/`

This is the production service. The plugin does not install any backend. Do not substitute another URL or create a second, standalone MCP registration for the same server.

## 1. Identify the host

- **Codex** (CLI or desktop app): use the Codex CLI that belongs to this host, not just the first `codex` on `PATH` — an older standalone CLI installed separately (for example through npm) often shadows it and has no plugin commands. Pick the first of these that answers `plugin --help`:
  1. the path in the `CODEX_CLI_PATH` environment variable, when set;
  2. the CLI bundled with the desktop app — Windows: the newest `%LOCALAPPDATA%\OpenAI\Codex\bin\*\codex.exe`; macOS: `codex` inside the app bundle (`/Applications/ChatGPT.app` or `/Applications/Codex.app`, search `Contents/` for it);
  3. `codex` on `PATH`.

  Use that same executable for every command below. Do not install or upgrade a CLI. Only if none of them has plugin commands, use the desktop Plugins UI.
- **Claude Code** (CLI, IDE extension or desktop app): check `claude --version` and `claude plugin --help`, or use the `/plugin` command inside a session.

If the same plugin is already installed and enabled from this repository, reuse it. If a marketplace with the same name points elsewhere, explain the collision instead of replacing it. Preserve unrelated plugins, MCP servers and credentials.

## 2. Add the marketplace and install

**Codex**

    codex plugin marketplace add nubint/plugins --ref main
    codex plugin add nubint@nubint-marketplace

Confirm each command's syntax with `--help` first. Desktop app: Plugins → Add marketplace → `https://github.com/nubint/plugins.git`, Git ref `main`, sparse paths empty → install **Nubint** from `nubint-marketplace`.

**Claude Code**

    claude plugin marketplace add nubint/plugins
    claude plugin install nubint@nubint-marketplace

Inside a session the same is `/plugin marketplace add nubint/plugins`, then `/plugin install nubint@nubint-marketplace`, then `/reload-plugins`.

Verify the installed plugin's identity, enabled state and version against the manifest fetched from the repository (`codex plugin list --json` / `claude plugin list`, `claude plugin details nubint`). Check that the bundled MCP configuration points to `https://api.nubint.ai/api/v1/mcp`. Cloning the repository is not installing it.

## 3. Connect and authenticate

Nubint uses OAuth in the browser. Start the host's connection flow for this plugin's `nubint` server — in Claude Code run `/mcp`, select the Nubint server and authenticate; in Codex run `codex mcp login nubint` with the same CLI as above (it prints and opens the sign-in URL and waits for the browser to come back; if it times out, run it again), or use the plugin's Connect / Authorize step in the desktop app. The user signs in to Nubint and clicks Allow on the consent page. Never ask the user to paste a password, token or API key into the conversation.

Before going further, require evidence of BOTH:

- authentication succeeded for this plugin's `nubint` server (the host's authorization result or connection status), and
- the host discovered the server's tools (for example `get_credits`, `search_papers` are listed).

Then call `get_credits` once: it costs nothing and confirms the account and plan. A login page opening, or the user saying "done", is not evidence. If anything is still pending, report "installed; connection pending" with the remaining step — do not report success.

## 4. Start (only if the user asked for it)

If the user asked to set up a new task or conversation, read `interface.defaultPrompt` from the installed `plugins/nubint/.codex-plugin/plugin.json` (first non-empty string) and start one task titled "Nubint" with it, after step 3 passed. In Claude Code, continue in the current session instead and follow the `nubint-onboarding` skill. Never create more than one task, and never claim one was created without an identifier from the host.

## 5. Report

Report only what you observed: installed version, connection and plan state, and any step the user still has to do. Do not create documents or reviews, and do not spend credits as part of installation.

## Plans and credits

Every tool needs the Starter plan or above. On the Free plan the connection works, and every tool — `get_credits` included — answers with a plan notice; that notice still shows the account is connected. Searching papers and the library, saving a manuscript, listing, reading and editing documents and reference verification use no credits. Reviews, AI-writing detection, source quality and literature reviews spend credits. An error about plan, credits or rate limits is written for the user — relay it as it is.

## Troubleshooting

- "marketplace root does not contain a supported manifest": the repository root must contain `.agents/plugins/marketplace.json` (Codex) or `.claude-plugin/marketplace.json` (Claude Code); leave sparse paths empty.
- Plugin missing after adding the marketplace: install `nubint@nubint-marketplace` explicitly.
- Tools missing: check that the plugin is enabled and the `nubint` server is authenticated; restart or reload plugins if the host requires it.
- OAuth error or "connection request expired": start the connection again from the host; report the exact error.
