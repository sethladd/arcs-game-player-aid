# Arcs Player Aid — source

Source for `Arcs_Player_Aid_Alternative.pdf`, a 2-page (double-sided US Letter, color)
player reference for **Arcs** with Leaders & Lore.

The PDF is generated from a single hand-written HTML file printed through headless
Chromium. There is no design-app file — `arcs_player_aid.html` *is* the source.

> **Two copies exist in this project.** `arcs-aid-source.zip` at the project root is the
> complete, buildable bundle — unzip it to work on the aid. The loose `source/` folder
> holds only the HTML and this README, for quick reading and diffing; it will not render
> on its own because it has no `img/` or `fonts/` beside it.

```
arcs_player_aid.html   the whole layout: content + CSS, one file
img/                   45 icons/artwork used by the layout
fonts/                 the two official Arcs typefaces (woff2)
build.sh               rebuild the PDF
font-specimen.html     type-test page used to pick sizes & emphasis
```

## Rebuild

```bash
./build.sh                 # writes Arcs_Player_Aid_Alternative.pdf
./build.sh somewhere.pdf   # or name your own output
```

Or open `arcs_player_aid.html` in Chrome and print to PDF with **Margins: None**,
**Background graphics: on**, paper **Letter**.

Always verify after a rebuild:

- exactly **2 pages**
- nothing clipped at a page or column edge
- the PDF embeds **only** the two official fonts (see *Missing glyphs* below)

## Where the art came from

Everything in `img/` is official Leder Games art, not a recreation:

- Dice faces, resource tokens, ambition markers, player pieces, and the initiative
  marker come from the `icon and punchboard/` dev-asset folder.
- The five ambition pictograms (Tycoon/Tyrant/Warlord/Keeper/Empath) were cropped at
  600 DPI from page 4 of `Arcs_Aid_Booklet.pdf` and converted to transparent ink.
- The four action-card thumbnails were cropped at 450 DPI from page 1 of the same booklet.
- `ship_tipped.png` is the blue ship rotated 62° to show a damaged (tipped) ship.
- The suit pips are inline SVG (a 4-pointed diamond), tinted per suit — the fonts have
  no pip glyph. Colors: Administration `#8b8574`, Aggression `#bf3a2f`,
  Construction `#c05a28`, Mobilization `#2e7f92`.

## Typography — read this before editing

Headers use **FM Bolyar Rough NPro 900**, body uses **Neue Kabel Book**. Three traps:

**1. Both fonts are single-weight — no bold, no italic.**
Any `font-weight` above 400 makes the browser fake a bold, which smears at small print
sizes. The stylesheet therefore forces `*{font-weight:400 !important}` and creates
emphasis with a hairline stroke instead:

```css
b,strong{-webkit-text-stroke:.3px currentColor}
```

Don't reintroduce `font-weight:700`. If you need heavier emphasis, raise the stroke
(`.3px` → `.36px`), don't change the weight. Italics render as a synthetic oblique,
which is acceptable at these sizes.

**2. Missing glyphs.** Neither font contains `→ ① ② ③ ④ ▸ ▼ ↔`. They fall back to a
system font silently — it looks almost right on screen and wrong in print. Substitutes
already in use: `»` for arrows, `<span class="ref">N</span>` circles for step
references, CSS border-triangles for `▸` and `▼`.

To audit after editing, check the built PDF's embedded fonts — anything beyond
`FMBolyarRoughNPro-900` and `NeueKabelW01-Book` means a character fell back:

```python
import fitz
doc = fitz.open("Arcs_Player_Aid_Alternative.pdf")
for i, page in enumerate(doc):
    print(i + 1, sorted({f[3] for f in page.get_fonts(full=True)}))
```

**3. Neue Kabel sets wide and has a low x-height** (443/1000). It needs more horizontal
room than most UI sans faces. Fixed-width labels (`.suit .nm`, `.amb .nm`) and the
`white-space:nowrap` section titles are the first things to overflow — if you lengthen a
heading, check it still fits, or tag it `class="long"` (9.4px) or `class="xlong"` (8.6px).

## Fitting content

Each page is scaled by its own CSS `zoom` so it fills the sheet:

```css
.pg1{zoom:1.30;width:6.5385in;height:8.4615in}
.pg2{zoom:1.13;width:7.5221in;height:9.7345in}
```

**Width must equal `8.5 / zoom` inches and height `11 / zoom` inches** — if you change
the zoom, recompute both. Raise zoom to fill a page with slack at the bottom; lower it
if content clips or spills to a third page. Tune the two pages independently.

## Layout

**Page 1 — the round and your turn.** A strip for the round (initiative leads → others
follow → check initiative → discard), a matching strip for the four steps of your turn,
then those steps expanded in reading order: ① play your card (lead / surpass / copy /
pivot), ② declare or seize, ③ prelude, ④ take actions.

**Page 2 — battle and winning.** Battle workflow and dice-resolution order, all eighteen
die faces, resolving a battle & triggering Outrage, the five ambitions, chapter-end
scoring, then reference: pieces, terms, the Court, and Leaders & Lore.

## Notes

- Rulebook page refs: turns 8–11 · actions 12–16 · resources 17 · scoring 18–19 ·
  Leaders & Lore 21.
- Arcs is by Cole Wehrle, art by Kyle Ferrin, © Leder Games. This is an unofficial
  player reference and not a substitute for the rules. The fonts and art in this folder
  are licensed/owned copies — don't redistribute them.
