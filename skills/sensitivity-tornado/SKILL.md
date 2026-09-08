---
name: sensitivity-tornado
description: Ranks variable sensitivities to a key output and builds a tornado chart by swinging each input over a defined range, computing output impact, and producing a horizontal bar chart data structure.
---

# Sensitivity Tornado

## When to use
Use this skill when a model has been built and the team needs to identify which assumptions actually drive the output, before investing further analytical effort. Trigger it when a project manager asks "what are the key risks to the model?", when a model review requires a sensitivity summary, or when an investment committee asks "what is the most important assumption?" It is most valuable at the end of a modeling exercise, as a final diagnostic before a model is presented or signed off.

## What it does
Produces a complete tornado chart analysis: identifies all input variables to test, defines the swing range for each, computes the output impact for each swing (both upside and downside), ranks variables by absolute impact, and specifies the horizontal bar chart data structure in Excel that creates a tornado diagram. Ranks assumptions by their contribution to output uncertainty.

## Method

1. **Identify the key output to be sensitized.** The tornado analysis focuses on one output at a time. Define which output is most decision-relevant: EBITDA, IRR, equity value, NPV, 5-year revenue, or another key metric. If multiple outputs are relevant, build one tornado per output.

2. **List all candidate input variables.** Compile every material assumption in the model that is (a) uncertain and (b) could have a meaningful impact on the output. A starting list for a financial model: revenue growth rate, gross margin, EBITDA margin, customer acquisition cost, churn rate, capital expenditure intensity, working capital days, WACC or discount rate, terminal growth rate, exit multiple, FX rate, tax rate. Aim for 8-15 variables.

3. **Define the swing range for each variable.** For each input variable, define a low value and a high value representing a credible range. The standard approach is plus and minus one standard deviation of the variable, or plus and minus 10-20% of the base value, or a specific scenario range (e.g., "management guidance low" to "management guidance high"). Key principle: the swings should be symmetrical around base for comparability, but can be asymmetric if the distribution is genuinely skewed.

4. **Compute the output for each swing, one variable at a time.** For each variable:
   a. Set the variable to its low value, hold all other variables at base. Record the output value. Label this "Low impact."
   b. Set the variable to its high value, hold all other variables at base. Record the output value. Label this "High impact."
   c. Reset the variable to its base value before testing the next variable.

   In Excel, this can be done manually (change each input cell, record the output in a table) or systematically using a one-variable Data Table for each variable. The Data Table approach is preferred for larger variable sets because it recalculates automatically when the model changes.

5. **Compute the impact for each variable.** Impact = High output - Low output (for a "higher is better" output). Absolute impact = ABS(High output - Low output). Sort variables by absolute impact in descending order. The variable with the largest absolute impact goes at the top of the tornado chart.

6. **Build the tornado data table.** Set up the following columns in Excel:

   Column A: Variable name.
   Column B: Base output value (the same for all rows, reference to the output cell).
   Column C: Low output (output when this variable is set to its low value).
   Column D: High output (output when this variable is set to its high value).
   Column E: Low impact = Low output - Base output (will be negative if the low swing reduces the output).
   Column F: High impact = High output - Base output (will be positive if the high swing increases the output).
   Column G: Absolute impact range = ABS(High output - Low output). Used for sorting.

   Sort the table by Column G descending. The first row is the most impactful variable.

7. **Build the tornado chart as a horizontal bar chart.** Excel does not have a native tornado chart type. Build it using a stacked bar chart:

   Data series 1 (invisible base): This positions the bars correctly. For each variable, the invisible base = MIN(Low output, High output) - a small offset to center the chart. Alternatively, the base = the low output value for variables where the low swing is negative.

   Simpler approach: Build two data series centered on zero. For each variable: Downside bar = Low impact (negative number). Upside bar = High impact (positive number). Set the category axis (X-axis) to cross at 0.

   The result is a "butterfly" bar chart centered on zero. The longer the pair of bars, the more sensitive the output is to that variable.

