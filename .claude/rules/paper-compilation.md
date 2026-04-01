---
paths:
  - "merit_aid-04012026/paper/**/*.tex"
---

# Paper Compilation Protocol

**This project uses `biblatex` with `biber` backend (NOT `bibtex`).**

## Compilation Sequence

```bash
cd merit_aid-04012026/paper
pdflatex -interaction=nonstopmode main.tex
biber main
pdflatex -interaction=nonstopmode main.tex
pdflatex -interaction=nonstopmode main.tex
```

## Conference Slides

```bash
cd merit_aid-04012026/paper
pdflatex -interaction=nonstopmode slides.tex
# (biber if slides use citations)
pdflatex -interaction=nonstopmode slides.tex
```

## Figure Path

The `\graphicspath` is set to `../output/figures/` in `main.tex`. All Python-generated PDFs should land in `merit_aid-04012026/output/figures/`.

## Common Issues

- **Undefined citations:** Run `biber main` (not `bibtex`). Check `expectations.bib` for the key.
- **Missing figures:** Verify the PDF exists in `output/figures/` and the filename matches `\includegraphics`.
- **Overfull hbox:** Check table widths and long equations. Use `\resizebox` or `\adjustbox` sparingly.
