---
name: nubint-literature
description: Find real scholarly papers with Nubint's index, save them to the user's Nubint library, and run a cited literature review. Use for literature questions, finding papers to cite, checking what research exists on a topic, or adding sources to a manuscript or .bib file.
---

# Literature with Nubint

## Search first — it is instant and free

`search_papers` with `query` (a research question or keywords). Narrow only with what the user asked for: `yearFrom` / `yearTo`, `minCitations`, `openAccessOnly`, `excludeRetracted`, `sort`. Each result has `id`, title, year, authors, venue, DOI, citation count and, when indexed, the passage that matches the query. Every result is a real, indexed paper — cite only papers that came back from a tool, never from memory.

- The user's own library: `search_library`.
- To keep a paper: `save_to_library` with its `id` (saving twice is harmless). No credits.

## Adding sources to a local manuscript

When the user wants a paper added to their `.bib` or reference list, build the entry only from the fields the search result returned (authors, year, title, venue, DOI). Do not invent missing fields, page numbers or DOIs. Prefer adding by DOI when the project has a tool that resolves DOIs to BibTeX.

## A cited literature review

`start_literature_review` with `question`, optional `depth`, `language` (the user's language) and an `idempotencyKey`. Credits, Starter plan and above — state the cost before starting. It returns a `taskId`; poll `get_task` every 15–30 seconds until done, never start it twice, and after about 20 minutes give the user the result url. Report the review with its citations as returned.

Systematic reviews (protocol, screening, synthesis) and data-extraction tables are being prepared for launch and are not available yet. Say so plainly and offer a literature review or a paper search instead; do not imitate them with other tools.
