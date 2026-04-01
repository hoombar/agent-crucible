# Agent Crucible — Orchestration Instructions

You are the orchestrator for Agent Crucible, a framework for evaluating AI agent collaboration methodologies using SVG generation as a proxy task.

## Commands

When the user says:
- **"Run {methodology} against {challenge}"** — execute a single run (see Single Run Procedure below)
- **"Run all experiments"** — execute all 9 combinations (3 methodologies × 3 challenges), sequentially
- **"Generate the report"** — build `runs/report.html` from all completed runs (see Report Generation below)

## File Locations

- Challenges: `challenges/tier-{N}-{name}.md`
- Methodologies: `methodologies/{name}.md`
- Output: `runs/{methodology}-{challenge}-{YYYYMMDD-HHmm}/`

## Single Run Procedure

### 1. Setup

1. Read the specified methodology file and challenge file
2. Create the output directory: `runs/{methodology}-{challenge}-{YYYYMMDD-HHmm}/`
3. Create the `iterations/` subdirectory inside it
4. Save `config.json`:
   ```json
   {
     "methodology": "{name}",
     "challenge": "{name}",
     "tier": {N},
     "timestamp": "{ISO 8601}",
     "agent_call_budget": 6
   }
   ```
5. Copy the challenge file as `challenge.md` into the run directory

### 2. Execute the Methodology

Follow the interaction sequence defined in the methodology file **exactly**. For each step:

1. **Spawn the agent** using the Agent tool with `subagent_type: "general-purpose"`. Construct the prompt according to the methodology's context passing rules — include the system prompt for that agent role, the challenge prompt, and any prior SVG/critique/preamble as specified.

2. **Extract the SVG** from the agent's response. The SVG is the content between `<svg` and `</svg>` tags (inclusive). If the agent wrapped it in markdown fences, strip them.

3. **Save the iteration SVG** as `iterations/step-{NN}.svg` (zero-padded: 01, 02, etc.)

4. **Save the conversation log** as `iterations/step-{NN}-conversation.md` in this format:
   ```markdown
   # Step {N} — {Agent Role} — {Hat/Phase if applicable}

   ## Prompt Given
   {Brief summary of what the agent was asked — not the full prompt, just the key instruction and what context was provided}

   ## Response Summary
   {Key decisions the agent made, trade-offs mentioned, corrections from previous step}

   ## SVG Changes
   {What changed from the previous step — elements added, removed, modified. "Initial generation" for step 1}
   ```

5. **For parallel steps** (e.g. prompt mutation steps 1 & 2): spawn both agents in a single message with two Agent tool calls. Save both outputs. Number them as consecutive steps (step-01, step-02).

### 3. Finalise

1. Copy the final step's SVG as `final.svg` in the run directory
2. Save `metrics.json`:
   ```json
   {
     "agent_calls": 6,
     "steps": {N},
     "started_at": "{ISO 8601}",
     "completed_at": "{ISO 8601}"
   }
   ```
3. Save `summary.md` — a brief (5-10 sentence) narrative of how the run went:
   - What was the initial approach?
   - What were the key corrections or improvements at each stage?
   - What trade-offs were made?
   - How did the methodology's pattern influence the trajectory?
   - What is the final quality like in your assessment?

### 4. SVG Extraction Rules

- The agent's response may contain text before/after the SVG. Extract only the SVG element.
- If the response contains multiple `<svg>` elements, use the last one (it's likely the final version).
- If the agent fails to produce valid SVG, note this in the conversation log and save whatever was produced. Do not retry — the methodology's failure mode is data.
- Ensure the saved SVG starts with `<svg` and ends with `</svg>`.

## Run All Experiments

Execute these 9 runs in order:

1. adversarial × tier-1-mandala
2. adversarial × tier-2-isometric-room
3. adversarial × tier-3-mountain-landscape
4. six-hats × tier-1-mandala
5. six-hats × tier-2-isometric-room
6. six-hats × tier-3-mountain-landscape
7. prompt-mutation × tier-1-mandala
8. prompt-mutation × tier-2-isometric-room
9. prompt-mutation × tier-3-mountain-landscape

After each run, briefly report the methodology, challenge, and a one-line assessment before moving to the next.

## Report Generation

When asked to generate the report, build a single self-contained `runs/report.html` file.

### Report structure:

1. **Read all run directories** in `runs/` (skip `report.html` itself)
2. For each run, read: `config.json`, `metrics.json`, `summary.md`, `final.svg`, and all `iterations/step-*.svg` and `iterations/step-*-conversation.md` files

### HTML layout:

```
┌─────────────────────────────────────────────────────────┐
│  Agent Crucible Report — {date}                         │
│  Metrics summary table                                  │
├─────────────┬─────────────┬─────────────────────────────┤
│             │ Tier 1      │ Tier 2      │ Tier 3        │
├─────────────┼─────────────┼─────────────┤───────────────┤
│ Adversarial │ [final SVG] │ [final SVG] │ [final SVG]   │
│ Six Hats    │ [final SVG] │ [final SVG] │ [final SVG]   │
│ Prompt Mut. │ [final SVG] │ [final SVG] │ [final SVG]   │
└─────────────┴─────────────┴─────────────┴───────────────┘
  Each cell expands to show iteration filmstrip + conversation logs
```

### HTML requirements:
- **Inline all SVGs** as `<svg>` elements directly in the HTML (not as `<img>` or `<object>`)
- **All CSS and JS inline** — no external files, the report must work as a standalone file
- **Metrics table** at the top showing: methodology, challenge, tier, agent calls, time elapsed
- **3×3 grid** of final SVGs, labelled by methodology (rows) and tier (columns)
- **Click to expand** each cell to reveal:
  - An iteration filmstrip: all step SVGs displayed left-to-right, small, showing the trajectory
  - Conversation logs: collapsible sections for each step's conversation log, rendered as formatted text
  - The run summary
- **Clean, readable styling** — dark background works well for SVG display. Use a monospace font for conversation logs.
- Each SVG in the grid should be rendered at a consistent size (e.g. 300×225 for the grid, larger when expanded)

## Important Rules

- **Agent model:** Use the default model for all agents. Do not specify a model override. All agents must use the same model to keep methodology as the only variable.
- **Budget enforcement:** Each methodology gets exactly 6 agent calls. Do not add extra calls, retries, or bonus rounds.
- **Failure is data:** If an agent produces bad SVG or misunderstands the task, save it anyway. The methodology's ability to recover (or not) is part of what we're evaluating.
- **No visual feedback to agents:** Agents work with SVG source text only. Never render an SVG and pass an image to an agent.
- **Conversation logs are essential:** The logs are as valuable as the SVGs. Document what happened at each step — don't skip this.
