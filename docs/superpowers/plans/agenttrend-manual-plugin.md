# AgentTrend Manual Plugin Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create and register a local Codex plugin whose `/AgentTrend:수동` skill performs one-shot full Agent Trend repository updates without touching automations.

**Architecture:** Scaffold a personal local plugin with one skill. The skill contains procedural instructions only; it relies on Codex's existing file editing, browsing, and verification tools rather than a custom MCP server or script.

**Tech Stack:** Codex plugin manifest JSON, Codex `SKILL.md`, `agents/openai.yaml`, plugin-creator validation scripts.

---

### Task 1: Scaffold Plugin

**Files:**
- Create: `/Users/hanoseok/plugins/agenttrend/.codex-plugin/plugin.json`
- Create or modify: `/Users/hanoseok/.agents/plugins/marketplace.json`

- [ ] **Step 1: Run the scaffold script**

```bash
python3 /Users/hanoseok/.codex/skills/.system/plugin-creator/scripts/create_basic_plugin.py agenttrend --with-skills --with-marketplace
```

Expected: The command creates `/Users/hanoseok/plugins/agenttrend` and registers it in the personal marketplace.

- [ ] **Step 2: Inspect generated files**

```bash
find /Users/hanoseok/plugins/agenttrend -maxdepth 3 -type f | sort
sed -n '1,220p' /Users/hanoseok/plugins/agenttrend/.codex-plugin/plugin.json
```

Expected: `plugin.json` exists and the plugin name is `agenttrend`.

### Task 2: Add Manual Update Skill

**Files:**
- Create: `/Users/hanoseok/plugins/agenttrend/skills/manual-update/SKILL.md`
- Create: `/Users/hanoseok/plugins/agenttrend/skills/manual-update/agents/openai.yaml`

- [ ] **Step 1: Create the skill directory**

```bash
mkdir -p /Users/hanoseok/plugins/agenttrend/skills/manual-update/agents
```

Expected: The directory exists.

- [ ] **Step 2: Write `SKILL.md`**

Create a skill with YAML frontmatter:

```yaml
---
name: manual-update
description: Use when the user asks for /AgentTrend:수동, AgentTrend 수동, or an 에이전트 트렌드 수동 업데이트. Performs a one-shot full Agent Trend repository update without creating, viewing, updating, deleting, or otherwise touching automations.
---
```

The body must instruct Codex to inspect the Agent Trend repository, browse current primary sources, write `YYYY-MM-DD/raw/HHmm_agent_trend_manual.md`, write `YYYY-MM-DD/HHmm_agent_trend_manual.html`, update `Agent_Trend_Brief.html`, `index.html`, `RESEARCH_LOG.html`, optionally `SOURCE_WATCHLIST.html`, and `DOCUMENT_HISTORY.html`, then verify files and links. It must explicitly say not to use automation tools.

- [ ] **Step 3: Write `agents/openai.yaml`**

```yaml
interface:
  display_name: "AgentTrend Manual Update"
  short_description: "Run a one-shot Agent Trend repository update."
  default_prompt: "Use $manual-update for /AgentTrend:수동 to run a one-shot full Agent Trend update."

policy:
  allow_implicit_invocation: true
```

Expected: UI metadata is concise and references `$manual-update`.

### Task 3: Validate And Commit

**Files:**
- Validate: `/Users/hanoseok/plugins/agenttrend`
- Modify: `/Users/hanoseok/.agents/plugins/marketplace.json`

- [ ] **Step 1: Validate plugin manifest**

```bash
python3 /Users/hanoseok/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py /Users/hanoseok/plugins/agenttrend
```

Expected: Exit code 0.

- [ ] **Step 2: Validate skill structure**

```bash
python3 /Users/hanoseok/.codex/skills/.system/skill-creator/scripts/quick_validate.py /Users/hanoseok/plugins/agenttrend/skills/manual-update
```

Expected: Exit code 0.

- [ ] **Step 3: Verify trigger and automation guardrails**

```bash
rg -n "/AgentTrend:수동|automations|자동화|automation tools" /Users/hanoseok/plugins/agenttrend/skills/manual-update
```

Expected: The trigger appears in metadata and body; guardrail text forbids automation management.

- [ ] **Step 4: Review marketplace entry**

```bash
sed -n '1,220p' /Users/hanoseok/.agents/plugins/marketplace.json
```

Expected: The `agenttrend` entry exists with local source path `./plugins/agenttrend`.

