# Plan: Four Estimators Everywhere + R&R Audit + Instrumentation

**Status:** DRAFT
**Date:** 2026-03-31 (session 2)
**Goal:** Run all 4 heterogeneity-robust estimators on every event-study specification, regenerate all figures, audit R&R, explore novel instrumentation.

---

## Context

Audit shows only TN public main effects have all 4 estimators. Everything else has 1-2. The working template is `block8_02_five_estimators.do` which correctly extracts from `e(b_iw)`/`e(V_iw)` for Sun-Abraham and `e(effect_h)`/`e(placebo_h)` for dCDH. Need to replicate this pattern across 8 specifications.

---

## Phase 1: Build Universal Four-Estimator .do File

Write ONE master .do file that takes a sample and outcome as parameters and runs all 4 estimators, exporting a standardized CSV. This avoids duplicating 200+ lines of estimation code 8 times.

**File:** `code/stata/block11_four_estimators_universal.do`

**Structure:**
```
Input: sample dataset (already filtered), outcome variable name, output filename prefix
For each of {TWFE, C&S, Sun-Abraham, dCDH}:
  - Estimate event study
  - Extract coefficients into postfile
  - Handle errors gracefully
Export: {prefix}_four_estimators.csv
```

**Key fixes from failed private-college attempt:**
- Sun-Abraham: use `e(b_iw)` / `e(V_iw)`, NOT `_b[varname]`
- dCDH: use `e(effect_h)` / `e(placebo_h)` with null checks
- Handle `capture drop` for variables that may already exist
- k=-2 as omitted period for TWFE (drop evt_m2 dummy instead of evt_m1)

## Phase 2: Run on All Specifications

Run the universal estimator on each sample, in this order (dependencies first):

| # | Specification | Sample Filter | Outcomes | Output Prefix |
|---|---|---|---|---|
| 1 | TN Private Spillover | type==2, TN+control | par_q1, par_rank | private |
| 2 | Level Decomposition | type==1, TN+control | q1_count, q5_count, total_count | level |
| 3 | Quintiles Q2-Q4 | type==1, TN+control | par_q2, par_q3, par_q4 | quintile |
| 4 | WV PROMISE | type==1, WV+control | par_q1, par_rank | wv |
| 5 | SD Opportunity | type==1, SD+control | par_q1, par_rank | sd |
| 6 | MA Adams | type==1, MA+control | par_q1, par_rank | ma |
| 7 | Within-TN | TN only, pub+priv | par_q1, par_rank | within_tn |

**Estimated runtime:** ~15-30 min per specification (dCDH is slow with bootstrap). Total ~2-3 hours. Can run specs 4-6 in parallel.

**Normalization:** TWFE/SA use k=-2 as omitted period. C&S/dCDH plotted raw.

## Phase 3: Verify Pre-Treatment CIs

After all estimates are generated, run a verification script:
- For each specification × estimator × outcome: check that ALL pre-treatment CIs contain zero
- Flag any violations
- Export a summary table

## Phase 4: Regenerate All Figures

Using the house-style Stata template (LM Roman Caps, navy palette, bar CIs, separate y-axes):

| Figure | Data Source | Style |
|---|---|---|
| Main C&S event study (combined) | Already done | Keep |
| Four-estimator overlay (combined) | Already done | Keep |
| Public vs Private 2x2 | NEW private 4-est data | Redo both panels with 4 est |
| Level decomposition 3-panel | NEW level 4-est data | Redo with 4 est overlay |
| Quintile 5-panel | NEW quintile 4-est data | Redo with 4 est per panel |
| Four-state comparison 2x2 | NEW state 4-est data | Redo each state with 4 est |
| Appendix figures A1-A7 | Various NEW data | Redo all with 4 est |

## Phase 5: Rambachan-Roth Audit

Read `code/R/05_rambachan_roth.R` carefully and check:
1. Is the correct base period specified? (Should be k=-2 given bridge provision)
2. Are the correct event-study coefficients fed in? (C&S group-time ATTs)
3. Is it using the bordering-state control or national?
4. Is the smoothness restriction parameterized correctly?
5. Re-run with corrected inputs and see if ΔSD fragility persists

## Phase 6: Novel Instrumentation Exploration

Systematic review of Chetty et al. (2020) data for IV opportunities:
1. Read the codebook/data documentation
2. Cross-tabulate available variables against treatment variation
3. Candidates: lottery revenue shocks, enrollment capacity constraints, neighboring-state spillovers, institutional selectivity interactions
4. Assess first-stage strength and exclusion restriction plausibility
5. Write up findings as a research memo

## Phase 7: Cleanup

- Drop Pennsylvania from synth placebo, re-run, update figure
- Fix synth p-value discrepancy
- Delete old expectations.bib
- Add coefficient interpretation throughout
- Proofread
- Recompile paper

---

## Execution Order

1. **Phase 1** (universal .do file) — must be done first, everything depends on it
2. **Phase 2** (run all specs) — sequential, ~2-3 hours Stata runtime
3. **Phase 3** (verify pre-treatment CIs) — quick script after Phase 2
4. **Phase 4** (regenerate figures) — depends on Phase 2 output
5. **Phase 5** (R&R audit) — independent of Phases 2-4, can interleave
6. **Phase 6** (instrumentation) — independent, can do while Stata runs
7. **Phase 7** (cleanup) — after everything else

---

## Verification

1. Every specification has a CSV with 4 estimators × 2 outcomes × 12 event times
2. All pre-treatment CIs contain zero (programmatic check)
3. All figures regenerated and compiled into paper
4. Paper compiles with 0 errors
5. R&R audit documented with findings
