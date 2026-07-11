# American Epochs — Project Context

## What This Is

A self-contained interactive HTML instrument for exploring American history through a legitimacy-cycle framework. The core claim: American political history since 1854 can be periodized into fourteen named epochs — one standalone rupture, plus thirteen more grouped into three cycles that each follow a Rise → Apex → Crisis arc.

The project is a single HTML file (`src/american-epochs.html`) with zero external dependencies — no build tooling, no framework, no CDN calls. Everything is inline: styles, SVG, JavaScript.

## The Framework

**One standalone rupture, preceding any cycle:**

- **The House Divided (1854–1865)** — phase `Rupture`, `cycle: null`. The party system fails to contain slavery and the constitutional order built around it goes to war with itself. This doesn't belong to a cycle: it's not a crisis phase of an existing order, it's the 1787 constitutional order ending outright. Nothing before it is modeled — pre–Civil War America runs on different structural logic (no standing federal bureaucracy, no income tax, slavery as the central organizing question) than the industrial-and-after nation-state the rest of the framework assumes.

**Three legitimacy cycles:**

1. **The Industrial Order (1865–1932)**
   - Genesis: Reconstruction (1865–1877)
   - Rise: The Gilded Age (1877–1897)
   - Apex: The Big Stick (1898–1912)
   - Crisis: The New Freedom (1913–1919) → Return to Normalcy (1920–1932)

2. **The New Deal Order (1933–1979)**
   - Genesis: A New Deal (1933–1940)
   - Rise: The American Ascendancy (1941–1952)
   - Apex: The Affluent Society (1953–1965)
   - Crisis: The Unravelling (1966–1979)

3. **The Neoliberal Order (1980–Present)**
   - Rise: Morning in America (1980–1991)
   - Apex: The End of History (1992–2007)
   - Crisis: The Interregnum (2008–2015) → The Reckoning (2016–Present)

**Genesis phase:** two cycles open with a "Genesis" epoch rather than starting straight at Rise — a founding period whose survival was genuinely contested (Reconstruction's federal enforcement withdrawn in 1877; the New Deal's own Supreme Court fights and the 1937 court-packing failure) before the cycle's Rise phase gets to build on secure ground. This is a fourth phase value alongside Rise/Apex/Crisis, not a replacement for them.

**Double-Crisis pattern:** two cycles close with a pair of sequential Crisis epochs rather than one (New Freedom → Return to Normalcy; Interregnum → Reckoning) — the first crisis exposes the old order's failure, the second is what happens while nothing yet replaces it.

**Ratification signal:** each cycle's Apex opens when the opposition party accepts the ruling order's terms (Eisenhower/New Deal; Clinton/Reaganism). The Industrial Order's analogous moment is less clean — McKinley's 1896 win settles the argument in favor of gold-standard industrial capital by defeating the Populist challenge rather than by cross-party adoption — worth treating as an open question rather than a fully worked-out parallel.

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

**Epoch hues** (equal visual weight, non-valenced — no epoch should read as "good" or "bad" through color). Token numbers are assignment order, not chronological order — the original seven (`e1`–`e7`) kept their numbers when the seven earlier epochs were appended as `e8`–`e14`, to avoid a gratuitous renumbering diff:
```css
--e1: #6B8E9E   /* Ascendancy */          --e8:  #6B7280  /* The House Divided */
--e2: #7FA07A   /* Affluent Society */    --e9:  #C2A66B  /* Reconstruction */
--e3: #B89B6E   /* Unravelling */         --e10: #B08D57  /* The Gilded Age */
--e4: #9E8AA8   /* Morning in America */  --e11: #5D7A94  /* The Big Stick */
--e5: #6E9B96   /* End of History */      --e12: #A97F8C  /* The New Freedom */
--e6: #A98B7D   /* Interregnum */         --e13: #8C93B8  /* Return to Normalcy */
--e7: #8C8A9E   /* Reckoning */           --e14: #6E8B6B  /* A New Deal */
```

The epoch-band text switches to dark ink instead of paper-white on the lighter tones — currently `e3`, `e9`, `e10` — via an explicit list in `drawEpochs()`. Any new epoch hue light enough to fail contrast against paper-colored text needs adding to that list by hand; it isn't computed from luminance.

