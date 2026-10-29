---
name: promotion-impact-analysis
description: "Analyze and test the effect of promotions, discounts, email campaigns, and homepage featuring on food demand using the project's meal-demand data. Use for promotion impact, causal inference, uplift, or promotional effectiveness questions."
---

# Promotion Impact Analysis

Evaluate whether promotional activity is associated with a change in meal orders, using the food-demand project data and defensible statistical methods.

## Project data

Load order-level observations from `data/raw/train.csv`. The target is `num_orders`; promotion indicators are `emailer_for_promotion` and `homepage_featured`. Join `meal_info.csv` on `meal_id` and `fulfilment_center_info.csv` on `center_id` when relevant. `test.csv` has no outcome and must not be used to estimate promotion effects.

## Workflow

1. **Define the question and treatment.** State which promotion is being evaluated, the outcome (`num_orders`), the unit of analysis, and the estimand. Analyze email and homepage featuring separately unless a combined treatment is explicitly justified.
2. **Check data quality and overlap.** Inspect missingness, duplicates, treatment counts, outcome distributions, and whether treated and untreated observations exist in comparable weeks, meals, and centers. Treat the flags as indicators, not continuous quantities.
3. **Describe before testing.** Compare order counts and distributions by promotion status. Report sample sizes, means and medians, differences, and visualizations; distinguish raw associations from adjusted estimates.
4. **Account for confounding.** Promotion assignment may depend on meal, center, week, price, and other campaign decisions. Use a defensible design: adjust for relevant pre-treatment covariates (such as meal, center, week, and price where appropriate), stratify or match only when common support is adequate, or use a quasi-experimental design only when its assumptions can be defended. Avoid controlling for variables caused by the promotion.
5. **Test and quantify uncertainty.** Choose a test suited to the question and data. Mann–Whitney U can compare independent distributions, but does not test a difference in means. For adjusted analyses, report the model specification, effect size, confidence interval, and an appropriate uncertainty estimate. Account for repeated observations or clustering at the meal-center or campaign assignment level when applicable. Correct for multiple comparisons if testing many outcomes or subgroups.
6. **Check robustness.** Examine overlap, influential observations, alternate reasonable specifications, and subgroup consistency. Do not claim causal impact unless the design and assumptions support a causal interpretation.
7. **Report actionable findings.** State the estimated effect in orders (and percentage only with a clear denominator), uncertainty, sample size, assumptions, limitations, and a cautious business recommendation. Separate statistical significance from practical significance.

## Guardrails

- Do not interpret a difference in promoted versus unpromoted orders as causal by itself; promotions may be targeted to expected high-demand meals or weeks.
- Do not infer the effect of a price discount solely from `emailer_for_promotion` or `homepage_featured`. Analyze prices separately and state the limitations of observational comparisons.
- Do not leak `num_orders` into features or use `test.csv` for evaluation of promotion effects.
- Do not fabricate results, thresholds, test output, or causal assumptions. Make the analysis reproducible and cite the exact data fields and filtering choices.
- Follow the project guidance in `AGENTS.md` and preserve raw source files.
