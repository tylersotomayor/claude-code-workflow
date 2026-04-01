# Manuscript Review: Merit Aid, Sorting, and Institutional Access

**Date:** 2026-03-31
**Reviewer:** review-paper skill
**File:** merit_aid-04012026/paper/main.tex

## Summary Assessment

**Overall recommendation:** Revise & Resubmit (strong)

This is an ambitious and well-executed paper that challenges the dominant "merit aid is regressive" narrative. The central empirical finding — a perfectly ordered quintile gradient at Tennessee public colleges — is striking and, if robust, an important contribution. The three-destination sorting model provides a clear theoretical lens that reconciles the authors' finding with Dynarski (2000). The paper is written with commendable transparency about limitations (the Rambachan-Roth fragility, the WV non-replication, the ecological inference caveat).


## Strengths

1. **Novel empirical finding.** The perfectly ordered quintile gradient (all five significant, monotonically ordered) is genuinely new to the merit aid literature. Using Chetty et al. (2020) tax-linked data rather than Pell proxies is a substantial improvement over prior work.

2. **Transparent about fragility.** Reporting both ΔRM (survives) and ΔSD (fragile) sensitivity results, and flagging this as a limitation rather than burying it, is exactly the kind of honest reporting top journals value. The permutation inference section is well-executed.

3. **Strong mechanism evidence.** The level decomposition (Q1 count up, Q5 count down, total unchanged) is clean and directly addresses the ecological inference concern. The spillover test and within-TN comparison are well-designed falsification tests.

4. **Clear writing.** The paper follows an exemplary structure: result first, then mechanism, then robustness. The voice is direct and the notation is consistent. The introduction effectively sets up the "reconciliation" framing.

5. **Methodological rigor.** Using Callaway-Sant'Anna as primary with TWFE for comparability, plus LP-DiD, synthetic control, and permutation inference, demonstrates thoroughness without being overwhelming.

## Major Concerns

### MC1: Numerical Inconsistencies Between Text and Tables
- **Dimension:** Presentation
- **Issue:** Several numbers in the results text don't match the tables precisely.
  - Text (Section 6.1): "1.27 percentage points (SE = 0.0026)" — but the ATT in Table A.3 is reported as 0.0127 with SE 0.0026. The text says "1.27 percentage points" which is correct if the coefficient is 0.0127, but then "decreased mean parental income rank by 1.33 points" alongside coefficient -0.0133 — are these percentile points or percentage points? The outcome is a percentile rank (0-1 scale or 0-100 scale?). This ambiguity pervades the paper.
  - The quintile gradient figure caption (Fig 5) reports Q1 as "+1.3 pp" and Q5 as "−1.1 pp" but the table (A.5) shows 0.0154 and −0.0179, which would be 1.54 and −1.79 pp. These appear to be CS estimates in the caption vs. TWFE in the table — but the caption says "Callaway-Sant'Anna ATT" while the table says "TWFE." Which is it?
- **Suggestion:** Audit every number in the text against the source table. Clarify the scale of par_rank (0-1 or 0-100) and be consistent about whether "percentage points" means the coefficient × 100 or the raw coefficient. Create a cross-reference document mapping each in-text claim to its table/figure source.
- **Location:** Sections 6.1, 6.2, and figure/table captions throughout

### MC2: Pre-Trends and the Identification Threat
- **Dimension:** Identification
- **Issue:** The paper acknowledges pre-trend divergence at k = −5 to k = −3 under the national control group but then pivots to a bordering-state specification. The bordering-state specification has only 4 state clusters (including TN), making inference fragile regardless of the estimator. The permutation test p-value of 0.037 for par_rank and 0.074 for par_q1 are suggestive but not overwhelming, especially since the permutation is done under TWFE (not C&S), and the ΔSD sensitivity fails. A referee at a top journal will ask: "How do I know this isn't a pre-existing trend that your preferred specification happens to mask?"
- **Suggestion:** (a) Report the bordering-state pre-trend coefficients explicitly in a table, not just described as "centered around zero." (b) Consider a formal pre-trend test statistic (joint F-test of pre-treatment coefficients). (c) Discuss more precisely what ΔSD fragility means for the bordering specification, not just the national one. (d) Consider dropping the 1980 cohort (which drives the k = −5 issue) as a robustness check and showing results are stable.
- **Location:** Sections 5.2, 7.4

### MC3: Cross-State Section is Structurally Disconnected
- **Dimension:** Argument Structure
- **Issue:** The introduction says Section 8 is "Discussion" covering cross-state evidence. But there appears to be a separate `cross_state.tex` section that is NOT included in `main.tex`. The Discussion section (Section 8) covers GA and WV, but the structure promise in the introduction ("Section 8 discusses cross-state evidence from Georgia and West Virginia") maps to discussion.tex, not to a standalone cross-state section. Meanwhile, `cross_state.tex` and `georgia.tex` exist as separate files. Are these unused drafts or intended to be integrated?
- **Suggestion:** Either (a) integrate the cross-state analysis as its own section between Robustness and Discussion, which would give it more prominence, or (b) confirm that the current Discussion section covers all the cross-state content and remove the orphaned files. The WV analysis in particular deserves more space — the non-replication is actually very informative and currently feels rushed.
- **Location:** main.tex structure, sections/cross_state.tex, sections/georgia.tex

