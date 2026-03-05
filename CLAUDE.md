# Project Context

This is a personal portfolio website hosted via GitHub Pages. The main working file is **index2.html** — this is the active draft where all development and testing happens. When the project is ready, the content will be migrated to **index.html** for production.

## Parallax Scrolling Divider

The centerpiece component is a parallax scrolling section divider designed as a reusable break between surface-level and deeper content. It uses layered SVG illustration with a "Go Farther" text element as its thematic anchor.

### SVG Layer System

- Nine SVG layers exported as full **1600×400 canvases with transparency**, numbered back-to-front (e.g., `layer-01-sky.svg`, `layer-02-mountains.svg`, etc.)
- Spatial relationships are **pre-baked into the SVG exports** — no manual positioning needed. Each layer is a full-canvas file; stacking them naturally reconstructs the scene.
- Do not resize or reposition individual layers relative to each other. The composition depends on all layers sharing the same 1600×400 coordinate space.

### Horizon Anchor

- **y=227** in the 400px canvas is the critical horizon reference point.
- All horizon-pinned layers scale from this exact origin to keep mountains grounded and the trail converging correctly.
- Mountains pin their bottom edge to the horizon; the trail pins its top edge to the horizon.
- Grass and near-tree layers scale from the bottom edge of the frame.

### "Go Farther" Text

- Rendered as **HTML (not SVG)** for independent animation physics.
- Travels from below the horizon (hidden behind the ground layer) upward.
- Its bottom edge lands precisely on the horizon line, measured using `offsetHeight`.
- Styled with a **three-layer drop shadow**.

### Animation Behavior

- Animation begins when the divider **enters the viewport**, not at scroll midpoint.
- Start threshold is set at **progress 0.25**.
- Grass layer speed is set high enough to **exit the frame completely** during scroll.

### Z-Index Stacking

- Ground layer occludes the rising "Go Farther" text (text rises from behind it).
- Lake/shrub and distant tree layers remain visible above the ground layer.
- Stacking order must be preserved — reordering layers will break the occlusion effect.

### Resolved Bugs (Do Not Reintroduce)

- **Horizon drift**: Fixed by removing `translateY` and using pure `scale()` from the correct transform origin. Do not add `translateY` to horizon-pinned layers.
- **Z-index stacking**: Ground must be above the text layer; lake/shrub and distant trees must be above ground.
- **Text animation travel path**: The text must travel upward from below the ground, not fade in or slide from the side.
- **Viewport-relative animation trigger**: Animation is tied to the divider entering the viewport, not to an absolute scroll position.

## Tech Stack

- Plain HTML, CSS, and JavaScript (no frameworks, no build tools)
- SVG layers exported from illustration software (Illustrator or similar)
- Hosted on GitHub Pages
