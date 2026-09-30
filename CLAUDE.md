# CLAUDE.md

Public GitHub Pages site of John M. Drake's rendered talks
(<https://jdrakephd.github.io/talks/>). Read `README.md` for the full
publishing procedure; these are the rules to follow when working here.

## What belongs in this repo

- Rendered output only. Never add `.qmd` source, `_extensions/`, render
  scripts or notes files. Source lives in the private `jdrakephd/talks-src`
  repo (local clone `~/Desktop/Projects/talks-src`) or in a research repo.
- One folder per talk, named `YYYY-MM-venue-topic`, containing exactly:
  - `index.html`, the deck as a single self-contained file
  - `<venue-topic>.pdf`, the Decktape export (the slug without the date)
- A landing-page entry in `index.html`, newest first, in the same `<li>`
  format as the existing entries (title link, meta line, `slides · PDF` line).
- `.nojekyll` must stay.

## Fully packaged decks

`index.html` must work when downloaded on its own and opened offline. That
means no `_files/`, `figures/`, `katex/` or CDN references at runtime.

- Publish with `talks-src/publish.sh <slug>`. It renders with
  `embed-resources`, runs `talks-src/embed-math.cjs` to pre-render equations
  and inline KaTeX, exports the PDF, and stages the folder here.
- `embed-resources` alone is not enough for decks with math: Quarto still
  loads KaTeX or MathJax from a URL at runtime. The equations must be
  pre-rendered by `embed-math.cjs`.
- Before committing, verify:
  1. The folder contains only `index.html` and the PDF.
  2. `index.html` has no runtime references to local paths or CDNs; the only
     `src`/`href` values should be `data:` URIs, anchors or external links in
     slide text. There should be no `renderMathElements` loader and no
     `<span class="math ...">` left unrendered.
  3. Copied alone into an empty directory, the deck renders its figures and
     equations. A quick check is Decktape on a few slides with equations:
     `npx decktape reveal --slides 1,9,12 "file:///tmp/iso/index.html" /tmp/t.pdf`.
- Keep each `index.html` under about 50 MB (GitHub's hard limit is 100 MB);
  downsample figures in the source if a deck grows past that.
- Older talks predate this rule: `2026-08-esa-how-to-be-a-good-editor/` and
  `2026-09-uga-law-pandemic-planet/` ship asset folders, and
  `2026-07-esa-spatial-spread/` loads KaTeX from a CDN. Don't copy their
  layout; package them properly when republishing. (`2026-06-eds-…` has no
  equations; its CDN strings are unused reveal.js defaults.)

## Speaker notes are public

Reveal.js embeds the `::: {.notes}` blocks in the HTML. Don't publish internal
reminders (`[CHECK …]`, `[TODO …]`), unverified claims flagged for the
speaker, or anything the speaker hasn't cleared for public view.
`publish.sh` blocks `[CHECK`, `[TODO` and `[FIXME`.

## Commits

- Author: John Drake <john@drakeresearchlab.com>.
- Messages: `Add <Venue Year> talk: <Short title>` for new talks,
  `Republish <slug>: <what changed>` for updates. Explain in the body what
  changed and why.
- After pushing, confirm the Pages build finished
  (`gh api repos/jdrakephd/talks/pages/builds/latest`) and that the slides
  and PDF URLs return 200.
