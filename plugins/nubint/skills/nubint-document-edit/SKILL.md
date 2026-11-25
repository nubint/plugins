---
name: nubint-document-edit
description: Read and edit a document in the user's Nubint editor, side by side with this conversation. Edits arrive in the Nubint editor as suggestions the user accepts or rejects. Use when the user asks to change, rewrite, shorten, restructure or check a Nubint document (not a local file).
---

# Edit a Nubint document side by side

The user keeps the document open in the Nubint editor next to this conversation. You never change it directly: every edit appears in their editor as a before/after suggestion, and nothing changes until they accept it.

## 1. Find the document

- `list_documents` (most recent first) — match the title the user means. Ask if two are plausible.
- Or use a `documentId` from `save_manuscript` earlier in the conversation.

## 2. Make sure the editor is open

`read_document` and `edit_document` return `editorOpen`. When it is `false`, ask the user to open the returned `url` in their browser and keep it beside this conversation before you continue. Suggestions made while it is closed are kept and appear when they open it, but the user cannot review them until then.

## 3. Read only what you need

`read_document` with `documentId`: a short document comes back whole; a long one comes back as a section map (id · heading · size). Then read sections by id (`sections: ["S4", "S5"]`) or find passages by meaning (`query`). No credits.

## 4. Write the new text and propose it

You write the new text yourself. `edit_document` takes `documentId` and `edits` — up to 20 changes in one call, each:
- `action` — `replace`, `delete`, `insert_after`, `insert_before` or `set_title`.
- `find` — text copied exactly from what `read_document` returned, long enough to occur once (a whole paragraph or a heading line is the usual unit). `insert_after` with an empty `find` adds at the end.
- `text` — the new Markdown (empty for `delete`).

Keep the passage's `[n]` citation marks where they belong — Nubint carries those citations over. Do not add new `[n]` marks: new papers cannot be cited this way. No credits.

The result has one line per edit, proposed or refused with the reason. Fix a refused edit (usually `find` not copied exactly) and send it again. Then tell the user in one line what you suggested and remind them to accept or reject it in the editor.
