---
name: scenario-manager
description: Builds a Base/Bull/Bear scenario switching layer for a model in a .xlsx with a live selector, CHOOSE or INDEX assumption links, a side-by-side output comparison, and a recalc check, then delivers the workbook. Use when someone wants an auditable scenario toggle instead of Excel's hidden Scenario Manager. Do not trigger for editing an existing workbook through the Claude for Excel add-in.
---

# Scenario Manager (Base / Bull / Bear)

## When to use
Use when a model needs to switch cleanly between Base, Bull, and Bear cases driven by a small set of assumptions, and you want every case visible and auditable on one sheet. This skill BUILDS a downloadable .xlsx from scratch in the Claude app (openpyxl plus data validation), with a live selector, formula-driven assumption pulls, a comparison table, and a recalc-and-verify pass. It is not the Claude for Excel add-in.

## What it builds
Five areas: a `Cover` tab (selector and summary), a `Scenario Inputs` tab (each driver under Base, Bull, Bear), live links INTO the model so assumptions pull the active case, an `Outputs` comparison tab (key results across all three cases side by side), and a `Checks` tab.

## Build workflow
1. List the drivers that differ across cases (for example growth, price, churn, capex) and find each driver's assumption cell in the model.
2. Build the `Scenario Inputs` block holding Base, Bull, Bear values for every driver.
3. Build a single selector cell (1, 2, or 3) with a data-validation dropdown.
4. Rewrite each model assumption cell to pull the active case via CHOOSE or INDEX referencing the selector.
5. Build the `Outputs` tab comparing key results across all three cases (parallel calc, see spec) so all cases show at once regardless of selector.
6. Add the `Checks` tab.
7. Recalculate the workbook in a real engine (headless LibreOffice).
8. Verify zero formula errors (#REF!, #DIV/0!, #VALUE!, #NAME?), confirm checks pass, then deliver the .xlsx.

## Tab-by-tab spec

### Cover tab
- `B2`: selector cell. Apply data validation list `1,2,3` (or named list Base, Bull, Bear mapped to 1/2/3).
- `B3`: active case name, `=CHOOSE(B2,"Base","Bull","Bear")`.
- A small results summary that reads the live model output under the active case, for example `B6 =Model!B40`.

### Scenario Inputs tab
One row per driver, one column per case:
- `A` driver name, `B` Base, `C` Bull, `D` Bear (all blue inputs), `E` active value (black formula).
- Active value pulls with the selector. Using CHOOSE: `E5 =CHOOSE(Cover!$B$2,B5,C5,D5)`.
- Equivalent with INDEX: `E5 =INDEX(B5:D5,Cover!$B$2)`. INDEX is cleaner when the case count may grow; CHOOSE reads more explicitly for three fixed cases.
- Copy the one chosen formula down every driver row so the column is consistent.

### Links into the model
- Each model assumption cell points at the active value, for example `Model!B6 =ScenarioInputs!E5`.
- Never let the model hold a typed assumption that a scenario should control; the value must flow selector -> active column -> model.
- Keep the link as the green cross-tab color so reviewers see the assumption is driven, not typed.

### Outputs tab (side-by-side comparison)
Show every case at once, not just the active one. Two robust patterns:
- Parallel calc (preferred for auditability): replicate the few output formulas three times, each fed by the Base, Bull, or Bear column directly rather than the active column. For example NPV under Bull recomputes the output chain using `ScenarioInputs!C5:C12` instead of `E`. This shows all three without touching the selector.
- Read-each: if the model is large, document that the comparison cells capture the live output for each case by setting the selector to 1, 2, 3 in turn during the recalc step and pasting each captured value; note clearly which cells are captured snapshots versus live.
- Layout: rows = key metrics (Revenue, EBIT, NPV, IRR), columns = Base, Bull, Bear, plus a Bull-minus-Base and Bear-minus-Base delta column.

### Checks tab
- Selector within range: `=AND(Cover!B2>=1,Cover!B2<=3,Cover!B2=INT(Cover!B2))` TRUE.
- Each driver has all three cases filled: `=COUNTBLANK(ScenarioInputs!B5:D12)=0` TRUE.
- Active column matches selector: for a spot driver, `=(ScenarioInputs!E5=CHOOSE(Cover!B2,ScenarioInputs!B5,ScenarioInputs!C5,ScenarioInputs!D5))` TRUE.

### openpyxl build notes
- Add the dropdown with `openpyxl.worksheet.datavalidation.DataValidation(type="list", formula1='"1,2,3"')`, then `ws.add_data_validation(dv)` and `dv.add(ws["B2"])`.
- Write every active-value and link cell as a formula string, never as the resolved case number; the value only appears after recalculation.
- If using named cases in the dropdown, keep a hidden mapping so the selector still resolves to an integer for CHOOSE/INDEX; mixing text and index in one selector breaks the formula.
- Fill the selector cell with a distinct background and a thick border so reviewers immediately see the single control cell.

### Directional sanity
- Bull should not produce a worse output than Base on the headline metric, and Bear should not beat Base; if it does, a driver's case values are likely swapped between columns.
- Add an optional informal check on the Outputs tab comparing the sign of each delta against the expected direction, so a swapped column is caught on recalc rather than in review.

## Formula and formatting conventions
- Blue font for the Base/Bull/Bear input values and the selector cell.
- Black font for in-tab formulas (the active-value CHOOSE/INDEX column, output calcs).
- Green font for cross-tab links such as `=ScenarioInputs!E5` inside the model.
- No hardcodes where a formula belongs: the active column and model assumptions are always formulas, never the case value retyped.
- One consistent formula per row or column: the same CHOOSE/INDEX pattern down the active column; the same output formula across the comparison columns.
- Number formats: rates `0.0%`, currency `#,##0`, multiples `0.00x`. Highlight the selector cell with a fill so it is obvious where to toggle.

## Trade-off vs built-in Scenario Manager
Excel's built-in Scenario Manager stores case values inside a dialog, invisible on the sheet, overwrites the live inputs when shown, and is easy to desync from the model. A visible toggle block keeps every case on the grid, makes the active pull a traceable formula, and lets reviewers see all three outcomes side by side without opening any menu. It is more auditable and survives copy, review, and version control.

## Checks
See the Checks tab above: selector in 1..3 and integer, no blank case cells, active value equals the selector's case. All three must return TRUE.

## Recalculate and verify
Save with openpyxl, recalc headless (for example `libreoffice --headless --convert-to xlsx`) so CHOOSE/INDEX and cross-sheet links resolve. Reload, scan for #REF!, #DIV/0!, #VALUE!, #NAME?. Toggle the selector to 1, 2, 3 and confirm outputs change and the active check holds at each setting. Fix and loop until clean, then deliver.

## Inputs to gather
- Path to the model and the assumption cell address for each driver.
- Base, Bull, and Bear value for every driver.
- The key output metrics to compare and their cells.
- Whether the comparison should be parallel-calc (live) or captured snapshots.

## Example
Hypothetical drivers: revenue growth (Base 4%, Bull 8%, Bear 1%), gross margin (Base 55%, Bull 58%, Bear 50%). Selector `Cover!B2 = 2` selects Bull, so `ScenarioInputs!E5 =CHOOSE(2,4%,8%,1%) = 8%` flows into `Model!B6`. The Outputs tab shows NPV of (for example) 760 Base, 1,140 Bull, 410 Bear in hypothetical units, with a Bull-minus-Base delta of 380, all visible at once.