8. **Sort rows so the most impactful variable is at the top.** In Excel bar charts, the categories display in reverse order of the data table. Sort the data table so the least impactful variable is in row 1 and the most impactful is in the last row. This ensures the chart displays with the most impactful variable at the top (which is the convention for tornado charts).

9. **Apply color coding.** Format the downside bars in red and the upside bars in blue (or green). Remove gridlines. Remove the chart border. This creates a clean, professional tornado diagram.

10. **Add variable labels and impact values.** Add data labels to each bar showing the output value (or the impact value). Add the variable name as the Y-axis category label. Add a vertical reference line at x=0 (the base case) using a scatter series overlaid on the bar chart.

11. **Interpret the tornado.** The top 3-5 variables in the tornado are the model's critical assumptions. These deserve: the most careful estimation and validation, sensitivity ranges in the board presentation, and ongoing monitoring if the model is a live planning tool. Variables in the bottom half of the tornado are relatively unimportant and can be fixed at base case values for simplicity.

12. **Document the swing assumptions.** On the same tab as the tornado data table, add a column showing the swing assumption: "Low: Base - 2pp; High: Base + 2pp" for each variable. This makes the analysis auditable and allows others to understand the basis for the sensitivity range without opening the Inputs tab.

## Inputs

- Completed financial or strategic model with a key output cell
- List of input variables to test (or permission to pull all inputs from the Inputs tab)
- Swing range for each variable (or a default of +/- 10-15% of base value to apply uniformly)
- Key output cell reference and units

## Output format

A complete tornado analysis specification with:

1. **Variable list with swing ranges**: Variable name | Base value | Low value | High value | Swing rationale. One row per variable.
2. **Tornado data table**: Variable | Base output | Low output | High output | Low impact | High impact | Absolute range. Sorted by absolute range descending.
3. **Top 5 findings**: The top 5 most impactful variables named and their absolute impact quantified.
4. **Chart data structure**: The two data series (downside and upside bars), sort order, and axis setup.
5. **Chart formatting instructions**: Colors, labels, reference line approach.
6. **Interpretation summary**: What the tornado reveals about model uncertainty and where to focus analytical effort.

## Example

**User says:** "I have a DCF model. Key output: Equity Value. Base case equity value = $125M. Test these variables: Revenue CAGR (base 15%), EBITDA margin (base 22%), WACC (base 12%), terminal growth rate (base 3%), capex as % of revenue (base 5%), churn rate (base 8%)."

**Claude responds:**

**Swing ranges:**

| Variable | Base | Low | High |
| Revenue CAGR | 15% | 10% | 20% |
| EBITDA margin | 22% | 18% | 26% |
| WACC | 12% | 10% | 14% |
| Terminal growth rate | 3% | 1.5% | 4.5% |
| Capex % revenue | 5% | 3% | 7% |
| Churn rate | 8% | 5% | 12% |

**Tornado data table (sorted by absolute impact):**

| Variable | Low Equity Value | High Equity Value | Downside | Upside | Abs Range |
| WACC | $95M | $163M | -$30M | +$38M | $68M |
| Revenue CAGR | $88M | $165M | -$37M | +$40M | $77M |
| Terminal growth rate | $103M | $148M | -$22M | +$23M | $45M |
| EBITDA margin | $108M | $142M | -$17M | +$17M | $34M |
| Churn rate | $112M | $137M | -$13M | +$12M | $25M |
| Capex % revenue | $117M | $133M | -$8M | +$8M | $16M |

**Top 3 critical assumptions:**
1. Revenue CAGR: $77M range ($88M to $165M). This is the largest single driver of equity value uncertainty. Validate with bottoms-up bookings pipeline analysis.
2. WACC: $68M range. Driven by beta and risk-free rate assumptions. Crosscheck against analyst consensus for comparable companies.
3. Terminal growth rate: $45M range. Crosscheck against long-run GDP and sector growth expectations.

**Chart structure:** Sort table with Capex in row 1, Revenue CAGR in row 6 (so chart displays Revenue CAGR at top). Two series: Downside (red bars, negative values) and Upside (blue bars, positive values). X-axis crosses at 0, labeled "vs. Base Case Equity Value ($125M)."
