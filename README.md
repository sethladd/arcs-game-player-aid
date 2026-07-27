# Alternative Arcs Player Aid

A two-page, double-sided player reference for **[Arcs](https://ledergames.com/arcs)** (Leder Games),
covering the base game plus Leaders & Lore. Print it on one sheet of US Letter, in color, and put it
on the table.

**→ [`Arcs_Player_Aid_Alternative.pdf`](Arcs_Player_Aid_Alternative.pdf)** — the thing you print.

It is a *reference*, not a tutorial. It assumes the game is already set up and skips setup entirely.

## Why this exists

A useful, complete player aid that includes a very clear turn structure inclusive of Prelude.

The turn is presented as four numbered steps, in order, and those same numbers
label the sections that expand them — so you can follow the sequence top to bottom without hunting.

## What's on each page

**Page 1 — the round and your turn.** How a round flows (initiative leads → others follow → check
initiative → discard), then the four steps of your own turn: ① play your card (lead / surpass / copy
/ pivot), ② declare an ambition or seize the initiative, ③ prelude, ④ take actions. Includes what
every resource does when spent, and a one-line glossary of all seven standard actions.

**Page 2 — battle and winning.** The battle workflow and the exact order dice resolve, all eighteen
die faces reproduced from the real dice, what happens after a battle including Outrage and
Ransacking, the five ambitions and what each counts, chapter-end scoring, and a reference block of
pieces and terms.

A few rulings are called out explicitly because the official aid leaves them ambiguous — for example,
on an ambition tie for first place, the tied players take second-place Power *and everyone else
scores nothing*, because second place is used up (Base Rulebook p. 18).

## Repository contents

```
Arcs_Player_Aid_Alternative.pdf      the deliverable — print this
arcs-aid-source.zip                  buildable source (unzip to work on it)
source/                              loose copy of the HTML + README, for reading and diffing
icon and punchboard/                 official icon & punchboard art from the Arcs dev kit
fonts/                               the two Arcs typefaces — untracked, supply your own (see below)
Arcs_Base_Rulebook.pdf               reference
Arcs_Aid_Booklet.pdf                 reference — the official aid this one is an alternative to
Arcs_Campaign_Rulebook_for_web.pdf
```

## Getting the fonts

**The two typefaces are not in this repository.** They're commercial fonts that can't be
redistributed, so `fonts/` is git-ignored and you need to supply your own licensed copies before you
can build.

| Role | Typeface | Where to license it |
| --- | --- | --- |
| Titles & headers | **FM Bolyar Rough NPro 900** | Fontfabric — <https://www.fontfabric.com/fonts/bolyar-sans/> |
| Body text & rules | **Neue Kabel**, Book weight | Monotype/Linotype — <https://www.myfonts.com/collections/neue-kabel-font-linotype> |

The build looks for exactly two files:

```
arcs-aid-source/fonts/bolyar900.woff2     FM Bolyar Rough NPro 900
arcs-aid-source/fonts/kabelbook.woff2     Neue Kabel Book
```

Most licensed font purchases include a `Web Fonts` folder with a `.woff2` already in it — just rename
it. If you only have `.otf` or `.ttf`, convert with `fonttools`:

```bash
pip install fonttools brotli
python -c "
from fontTools.ttLib import TTFont
f = TTFont('FMBolyarRoughNPro-900.otf'); f.flavor='woff2'; f.save('fonts/bolyar900.woff2')
f = TTFont('Neue Kabel W01 Book.otf');   f.flavor='woff2'; f.save('fonts/kabelbook.woff2')
"
```

`arcs-aid-source/fonts/README.md` repeats all of this next to where the files belong.

## Editing and rebuilding

There is no design-app file. The PDF is generated from a single hand-written HTML file printed
through headless Chromium — `arcs_player_aid.html` *is* the source.

```bash
unzip arcs-aid-source.zip
cd arcs-aid-source
cp /path/to/your/*.woff2 fonts/    # see "Getting the fonts" above
./build.sh                         # writes Arcs_Player_Aid_Alternative.pdf
```

Or open the HTML in Chrome and print to PDF with **Margins: None**, **Background graphics: on**,
paper **Letter**.

`build.sh` refuses to run if either font is missing. That's deliberate — a browser silently
substitutes a default font rather than erroring, which gives you a PDF that looks approximately right
on screen and wrong in print.

The bundle's `README.md` documents the layout system and the non-obvious traps — chiefly that both
fonts are single-weight (so emphasis uses a text stroke, never `font-weight`), that neither font
contains arrows or circled numerals, and how the per-page scaling arithmetic works. Read it before
making changes.

## Where the assets came from

The icons, punchboard art, and the game's font specification all come from the
**[Arcs Official Development Kit](https://ledergames.com/blogs/news/arcs-the-official-development-kit)**,
which Leder Games released for people making custom Arcs content. It contains, in their words,
"pretty much every icon and card template in the game (and the campaign game)," along with an
InDesign file carrying the game's paragraph styles.

Per the kit, the game's typefaces are:

| Role | Typeface |
| --- | --- |
| Titles & headers | **FM Bolyar Rough NPro 900** |
| Body text & rules | **Neue Kabel** (Book weight) |

Both are commercial fonts and are **not** distributed by the kit, nor by this repository — see
[Getting the fonts](#getting-the-fonts).

A handful of marks weren't in the kit and were extracted from the official PDFs instead: the five
ambition pictograms and the action-card corners were cropped at high resolution from
`Arcs_Aid_Booklet.pdf`. The suit pips are drawn as inline SVG, since they exist only as artwork.

## Before publishing this repository

The licensed fonts are git-ignored and are not in the commit history, so they won't leak.

Still tracked, however, are the rulebook PDFs and the dev-kit icon art, both originally released by
Leder Games. Those are lower risk — they were distributed freely, and the dev kit existed precisely so
people could make things like this — but they are still Arcs' copyrighted material (now held by
Buried Giant Studios, see [License](#license)), not yours to republish. If you push this somewhere
public, consider ignoring `icon and punchboard/` and the three rulebook PDFs too, and pointing
contributors at the dev kit to fetch their own copies.

## License

The original text, layout, and design in this repository are licensed under
[CC BY-NC-SA 4.0](LICENSE). That license does not extend to Arcs' copyrighted
material (icons, art, rules text, the Arcs name/logo), which stays with its
rights holder — see [`LICENSE`](LICENSE) for the exact split and the ownership
history behind it.

## Credits

*Arcs* is designed by **Cole Wehrle** and illustrated by **Kyle Ferrin**. It was originally published
by Leder Games; as of early 2026, ownership of Arcs has moved to
**[Buried Giant Studios](https://buriedgiant.com)**, the studio Wehrle and Ferrin founded after
leaving Leder (Leder retained Root). All game art, icons, and text are the property of Arcs' current
rights holder. The rulebook PDFs and dev-kit icon art in this repository predate that move and were
originally released by Leder Games.

This is an unofficial fan-made player aid. It is not affiliated with or endorsed by Leder Games or
Buried Giant Studios, and it is not a substitute for the rulebook — where the two disagree, the
rulebook wins.

This player aid was co-created by Seth Ladd and Claude (Anthropic), working together on the rules
distillation, layout, and build tooling.
