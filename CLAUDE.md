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

All 9 runs execute **in parallel** — there are no dependencies between runs. Each run is spawned as a separate background agent that manages its own 6-step sequence internally.

### Procedure

1. **Spawn 9 agents in a single message** using the Agent tool with `run_in_background: true`. Each agent receives:
   - The full methodology file content
   - The full challenge file content
   - The output directory path for that run
   - The complete Single Run Procedure (copy the relevant sections into the agent prompt so it is self-contained)
   - Instruction to write all output files (config.json, iterations/, final.svg, metrics.json, summary.md) directly to the output directory

2. Each agent prompt must be **fully self-contained** — include the methodology definition, challenge prompt, SVG extraction rules, conversation log format, and output file structure. The agent cannot read CLAUDE.md, so everything it needs must be in its prompt.

3. As agents complete, they will report back. Note completions and any failures.

4. Once all 9 are done, report a summary: which runs completed successfully, one-line assessment per run.

### The 9 combinations

| # | Methodology | Challenge |
|---|---|---|
| 1 | adversarial | tier-1-mandala |
| 2 | adversarial | tier-2-isometric-room |
| 3 | adversarial | tier-3-mountain-landscape |
| 4 | six-hats | tier-1-mandala |
| 5 | six-hats | tier-2-isometric-room |
| 6 | six-hats | tier-3-mountain-landscape |
| 7 | prompt-mutation | tier-1-mandala |
| 8 | prompt-mutation | tier-2-isometric-room |
| 9 | prompt-mutation | tier-3-mountain-landscape |

### Agent prompt template for parallel runs

Each of the 9 agents should be prompted with:

```
You are running a single Agent Crucible experiment. Your job is to execute the methodology
and save all output files. Work autonomously — do not ask questions, just execute.

## Output Directory
{runs/methodology-challenge-YYYYMMDD-HHmm/}

Create this directory and an iterations/ subdirectory inside it.

## Config
Save config.json with: methodology, challenge, tier, timestamp, agent_call_budget: 6

## Challenge
{paste full challenge file content}

## Methodology
{paste full methodology file content}

## Execution
Follow the methodology's interaction sequence exactly. For each step:
1. The step's agent work is done by YOU directly (you cannot spawn sub-agents).
   Adopt the role described for that step — follow the system prompt and constraints
   for that agent role. Clearly separate your work for each step.
2. Extract the SVG from your output and save as iterations/step-NN.svg
3. Save the conversation log as iterations/step-NN-conversation.md in this format:

   # Step N — {Agent Role} — {Hat/Phase if applicable}

   ## Prompt Given
   {Brief summary of the task for this step}

   ## Response Summary
   {Key decisions, trade-offs, corrections}

   ## SVG Changes
   {What changed from previous step}

4. For the final step, also copy the SVG as final.svg in the run directory

## SVG Rules
- Extract only the <svg>...</svg> element
- If multiple SVGs, use the last one
- Save failures as-is — failure is data
- Valid SVG 1.1, self-contained, match challenge viewport

## Finalise
Save metrics.json with: agent_calls: 6, steps, started_at, completed_at (ISO 8601)
Save summary.md: 5-10 sentence narrative of how the run went

## Important
- Budget: exactly 6 steps, no more
- Adopt each agent role fully when executing that step
- Failure is data — save bad SVGs, don't retry
- Conversation logs are essential — document every step
```

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
