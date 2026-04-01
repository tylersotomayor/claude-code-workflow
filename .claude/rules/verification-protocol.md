---
paths:
  - "merit_aid-04012026/paper/**/*.tex"
  - "merit_aid-04012026/code/**"
  - "merit_aid-04012026/output/**"
---

# Task Completion Verification Protocol

**At the end of EVERY task, Claude MUST verify the output works correctly.** This is non-negotiable.

## For LaTeX Paper:
1. Compile with pdflatex + biber (3-pass) and check for errors
2. Open the PDF to verify (`open` on macOS)
3. Check for overfull hbox warnings
4. Verify citations resolve (no `[?]` in output)
5. Verify figures render (no missing file errors)

## For Stata Scripts (.do):
1. Run `stata-mp -b do code/stata/filename.do` from project root
2. Check the .log file for errors and warnings
3. Verify output files (CSV estimates, .tex tables) were created with non-zero size
4. Spot-check estimates for reasonable magnitude

## For R Scripts (.R):
1. Run `Rscript code/R/filename.R` from project root
2. Verify output files (PDF, RDS) were created with non-zero size
3. Spot-check estimates for reasonable magnitude

## For Python Figure Scripts (.py):
1. Run `python3 code/python/fig_name.py` from project root
2. Verify PDF figure files were created in `output/figures/`
3. Open figure to verify visual quality (`open` on macOS)
4. Check axis labels, legend, title are present and correct

## Common Pitfalls:
- **biber vs bibtex**: This project uses biber — never run bibtex
- **Relative paths**: All code should use paths relative to project root
- **Assuming success**: Always verify output files exist AND contain correct content
- **Stale figures**: If estimates changed, regenerate figures before recompiling paper

## Verification Checklist:
```
[ ] Output file created successfully
[ ] No compilation/execution errors
[ ] Figures display correctly
[ ] Estimates are reasonable in magnitude
[ ] Reported results to user
```
