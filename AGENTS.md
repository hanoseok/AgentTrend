# Agent Trend Agent Instructions

## Repository Contract

This repository is organized for recurring AI agent trend research and static GitHub Pages publishing.

## Directory Rules

- `raw/`: source material only. Treat this directory as read-only unless the user explicitly asks to add a new original file.
- `wiki/`: LLM-maintained Markdown knowledge base. Summaries, entities, concepts, comparisons, and syntheses belong here.
- `history/`: chronological research history and generated Markdown originals. Use `history/YYYY-MM-DD/` for dated research materials.
- `report/`: public HTML reports. Each report uses `index.html`; report update history uses `update.html`.
- `source/`: source bundles for report automation. Use `source/{category}/`.
- `tools/`: optional CLI helpers for search, lint, export, and validation.

## Daily AgentTrend Automation

For the daily AgentTrend report:

1. Start with `git status --short`. Stop and report if unrelated uncommitted changes exist.
2. Confirm branch is `main`; run `git pull --ff-only origin main` when safe.
3. Create the daily brief under `report/agent-trend-brief/YYYY-MM-DD-HHmm/index.html`.
4. Store the Markdown original under `history/YYYY-MM-DD/HHmm_agent_trend_brief.md`.
5. Update:
   - `report/agent-trend-brief/index.html`
   - `index.html`
   - `history/index.html`
   - `wiki/index.md`
   - `wiki/log.md`
   - relevant `source/{category}/index.md`
6. Validate local HTML links and anchors.
7. Stage only the intended files, commit, and push `main`.

## Writing Rules

- Separate confirmed facts from interpretation.
- Keep source links near the claim they support.
- Do not put generated summaries in `raw/`.
- Do not overwrite user or external changes.
- Keep public report URLs stable under `report/`.
