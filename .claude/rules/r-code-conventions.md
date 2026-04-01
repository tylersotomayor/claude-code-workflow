---
paths:
  - "merit_aid-04012026/code/R/**/*.R"
---

# R Code Standards

**Standard:** Senior Principal Data Engineer + PhD researcher quality

---

## 1. Reproducibility

- `set.seed()` called ONCE at top (YYYYMMDD format)
- All packages loaded at top via `library()` (not `require()`)
- All paths relative to repository root
- `dir.create(..., recursive = TRUE)` for output directories

## 2. Function Design

- `snake_case` naming, verb-noun pattern
- Roxygen-style documentation
- Default parameters, no magic numbers
- Named return values (lists or tibbles)

## 3. Domain Correctness

- Verify estimator implementations match paper specifications
- Wild cluster bootstrap: check number of clusters, seed, and reps match paper
- Rambachan-Roth: verify delta parameterization matches claimed sensitivity

## 4. Visual Identity

```r
# --- Columbia palette ---
columbia_blue  <- "#012169"
columbia_light <- "#75AADB"
accent_gray    <- "#525252"
positive_green <- "#15803d"
negative_red   <- "#b91c1c"
```

### Custom Theme
```r
theme_custom <- function(base_size = 12) {
  theme_minimal(base_size = base_size) +
    theme(
      plot.title = element_text(face = "bold", color = columbia_blue),
      legend.position = "bottom"
    )
}
```

### Figure Dimensions for Article
```r
ggsave(filepath, width = 6.5, height = 4.5, dpi = 300)
```

## 5. RDS Data Pattern

**Heavy computations saved as RDS; downstream scripts load pre-computed data.**

```r
saveRDS(result, file.path(out_dir, "descriptive_name.rds"))
```

## 6. Common Pitfalls

| Pitfall | Impact | Prevention |
|---------|--------|------------|
| Hardcoded paths | Breaks on other machines | Use relative paths from project root |
| Missing `set.seed()` | Non-reproducible bootstrap | Always set at top of script |
| Wrong cluster variable | Invalid inference | Cluster at state level per paper spec |

## 7. Line Length & Mathematical Exceptions

**Standard:** Keep lines <= 100 characters.

**Exception: Mathematical Formulas** -- lines may exceed 100 chars **if and only if:**

1. Breaking the line would harm readability of the math
2. An inline comment explains the mathematical operation
3. The line is in a numerically intensive section

## 8. Code Quality Checklist

```
[ ] Packages at top via library()
[ ] set.seed() once at top
[ ] All paths relative
[ ] Functions documented (Roxygen)
[ ] Figures: explicit dimensions, 300 dpi
[ ] RDS: every computed object saved
[ ] Comments explain WHY not WHAT
```
