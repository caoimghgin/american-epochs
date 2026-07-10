# American Epochs — Project Context

## What This Is

A self-contained interactive HTML instrument for exploring American history through a legitimacy-cycle framework. The core claim: American political history since 1941 can be periodized into seven named epochs, grouped into two cycles that each follow a Rise → Apex → Crisis arc.

The project is a single HTML file (`src/american-epochs.html`) with zero external dependencies — no build tooling, no framework, no CDN calls. Everything is inline: styles, SVG, JavaScript.

## The Framework

**Two legitimacy cycles:**

1. **The New Deal Order (1941–1979)**
   - Rise: The American Ascendancy (1941–1952)
   - Apex: The Affluent Society (1953–1965)
   - Crisis: The Unravelling (1966–1979)

2. **The Reagan-Clinton Synthesis (1980–Present)**
   - Rise: Morning in America (1980–1991)
   - Apex: The End of History (1992–2007)
   - Crisis: The Interregnum (2008–2015) → The Reckoning (2016–Present)

**Ratification signal:** Each cycle is ratified when the opposition party accepts the ruling order's terms (Eisenhower/New Deal; Clinton/Reaganism).

## Design System

The visualization uses an archival paper aesthetic — deliberate, not decorative. It signals that this is a document, not a dashboard.

**Color tokens:**
```css
--paper: #E8EAE6      /* cool archival paper */
--paper-2: #DFE2DD    /* slightly deeper panel */
--ink: #1B1D1A        /* near-black warm */
--ink-soft: #5A5E58   /* secondary text */
--ink-faint: #8A8E87  /* tertiary / axis */
--rule: #C6C9C1       /* hairlines */
--now: #B5503C        /* RESERVED: standpoint marker + active selection only */
--argued: #7A746A     /* interpretive ink */
```

**Epoch hues** (equal visual weight, non-valenced — no epoch should read as "good" or "bad" through color):
```css
--e1: #6B8E9E   /* Ascendancy */
--e2: #7FA07A   /* Affluent Society */
--e3: #B89B6E   /* Unravelling */
--e4: #9E8AA8   /* Morning in America */
--e5: #6E9B96   /* End of History */
--e6: #A98B7D   /* Interregnum */
--e7: #8C8A9E   /* Reckoning */
```

**Typography:** Georgia serif for prose/headings; `ui-monospace` for data, labels, controls.

## Register System

Events are tagged with an epistemic register:
- **factual** — documented, uncontested
- **structural** — widely recognized structural turning points
- **argued** — interpretive claims; rendered with a hatched pattern to flag that they're contestable

The `--argued` color and hatched fill are the visual signal that you're in interpretive territory. This is a design decision, not decoration.

## Visualization Layers

The chart is an SVG rendered at 100% viewport width. Layers (all toggleable):
1. **Epoch bands** — colored regions scaled proportionally by year-span
2. **Event dots** — key historical events, filterable by register
3. **President bars** — administration spans with party color (blue/red/mixed)
4. **Statistical overlays** — crime rate, unemployment, S&P 500

Clicking an epoch region or event dot populates the detail panel below the chart with interpretive prose.

## File Structure

```
american-epochs/
├── README.md
├── CLAUDE.md
└── src/
    ├── american-epochs.html    # The instrument — open directly in browser
    └── american-epochs_v1.md  # Written framework: full epoch justifications
```

## Development Notes

- No build step. Open the HTML file directly.
- All styles are in a `<style>` block in `<head>`. All JS is in a `<script>` block before `</body>`.
- The SVG chart is generated programmatically from a data array in JS — edit the `EPOCHS` and `EVENTS` arrays to change content.
- `--now` (terracotta red) is intentionally reserved for only two uses: the standpoint marker (a line at the current year) and the active selection highlight. Do not repurpose it.
- The "argued" register uses both color (`--argued`) and texture (hatched fill) to ensure the interpretive signal survives color-blind viewing.
