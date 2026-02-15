# Copilot instructions for Power-BI-Visualisation-Picker

## Project shape (PBIP + TMDL)
- This repo is a Power BI Project (`.pbip`), not an app/service codebase.
- Entry point: `Power BI Visualization Picker/Power BI Visualisation Picker.pbip`.
- Report artifact: `Power BI Visualization Picker/Power BI Visualisation Picker.Report`.
- Semantic model artifact: `Power BI Visualization Picker/Power BI Visualisation Picker.SemanticModel`.
- The report binds to the model by relative path in `definition.pbir` (`datasetReference.byPath`).

## How report metadata is organized
- Global report settings, theme package, and custom visuals are in `...Report/definition/report.json`.
- Page order + active page are controlled centrally in `...Report/definition/pages/pages.json`.
- Each page lives in `...Report/definition/pages/<pageId>/page.json`.
- Each visual lives in `.../pages/<pageId>/visuals/<visualId>/visual.json`.
- `name` fields are stable IDs; user-facing labels are `displayName` (pages) and visual properties.

## Known page taxonomy in this repo
- `CHRTS` is the active landing page (`6d9234e03ce5f4af03c6`).
- Category pages are represented as separate tabs: `Categorical`, `Hierarchical`, `Relational`, `Temporal`, `Spatial`.
- `Information` page (`6bff5e3e453efc1b59ab`) is hidden (`visibility: HiddenInViewMode`) and opened via an `actionButton` using `visualLink` page navigation.

## Editing conventions that matter here
- Preserve existing GUID-like folder names for pages/visuals; they are identity keys referenced across files.
- When adding/reordering pages, update both the page folder metadata and `pages/pages.json` `pageOrder`.
- Keep report theme wiring intact: `report.json` references `StaticResources/SharedResources/BaseThemes/CY25SU12.json`.
- Prefer editing `displayName`, layout (`position`), and visual `visualType` settings over renaming IDs.
- Many sample visuals are intentionally "shell" visuals (type set, minimal bindings) to showcase visual choices.

## Semantic model expectations
- Model is intentionally minimal (`model.tmdl` + `Measures (2).tmdl`) and currently contains a tiny import table.
- Preserve annotations like `PBI_QueryOrder` and `PBI_ProTooling`; they affect Power BI tooling behavior.
- Culture metadata is in `definition/cultures/en-GB.tmdl`; keep locale edits consistent with model culture.

## Workflow and validation
- Primary workflow is opening the `.pbip` in Power BI Desktop (Project mode) and letting it round-trip metadata.
- There are no repo-defined build/test scripts; validation is metadata integrity + successful open in Desktop.
- After text edits, check JSON/TMDL syntax carefully (trailing commas, quote style, numeric literal suffixes like `0D`/`0L`).
- Keep changes surgical: avoid reformatting large JSON blocks because PBIP diffs are review-sensitive.
