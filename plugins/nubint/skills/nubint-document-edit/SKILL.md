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

## 4. Propose edits

`edit_document` with:
- `instruction` — what to change, complete enough to act on without this conversation (the user's request in their own words; text they dictated, word for word).
- `where` — ONE place per call: words copied exactly from the passage, its heading, a section id (`S12`), or how the user pointed at it.
- `scope: "whole"` only for changes across the document (style, terminology, tense).
- `lookFor` — only when the change rests on what the papers the passage already cites say; Nubint reads those papers itself.

Nubint's editor model rewrites just those paragraphs and keeps their citations. It cannot add new papers. Credits: the model cost of the edit (Starter plan and above), nothing when no place could be found.

After each call, tell the user in one line what you suggested and remind them to accept or reject it in the editor. If the result says something could not be done, say so plainly.
