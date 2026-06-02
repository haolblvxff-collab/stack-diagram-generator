# Stack Diagram Generator

Single-file HTML tool for semiconductor device stack cross-section diagrams.

## Tech Stack
- Vanilla HTML/CSS/JS (no frameworks, no build step)
- SVG rendering engine with path-based geometry
- Must run in Safari (no ES6+ features like arrow functions, let/const, template literals)

## File Structure
```
index.html   — Everything (CSS + HTML + JS in one file, ~33KB)
```

## Key Architecture

### Rendering Engine (`render()` function)
- Surface array tracks cumulative height at NSEG=120 sample points
- Layers sorted by y_bottom, rendered bottom-up
- Overlap groups: layers sharing same y_bottom get a cloned baseline surface
- Bevel system: 45° sloped sidewalls at mask edges using interpolated surface heights via `sfAt()`
- Two deposition modes:
  - **Blanket**: flat top at absolute y coordinate, fills steps
  - **Conformal**: follows surface with uniform thickness offset
- Mask system: 15 predefined patterns (gate, gsg, grating, ridge, via, etc.) as [0..1] intervals

### Coordinate System
- y0 = drawing bottom, layers stack UP (decreasing y pixel values)
- ThickPx = (y_top - y_bottom) / yMax * chartHeight
- Labels use surface[k] for positioning with auto-stagger

### Presets
- MEMS, LNOI, SiPH, GaAs HEMT, LNOI Modulator
- `tty()` helper stacks layers sequentially from y=0

## Constraints
- MUST remain single HTML file — no external CSS/JS/images
- MUST work in Safari (no Float64Array, no arrow functions, no ES modules)
- All 5 presets must render identically after changes
- SVG output must be pixel-identical
- Material color database (CDB) lookup: longest-key-first matching after subscript normalization
