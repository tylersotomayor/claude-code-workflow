# Plan: IV Second-Stage Regressions, Figures, and Narrative

**Status:** DRAFT
**Date:** 2026-03-31

## Context

We have a strong CZ-level Bartik instrument (F=119) that predicts college composition (par_q1) using local income trends. The paper currently treats the mobility question descriptively: "upward mobility didn't decline" (Section 8.3, explicitly disclaimed as not causal). With this instrument, we can upgrade to causal: **does increasing bottom-quintile representation at a college improve or harm upward mobility?**

This transforms the paper's welfare conclusion from "descriptive reassurance" to "causal evidence."

## The Narrative Arc

**Current paper story (5 beats):**
1. HOPE changed who attends public colleges (composition effect) — **causal, proven**
2. The effect is progressive: more low-income students, fewer high-income — **causal, proven**
3. The mechanism is the price/eligibility channel (gender test) — **causal, proven**
4. Mobility rates didn't decline — **descriptive only, explicitly disclaimed**
5. Cross-state variation confirms the model's predictions — **causal, proven**

**New story (6 beats, the upgrade):**
1–3: Same
4. **NEW:** Composition changes driven by local demographic shifts predict [no decline / improvement] in upward mobility — **causal via IV**
5. This means expanding access didn't come at the cost of returns — **welfare conclusion, now causal**
6. Cross-state evidence confirms — same

**Where it goes in the paper:** Replace or substantially expand the "Mobility Implications" subsection (discussion.tex, currently lines 50-89). The descriptive figure stays as motivation; the IV results are the payoff.

## Second-Stage Outcomes (4 regressions)

All use CZ-level Bartik as instrument for par_q1, with college + cohort FE, state-clustered SEs.

| Outcome | Variable | What It Measures |
|---------|----------|-----------------|
| Upward mobility | `mr_kq5_pq1` | P(child Q5 \| parent Q1) — the headline |
| Child earnings rank | `k_rank` | Mean child income percentile |
| Q1-conditional earnings | `k_rank_cond_parq1` | Earnings rank for Q1 students specifically |
| Zero earnings | `k_0inc` | Fraction with zero labor income |

The critical outcome is `k_rank_cond_parq1`: if this doesn't decline when par_q1 rises, it means the marginal Q1 student isn't pulling down returns for inframarginal Q1 students. This is the "no crowding out" result.

Note: `k_rank_cond_parq1` is in raw Table 3 but NOT in the current pooled dataset. Must merge from Table 3.

## Files to Create

| File | Purpose |
|------|---------|
| `code/stata/46_iv_second_stage.do` | Merge conditional outcomes, run OLS + first stage + reduced form + 2SLS for all 4 outcomes |
| `code/python/fig_iv_second_stage.py` | 4 figures: decomposition, first-stage scatter, 2SLS coefficients, reduced-form event study |

## File to Modify

| File | Change |
|------|--------|
| `paper/sections/discussion.tex` | Expand "Mobility Implications" with IV results |

## Script Design: `46_iv_second_stage.do`

### Data Prep
```stata
use "data/clean/pooled_iv_analysis.dta", clear

// Merge conditional outcomes from Table 3
preserve
    use "data/raw/chetty/mrc_table3.dta", clear
    keep super_opeid cohort k_rank_cond_parq1 k_rank_cond_parq2 ///
         k_rank_cond_parq5 k_married_cond_parq1
    tempfile t3_cond
    save `t3_cond'
restore
merge 1:1 super_opeid cohort using `t3_cond', keep(master match) nogen
```

### Regressions (for each outcome Y)
```
1. OLS:          Y = β*D + college FE + cohort FE + ε
2. First stage:  par_q1 = γ*bartik_cz_q1 + college FE + cohort FE + u
3. Reduced form: Y = π*bartik_cz_q1 + college FE + cohort FE + v
4. 2SLS:         Y = β_iv * par_q1_hat + college FE + cohort FE + ε
```

### Decomposition Data
For Figure A (actual vs predicted composition):
```stata
// Compute cohort means for TN public
collapse (mean) par_q1 bartik_cz_q1 bartik_predicted=bartik_cz_full ///
    if state == "TN", by(cohort)
export delimited "output/estimates/iv_decomposition_tn.csv", replace
```

