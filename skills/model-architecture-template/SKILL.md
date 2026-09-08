---
name: model-architecture-template
description: Defines the master tab structure, naming conventions, color codes, number formats, and print area setup for any new strategy or finance Excel model, serving as the structural template before any content is added.
---

# Model Architecture Template

## When to use
Use this skill at the very start of any new Excel model build -- before a single formula is written -- to establish the structural foundation. Trigger it when a team is starting a new model from scratch, when a model has been handed over from another team and needs to be restructured to a consistent standard, or when an organization wants to establish a house standard for all Excel models. It is the most upstream skill in the Modelling OS category and produces the empty architectural shell that all other skills populate.

## What it does
Produces a complete master architecture for any new strategy or finance Excel model: the full tab set with naming conventions, purpose, and color labels; a conventions sheet defining color codes, font sizes, number formats, and structural rules; print area and page setup for every tab; and a version history system. This is the skeleton into which all content skills (DCF, Scenario Model, KPI Dashboard) are built.

## Method

1. **Define the universal tab set.** Every strategy or finance model, regardless of its specific purpose, should include these structural tabs:

   Cover tab: Model identity and navigation.
   Legend/Conventions tab: Color codes, formatting rules, named range registry.
   Inputs tab: All user-editable assumptions.
   Data/Lookup tab: Reference tables, named ranges, lookup data.
   One or more Calc tabs: Named by workstream.
   Checks tab: Verification formulas, error flags.
   Outputs/Summary tab: Executive-facing summary, no calculations.
   Documentation tab: Model history, assumption rationale, audit log.

