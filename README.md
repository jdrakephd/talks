# talks

Public, rendered slide decks for John M. Drake, served via GitHub Pages at
**https://jdrakephd.github.io/talks/**.

Source `.qmd` files live in their respective (private) research repositories.
This repo holds only the *rendered* output, as the research repos are not typically made public and the source is never duplicated here.

## Publish a new talk

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
