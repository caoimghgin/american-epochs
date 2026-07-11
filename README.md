# American Epochs

An interactive instrument for navigating American history through a legitimacy-cycle framework — fourteen named epochs from 1854 to the present, rendered as an explorable, zoomable timeline.

## The Epochs

| Epoch | Years | Theme |
|---|---|---|
| The House Divided | 1854–1865 | The party system fails to contain slavery; the constitutional order goes to war with itself |
| Reconstruction | 1865–1877 | Multiracial democracy attempted, backed by federal troops, abandoned within a generation |
| The Gilded Age | 1877–1897 | Industrial capitalism unchecked for two decades, calling it prosperity |
| The Big Stick | 1898–1912 | Imperial and regulatory confidence compounding — until the coalition splits at its own peak |
| The New Freedom | 1913–1919 | Reform triumph curdles into moral catastrophe inside a single presidency |
| Return to Normalcy | 1920–1932 | The same laissez-faire confidence produces the boom and the bust |
| A New Deal | 1933–1940 | Bold, persistent experimentation whose survival was genuinely in doubt |
| The American Ascendancy | 1941–1952 | Rise of hegemony; the structural pivot before victory |
| The Affluent Society | 1953–1965 | Postwar consensus at its apex; Keynesianism working |
| The Unravelling | 1966–1979 | Civil rights fracture, stagflation, Watergate, malaise |
| Morning in America | 1980–1991 | Reagan ideological regime change; Cold War vindication |
| The End of History | 1992–2007 | Liberal democratic consensus; the Fukuyama era |
| The Interregnum | 2008–2015 | Legitimacy collapse without a successor order |
| The Reckoning | 2016–Present | Accumulated debts arriving; no settled new order |

## Framework

**The House Divided** stands alone, before any cycle: not a crisis phase of an existing order, but the 1787 constitutional order — the one built around slavery — ending outright at Appomattox.

Everything after traces three legitimacy cycles, each following a **Rise → Apex → Crisis** arc (two of them opening with a **Genesis** phase — a founding period whose survival was itself contested, before Rise gets to build on secure ground):

- **The Industrial Order (1865–1932)** — Reconstruction (Genesis) → The Gilded Age (Rise) → The Big Stick (Apex) → The New Freedom → Return to Normalcy (Crisis, in two parts)
- **The New Deal Order (1933–1979)** — A New Deal (Genesis) → The American Ascendancy (Rise) → The Affluent Society (Apex) → The Unravelling (Crisis)
- **The Neoliberal Order (1980–Present)** — Morning in America (Rise) → The End of History (Apex) → The Interregnum → The Reckoning (Crisis, in two parts)

Each cycle's Apex opens when the opposition party adopts its framework (Eisenhower accepting the New Deal; Clinton accepting Reaganism) and each cycle's Crisis begins when its legitimacy conditions fail. The Industrial Order's own ratification moment is less clean-cut — McKinley's 1896 win settles the argument by defeating the Populist challenge outright, rather than by a cross-party adoption — and is treated as an open question rather than a fully worked-out parallel.

## The Instrument

`src/american-epochs.html` is a self-contained, zero-dependency interactive visualization:

- **Three zoom levels** — all history, one cycle, or one epoch. Click the dashed cycle rail or any epoch band to zoom in; the "← [name]" button or `Escape` steps back out one level at a time.
- **Timeline bands** — colored epoch regions scaled by duration; labels that don't fit a narrow band at full zoom-out are suppressed rather than overlapping — hover for the name instead.
- **Event layer** — key historical events plotted by year, filterable by register and by depth (Overview / Standard / Detail).
- **President layer** — administration bars with party color, Pierce (1853) to present.
- **Statistical overlays** — NBER recessions (1854–present), violent crime (FBI UCR, 1960–present only), an argued "RFT regime" thesis (1941–present only) — each toggleable, and each honest about where its own data coverage starts.
- **Detail panel** — click any cycle, epoch, or event to read its interpretive annotation.
- **Register system** — events tagged as *factual*, *structural*, or *argued* (the last rendered with a hatched pattern to flag interpretive claims).

## Usage

Open `src/american-epochs.html` directly in a browser — no build step, no server required.

## Source

`src/american-epochs_v1.md` contains the original written framework covering the first seven epochs (1941–present): full justifications, structural logic, and theoretical grounding. It has not yet been extended to cover the seven earlier epochs added since.
