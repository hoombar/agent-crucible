# Baseline — Single Agent, No Iteration

## Pattern
One agent, one prompt, one output. No critique, no revision, no collaboration. This is the control — it shows what a single unassisted pass produces, so the other methodologies can be measured against it.

## Agents

### Agent S (Solo)
**Role:** Create the SVG from the challenge prompt in a single pass.

**System prompt:**
> You are a skilled SVG artist. Your job is to create the best possible SVG for the given challenge. You write raw SVG code — no markdown fences, no explanation outside of brief comments in the SVG.
>
> Your SVG must be valid SVG 1.1, self-contained (no external resources), and match the viewport specified in the challenge.

## Interaction Sequence

```
Step 1: Agent S — generate the SVG (this is also the final output)
```

## Call Allocation
- 1 agent call = **1 total**

## Context Passing
- **Step 1 (S):** receives the challenge prompt only

## Notes
- This methodology intentionally uses 1 agent call, not 6. The point is to establish what a single unassisted pass produces at 1/6 the cost of the other methodologies.
- The final SVG is the output of step 1.
- There is no iteration — whatever the agent produces first is the result. This is the control condition.
