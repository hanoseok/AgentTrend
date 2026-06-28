# AgentTrend Manual Update Plugin Design

## Purpose

Create a local Codex plugin for the Agent Trend workspace with a manual, one-shot update skill triggered by `/AgentTrend:수동`.

The skill performs a full Agent Trend update on demand. It must not create, update, delete, inspect, or otherwise manage the existing scheduled automation. The automation remains responsible for the daily 07:00 run; `/AgentTrend:수동` is only for explicit manual runs.

## Scope

The plugin will be named `agenttrend` or `agent-trend`, depending on the scaffold normalization that best matches Codex plugin rules. The skill folder will use an ASCII-safe name, `manual-update`, while its description and instructions will explicitly mention `/AgentTrend:수동`, `AgentTrend 수동`, and `에이전트 트렌드 수동 업데이트` as trigger phrases.

When the skill is invoked, Codex should:

1. Read the current Agent Trend repository context.
2. Check the latest existing briefs, source watchlist, research log, reporting standard, and document history.
3. Browse current sources for AI agent trend updates.
4. Distinguish verified facts from interpretation or weak signals.
5. Create a manual update brief for the current date and time.
6. Update the repository HTML and raw Markdown artifacts needed for a complete update.
7. Verify the generated files and summarize the changes.

## Explicit Non-Goals

- Do not touch Codex automations.
- Do not create a new scheduled job.
- Do not rename or restructure the existing Agent Trend repository.
- Do not replace the daily 07:00 automation.
- Do not create unrelated agent research beyond the current update.

## Repository Update Contract

For each manual update, create or update:

- `YYYY-MM-DD/raw/HHmm_agent_trend_manual.md`
- `YYYY-MM-DD/HHmm_agent_trend_manual.html`
- `report/agent-trend-brief/`
- `index.html`
- `report/research-log/`
- `source/` when new or changed sources matter
- `history/`

The skill should preserve existing naming and style conventions. It should use the current report structure from the repository, including Korean language, source links, and the AI platform/service implication framing.

## Report Structure

The manual brief should include:

- Executive summary
- What changed
- High-importance updates
- Project, paper, product, and protocol signals
- AI platform and service implications
- Recommended actions
- Risks, uncertainties, and watch items
- Inline evidence links
- Source list

The report should avoid vendor-promo summaries. It should prioritize official specs, release notes, papers, GitHub releases, and company announcements, then use media or community signals only with clear confidence labels.

## File Naming

Use KST-local current date and time for filenames:

- Date directory: `YYYY-MM-DD`
- Manual raw brief: `YYYY-MM-DD/raw/HHmm_agent_trend_manual.md`
- Manual HTML brief: `YYYY-MM-DD/HHmm_agent_trend_manual.html`

If the same timestamp file already exists, update it only when the invocation is clearly continuing the same manual run. Otherwise choose the current timestamp.

## Plugin Layout

Expected plugin structure:

```text
agenttrend/
├── .codex-plugin/
│   └── plugin.json
└── skills/
    └── manual-update/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

No MCP server, app connector, hook, or script is required for the first version. Scripts can be added later if repeated manual runs reveal that HTML generation or index updates need deterministic tooling.

## Skill Behavior

`manual-update/SKILL.md` should instruct Codex to:

- Announce that it is using the AgentTrend manual update skill.
- Confirm it will not touch automations.
- Inspect repository context before writing files.
- Use web browsing for latest information because the task requires current trend data.
- Prefer primary sources.
- Write raw Markdown first, then HTML, then update hub/index/history files.
- Use `apply_patch` for manual edits.
- Run verification commands before claiming completion.
- Report created and updated files in the final response.

## Validation

Before returning final output, Codex should verify:

- The plugin manifest validates with the plugin creator validation script.
- The skill has valid YAML frontmatter.
- The manual skill description includes `/AgentTrend:수동`.
- No skill instruction asks to manage automations.
- Generated marketplace metadata exists if the plugin is registered in the personal marketplace.

For actual `/AgentTrend:수동` runs, Codex should verify:

- Raw and HTML files exist.
- Updated hub and index links point to the new manual brief.
- `git status --short` clearly shows the intended changes.
- External links used in high-importance claims are present in the report.

## Risks

- HTML updates are currently manual and can drift from repository style.
- The phrase `/AgentTrend:수동` may not trigger automatically unless the skill description is explicit enough.
- The first version depends on Codex following instructions rather than deterministic generation scripts.

These risks are acceptable for the first version because the user wants a usable manual trigger quickly, and the existing repository structure is still small enough for guided updates.