Wait — the Bartik predicted par_q1 isn't quite right. The Bartik variable is `pre_q1 × cz_trend_q1`, not a predicted par_q1 level. For the decomposition figure, we need the predicted *level*, not the product. Better approach: compute the fitted values from the first-stage regression.

```stata
// After first stage regression
reghdfe par_q1 bartik_cz_q1, absorb(super_opeid cohort) vce(cluster state_id)
predict par_q1_hat, xbd  // fitted values including FEs
```

Then collapse `par_q1` and `par_q1_hat` by cohort for TN colleges.

### Exports
- `output/estimates/iv_second_stage_results.csv` — all 4 outcomes: OLS, RF, 2SLS coefficients
- `output/estimates/iv_decomposition_tn.csv` — cohort-level actual vs predicted for TN
- `output/estimates/iv_firststage_scatter.csv` — residualized bartik vs par_q1 for scatter
- `paper/tables/tab_iv_second_stage.tex` — LaTeX table

## Figure Design: `fig_iv_second_stage.py`

### Figure A: Actual vs Bartik-Predicted Composition
- X-axis: birth cohort (1980–1991)
- Two lines: actual par_q1 (navy, solid) and Bartik-predicted (cranberry, dashed)
- Shaded area between = HOPE's contribution
- Vertical treatment line at t*=1986
- Caption: "The wedge between actual and predicted composition is the policy-attributable shift"

### Figure B: First-Stage Scatter (Binned)
- Residualized par_q1 (Y) vs residualized bartik_cz_q1 (X)
- Bin into ~20 equal-sized bins for visual clarity
- Fitted line + R² and F-stat annotation
- Shows the instrument's predictive power at the observation level

### Figure C: OLS vs 2SLS Coefficient Comparison
- 4 panels (or grouped bars): one per outcome
- Each shows OLS bar and 2SLS bar with 95% CIs
- Key comparison: does 2SLS differ from OLS? Direction of selection bias?

### Figure D: Reduced-Form Event Study (if time permits)
- Reduced-form coefficient of bartik_cz_q1 on mr_kq5_pq1 by cohort
- Would need to run interaction: bartik × cohort dummies
- Shows whether the predictive relationship changes over time

## Exclusion Restriction Discussion

The exclusion restriction requires that CZ income trends affect college mobility ONLY through changing who attends (composition), not through:
- Local labor market conditions for graduates
- College funding via tax base
- Neighborhood effects on student outcomes

**Defense:** College + cohort FEs absorb level differences across CZs and time trends. The instrument is the *interaction* of pre-period college exposure with local trends — this asks whether colleges in CZs with faster Q1 growth see larger mobility changes, conditional on the college's own fixed level and aggregate time effects. For a CZ labor market shock to violate exclusion, it would need to differentially affect mobility at colleges that happened to have high pre-period Q1 shares, beyond what college FE and cohort FE absorb.

**Honest caveat for the paper:** "We cannot fully rule out that CZ income trends directly affect college quality or local labor markets in ways correlated with the instrument. The estimates should be interpreted as the combined effect of compositional change and any correlated quality adjustments."

## Paper Integration

Expand "Mobility Implications" subsection in discussion.tex:
1. Keep the existing descriptive figure as motivation
2. Add: "We formalize this descriptive evidence using an IV strategy..."
3. Present the 2SLS estimates for all 4 outcomes
4. The key result sentence: "A one-percentage-point increase in bottom-quintile representation, instrumented by CZ-level income trends, [increases/does not change/decreases] upward mobility by X percentage points"
5. Reference the decomposition figure
6. Honest exclusion restriction discussion
7. Conclude: the progressive composition shift did not reduce returns — now a causal claim

## Verification
1. All Stata regressions run; CSVs created with non-zero rows
2. First-stage F matches prior result (~119)
3. Sign check: OLS and 2SLS should agree on direction for mr_kq5_pq1
4. Python figures render; PDFs are non-zero
5. Paper compiles with new figures and expanded text
