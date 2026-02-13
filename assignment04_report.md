# Assignment 04 Interpretation Memo

**Student Name:** Daemian Cowan
**Date:** 4/13/2026
**Assignment:** REIT Annual Returns and Predictors (Simple Linear Regression)

---

## 1. Regression Overview

You estimated **three** simple OLS regressions of REIT *annual* returns on different predictors:

| Model | Y Variable | X Variable | Interpretation Focus |
|-------|------------|------------|----------------------|
| 1 | ret (annual) | div12m_me | Dividend yield |
| 2 | ret (annual) | prime_rate | Interest rate sensitivity |
| 3 | ret (annual) | ffo_at_reit | FFO to assets (fundamental performance) |

For each model, summarize the key results in the sections below.

---

## 2. Coefficient Comparison (All Three Regressions)

**Model 1: ret ~ div12m_me**
- Intercept (β₀): 0.1082 (SE: 0.006, p-value: 0.000)
- Slope (β₁):  -0.0687 (SE: 0.032, p-value: 0.035)
- R²: 0.002 | N: 2527

**Model 2: ret ~ prime_rate**
- Intercept (β₀): 0.1998 (SE: 0.016, p-value: 0.000)
- Slope (β₁): -0.0194 (SE: 0.003, p-value: 0.000)
- R²: 0.016 | N: 2527

**Model 3: ret ~ ffo_at_reit**
- Intercept (β₀): 0.0973 (SE: 0.009, p-value: 0.000)
- Slope (β₁): 0.5770 (SE: 0.567, p-value: 0.309)
- R²: 0.000 | N: 2518

*Note: Model 3 may have fewer observations if ffo_at_reit has missing values; statsmodels drops those rows.*

---

## 3. Slope Interpretation (Economic Units)

**Dividend Yield (div12m_me):**
- A 1 percentage point increase in dividend yield (12-month dividends / market equity) is associated with a [-0.0687] change in annual return.
- [Your interpretation: A higher dividend yield is related to an ever so slightly less annual return. As a company has to provide more a higher yield, it strains the overall return.]

**Prime Loan Rate (prime_rate):**
- A 1 percentage point increase in the year-end prime rate is associated with a [-0.0194] change in annual return.
- [Your interpretation: The evidence suggests REIT returns are sensitive to an increase in interest rates.]

**FFO to Assets (ffo_at_reit):**
- A 1 unit increase in FFO/Assets (fundamental performance) is associated with a [0.5770] change in annual return.
- [Your interpretation: More profitable REITs earn higher annual returns.]

---

## 4. Statistical Significance

For each slope, at the 5% significance level:
- **div12m_me:** [Significant] — [The results of the regression suggest that is is an effective measure for the relationship with annual returns.]
- **prime_rate:** [Significant] — [The results of the regression suggest that is is an effective measure for the relationship with annual returns.]
- **ffo_at_reit:** [Not significant] — [The results of the regression suggest that it is not an effective measure for the relationship with annual return.]

**Which predictor has the strongest statistical evidence of a relationship with annual returns?** [Prime Rate is has the strongest statistical evidence, with the lowest p score.]

---

## 5. Model Fit (R-squared)

Compare R² across the three models:
- [Your interpretation: Prime Rate has the highest R squared, consistent with it being the best fit. The other two measures have rather low r squares. This suggests that Prime interest rate is the largest factor in determining the annual return on an REIT.]

---

## 6. Omitted Variables

By using only one predictor at a time, we might be omitting:
- [combined values]: [If a p score is extremely low, but the r squared is also low, the regression might not be significant at all.]
- [F test]: [without comparing the models to each other using an F test, there might be ]
- [Variable 3]: [Why it might matter]

**Potential bias:** If omitted variables are correlated with both the X variable and ret, our slope estimates may be biased. [Brief discussion of direction if possible]

---

## 7. Summary and Next Steps

**Key Takeaway:**
[2-3 sentences summarizing which predictor(s) show the strongest relationship with REIT annual returns and whether the evidence is consistent with economic theory] The predictor that shows the strongest relationship with REIT annual returns is the prime loan rate. This follows the economic theory that the interest rate is one of the major factors in determining financial outcomes.

**What we would do next:**
- Extend to multiple regression (include two or more predictors)
- Test for heteroskedasticity and other OLS assumption violations
- Examine whether relationships vary by time period or REIT sector

---

## Reproducibility Checklist
- [Y] Script runs end-to-end without errors
- [Y] Regression output saved to `Results/regression_div12m_me.txt`, `regression_prime_rate.txt`, `regression_ffo_at_reit.txt`
- [Y] Scatter plots saved to `Results/scatter_div12m_me.png`, `scatter_prime_rate.png`, `scatter_ffo_at_reit.png`
- [Y] Report accurately reflects regression results
- [Y] All interpretations are in economic units (not just statistical jargon)
