# CV source

- `cv.tex` — **the CV**. Charter body, mixed-case bold headings, fixed right-hand date column. `static/cv.pdf` is built from this file.
- `old/` — superseded files kept for documentation only, not built or published:
  - `cv-original-format.tex` — the original margin-note template with only the CMU affiliation update applied.
  - `cv-original-format-revised-content.tex` — the revised content (Summary, links, DOIs, talks, software, condensed teaching) in the original template.
  - `cv-review-diff.html` — side-by-side review of those two.

Build and install:

```bash
cd cv
latexmk -pdf cv.tex
cp cv.pdf ../static/cv.pdf
latexmk -c cv.tex   # remove aux files
```

Hugo ignores this directory; only `static/cv.pdf` is published.
