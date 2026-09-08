---
name: inputs-calcs-outputs-design
description: Enforces the three-tab separation principle for any Excel model, producing a full design for a dedicated Inputs tab, one or more Calculations tabs, and a clean Outputs tab with a conventions checklist.
---

# Inputs-Calcs-Outputs Design

## When to use
Use this skill when building or restructuring any Excel model that needs to be shared, audited, or maintained over time. It is specifically the right tool when a model has assumptions mixed in with formulas, when it is unclear which cells are inputs vs. calculations, or when a client or reviewer has flagged that the model is "hard to follow." It is also the starting point for any new model where the three-tab principle will be applied from scratch.

## What it does
Produces the complete structural design for three distinct zones: an Inputs tab (all user-editable assumptions, clearly labeled and color-coded blue), one or more Calculations tabs (all formula logic, no hardcoded numbers, referencing only the Inputs tab and other Calc tabs), and an Outputs tab (no calculations, only references to final Calc results, formatted for communication). Includes a conventions checklist to verify compliance.

## Method

1. **Understand the three-tab principle.** The core rule is: separate what you control (inputs) from how the model works (calculations) from what you present (outputs). This separation means: any user can change assumptions without touching formulas; any reviewer can audit the logic without hunting through merged cells; any updated version can be handed over without a briefing. It is the single most important structural discipline in financial modelling.

