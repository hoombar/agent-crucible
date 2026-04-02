# Faces — Pencil Sketch Portraits at Three Skill Levels

## Description
A single SVG containing 6 faces arranged in a 3×2 grid: three artist skill levels (beginner, intermediate, advanced) × two emotions (happiness, sadness). All faces are rendered as **graphite pencil sketches** — monochrome, using only strokes and hatching for shading, on a warm off-white paper background.

This challenge exploits human face perception — we're hardwired to detect even tiny proportion errors, making faces the ultimate test of whether a collaboration methodology can produce visually convincing output. The pencil sketch medium pushes SVG generation to its limits: the advanced level requires simulating cross-hatching, varied stroke weight, and shading through line density using raw SVG paths.

The skill-level progression adds an extra dimension: agents must deliberately vary their detail level and technique, not just produce one quality. A beginner sketch should look intentionally rough, not like a failed attempt at the advanced style.

## SVG Prompt

Create a single SVG containing 6 pencil-sketch faces arranged in a 3-row × 2-column grid. Each row represents an artist skill level, each column represents an emotion. The entire image should look like a page from an artist's sketchbook.

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

Include text labels for each row (skill level) and column (emotion), styled as handwritten pencil annotations.

### Medium: Graphite Pencil Sketch

All faces must be rendered as pencil drawings:
- **Monochrome only** — black, greys, and the off-white paper background. No colour.
- **Strokes, not fills** — shading is achieved through line density, hatching, and cross-hatching, not through solid grey fills or gradients
- **Varied stroke weight** — use `stroke-width` variations to simulate pencil pressure (light construction lines vs heavy contour lines)
- **Paper background** — warm off-white (#F5F0E8 or similar) to simulate sketchbook paper
- **Pencil-like line quality** — slightly imperfect, organic-feeling paths rather than perfectly smooth curves. Real pencil lines have subtle wobble.

### Skill Level Definitions

**Beginner:**
- Simple contour lines only — single-weight outlines
- Circle for head, basic arcs and dots for features
- No shading, no hatching
- Rough proportions (eyes too high, face too round) — like a first attempt at drawing a face
- Think: someone picking up a pencil for the first time
- The emotion should still be readable despite the crudeness

**Intermediate:**
- Proper face proportions (eyes at midpoint, nose halfway between eyes and chin)
- Contour lines with some weight variation (heavier for jawline and features, lighter for construction)
- Basic directional hatching for shadows (under nose, under chin, one side of face)
- Defined features: nose with bridge and nostrils, eyebrows with individual strokes, lips with upper/lower distinction
- Hair rendered as grouped directional strokes, not a solid shape
- Think: an art student who has learned the fundamentals

**Advanced:**
This is the stress test. The advanced face should aim for the quality of a professional commissioned graphite portrait. This means an extremely high SVG element count — expect **200+ individual `<line>` or `<path>` elements per face** just for hatching alone. The specific technical requirements are:

- **Face contour:** NOT a single ellipse or path. The jawline, chin, and forehead should be built from multiple overlapping strokes of varying weight (stroke-width 1-2px), like a real pencil being drawn and redrawn to find the form
- **Hatching for shading:** Parallel `<line>` elements at approximately 45° for base shadows. Lines should be spaced 2-4px apart in mid-tones, 1-2px apart in deep shadows, and absent in highlights (paper shows through). Use stroke-width 0.5-1px for hatching lines
- **Cross-hatching for depth:** A second layer of lines at approximately 135° (perpendicular to the first) in the darkest areas only — under the chin, side of the nose, deep eye sockets. This creates the richest darks
- **Tonal range:** At least 4 distinct tonal zones per face — paper-white highlights (no strokes), light tone (sparse hatching), mid-tone (dense hatching), dark (cross-hatching). The transitions between zones should be gradual, achieved by changing line spacing
- **Hair:** Individual stroke groups (5-10 `<line>` elements per group) following the direction of hair growth. Varying stroke-width (0.5-1.5px) and spacing to show light/dark areas in the hair. NOT a solid filled shape with lines on top
- **Eyes:** The most detailed feature. Iris rendered with tiny radial strokes. Heavy lids drawn with multiple overlapping strokes. Highlight dot left as negative space (no stroke). Lashes as individual fine lines
- **Nose:** Built entirely from shadow hatching — minimal outline. The nose shape should emerge from the hatching beneath it and beside it, not from a drawn contour line
- **Clothing suggestion:** Collar/neckline with fabric folds rendered as directional hatching following the drape of fabric
- **Background vignette:** Loose, long hatching strokes (stroke-width 1-2px) radiating behind the head, denser near the head and fading out toward the edges. This lifts the portrait off the page
- Think: a professional graphite portrait like those sold on Etsy for £100+ — someone would frame this

### Emotion Requirements

**Happiness:**
- Beginner: upward arc mouth, dot eyes — classic simple smiley sketch
- Intermediate: genuine smile with raised cheeks visible in contour, slightly squinted eyes shown through line weight, raised eyebrows with individual hair strokes
- Advanced: Duchenne smile — crow's feet rendered as fine radiating lines, raised cheek muscles shown through hatching density shift, relaxed forehead with minimal lines, natural teeth suggestion (not every tooth drawn), warmth conveyed through softer stroke quality around the eyes

**Sadness:**
- Beginner: downward arc mouth, simple dot eyes — basic frowny sketch
- Intermediate: drooped mouth corners with heavier line weight, lowered eyebrows with downward angle, downcast eyes with heavier upper lid lines
- Advanced: subtle grief — slight lip compression shown through tighter cross-hatching around the mouth, inner eyebrows raised (corrugator muscle), heavy shading under eyes, tension in forehead rendered as light horizontal hatching, slight head tilt, overall heavier tonal quality than the happy version

### Consistency Requirements
- All 6 faces should be the same person at different skill levels — similar hair style, face shape, and general features, just rendered with increasing pencil technique
- The grid should have consistent cell sizing with even spacing
- Each face should be centred within its cell
- The entire SVG should feel like one sketchbook page, not 6 disconnected drawings

## Complexity Indicators
- Facial proportions are extremely sensitive — humans detect errors instantly (uncanny valley)
- Pencil hatching in SVG requires many individual path strokes — each advanced face needs 200+ elements for hatching alone, so the full SVG could contain 500+ elements
- Cross-hatching requires coordinating stroke angles and density to create tonal variation
- Varied stroke weight tests precise `stroke-width` control
- Deliberate skill-level variation is harder than a single quality level — the agent must control its own technique
- Conveying emotion through pencil strokes (not colour or fills) is much harder than with colour
- 6 faces in one SVG tests compositional layout and consistency
- The monochrome constraint means shading does all the work — no colour to hide behind

## SVG Constraints
- Viewport: 1200×800 (landscape to fit the 3×2 grid)
- Valid SVG 1.1
- No external dependencies (no linked images, fonts, or stylesheets)
- All elements must be defined inline
- Text labels for rows and columns should use basic SVG `<text>` elements
- **No solid fills for shading** — use stroke-based hatching and cross-hatching only
- **No `<filter>` elements** — achieve all effects through path strokes
- Colour palette restricted to: paper background (#F5F0E8), and strokes in black/dark grey (#1A1A1A to #666666)
