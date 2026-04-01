# Tier 1 — Geometric Mandala

## Tier
1 (Baseline)

## Description
A mathematically precise mandala with rotational symmetry. This is the baseline challenge — every methodology should produce a reasonable result, but differences in precision and aesthetic coherence will still be visible.

## SVG Prompt

Create an SVG mandala with the following requirements:

- **8-fold rotational symmetry** — the pattern must repeat exactly 8 times around the centre
- **At least 3 concentric rings**, each with distinct geometric elements:
  - Inner ring: a pattern using circles or dots
  - Middle ring: a pattern using petals or leaf shapes
  - Outer ring: a pattern using triangles or pointed elements
- **Colour palette:** at least 5 harmonious colours. Choose a cohesive palette (e.g. jewel tones, earth tones, or ocean tones) — not random colours
- **Mathematical precision:** elements should be evenly spaced and symmetrically placed. Use `transform="rotate(...)"` for symmetry rather than manually positioning each element
- **A decorative border** enclosing the mandala

The mandala should feel balanced, intentional, and visually pleasing.

## Complexity Indicators
- Rotational symmetry requires precise coordinate math
- Concentric rings must be properly nested without overlap
- Colour harmony is a subjective quality that requires aesthetic judgment
- Use of SVG transforms tests understanding of the coordinate system

## SVG Constraints
- Viewport: 800×800
- Valid SVG 1.1
- No external dependencies (no linked images, fonts, or stylesheets)
- All elements must be defined inline
