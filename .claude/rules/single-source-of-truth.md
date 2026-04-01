---
paths:
  - "merit_aid-04012026/paper/**/*.tex"
  - "merit_aid-04012026/output/**"
  - "merit_aid-04012026/code/**"
---

# Single Source of Truth: Enforcement Protocol

**The code pipeline is the authoritative source for all empirical results.**

## The SSOT Chain

```
Stata (.do files) = SOURCE OF TRUTH for estimates
  ├── output/estimates/*.csv (coefficient exports)
  ├── output/tables/*.tex (esttab table fragments)
  └── output/logs/ (regression logs)

Python (.py files) = SOURCE OF TRUTH for figures
  ├── Reads: output/estimates/*.csv
  └── Writes: output/figures/*.pdf

LaTeX (paper/main.tex) = SOURCE OF TRUTH for narrative
  ├── \input{sections/*} (prose)
  ├── \includegraphics from output/figures/
  ├── \input from output/tables/
  └── expectations.bib (citations)

NEVER manually edit output/ artifacts.
ALWAYS regenerate from code when estimates change.
```

---

## Consistency Checks

Before any commit that includes paper changes:

1. **Numbers in text match tables:** Every estimate cited in prose must match the corresponding table cell
2. **Figures match estimates:** Plotted coefficients must come from the CSV exports, not hardcoded
3. **Table fragments are current:** Re-run Stata if table .tex files are stale
4. **Figure captions match content:** Description matches what the figure actually shows

---

## When to Re-Run Code

Re-run the affected pipeline step when:
- A .do file has been modified → re-run Stata, then Python figures, then recompile LaTeX
- A .py figure script has been modified → re-run Python, then recompile LaTeX
- Sample restrictions change → re-run full pipeline from Stata
- A new robustness check is added → new .do file + new .py figure + update LaTeX

---

## Content Fidelity Checklist

```
[ ] All estimates in text match table values
[ ] All figures generated from current CSV exports
[ ] No manually hardcoded numbers in LaTeX (derive from tables)
[ ] Citation keys in text resolve in expectations.bib
[ ] Figure numbering matches \label/\ref
[ ] Table numbering matches \label/\ref
```