2. **Design the Cover tab.** The Cover tab is the first tab the user sees. It contains:

   Row 1-3: Model title (large font, bold), sub-title or client name (medium font), date range covered (small font).
   Rows 5-8: Key metadata: Version number, Last updated date, Prepared by, Reviewed by, Status (Draft/Final/Archived).
   Rows 10-15: Purpose statement (2-3 sentences describing what the model does and its primary audience).
   Rows 17-25: Key output summary (a 3-6 row table showing the model's headline outputs -- values referenced from the Outputs tab, not calculated here).
   Rows 27-35: Navigation guide -- a tab index showing every tab, its purpose, and a hyperlink to it.
   Rows 37-40: Confidentiality and disclaimer statement.

3. **Design the Legend/Conventions tab.** The Legend tab documents all structural rules used in the model. It serves as a guide for any user who did not build the model. Contents:

   Color code table: Color | Fill type | Meaning | Usage rule.
   Font convention table: Font type | Size | Usage (e.g., tab title: bold 12pt; section header: bold 10pt; body: regular 9pt).
   Number format table: Data type | Format string | Example (e.g., Currency: $#,##0.0 -> $1,234.5; Percentage: 0.0% -> 12.3%).
   Tab inventory: Tab name | Purpose | Color label | Status (Active/Archive).
   Named range registry: Named range | Refers to | Purpose | Last verified date.
   Structural rules: A bullet list of the model's architectural principles (no hardcodes in Calc tabs, all inputs on Inputs tab, etc.).

4. **Define the color coding standard.** Apply this consistently:

   Blue fill (#DDEEFF or custom), blue or dark blue font: Input cells (user-editable, hardcoded values).
   White fill, black font: Formula cells (never edit directly).
   Light green fill (#DDFFDD) or green font: Output cells and check cells that pass verification.
   Light grey fill (#F5F5F5): Section headers, spacers, non-active areas.
   Light orange fill (#FFE4B5): Override cells (temporary hardcodes, flagged for review).
   Red fill (#FFDDDD) or red font: Error flags, failed checks, alerts.
   Dark grey fill (#404040), white font: Tab-level headers and major section dividers.
   Tab color-coding: Inputs tab = blue tab; Data tab = grey tab; Calc tabs = no color (white); Checks tab = green tab; Outputs tab = dark grey tab; Cover and Legend = no color.

5. **Define number format conventions.** Apply globally using Excel's custom number format (Ctrl+1):

   Financial figures in $000s: #,##0 (no decimal); or #,##0.0 for one decimal.
   Financial figures in $M: #,##0.0 with "M" label in the column header.
   Percentages: 0.0% (one decimal, never 0% or 0.00% unless the context requires).
   Multipliers: 0.0x.
   Headcount: #,##0 (no decimal).
   Dates: MMM-YY for period headers (Jan-24, Feb-24); YYYY for year labels.
   Ratios: 0.00 (two decimals).
   Per-unit metrics (e.g., revenue per FTE): #,##0 with unit label in header.

6. **Define the Calc tab naming convention.** Each Calc tab name follows the pattern: [Category abbreviation] - [Description]. Maximum 25 characters. Category abbreviations: Rev (revenue), Cost (costs), FS (financial statements), Val (valuation), Mkt (market/TAM), Ops (operations), Cap (capital/investment). Examples: "Rev - ARR Build", "Cost - Headcount", "FS - Income Statement", "Val - DCF Engine".

7. **Design the Checks tab.** The Checks tab is the model's integrity layer. It should include:

   A summary traffic light at the top: IF(COUNTIF(checks_range,"FAIL")>0,"MODEL HAS ERRORS","ALL CHECKS PASS"), formatted in red or green respectively.

   Balance sheet balance check: Assets - Liabilities - Equity = 0 (IF(ABS(check)<0.01,"PASS","FAIL")).
   
   Cash flow tie-out: Opening cash + net cash flow - closing cash = 0.
   
   WACC vs. terminal growth rate: IF(WACC>terminal_growth,"PASS","FAIL -- WACC must exceed terminal growth").
   
   Probability sum check: IF(ABS(SUM(probabilities)-1)<0.001,"PASS","FAIL -- Probabilities do not sum to 100%").
   
   Revenue agreement: Revenue total in P&L tab = Revenue total in DCF tab (IF(ABS(difference)<0.1,"PASS","FAIL")).

8. **Define the version control system.** On the Cover tab, build a version history table: Version | Date | Changed by | Change summary. Start at v1.0. Increment the minor version for small updates (v1.1, v1.2), the major version for structural changes (v2.0). The version number in the Cover tab header should reference this table.

9. **Set print areas and page setup for each tab.** Define these settings:

   Cover tab: Portrait, A4, fit to 1 page. Print area: A1 to end of content.
   Legend tab: No print area required (internal reference document).
   Inputs tab: Portrait, A4, fit to 1-2 pages. Print area: all input sections.
   Each Calc tab: Landscape, A4, repeat row 1 on every page (Page Layout > Print Titles > Rows to repeat at top). Scale to fit width.
   Checks tab: Portrait, A4, fit to 1 page.
   Outputs tab: Landscape, A4, fit to 1 page. Print area: the full dashboard zone.

10. **Establish the row and column structure standard.** Apply these to every Calc tab for consistency:

    Row 1: Tab title (merged across columns, bold, dark fill, white font, 14pt).
    Row 2: Tab purpose statement (one sentence, italic, light fill).
    Row 3: Empty spacer row.
    Row 4: Section header (first section, bold, grey fill).
    Rows 5 onward: Data rows (row header in column A, data in columns B onward).
    Column A: Row labels (minimum 30 characters wide).
    Column B: First time period or first data column.
    Columns onward: Subsequent periods, scenarios, or categories.
    Last column: Total or summary column (bold, thin left border).

11. **Set column widths and row heights.** Column A (row labels): 28-35 characters wide. Data columns: 10-12 characters wide for annual data; 8-10 for monthly data. Header rows: 18-20pt height. Data rows: 15pt height. Section header rows: 18pt height. This creates a clean, scannable layout without wasted space.

12. **Create a model startup checklist.** When starting any new model using this architecture, verify:

    Cover tab: version set to v1.0, date set, author name entered.
    Legend tab: all color codes documented.
    Inputs tab: all blue fill cells identified, no formulas in input cells.
    Data tab: named ranges defined and documented in Legend.
    Calc tabs: no hardcoded numbers in formulas.
    Checks tab: all checks return PASS (even on empty data, check formulas work).
    Outputs tab: references to Calc tabs confirmed (no #REF! errors).
    Documentation tab: model purpose and initial assumptions recorded.

## Inputs

- Model purpose (one paragraph)
- List of analytical modules to be built (determines which Calc tabs are needed)
- Audience (internal use vs. client-facing, determines documentation depth)
- Team size (how many people will use and edit the model)
- Preferred number scale ($000s or $M)

## Output format

A complete model architecture specification with:

1. **Tab inventory**: Full list of all tabs with name, purpose, color label, fed by, feeds into, and protection status.
2. **Color coding reference**: 7-color table with fill codes, font conventions, and usage rules.
3. **Number format table**: 8-10 data types with format strings and examples.
4. **Calc tab naming examples**: 6-8 example tab names applying the convention.
5. **Checks tab design**: List of 5-8 checks with their IF formula patterns and the summary traffic light.
6. **Cover tab layout**: Row-by-row description of content and formatting.
7. **Row/column structure standard**: The universal Calc tab layout rules.
8. **Startup checklist**: 12-15 items to verify before adding any content.

## Example

**User says:** "I'm starting a new model for a retail chain acquisition. The model needs revenue forecasting, cost structure, a DCF valuation, and an LBO returns analysis. 4-person team, client-facing."

**Claude responds:**

**Tab inventory (17 tabs):**

| Tab Name | Purpose | Tab Color | Fed By | Feeds Into |
| Cover | Title, metadata, navigation | None | Outputs | - |
| Legend | Color codes, conventions, named ranges | None | - | - |
| Inputs | All assumptions (revenue, costs, capital) | Blue | - | All Calc tabs |
| Data | Historical financials, market benchmarks | Grey | External | Calc tabs |
| Rev - Stores | Store count and revenue by format | None | Inputs, Data | FS - P&L |
| Rev - LFL | Like-for-like growth model | None | Inputs | FS - P&L |
| Cost - COGS | Gross margin build | None | Inputs, Rev | FS - P&L |
| Cost - Opex | SG&A and overhead | None | Inputs | FS - P&L |
| FS - P&L | Income statement | None | Cost, Rev tabs | Val - DCF |
| FS - BS | Balance sheet | None | P&L, Inputs | Checks |
| FS - CF | Cash flow statement | None | P&L, BS | DCF, Checks |
| Val - DCF | DCF engine and equity bridge | None | FS tabs, Inputs | Outputs |
| Val - LBO | LBO debt schedule and returns | None | FS tabs, Inputs | Outputs |
| Checks | All balance and tie-out checks | Green | All FS + Val | - |
| Outputs | Executive summary, KPI tiles | Dark grey | All Val tabs | - |
| Scenarios | Scenario comparison table | None | Inputs, FS tabs | Outputs |
| Documentation | Version history, rationale | None | - | - |

**Color codes:**
Blue fill #DDEEFF = input cells; White fill black font = formulas; Green fill #DDFFDD = outputs/checks pass; Orange fill = override cells; Red fill = error; Grey fill = headers/labels; Dark grey fill = major headers.