### MC4: Theory-to-Empirics Mapping Is Incomplete
- **Dimension:** Identification / Argument Structure
- **Issue:** The theory section lists 7 predictions (P1-P7), which is a strong organizing device. However, the empirical sections don't systematically map results back to predictions. P6 (spatial heterogeneity) is tested in the results section but P7 (cross-state variation) is tested in the discussion section, creating an asymmetry. P3 and P4 are tested together under "Mechanism Evidence" but the label mapping (e.g., "consistent with Prediction P4") appears only once. The tier heterogeneity test is labeled as testing P4 in the theory but P5 in the text.
- **Suggestion:** Add a summary table or systematic callout that maps each prediction to its test and result (e.g., "P1: Tested in Table A.3, CONFIRMED" etc.). Fix the P4/P5 labeling inconsistency in Section 6.3.2 (the text says "consistent with Prediction P4" but the two-tier channel is P5).
- **Location:** Section 6.3.2 (line referencing P4 should be P5), Section 6

### MC5: Clustering and Inference with 27 States
- **Dimension:** Econometrics
- **Issue:** With 27 state clusters and treatment assigned to a single state, the asymptotic properties of cluster-robust standard errors are questionable. The paper acknowledges this for the bordering specification (4 clusters) but is less explicit about the baseline specification (27 clusters). The few-clusters literature (Cameron, Gelbach, & Miller 2008; MacKinnon & Webb 2017) suggests that even 27 clusters may produce over-rejection when treatment is concentrated in one cluster. The permutation test addresses this, but only for TWFE — there's no permutation analogue for the C&S estimates.
- **Suggestion:** (a) Report wild cluster bootstrap p-values for the baseline 27-cluster specification (not just the bordering specification where it's uninformative). (b) Discuss the effective number of treated clusters (G₁ = 1) more prominently. (c) Consider the randomization inference approach of Conley and Taber (2011) which is designed for exactly this setting.
- **Location:** Section 5.3

## Minor Concerns

### mc1: Placeholder Comment Remains in Data Section
- **Issue:** Line 77-79 of data.tex contains a `% [PLACEHOLDER: Full summary statistics table...]` comment. This should either be filled or removed before submission.
- **Suggestion:** Generate the summary statistics table from 02_descriptive_stats.do output and include it.

### mc2: Placeholder Comment in Results Section
- **Issue:** Lines 229-233 of results.tex contain a `% [PLACEHOLDER: When RA-2 produces...]` comment about Q5-specific tier decomposition figures.
- **Suggestion:** Remove or resolve before submission.

### mc3: "count count" Typo
- **Issue:** Section 6.3.1, line 154: "estimating effects on count count outcomes" — duplicated word.
- **Location:** results.tex line 154

### mc4: Abstract Length and Precision
- **Issue:** The abstract is good but could be tightened. "A three-destination sorting framework reconciles these progressive effects with the regressive attendance findings" — specify Tennessee vs. Georgia more clearly. Also, "p = 0.037 for parental income rank" in the abstract but no mention of the Q1 share p-value (0.074) — this selective reporting may draw referee attention.
- **Suggestion:** Either report both p-values or frame the permutation result more carefully.

### mc5: Table Numbering
- **Issue:** Tables in the appendix use \label names (tab:cs_main, tab:twfe_did) but the reader encounters them as Table A.1, A.2, etc. In-text references like "Table~\ref{tab:cs_main}" correctly resolve, but the current labeling scheme doesn't distinguish main-body tables from appendix tables. Currently, ALL tables and figures are in the appendix — consider promoting the most important ones (C&S ATT, quintile decomposition, robustness summary) to the main body.
- **Suggestion:** Move 2-3 key tables into the main body near their first reference.

### mc6: HOPE Distribution Table Not Referenced in Text
- **Issue:** Table A.1 (HOPE distribution across sectors) is referenced in Section 2.1 but never discussed in the results. Its most useful role would be establishing that ~85% of HOPE recipients are at public colleges — this is mentioned in the text but could be strengthened by referencing the table directly as evidence for the treatment intensity assumption.

### mc7: Inconsistent Estimator Labels
- **Issue:** The quintile gradient figure (Figure 5) says "Callaway-Sant'Anna ATT" but Table A.5 reports "TWFE Static DiD." The in-body quintile table (Section 6.2) also reports TWFE. If the figure uses C&S and the table uses TWFE, the coefficients should differ — and they do (1.3 pp in caption vs. 1.54 pp in table). This needs clarification.
- **Suggestion:** Either use the same estimator in both the figure and table, or clearly label which estimator each reports and discuss the difference.

### mc8: Missing Standard Summary Statistics Table
- **Issue:** The paper has no standard summary statistics table (means, SDs, min, max for all variables by treatment/control and pre/post). This is expected in any empirical micro paper.
- **Suggestion:** Generate from 02_descriptive_stats.do and add as Table 1 in the main body.

## Referee Objections

### RO1: "With N=1 treated state, how is this different from a case study?"
**Why it matters:** The fundamental design has treatment assigned to a single state. All the sophisticated econometrics (C&S, synthetic control, permutation) cannot overcome the fact that any Tennessee-specific shock coinciding with HOPE implementation could generate the results. The 2003-2004 period saw other changes in Tennessee higher education.
**How to address it:** (a) Systematically identify and discuss potential confounders contemporaneous with HOPE implementation in Tennessee (2003-2005). (b) The quintile gradient's perfect monotonicity is hard to explain with confounders — make this argument explicitly. A generic enrollment shock wouldn't produce a perfectly ordered 5-quintile gradient. (c) The spatial heterogeneity (border vs. interior) provides within-state variation that a simple confounder wouldn't predict. Emphasize this as a "signature" of the sorting mechanism.

### RO2: "The ΔSD fragility means you can't reject pre-existing trends"
**Why it matters:** The Rambachan-Roth smoothness sensitivity is the paper's Achilles' heel. A skeptical referee will argue that the growing post-treatment path could be a continuation of differential trends that happen to accelerate smoothly.
**How to address it:** (a) Explain why ΔRM is the more appropriate restriction in this setting — the pre-treatment violations are small and noisy, so bounding by their magnitude (ΔRM) is more natural than bounding by their rate of change (ΔSD). (b) Show that the 1980 cohort is the primary driver of pre-trend curvature and demonstrate results are stable when it's dropped. (c) Argue that the monotonic quintile gradient is overdetermined — even if the level of par_q1 is affected by trends, the across-quintile pattern is not.

### RO3: "You can't observe student destinations — the sorting interpretation is indirect"
**Why it matters:** The paper claims three-destination sorting but uses aggregate institutional data. You never see an individual student switch from public to private or exit the state.
**How to address it:** This limitation is acknowledged but could be sharpened. The level decomposition (Q1 count up, Q5 count down, total flat) and the spillover test (no mirror at TN privates) are the strongest evidence. Make explicit: "We observe the residual of the sorting process. To the extent that Q5 students leave TN publics, do not appear at TN privates, and total enrollment is flat, the only remaining destination is out-of-state." This is an indirect argument but a compelling one. Consider whether IPEDS migration data could provide any direct evidence.

### RO4: "The WV non-replication undermines generalizability"
**Why it matters:** The paper's theory predicts WV should replicate TN, but it doesn't. The explanation (enrollment contraction) is reasonable but post-hoc. A referee may see this as the theory failing its out-of-sample test.
**How to address it:** Reframe: the theory predicts conditional on institutional capacity. WV fails the capacity condition, not the sorting condition. This isn't a failure of the model — it's a scope condition. Make this distinction crisper: "The model predicts progressive sorting when the public system has absorptive capacity. WV's enrollment contraction violates this maintained assumption." Consider adding the capacity condition explicitly to the model's assumptions.

### RO5: "The effect sizes are small — do they matter for mobility?"
**Why it matters:** 1.3 pp increase in bottom-quintile share is a 9.2% gain relative to baseline, but in absolute terms it's modest. The mobility discussion (Section 8.3) is descriptive and doesn't causally identify whether this compositional shift improves outcomes.
**How to address it:** Contextualize the magnitude: (a) Compare to the effect sizes from other policy interventions on college composition (affirmative action bans, tuition changes). (b) Use the Chetty et al. (2020) mobility rate estimates to compute a back-of-envelope welfare calculation: if X additional Q1 students attend colleges with mobility rate Y, the expected number of upwardly mobile students changes by Z. This gives a concrete welfare interpretation even without a causal mobility estimate.

## Specific Comments

- **Introduction, para 2:** "A systematic review of 71 quasi-experimental studies" — cite Herbaut & Geven (2020) here, which you do. But the transition "We document an exception" is punchy and effective.
- **Section 2.1:** "56 per cohort" in parenthetical about multi-campus systems is confusing. Is this 56 observations per cohort? Clarify.
- **Section 3:** The sorting model is clean but could benefit from a brief discussion of the functional form assumption (V = q − C/y + ε). The quasilinear-in-cost specification drives the monotonic gradient result — is this a feature or a knife-edge assumption?
- **Section 6.3.2, line 186:** "consistent with Prediction P4" — should be P5 (tier heterogeneity). P4 is about private colleges showing no mirror image.
- **Section 7.5:** Synthetic control section is strong. The footnote about Pennsylvania's near-zero pre-RMSPE is important — consider promoting to main text.
- **Section 9 (Conclusion):** The policy paragraph is excellent. "The relevant counterfactual is not whether merit aid is more progressive than need-based aid — it almost certainly is not — but whether merit aid at the margin improves or harms the composition of the institutions that serve the most students" is the paper's most important sentence for policy audiences.

## Summary Statistics

| Dimension | Rating (1-5) |
|-----------|-------------|
| Argument Structure | 4 |
| Identification | 3.5 |
| Econometrics | 4 |
| Literature | 4 |
| Writing | 4.5 |
| Presentation | 3.5 |
| **Overall** | **3.9** |
