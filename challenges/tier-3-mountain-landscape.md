# Tier 3 — Mountain Landscape

## Tier
3 (Stress Test)

## Description
A layered landscape scene at sunset with atmospheric perspective. This is the hardest challenge — it requires conveying depth through colour gradation, managing many overlapping elements, and creating a naturalistic scene that "feels right" to the human eye. Most approaches will struggle here; the interesting question is which methodology degrades most gracefully.

## SVG Prompt

Create an SVG showing a mountain landscape at sunset with the following elements:

- **Sky:** a gradient transitioning from warm orange/pink at the horizon to deep purple/navy at the top. Include 2-3 clouds with realistic soft shading (not flat shapes)
- **Mountains:** a range of at least 3 overlapping mountain peaks with snow caps. Mountains should get progressively lighter/hazier as they recede (atmospheric perspective)
- **Trees:** a mid-ground tree line of pine/evergreen trees at varying sizes. Trees closer to the viewer should be larger, darker, and more detailed. Trees further away should be smaller, lighter, and simpler
- **Lake:** a foreground lake that reflects the sky gradient and the silhouettes of the nearest mountains. The reflection should be a muted, slightly distorted version of the scene above
- **Depth cues:** the entire scene should convey depth through:
  - Atmospheric perspective (distant elements are lighter, less saturated, hazier)
  - Size gradation (closer elements are larger)
  - Overlap (closer elements in front of distant ones)
  - Colour temperature shift (warm foreground, cool background)

The scene should evoke a sense of calm and grandeur.

## Complexity Indicators
- Atmospheric perspective requires progressive colour/opacity adjustments across layers
- Reflections in the lake require mirroring and distorting elements
- Natural shapes (mountains, trees, clouds) are harder than geometric shapes
- Managing 5+ depth layers with correct ordering
- Colour gradients across the entire scene must feel cohesive
- Clouds with realistic shading push SVG path complexity

## SVG Constraints
- Viewport: 1000×600
- Valid SVG 1.1
- No external dependencies (no linked images, fonts, or stylesheets)
- All elements must be defined inline
- Use SVG gradients (`<linearGradient>`, `<radialGradient>`) and filters where appropriate
