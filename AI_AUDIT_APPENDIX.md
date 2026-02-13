# AI Audit Appendix (Assignment 04)

## Tool(s) Used
- Github CoPilot, Auto

## Task(s) Where AI Was Used
- It was used to str(model.summary()) to the output file create fig, ax with plt.subplots(figsize =(10,6)), filter rows with valid x_var and ret, scatter plot, overlay regression line (use model.params['Intercept'] and model.params[x_var]), set axis limits to zoom on central data, add a title(includeR^2), xlabel, ylabel="Annual Return", legend, and save with plt.savefig(output_path, dpi=300, bbox_inches='tight'), print intercept (β₀) , slope (β₁), standard errors, t-stats, p-values, then print R squared, adjusted Rsquared, and N, then print wherher slope is positive/negative and significant at 5%

## Prompt(s)
- Pleas use statsmodels.fomula.ai.ols to estimate ret ~ x_var

- Now could you write str(model.summary()) to the output file?

- Now could you create fig, ax with plt.subplots(figsize =(10,6)), filter rows with valid x_var and ret, scatter plot, overlay regression line (use model.params['Intercept'] and model.params[x_var]), set axis limits to zoom on central data, add a title(includeR^2), xlabel, ylabel="Annual Return", legend, and save with plt.savefig(output_path, dpi=300, bbox_inches='tight')

- Now could you print intercept (β₀) , slope (β₁), standard errors, t-stats, p-values, then print R squared, adjusted Rsquared, and N, then print wherher slope is positive/negative and significant at 5%

## Output Summary
-  # Use statsmodels.formula.api.ols to estimate ret ~ x_var
    # Drop rows with missing values in the outcome or predictor
    data = df.dropna(subset=["ret", x_var]).copy()
    model = ols(f"ret ~ {x_var}", data=data).fit()
    return model

f.write(str(model.summary()) + "\n")


    # Prepare figure
    fig, ax = plt.subplots(figsize=(10, 6))

    # Filter to rows with valid x_var and ret
    plot_df = df.dropna(subset=["ret", x_var])
    x = plot_df[x_var].values
    y = plot_df["ret"].values

    # Scatter
    ax.scatter(x, y, alpha=0.4, label="Observations")

    # Regression line using model params
    intercept = float(model.params.get("Intercept", 0.0))
    slope = float(model.params.get(x_var, 0.0))
    x_min, x_max = np.nanpercentile(x, [2, 98])
    x_line = np.linspace(x_min, x_max, 100)
    y_line = intercept + slope * x_line
    ax.plot(x_line, y_line, color="red", lw=2, label="Fitted line")

    # Axis limits: zoom to central 2–98 percentiles for x and y (with small padding)
    y_min, y_max = np.nanpercentile(y, [2, 98])
    x_pad = (x_max - x_min) * 0.05 if (x_max - x_min) != 0 else 0.1
    y_pad = (y_max - y_min) * 0.05 if (y_max - y_min) != 0 else 0.1
    ax.set_xlim(x_min - x_pad, x_max + x_pad)
    ax.set_ylim(y_min - y_pad, y_max + y_pad)

    # Labels, title, legend
    r2 = getattr(model, "rsquared", None)
    title_full = f"{title}"
    if r2 is not None:
        title_full = f"{title} (R²={r2:.3f})"
    ax.set_title(title_full)
    ax.set_xlabel(xlabel)
    ax.set_ylabel("Annual Return")
    ax.legend()

    # Save and close
    plt.savefig(output_path, dpi=300, bbox_inches="tight")
    plt.close(fig)

    # Mark plotting todo completed
    try:
        # update todo list status if available
        pass
    except Exception:
        pass

         # Extract parameters and statistics
    params = model.params
    bse = getattr(model, "bse", None)
    tvals = getattr(model, "tvalues", None)
    pvals = getattr(model, "pvalues", None)

    # Intercept name can be 'Intercept' or 'const'
    if "Intercept" in params:
        intercept_name = "Intercept"
    elif "const" in params:
        intercept_name = "const"
    else:
        intercept_name = params.index[0]

    def safe_get(series, key):
        try:
            return float(series[key])
        except Exception:
            return float("nan")

    b0 = safe_get(params, intercept_name)
    b1 = safe_get(params, x_var)
    se_b0 = safe_get(bse, intercept_name) if bse is not None else float("nan")
    se_b1 = safe_get(bse, x_var) if bse is not None else float("nan")
    t_b0 = safe_get(tvals, intercept_name) if tvals is not None else float("nan")
    t_b1 = safe_get(tvals, x_var) if tvals is not None else float("nan")
    p_b0 = safe_get(pvals, intercept_name) if pvals is not None else float("nan")
    p_b1 = safe_get(pvals, x_var) if pvals is not None else float("nan")

    print(f"Intercept (β₀): {b0:.4f}  SE={se_b0:.4f}  t={t_b0:.3f}  p={p_b0:.4f}")
    print(f"Slope (β₁) [{x_var}]: {b1:.4f}  SE={se_b1:.4f}  t={t_b1:.3f}  p={p_b1:.4f}")

    # R-squared, adjusted R-squared, N
    r2 = getattr(model, "rsquared", float("nan"))
    adj_r2 = getattr(model, "rsquared_adj", float("nan"))
    n_obs = int(getattr(model, "nobs", 0))
    print(f"R²={r2:.4f}  Adj R²={adj_r2:.4f}  N={n_obs}")

    # Direction and significance of slope at 5%
    if not np.isnan(b1):
        direction = "positive" if b1 > 0 else "negative" if b1 < 0 else "zero"
        significant = (p_b1 < 0.05) if (not np.isnan(p_b1)) else False
        sig_text = "significant at 5%" if significant else "not significant at 5%"
        print(f"Slope is {direction} and {sig_text} (p={p_b1:.4f})")
    else:
        print("Slope information unavailable for this model.")

## Verification & Modifications (Disclose • Verify • Critique)
- I ran the program, then spot checked the tables and the graphs to make sure they were realistic and completely put together
- Nothing was not completed the first time
- Everything worked fine, so I did not have to modify anything.

## If No AI Tools Used
Write: "No AI tools were used for this assignment."
