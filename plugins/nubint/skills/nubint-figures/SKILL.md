---
name: nubint-figures
description: Make or revise scientific figures with Nubint — illustrations, graphical abstracts, flowcharts of methods, and charts drawn from real data. Use when the user asks for a figure, diagram, flowchart or plot, including from a local CSV/XLSX file or the methods section of a manuscript.
---

# Make figures with Nubint

Results land in a Nubint figure project (an editable canvas). Always give the user the project `url` — the canvas is where they fix labels, redraw a region and export (PNG, SVG, PPTX).

## Charts of real data → `create_figure_plot`

Never draw a data chart with `start_figure`: an image model cannot copy numbers. `create_figure_plot` plots the user's data with code, so nothing is invented.
- Pass the data unchanged: small tables as CSV text in `csv`; a spreadsheet or larger file via `upload_figure_file` (`purpose` for data, `dataBase64` for binary files) and then its returned id as `dataFile`.
- Describe the chart in `chart` (type, which columns, grouping). Optional `journalStyle`, `palette`, `ratio`, `includeSvg` (adds the SVG export cost).
- Quote numbers only from the returned `summary`. If the data cannot make the requested chart, say so instead of approximating.

## Illustrations and flowcharts → `start_figure`

- `prompt` describes the content; `kind` picks `illustration` or `flowchart` (for a methods flowchart, base the steps on the user's methods text — read their manuscript file if they point to it).
- Source material, a sketch or a style reference: upload it first with `upload_figure_file`, then pass it in `attachments` or `styleReferences`.
- To revise an earlier result, pass its `projectId` and the image to change (`baseImageUrl`) with a `prompt` describing the change, or an `action` (recolor, background removal, ratio, upscale).
- It runs in the background: it returns a `taskId`. Poll `get_task` every 15–30 seconds until `completed` or `failed`. Never start the same figure twice because polling is slow. Pass an `idempotencyKey` and reuse it if a call fails or times out.

## Credits

Every drawing action has a fixed price, stated in the tool descriptions; a chart costs the same as a figure. Figures need the Starter plan or above. State the cost before you start a paid run.

## Local files

If the user wants the figure in their project folder, export it from the canvas yourself only if the host gives you a download; otherwise give the canvas link and tell them where to export.
