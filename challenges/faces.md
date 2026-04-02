# Faces — Expressive Portraits at Three Skill Levels

## Description
A single SVG containing 6 faces arranged in a 3×2 grid: three artist skill levels (beginner, intermediate, advanced) × two emotions (happiness, sadness). This challenge exploits human face perception — we're hardwired to detect even tiny proportion errors, making faces the ultimate test of whether a collaboration methodology can produce visually convincing output.

The skill-level progression adds an extra dimension: agents must deliberately vary their detail level, not just produce one quality. A beginner face should look intentionally simple, not like a failed attempt at realism.

## SVG Prompt

Create a single SVG containing 6 faces arranged in a 3-row × 2-column grid. Each row represents an artist skill level, each column represents an emotion.

### Grid Layout

```
          HAPPY              SAD
         ┌──────────┬──────────┐
BEGINNER │  face 1  │  face 2  │
         ├──────────┼──────────┤
INTERMED │  face 3  │  face 4  │
         ├──────────┼──────────┤
ADVANCED │  face 5  │  face 6  │
         └──────────┴──────────┘
```

Include text labels for each row (skill level) and column (emotion).

### Skill Level Definitions

**Beginner:**
- Simple geometric shapes — circles for head and eyes, arcs for mouth
- Minimal detail — no shading, no complex features
- Should look like a deliberate, charming smiley-face style drawing
- Think: what a child or first-time artist would draw
- The emotion should still be clearly readable despite the simplicity

**Intermediate:**
- Properly proportioned face shape (oval, not a perfect circle)
- Defined features: nose, eyebrows, ears, hair
- Basic shading using lighter/darker tones
- Expressive features that convey the emotion through multiple cues (eyebrow angle, mouth shape, eye shape)
- Think: a competent hobbyist artist

**Advanced:**
- Portrait-quality face with realistic proportions
- Nuanced expression — emotion conveyed through subtle muscle positions, not just mouth shape
- Skin tone gradients, highlights, and shadows
- Detailed eyes with highlights/reflections
- Hair with volume and texture
- Think: a skilled portrait artist working in a stylised medium

### Emotion Requirements

**Happiness:**
- Beginner: upward arc mouth, dot eyes — classic smiley
- Intermediate: genuine smile with raised cheeks, slightly squinted eyes, raised eyebrows
- Advanced: Duchenne smile — crow's feet at eyes, raised cheek muscles, relaxed forehead, warmth in the eyes

**Sadness:**
- Beginner: downward arc mouth, simple dot eyes — classic frowny face
- Intermediate: drooped mouth corners, lowered eyebrows, downcast eyes
- Advanced: subtle grief — slight lip compression, inner eyebrows raised, glistening eyes, tension in the forehead, slight head tilt

### Consistency Requirements
- All 6 faces should appear to be the same person at different skill levels — similar hair colour, face shape, and general features, just rendered with increasing detail
- The grid should have consistent cell sizing with even spacing
- Each face should be centred within its cell
- Background colour should be neutral (light grey or off-white) to let the faces stand out

## Complexity Indicators
- Facial proportions are extremely sensitive — humans detect errors instantly (uncanny valley)
- Deliberate skill-level variation is harder than just "draw a face" — the agent must control its own detail level
- Conveying emotion through geometry (not labels) requires understanding of facial anatomy
- Advanced faces push into portrait territory where SVG path complexity gets very high
- 6 faces in one SVG tests compositional layout and consistency across the grid
- The same person at different skill levels tests ability to maintain identity across rendering styles

## SVG Constraints
- Viewport: 1200×800 (landscape to fit the 3×2 grid)
- Valid SVG 1.1
- No external dependencies (no linked images, fonts, or stylesheets)
- All elements must be defined inline
- Text labels for rows and columns should use basic SVG `<text>` elements
