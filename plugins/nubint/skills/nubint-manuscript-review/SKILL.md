---
name: nubint-manuscript-review
description: Check or review a research manuscript with Nubint — reference verification, AI-writing detection, source quality, and the full reviews (peer review, claim credibility, proofreading, tone, source overlap). Use when the user asks to check references, review, critique or proofread a paper, including a local LaTeX (.tex + .bib) or Markdown manuscript.
---

# Review a manuscript with Nubint

## 1. Get the manuscript text and the reference list

- From the conversation, or from local files the user points to. For LaTeX, read the `.tex` files (follow `\input` / `\include`) and the `.bib` file. Convert the body to plain Markdown: keep headings, paragraphs and in-text citations; drop preamble, macros, figure/table environments' code.
- Keep the reference list SEPARATE from the body: one reference per string, in the manuscript's own order. From `.bib`, write each cited entry as a normal reference line (authors, year, title, venue, DOI) — use only fields present in the entry; never invent missing ones.
- In-text citations must point to those references: `[3]`, `[1, 4]` by position, or (Author, Year). For LaTeX `\cite{key}`, replace each key with the matching author-year or number so Nubint can link it.

## 2. Quick checks (nothing saved)

`run_manuscript_check` with `text`, `references` and `checks`:
- `reference_verification` — does each reference exist in a scholarly index. **No credits.** For a reference list alone, pass only `references` and this check.
- `ai_detection`, `source_quality` — credits.

Report each result as it is. `off_index` means "not found in the index", **not** fabricated — books, statutes and many regional journals are simply absent. Never call a reference fake from this tool alone; for `uncertain`, show `matchedTitle` so the user can compare.

## 3. Full reviews (saved as a Nubint document)

1. `save_manuscript` with `text` (Markdown body, no reference list), `references`, optional `title`, and an `idempotencyKey` you make up. No credits. Tell the user `notLinkedInText` if any reference is never cited.
2. Show the user `reviewCosts` (credits per check for this manuscript) and let them choose. Recommend `peer_review`, `claim_credibility` and `source_quality` first.
3. `start_review` once per chosen check, with `documentId`, `check`, `language` (the user's language) and an `idempotencyKey`. It returns a `taskId` immediately.
4. Poll `get_task` every 15–30 seconds until `completed`, `failed` or `cancelled`. Never start the same review twice because polling is slow. After about 20 minutes, stop and give the user the document url.

Report findings as they are — the quoted sentence, the problem, the suggested replacement. Do not soften or drop findings.

## 4. Applying fixes

- Local files: edit the user's `.tex` / `.md` directly only when they ask, one finding at a time, and show what changed.
- Nubint document: use the nubint-document-edit skill (edits arrive as suggestions in the Nubint editor).
