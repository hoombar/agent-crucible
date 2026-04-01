# Tier 2 — Isometric Room

## Tier
2 (Differentiating)

## Description
An isometric view of a furnished room. This challenge requires spatial reasoning — objects must sit on surfaces convincingly, perspective must be consistent, and the scene must feel coherent as a 3D space projected onto 2D. A single-pass attempt will likely have perspective inconsistencies that iterative collaboration should catch and fix.

## SVG Prompt

Create an SVG showing an isometric view of a cozy room with the following contents:

- **A desk** with a lamp and an open book on it
- **A bookshelf** against one wall, with at least 6 visible books of varying heights and colours
- **A window** on another wall showing a night sky with stars and a crescent moon
- **A rug** on the floor with a simple geometric pattern
- **Consistent isometric perspective** — all objects must follow the same isometric projection angles (approximately 30° from horizontal for both axes)
- **Shadows:** objects should cast soft shadows on the floor/walls to ground them in the scene
- **Warm lighting:** the lamp should cast a warm glow, with colours suggesting a cozy evening atmosphere

The room should feel like a liveable space, not a collection of disconnected objects.

## Complexity Indicators
- Isometric projection requires consistent angle discipline across all objects
- Objects must be spatially related (book *on* desk, lamp *on* desk, rug *on* floor)
- Depth ordering (z-sorting) must be correct — objects in front occlude objects behind
- Shadows and lighting add atmospheric coherence
- The window creates a scene-within-a-scene (interior + exterior)

## SVG Constraints
- Viewport: 800×600
- Valid SVG 1.1
- No external dependencies (no linked images, fonts, or stylesheets)
- All elements must be defined inline
