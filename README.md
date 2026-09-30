# talks

Public, rendered slide decks for John M. Drake, served via GitHub Pages at
**https://jdrakephd.github.io/talks/**.

Source `.qmd` files live in the private
[`talks-src`](https://github.com/jdrakephd/talks-src) repo, or in a research
repo when a talk belongs to one project (the UGA Law decks are in
`pandemic-planet/talks/`). This repo holds only the *rendered* output; the
source is never duplicated here.

## Every deck is fully packaged

Each talk folder holds exactly two files:

```
<YYYY-MM-venue-topic>/
  index.html                  # the deck, as ONE self-contained file
  <venue-topic>.pdf           # Decktape export of the same deck
```

`index.html` must work when someone downloads it on its own and opens it
offline. That means every figure, library, font and equation is embedded in
the file, and nothing is loaded from a sibling folder or a CDN. Expect decks
of 10–25 MB; GitHub warns above 50 MB and rejects files over 100 MB, so
downsample oversized figures before rendering if a deck gets near that.

Quarto's `embed-resources` does most of this, but not all of it. It inlines
images, reveal.js and theme CSS, yet it always loads the math library (KaTeX
or MathJax) at runtime, so equations break offline. `talks-src/embed-math.cjs`
fixes that by pre-rendering every equation with KaTeX, removing the runtime
loader and inlining KaTeX's CSS and fonts.

Older talks predate this rule. `2026-08-esa-how-to-be-a-good-editor/` and
`2026-09-uga-law-pandemic-planet/` still ship `_files/` and `figures/`
alongside `index.html`, and `2026-07-esa-spatial-spread/` is a single file
whose equations still load KaTeX from a CDN. Republish them with `publish.sh`
to package them fully.

## Publish with `publish.sh` (preferred)

From a clone of `talks-src`, with this repo cloned at `~/Desktop/Projects/talks`:

```sh
./publish.sh 2026-10-columbia-anticipating-resurgence          # stage for review
./publish.sh 2026-10-columbia-anticipating-resurgence --push   # commit and push
```

It renders the deck self-contained, embeds the math, exports the PDF with
Decktape, replaces the talk's folder here with `index.html` and the PDF, and
adds the deck's `listing.html` to the landing page. See the `talks-src`
README for details.

## Publish a new talk by hand

1. In the source repo, render the deck as one self-contained file:

   ```sh
   quarto render slides.qmd -M embed-resources:true
   ```

   If the deck has equations, also run
   `node embed-math.cjs slides.html <path-to-katex-dir>` (from `talks-src`),
   with the deck set to `html-math-method: {method: katex, url: "katex/"}`
   and a local KaTeX copy in `katex/`.

2. Export the PDF with Decktape at the deck's slide size (Quarto's default is
   1050×700):

   ```sh
   npx decktape reveal -s 1050x700 "file://$PWD/slides.html" venue-topic.pdf
   ```

3. Create the dated folder here and copy in just the two files:

   ```sh
   mkdir -p 2026-06-eds-age-agency-epidemics
   cp slides.html 2026-06-eds-age-agency-epidemics/index.html
   cp venue-topic.pdf 2026-06-eds-age-agency-epidemics/
   ```

4. Check the packaging: copy `index.html` alone into an empty folder, open it
   with networking off, and confirm the figures and equations render.

5. Add an entry at the top of the list in `index.html` (the landing page),
   commit and push:

   ```sh
   git add -A && git commit -m "Add EDS 2026 keynote" && git push
   ```

   The deck goes live at
   `https://jdrakephd.github.io/talks/<slug>/`.

## Notes

- `.nojekyll` is required: it stops GitHub Pages from running Jekyll, which
  would otherwise drop files whose names begin with `_`.
- Speaker notes are embedded in the deck and are public. Keep reminders
  like `[CHECK …]` out of them; `publish.sh` refuses to run if any remain.
- To retire a talk, delete its folder and its line in `index.html`.
