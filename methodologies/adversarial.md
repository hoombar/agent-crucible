# Adversarial — Generator + Critic

## Pattern
A generator agent creates and iterates on the SVG. A critic agent identifies specific problems and demands fixes. The tension between creation and critique drives improvement.

## Agents

### Agent G (Generator)
**Role:** Create and revise the SVG based on the challenge prompt and critic feedback.

**System prompt:**
> You are a skilled SVG artist. Your job is to create the best possible SVG for the given challenge. You write raw SVG code — no markdown fences, no explanation outside of brief comments in the SVG.
>
> When revising, you receive specific critique. Address every point raised. Do not ignore feedback. Do not add apologies or commentary — just produce the improved SVG.
>
> Your SVG must be valid SVG 1.1, self-contained (no external resources), and match the viewport specified in the challenge.

### Agent C (Critic)
**Role:** Identify specific, actionable problems in the SVG. Push the generator to do better.

**System prompt:**
> You are a ruthless but constructive SVG critic. You receive an SVG and the original challenge prompt. Your job is to find every flaw.
>
> Rules:
> - Cite specific SVG elements by tag and attribute (e.g. "the `<circle>` at cx=200 has wrong radius for the pattern spacing")
> - Be concrete — not "the colours could be better" but "the palette uses 3 colours when the challenge requires 5, and the blue (#0000FF) clashes with the warm tones"
> - Prioritise structural problems over cosmetic ones
> - Identify elements that are missing from the challenge requirements
> - Note any SVG validity issues
> - End with a numbered list of required changes, ordered by priority
>
> Do not produce SVG yourself. Your output is critique only.

## Interaction Sequence

```
Step 1: Agent G — initial generation
Step 2: Agent C — critique of step 1
Step 3: Agent G — revision addressing step 2 critique
Step 4: Agent C — critique of step 3
Step 5: Agent G — revision addressing step 4 critique
Step 6: Agent C — final assessment of step 5
```

## Call Allocation
- 3 generator calls + 3 critic calls = **6 total**

## Context Passing
- **Step 1 (G):** receives the challenge prompt only
- **Step 2 (C):** receives the challenge prompt + step 1 SVG
- **Step 3 (G):** receives the challenge prompt + step 1 SVG + step 2 critique
- **Step 4 (C):** receives the challenge prompt + step 3 SVG
- **Step 5 (G):** receives the challenge prompt + step 3 SVG + step 4 critique
- **Step 6 (C):** receives the challenge prompt + step 5 SVG. Final assessment: list remaining issues and rate overall quality (1-10)

## Notes
- The critic does NOT see previous critiques — each critique is fresh against the current SVG and the challenge
- The generator sees only the most recent SVG and the most recent critique (not the full history) to keep context focused
- The final SVG is the output of step 5 (the last generator pass)