2. **Audit the existing model for violations.** If restructuring an existing model, perform an audit first. In Excel, use Ctrl+` to toggle formula view. Identify: (a) hardcoded numbers inside formulas (e.g., =A1*0.15 where 0.15 should be an input), (b) formula cells on the Inputs tab, (c) raw data mixed with calculated results on the same tab. Flag each violation before redesigning.

3. **Design the Inputs tab.** The Inputs tab is the only place where numbers are hardcoded. Design it with the following structure:

   Header block at top: Model name, version, date, analyst name.

   Assumption sections, one per analytical module. Each section has: a bold section header row with dark fill, then rows for each assumption with three columns: Assumption Label (text, left-aligned) | Value (blue fill, right-aligned, editable) | Unit or Note (text, grey, right-aligned, e.g., "% per year", "2024 USD").

   Example sections: Revenue Assumptions, Cost Assumptions, Capital Structure, Macro/Market Parameters, Operational Drivers.

   Rules for the Inputs tab: no formulas in the Value column (only hardcoded numbers), every assumption on this tab should have a label, never leave an unlabeled number. Apply blue fill to every Value cell. Lock all non-value cells.

4. **Handle calculated inputs.** Some "inputs" are derived from other inputs (e.g., a midpoint of a range, or a weighted average of two rates). These go on the Inputs tab in a separate "Derived Assumptions" section, clearly marked as formulas (black font, not blue fill). This keeps them visible to the user while flagging that they are not free-input cells.

5. **Design the Calculations tabs.** One Calc tab per analytical module. Rules for Calc tabs:

   No hardcoded numbers in formulas. Every constant must reference the Inputs tab.
   
   Column headers are time periods (years, quarters, months) or categories.
   
   Row headers are line items (e.g., Revenue, COGS, Gross Profit).
   
   Every formula references either: the Inputs tab (for assumptions), the Lookup tab (for reference data), or another cell on the same Calc tab or a sibling Calc tab (for intermediate calculations).

   Use simple, readable formulas. Avoid nested IF statements deeper than two levels. Instead, break complex logic into helper rows.

   Apply black font on white fill to all formula cells. Add row headers in grey fill.

6. **Structure a standard Calc tab layout.** Each Calc tab follows this layout: (Row 1) Tab title and purpose statement. (Row 2) Column headers (years or periods). (Row 3 onward) Calculation rows, grouped into sections with bold section headers. (Last rows) Summary or output rows that will be referenced by the Outputs tab. Add a thin border around output rows to signal they are referenced elsewhere.

7. **Enforce the no-hardcode rule in Calc tabs.** Use Excel's Find and Replace to check for hardcoded numbers in formulas. Press Ctrl+H, click Options, check "Look in: Formulas". Search for common hardcoded values (0.25, 12, 1000). Flag every hit. Replace with a reference to an Inputs cell, creating a new assumption row on the Inputs tab if one does not exist.

8. **Design the Outputs tab.** The Outputs tab has one rule: no calculations. It contains only references to cells on Calc tabs (using = formulas that point to specific cells) and formatting. It may contain charts (charts reference Calc tab data), formatted tables (values pulled via cell references), and narrative text blocks (in text boxes or merged cells). It should look like a finished report page, not a working spreadsheet.

9. **Structure the Outputs tab layout.** Use a grid structure: top section for KPI tiles (4-6 key metrics, each in a formatted box), middle section for summary table (key results by year or scenario), bottom section for charts. Apply professional formatting: no gridlines visible, clean borders, company color palette.

10. **Set up cross-tab referencing conventions.** When a Calc tab cell references an Inputs tab cell, the formula should read clearly. Good practice: use the tab name explicitly in the formula (e.g., =Inputs!B12 rather than just =B12). For Named Ranges, the formula reads even more clearly: =inp_RevenueGrowthY1. Always use absolute references ($) when referencing inputs from Calc tabs to prevent accidental relative reference shifts.

11. **Apply protection.** Protect the Calculations tabs (allow only selection of unlocked cells). On Inputs tab, lock everything except the blue Value cells. On Outputs tab, lock everything (no editing). Use a simple password or simply enable sheet protection without a password to prevent accidental edits.

12. **Build the conventions checklist.** After designing the model, verify compliance against this list:
    - All assumption values are on the Inputs tab, not embedded in formulas.
    - All cells on the Inputs tab value column are blue fill.
    - No Calc tab formula contains a hardcoded number.
    - No formula appears in the Outputs tab (only cell references).
    - All Calc tabs reference Inputs tab by name (Inputs!cellref or named range).
    - Outputs tab has no gridlines visible and is print-ready.
    - All three tab zones are color-labeled in the tab bar.
    - A Legend tab documents the conventions.

## Inputs

- Description of the model's purpose and analytical modules
- List of the key assumptions that drive the model (to populate the Inputs tab structure)
- List of the key outputs required (to design the Outputs tab)
- Current model state (new build vs. restructuring an existing model)
- Excel version (affects available features for protection and naming)

## Output format

A complete three-tab design specification with:

1. **Inputs tab design**: Section headers list, column layout (Label, Value, Unit), and a table of every key assumption with its default value, unit, and section assignment.
2. **Calc tab designs**: One section per Calc tab, showing the row structure (line items), column structure (periods), and which rows reference Inputs and which are internal calculations.
3. **Outputs tab design**: Grid layout description with KPI tile positions, table structure, and chart descriptions.
4. **Cross-reference formula patterns**: Written examples of how Calc tabs reference Inputs tab.
5. **Conventions checklist**: 12-15 items as a verifiable checklist.
6. **Common violations log** (if restructuring): List of hardcodes and mixing violations found in the existing model.

## Example

**User says:** "I'm building a SaaS revenue model. Assumptions include: starting ARR, annual growth rate, churn rate, expansion rate, and headcount additions per year. Outputs needed: ARR by year, net revenue retention, headcount cost, and EBITDA margin."

**Claude responds:**

**Inputs tab sections:**

Section 1 - Revenue Assumptions:
| Assumption | Value | Unit |
| Starting ARR (Year 1) | [blue] | $000s |
| Annual new logo growth rate | [blue] | % per year |
| Annual churn rate | [blue] | % of ARR |
| Net expansion rate (upsell/cross-sell) | [blue] | % of ARR |

Section 2 - Cost Assumptions:
| Annual headcount additions | [blue] | FTEs per year |
| Fully loaded cost per FTE | [blue] | $000s per FTE |
| Other OpEx as % of ARR | [blue] | % |

**Calc tab: Rev - ARR Build (columns: Year 1 through Year 5)**
Row structure:
- Opening ARR: =prior year Closing ARR (or Inputs!StartingARR for Year 1)
- New logo ARR: =Opening ARR * Inputs!NewLogoGrowthRate
- Expansion ARR: =Opening ARR * Inputs!ExpansionRate
- Churned ARR: =-Opening ARR * Inputs!ChurnRate
- Closing ARR: =Opening + New Logo + Expansion + Churned
- NRR: =(Opening + Expansion + Churned) / Opening

**Outputs tab:**
KPI tile 1: Closing ARR Year 5 (reference: ='Rev - ARR Build'!ClosingARR_Y5)
KPI tile 2: NRR Year 5 (reference: ='Rev - ARR Build'!NRR_Y5)
KPI tile 3: EBITDA Margin Year 5 (reference: ='Cost - P&L'!EBITDAMargin_Y5)
Chart: ARR waterfall by year (data from Rev - ARR Build tab)

**Conventions check:**
- All growth rates hardcoded only in Inputs tab. [verify]
- No numeric constants in Calc tab formulas. [verify]
- Outputs tab: formula bar shows = reference only, never =A1+B1 style. [verify]
