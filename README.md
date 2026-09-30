# talks

Public, rendered slide decks for John M. Drake, served via GitHub Pages at
**https://jdrakephd.github.io/talks/**.

Source `.qmd` files live in the private
[`talks-src`](https://github.com/jdrakephd/talks-src) repo, or in a research
repo when a talk belongs to one project (the UGA Law decks are in
`pandemic-planet/talks/`). This repo holds only the *rendered* output; the
source is never duplicated here.

## Publish with `publish.sh` (preferred)

From a clone of `talks-src`, with this repo cloned at `~/Desktop/Projects/talks`:

```sh
./publish.sh 2026-10-columbia-anticipating-resurgence          # stage for review
./publish.sh 2026-10-columbia-anticipating-resurgence --push   # commit and push
```

It renders the deck, exports the PDF with Decktape, copies the rendered output
into the dated folder here as `index.html`, and adds the deck's `listing.html`
to the landing page. See the `talks-src` README for details.

## Publish a new talk by hand

1. In the private research repo, render the Quarto reveal deck as a
   self-contained folder:

   ```sh
   quarto render slides.qmd --to revealjs
   ```

   (If the deck references local images/assets, keep the generated
   `slides_files/` folder alongside `slides.html`.)

2. Copy the rendered output into a dated, slugged folder in this repo, named so
   the URL reads well:

   ```sh
   mkdir -p 2026-06-eds-age-agency-epidemics
   cp -R /path/to/render/output/* 2026-06-eds-age-agency-epidemics/
   # rename the deck's entry file to index.html so the folder URL loads it
   mv 2026-06-eds-age-agency-epidemics/slides.html \
      2026-06-eds-age-agency-epidemics/index.html
   ```

3. Add a line to `index.html` (the landing page), commit, and push:

   ```sh
   git add -A && git commit -m "Add EDS 2026 keynote" && git push
   ```

   The deck goes live at
   `https://jdrakephd.github.io/talks/<slug>/`.

## Notes

- `.nojekyll` is required: it stops GitHub Pages from running Jekyll, which
  would otherwise drop folders and files whose names begin with `_` (Quarto and
  reveal.js use several).
- Prefer folder-per-talk over a single self-contained HTML when a deck has many
  images; it keeps the repo readable and avoids multi-megabyte HTML files.
- To retire a talk, delete its folder and its line in `index.html`.
