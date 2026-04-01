---
paths:
  - "merit_aid-04012026/paper/**/*.tex"
  - "merit_aid-04012026/code/**"
---

# Project Knowledge Base: Merit Aid, Sorting, and Institutional Access

## Notation Registry

| Rule | Convention | Example | Anti-Pattern |
|------|-----------|---------|-------------|
| Outcomes | lowercase with subscript | y_ct, par_q1 | Y, Q1 |
| Fixed effects | Greek lowercase | alpha_c, lambda_t | FE_c, fe_t |
| Treatment indicator | Treated_c | Treated_c = 1 for TN public | D, treat |
| First treated cohort | t* | t* = 1986 | t0, T |
| Theory parameters | Greek | tau (threshold), S (scholarship) | threshold, amount |

## Symbol Reference

| Symbol | Meaning | Introduced |
|--------|---------|------------|
| y_ct | Outcome for college c, cohort t | Empirical Strategy |
| par_q1 | Bottom-quintile parental income share | Data |
| par_rank | Mean parental income rank | Data |
| alpha_c | College fixed effects | Empirical Strategy |
| lambda_t | Cohort fixed effects | Empirical Strategy |
| Treated_c | TN public college indicator | Empirical Strategy |
| tau | Eligibility threshold (theory) | Theory |
| S | Scholarship amount (theory) | Theory |

## Empirical Applications

| Application | Estimator | Dataset | Section | Purpose |
|------------|-----------|---------|---------|---------|
| TN HOPE main effects | Callaway & Sant'Anna | Chetty et al. (2020) | Results | Primary identification |
| TWFE comparison | Two-way FE | Chetty et al. (2020) | Results | Comparability |
| Synthetic control | Abadie et al. | Chetty et al. (2020) | Robustness | Alternative identification |
| Permutation inference | Fisher randomization | Chetty et al. (2020) | Robustness | p = 0.037 for par_rank |
| Rambachan-Roth | Sensitivity analysis | Chetty et al. (2020) | Robustness | Parallel trends sensitivity |
| WV/SD/MA extension | CS DiD | Chetty et al. (2020) | Cross-State | External validity |

## Anti-Patterns (Don't Do This)

| Anti-Pattern | What Happened | Correction |
|-------------|---------------|-----------|
| Assume t* = 1985 | 1985 cohort contaminated by bridge provision | Use t* = 1986, verified via TSAC/statute |
| Use xtreg for event studies | Wrong estimator | Use reghdfe (Stata) |
| Run bibtex instead of biber | Compilation fails | This project uses biblatex/biber |
| Hardcode state FIPS codes | Fragile | Use named state identifiers |

## Key Data Facts

- Chetty et al. (2020): 2,202 colleges, 52 states, cohorts 1980-1991
- TN: 51 colleges (26 public), 291/312 public college-cohort obs non-missing
- Control group: all states without broad-based merit programs per Sjoquist & Winters (2014)
- Must drop all 25 merit-adopting states from controls
