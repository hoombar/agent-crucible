# Six Thinking Hats — Dual Agent

## Pattern
Two agents collaborate on the SVG, each cycling through De Bono's six thinking hats but in different orders. This tests whether the *sequence* of thinking perspectives affects the outcome. The agents alternate turns, each wearing their next hat and building on the other's work.

## Hat Definitions

| Hat | Perspective | Focus |
|---|---|---|
| White | Facts & Information | Analyse the challenge constraints, list concrete requirements, identify technical SVG considerations |
| Green | Creative & Generative | Propose novel approaches, generate new elements, explore aesthetic possibilities |
| Black | Critical & Cautious | Identify problems, risks, what's wrong, what doesn't work |
| Yellow | Optimistic & Constructive | Identify strengths, what's working well, opportunities to build on |
| Red | Intuitive & Emotional | Gut reaction — does this *feel* right? First impressions, aesthetic intuition |
| Blue | Process & Meta | (Handled by orchestrator, not assigned to agents) |

## Agents

### Agent A
**Hat order:** White → Green → Black

**System prompt template** (hat-specific instruction injected per step):
> You are collaborating on an SVG with another agent. You are currently wearing the **{HAT_COLOUR} Hat** ({HAT_PERSPECTIVE}).
>
> **Your current lens:** {HAT_FOCUS}
>
> You must respond *entirely* from this hat's perspective. Do not mix in other perspectives.
>
> If this is the first step (no existing SVG), produce a structured analysis or initial SVG appropriate to your hat. If there is an existing SVG from the other agent, build on their work through your hat's lens.
>
> Always output an SVG — either a new one or a revised version of the existing one. Include a brief preamble (3-5 sentences max) explaining your perspective before the SVG.
>
> The SVG must be valid SVG 1.1, self-contained, and match the challenge viewport.

### Agent B
**Hat order:** Green → Yellow → Red

## Interaction Sequence

```
Step 1: Agent A — White Hat (facts/constraints analysis + initial structural SVG)
Step 2: Agent B — Green Hat (creative generation, building on A's structural foundation)
Step 3: Agent A — Green Hat (creative additions, building on B's initial creative work)
Step 4: Agent B — Yellow Hat (identify strengths, reinforce what's working)
Step 5: Agent A — Black Hat (critical assessment, identify problems to fix)
Step 6: Agent B — Red Hat (intuitive gut check, final aesthetic adjustments)
```

## Call Allocation
- 3 calls per agent = **6 total**

## Context Passing
- **Step 1 (A, White):** receives the challenge prompt only
- **Step 2 (B, Green):** receives the challenge prompt + step 1 SVG + step 1 preamble
- **Step 3 (A, Green):** receives the challenge prompt + step 2 SVG + step 2 preamble
- **Step 4 (B, Yellow):** receives the challenge prompt + step 3 SVG + step 3 preamble
- **Step 5 (A, Black):** receives the challenge prompt + step 4 SVG + step 4 preamble
- **Step 6 (B, Red):** receives the challenge prompt + step 5 SVG + step 5 preamble

Each agent sees the challenge, the most recent SVG, and the most recent preamble from the other agent.

## Notes
- The hat ordering is deliberate: Agent A starts analytical (White), goes creative (Green), then critical (Black). Agent B starts creative (Green), goes constructive (Yellow), then intuitive (Red). This means the early phase is fact→create, the middle is create→affirm, and the end is critique→gut-check.
- The Blue Hat (process management) is handled by the orchestrator — it decides when to move to the next step and what context to pass.
- The final SVG is the output of step 6.
