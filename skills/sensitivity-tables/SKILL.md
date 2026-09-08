---
name: sensitivity-tables
description: Builds one- and two-variable sensitivity (Data Table) analysis on a model output in a .xlsx with live formulas and a recalc check, then delivers the workbook. Use when someone wants to see how an output (NPV, IRR, EPS, margin) moves as one or two inputs change. Do not trigger for editing an existing workbook through the Claude for Excel add-in.
---

# Sensitivity Tables (One- and Two-Variable)

## When to use
Use when a strategy or finance model has a single key output and you want to show how that output responds as one or two drivers vary across a range. Typical drivers are price, volume, discount rate, growth, churn, or cost per unit. This skill BUILDS a downloadable .xlsx from scratch in the Claude app (openpyxl plus numpy), with live formulas and a recalc-and-verify pass. It is not the Claude for Excel add-in.

## What it builds
A single `Sensitivity` tab that attaches to an existing model. It references the model's input cells and output cell, lays out one-variable and two-variable grids, and color-scales the result block. If the source model lives on another sheet, the grid links to it with cross-sheet references so it stays live.

## Build workflow
1. Identify the output cell (for example `Model!B40` holding NPV) and the one or two input cells that drive it (for example `Model!B6` discount rate, `Model!B7` growth).
2. Confirm the output is a live formula chain back to those inputs. If the output is hardcoded, stop and wire the model first; a Data Table over a constant returns the constant.
3. Choose input ranges around the base case (for example base discount rate 10 percent, sweep 6 to 14 percent in steps of 2).
4. Build the native Excel Data Table structure: corner formula, input axes, and the `{=TABLE(r,c)}` array (see spec).
5. ALSO build the code-computed grid as a robust fallback: re-evaluate the model in Python across every input combination and write the resulting values as a labeled matrix.
6. Apply a 3-color scale to the result block and label both axes with the input cell names.
7. Add the Checks block.
8. Recalculate the workbook in a real engine (headless LibreOffice).
9. Verify zero formula errors (#REF!, #DIV/0!, #VALUE!, #NAME?), confirm the checks pass, then deliver the .xlsx.

## Tab-by-tab spec

### Sensitivity tab, one-variable block
Pick a layout direction. For inputs running DOWN a column:
- `B3`: the output formula, referencing the live output: `=Model!B40`. This is the corner cell.
- `A4:A10`: the input values you sweep (blue inputs), for example 6%, 8%, 10%, 12%, 14%.
- `B4:B10`: the Data Table results column.
- Select `A3:B10`, then Data > What-If Analysis > Data Table, leaving Row input cell blank and setting Column input cell to `Model!B6`. Excel writes `{=TABLE(,Model!B6)}` into `B4:B10`.
- openpyxl can write the array formula string `=TABLE(,Model!B6)` into the block and the `=Model!B40` corner, but the values only populate after recalculation in a real engine.

Code-computed fallback for the same block (recommended when generating via code), written to a labeled matrix below, say starting `A14`:
- `A14`: label "Discount rate". `B14`: "NPV (computed)".
- For each input value in `A15:A21`, Python sets `Model!B6` to that value, recomputes the model logic in numpy, and writes the resulting NPV as a plain value in column B.
- Keep both: the native Data Table proves it is live; the computed matrix guarantees populated numbers even before the user opens Excel.

### Sensitivity tab, two-variable block
- Corner cell `E3`: `=Model!B40` (the live output).
- `E4:E10`: row-input values DOWN the left edge (first driver, for example growth 0%, 1%, ... 6%).
- `F3:L3`: column-input values ACROSS the top row (second driver, for example discount rate 6% ... 14%).
- Select `E3:L10`, Data > What-If Analysis > Data Table, Row input cell = `Model!B7` (the across driver), Column input cell = `Model!B6` (the down driver). Excel fills `F4:L10` with `{=TABLE(Model!B7,Model!B6)}`.
- Watch the orientation: the cell tied to the TOP row goes in Row input cell; the cell tied to the LEFT column goes in Column input cell. Swapping them transposes the result silently.
- Code-computed fallback: a double loop in Python over both axes, writing the value matrix to a labeled block (axis headers in green if they pull from the model, blue if literal sweep values).

### Choosing axis ranges
- Center each axis on the base-case value so the base sits in the middle row and middle column, which makes the center-cell check meaningful.
- Use an odd number of steps (5, 7, or 9) so a single middle cell lines up exactly with the base case rather than falling between two cells.
- Keep step sizes uniform on each axis (constant increment) so the grid reads as a clean gradient and the color scale is interpretable.
- For rates, sweep in absolute points (8%, 10%, 12%), not relative percent of the base, to avoid confusing readers.

### openpyxl build notes
- Write the corner as a real formula string: `ws["B3"] = "=Model!B40"`. Do not paste the current numeric value of the output.
- For the native Data Table array, openpyxl can store the `=TABLE(...)` text, but a real engine must recalculate to fill the block; treat the array as a placeholder until the recalc step runs.
- When writing the computed fallback, set the data values with Python (`ws.cell(row=r, column=c, value=float(npv))`) and keep them visually distinct from the live block with a header note such as "computed snapshot".
- Apply the color scale with `openpyxl.formatting.rule.ColorScaleRule` over the result block, mapping min to one color and max to another through a midpoint.

## Formula and formatting conventions
- Blue font for inputs (the swept axis values you type in).
- Black font for in-tab formulas.
- Green font for cross-tab links such as `=Model!B40`.
- No hardcodes where a formula belongs: the corner cell must be `=Model!B40`, never the typed base-case number.
- One consistent formula per row or column: the entire Data Table block is a single array; the computed fallback uses one Python expression applied across the grid.
- Number formats: rates as `0.0%`, currency outputs as `#,##0`, ratios as `0.00`. Axis values match the unit of the input cell they replace.

## Checks
- Corner cell equals the live output: `=(B3=Model!B40)` returns TRUE.
- Axes labeled with the input cell names (a text label above or beside each axis naming the driver and its cell address).
- Center of the grid matches the base case: the grid cell at the base-case row and base-case column equals the model's current output, `=(<center cell>=Model!B40)` TRUE.
- For the computed fallback, the cell at the base inputs equals the native Data Table cell at the same inputs (within rounding).

## Recalculate and verify
Save with openpyxl, then recalc headless (for example `libreoffice --headless --convert-to xlsx`) so `=TABLE(...)` and cross-sheet links populate. Reload and scan every cell for #REF!, #DIV/0!, #VALUE!, #NAME?. Confirm the three checks return TRUE. If any error or FALSE appears, fix the reference or range and loop until clean, then deliver.

## Inputs to gather
- Path to the source model and the exact output cell address.
- The one or two input cell addresses to sweep.
- Base-case values and the desired range and step for each axis.
- Units and preferred number formats for axes and output.

## Example
Hypothetical model: `Model!B40` = project NPV, driven by `Model!B6` discount rate (base 10%) and `Model!B7` revenue growth (base 3%). One-variable block sweeps discount rate 6% to 14% by 2%, giving NPV of (for example) 1,210 / 980 / 760 / 560 / 380 in hypothetical units. Two-variable block crosses growth 0% to 6% against discount rate 6% to 14%; the center cell at 3% growth and 10% discount equals `Model!B40` of 760, confirming the base case sits in the middle of the grid.
