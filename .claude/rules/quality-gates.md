---
paths:
  - "merit_aid-04012026/paper/**/*.tex"
  - "merit_aid-04012026/code/**/*.R"
  - "merit_aid-04012026/code/**/*.py"
  - "merit_aid-04012026/code/**/*.do"
---

# Quality Gates & Scoring Rubrics

## Thresholds

- **80/100 = Commit** -- good enough to save
- **90/100 = PR** -- ready for deployment
- **95/100 = Excellence** -- aspirational

## LaTeX Paper (.tex)

| Severity | Issue | Deduction |
|----------|-------|-----------|
| Critical | Compilation failure (pdflatex + biber) | -100 |
| Critical | Undefined citation | -15 |
| Critical | Typo in equation | -10 |
| Critical | Overfull hbox > 10pt | -10 |
| Major | Missing figure file | -5 |
| Major | Notation inconsistency | -3 |
| Major | Table formatting error | -3 |
| Minor | Orphaned/widow lines | -1 |
| Minor | Long lines in source (>100 chars) | -1 (EXCEPT math formulas) |

## R Scripts (.R)

| Severity | Issue | Deduction |
|----------|-------|-----------|
| Critical | Syntax errors | -100 |
| Critical | Domain-specific bugs | -30 |
| Critical | Hardcoded absolute paths | -20 |
| Major | Missing set.seed() | -10 |
| Major | Missing figure generation | -5 |

## Python Scripts (.py)

| Severity | Issue | Deduction |
|----------|-------|-----------|
| Critical | Import/syntax errors | -100 |
| Critical | Hardcoded absolute paths | -20 |
| Major | Figure not saved to output/ | -10 |
| Major | Missing axis labels/title | -5 |
| Minor | Non-publication font size | -2 |

## Stata Scripts (.do)

| Severity | Issue | Deduction |
|----------|-------|-----------|
| Critical | Syntax errors | -100 |
| Critical | Hardcoded absolute paths | -20 |
| Major | Missing `set seed` | -10 |
| Major | Undocumented sample restriction | -5 |
| Major | Missing clustering specification | -5 |

## Enforcement

- **Score < 80:** Block commit. List blocking issues.
- **Score < 90:** Allow commit, warn. List recommendations.
- User can override with justification.

## Quality Reports

Generated **only at merge time**. Use `templates/quality-report.md` for format.
Save to `quality_reports/merges/YYYY-MM-DD_[branch-name].md`.

## Tolerance Thresholds (Econometric Estimates)

| Quantity | Tolerance | Rationale |
|----------|-----------|-----------|
| Sample sizes (N) | Exact match | Integer, no reason for difference |
| Point estimates | < 0.01 | Display rounding |
| Standard errors | < 0.05 | Clustering/bootstrap variation |
| P-values | Same significance level | Exact p may differ slightly |
| Quintile shares | < 0.1pp | Display rounding |
