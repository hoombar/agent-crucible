# Agent Crucible

A framework for evaluating AI agent collaboration methodologies using SVG generation as a human-evaluable proxy task.

## Concept

Different agent collaboration patterns (adversarial, six thinking hats, prompt mutation) are run against the same SVG challenges at increasing difficulty. The SVGs and agent conversation logs are captured at every iteration, then compiled into a single HTML report for side-by-side comparison.

SVG is chosen because humans can instantly assess visual quality — but the framework is evaluating the *methodology*, not optimising for SVG output. Agents work blind (text-only SVG source), and the human eye is the oracle.

## How it works

1. **Challenges** (`challenges/`) define SVG prompts at 3 difficulty tiers
2. **Methodologies** (`methodologies/`) define agent roles, interaction patterns, and prompts
3. **CLAUDE.md** instructs Claude Code how to orchestrate runs
4. Open Claude Code in this repo and ask it to run experiments

## Running experiments

```
# In Claude Code, from this directory:
"Run the adversarial methodology against the tier-1 mandala challenge"
"Run all experiments"
"Generate the report"
```

## Output

Each run produces:
- Iteration SVGs showing the trajectory of improvement
- Conversation logs documenting agent reasoning, trade-offs, and corrections
- A final SVG and metrics summary

After runs complete, a self-contained `runs/report.html` inlines all SVGs and logs for comparison.

## Methodologies

| Methodology | Pattern | Agents |
|---|---|---|
| Adversarial | Generator + Critic loop | 2 |
| Six Thinking Hats | Two agents cycling hats in different orders | 2 |
| Prompt Mutation | Parallel generation + synthesis | 3 |

## Challenge Tiers

| Tier | Example | Difficulty |
|---|---|---|
| 1 | Geometric mandala | Baseline — any approach should handle |
| 2 | Isometric room | Differentiating — naive single-pass fails |
| 3 | Mountain landscape | Stress test — tests graceful degradation |
