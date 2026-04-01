# Prompt Mutation — Parallel Generation + Synthesis

## Pattern
The base challenge prompt is mutated into two variants that emphasise different qualities. Two agents independently generate SVGs from these mutations. A revision agent then synthesises the best elements from both, and iterates to refine.

## Prompt Mutations

The orchestrator creates two mutations of the base challenge prompt before spawning agents:

### Mutation 1 — Structure & Precision
Prepend to the challenge prompt:
> **Your priority is structural accuracy and technical precision.** Focus on correct geometry, exact spacing, proper use of SVG transforms, and mathematical relationships between elements. Clean, well-organised SVG code matters. Get the skeleton right — proportions, alignment, symmetry. Aesthetic flourishes are secondary to structural correctness.

### Mutation 2 — Aesthetics & Creativity
Prepend to the challenge prompt:
> **Your priority is visual beauty and creative expression.** Focus on colour harmony, organic shapes, atmospheric effects, and emotional impact. Use gradients, subtle variations, and layering to create depth and richness. Take creative liberties with the prompt where it serves the overall aesthetic. Technical perfection is secondary to visual impact.

## Agents

### Agent M1 (Mutation 1 — Structure)
**System prompt:**
> You are an SVG artist focused on precision and structural integrity. You write raw SVG code — no markdown fences, no explanation. Just the SVG.
>
> Your SVG must be valid SVG 1.1, self-contained, and match the challenge viewport.

### Agent M2 (Mutation 2 — Aesthetics)
**System prompt:**
> You are an SVG artist focused on visual beauty and creative expression. You write raw SVG code — no markdown fences, no explanation. Just the SVG.
>
> Your SVG must be valid SVG 1.1, self-contained, and match the challenge viewport.

### Agent R (Revision — Synthesiser)
**System prompt:**
> You receive two SVGs created from different perspectives on the same challenge — one focused on structure/precision, the other on aesthetics/creativity.
>
> Your job is to synthesise the best elements from both into a single superior SVG. You must:
> 1. Explicitly state what you are taking from SVG 1 (structure) and what from SVG 2 (aesthetics)
> 2. Resolve any conflicts between the two approaches
> 3. Produce a complete SVG that combines structural correctness with visual beauty
>
> On subsequent iterations, you receive your previous SVG and must refine it further. Each iteration should make targeted improvements — don't start over.
>
> Output format: a brief preamble (what you changed and why, 3-5 sentences) followed by the complete SVG.
>
> Your SVG must be valid SVG 1.1, self-contained, and match the challenge viewport.

## Interaction Sequence

```
Step 1: Agent M1 — generate from mutation 1 (structure)     ⎫ parallel
Step 2: Agent M2 — generate from mutation 2 (aesthetics)    ⎭
Step 3: Agent R  — synthesise M1 + M2 outputs
Step 4: Agent R  — refine (iteration 1)
Step 5: Agent R  — refine (iteration 2)
Step 6: Agent R  — refine (iteration 3, final)
```

## Call Allocation
- 2 generator calls (parallel) + 4 revision calls = **6 total**

## Context Passing
- **Step 1 (M1):** receives mutation 1 prompt (structure emphasis + challenge)
- **Step 2 (M2):** receives mutation 2 prompt (aesthetics emphasis + challenge)
- **Step 3 (R):** receives the challenge prompt + M1's SVG (labelled "Structure version") + M2's SVG (labelled "Aesthetics version")
- **Step 4 (R):** receives the challenge prompt + step 3 SVG + step 3 preamble
- **Step 5 (R):** receives the challenge prompt + step 4 SVG + step 4 preamble
- **Step 6 (R):** receives the challenge prompt + step 5 SVG + step 5 preamble + instruction "this is your final pass — polish and finalise"

## Notes
- Steps 1 and 2 are spawned in parallel (two Agent tool calls in a single message)
- The revision agent always sees the original challenge prompt to stay grounded in the requirements
- Each revision pass should be targeted (not a full rewrite) — the preamble forces the agent to articulate what changed
- The final SVG is the output of step 6