**Typography:** Georgia serif for prose/headings; `ui-monospace` for data, labels, controls.

## Register System

Events are tagged with an epistemic register:
- **factual** — documented, uncontested
- **structural** — widely recognized structural turning points
- **argued** — interpretive claims; rendered with a hatched pattern to flag that they're contestable

The `--argued` color and hatched fill are the visual signal that you're in interpretive territory. This is a design decision, not decoration.

## Visualization Layers

The chart is an SVG. At full zoom-out it fills the container width; clicking a cycle or an epoch expands the SVG so that span fills the width instead, and the container scrolls horizontally to it — three navigable levels: all history → one cycle → one epoch (`Escape` or the "← [name]" button steps back out one level at a time). Layers (all toggleable except cycle/epoch bands):
1. **Cycle rail** — dashed boxes spanning each cycle's full range, above the epoch band; click to zoom to cycle level
2. **Epoch bands** — colored regions scaled proportionally by year-span; the epoch name and year range render inside the band only if it's wide enough to hold the text without overlapping its neighbor — narrow bands (common at full zoom-out with 172 years on screen) show as plain color with the name available on hover instead
3. **Event dots** — key historical events, filterable by register and by tier (Overview/Standard/Detail depth control); a dot's label is likewise suppressed rather than forced into an overlapping row when its neighborhood is too crowded — the dot and tooltip still work
4. **President bars** — administration spans with party color (blue/red/mixed), currently Pierce (1853) to present
5. **Statistical overlays** — recessions (NBER, 1854–present), violent crime (FBI UCR, 1960–present only), RFT regime (argued, 1941–present only)

Overlay lanes are not required to span the full timeline — a lane simply doesn't render before its data exists, and the sources footer says so explicitly, rather than the gap reading as "zero" or "not applicable." Crime has no comparable series before UCR began in 1930; RFT is a thesis about the postwar order specifically, not a claim about earlier epochs.

Clicking an epoch region, a cycle region, or an event dot populates the detail panel below the chart with interpretive prose.

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
- The SVG chart is generated programmatically from data arrays in JS:
  - `EPOCHS` — one entry per epoch (`id`, `name`, `start`, `end`, `color`, `cycle`, `phase`, `oneliner`, `rationale[]`). `cycle` is `null` for The House Divided; `phase` is one of `Rise`/`Apex`/`Crisis` plus `Genesis` and `Rupture`. The array must stay in chronological order — `drawEpochs()` uses adjacent-index comparison to render the seam gradient between neighboring epochs.
  - `CYCLES` — `{ id, name }` only; each cycle's date span is derived on the fly from whichever `EPOCHS` entries reference that `cycle` id, not stored redundantly.
  - `EVENTS`, `PRESIDENTS`, `RECESSIONS`, `CRIME`, `RFT` — unchanged in shape from before; `PRESIDENTS` now starts at Pierce (1853), `RECESSIONS` uses NBER's dated contractions back to December 1854.
  - `YEAR_MIN`/`YEAR_MAX` bound the whole axis (currently 1852–2027) — the small lead-in before `YEAR_MIN` matches the first epoch's `lead` field, same convention used for Ascendancy's own 1939 lead before its 1941 start.
- Zoom state is a single `zoomTarget` (`null`, `{type:'cycle', id}`, or `{type:'epoch', id}`), with `targetSpan()`/`cycleSpan()`/`epochSpan()` as the shared helpers between the zoom-width math and the cycle-rail rendering. `backTarget()`/`backLabel()` derive the "step back one level" target and its button text from `EPOCHS`/`CYCLES` directly rather than tracking a separate navigation stack.
- `--now` (terracotta red) is intentionally reserved for only two uses: the standpoint marker (a line at the current year) and the active selection highlight. Do not repurpose it.
- The "argued" register uses both color (`--argued`) and texture (hatched fill) to ensure the interpretive signal survives color-blind viewing.
- Epoch name/year-range `<text>` elements are children of their epoch's own `<g class="epoch-region">`, not siblings appended to the chart root — this matters because SVG events only bubble through actual ancestors; a text node appended as a later sibling sits visually on top of the band (intercepting clicks) without ever forwarding them to the band's own listeners.
