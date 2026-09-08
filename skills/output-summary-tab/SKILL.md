---
name: output-summary-tab
description: Designs the executive summary tab of an Excel model with KPI tiles, a narrative block, a sensitivity summary, and an assumption log, formatted for print or screenshot use in a board pack.
---

# Output Summary Tab

## When to use
Use this skill when a financial or strategic model is complete and needs a clean, executive-facing front page that summarizes all key results on a single tab. Trigger it when a model needs to be presented to a board, an investment committee, or a senior client, and the full model is too detailed to walk through directly. Also use it when a model needs to be submitted as a standalone document where the Outputs tab must communicate the full story without the presenter in the room.

## What it does
Designs a single-tab executive summary that contains: 4-6 KPI tiles (key metrics in formatted boxes), a concise narrative block (the "so what" of the model output), a one-table sensitivity summary (1-2 key sensitivities in a compact format), and an assumption log (10-15 key assumptions with base values). All values are references to Calculation tabs -- no formulas on this tab. The tab is designed to be printed on one A4 landscape page or screenshotted as a single image for a board pack.

## Method

1. **Define the layout grid.** The Outputs tab uses a page-layout approach, not a standard spreadsheet approach. Think of it as a designed page with defined zones. For an A4 landscape orientation, the usable grid is approximately 32 columns wide by 50 rows tall (at a standard column width and row height). Design these zones:

   Zone A (top strip): Title bar. Contains: model name, reporting period, version number, date. Full width, 3-4 rows tall.
   
   Zone B (upper left, 40% of width): KPI tiles grid. 4-6 tiles in a 2x2 or 2x3 arrangement.
   
   Zone C (upper right, 60% of width): Narrative block. A text-heavy section describing the key finding.
   
   Zone D (lower left, 50% of width): Sensitivity summary. A compact 2-variable sensitivity table or a 1x6 row showing a key metric at Bear, Base, and Bull.
   
   Zone E (lower right, 50% of width): Assumption log. A 2-column table (assumption name, base value) listing the 10-15 most important model inputs.

2. **Design the KPI tiles.** Each KPI tile is a formatted cell or group of cells displaying one metric. Standard tile design:
   - Title (small font, grey): metric name, e.g., "Year 5 EBITDA"
   - Value (large font, black or dark blue): the metric value, e.g., "$42M"
   - Unit/context (small font, grey): unit and period, e.g., "$M, FY2029"
   - Optional: a one-line delta vs. prior year or vs. budget, formatted in green (positive) or red (negative)

   KPI tile borders: a medium border around each tile, with a light blue or grey fill for the title row. Use consistent tile dimensions (same width, same height) for visual alignment.

   The value cell formula is a simple reference: ='Rev - Revenue'!$G$42 (the specific output cell on the relevant Calc tab). No calculations.

3. **Choose the 4-6 KPIs.** Select the metrics that matter most to the audience. For a P&L and DCF model: Revenue (Year 5), EBITDA Margin (Year 5), Enterprise Value, Equity Value per Share, IRR, and Payback Period. For a strategic model: Market Size (TAM), Serviceable Market (SAM), Market Share (Year 3), Revenue (Year 3), EBITDA (Year 3), and a strategic metric like NPS or Customer Count.

4. **Write the narrative block.** The narrative block contains 100-200 words of structured analysis text. Structure it using the Pyramid Principle: lead with the key finding (the answer), then support with 2-3 evidence statements, then close with the implication for the decision. Use a heading (the answer, in bold) and 3-4 bullet points (the supporting evidence). This is the "so what" layer that prevents the audience from having to interpret the numbers themselves.

   The narrative block is a text box or merged cell range in Excel. It should be manually typed, not formula-driven. Update it each time the model is refreshed.

5. **Design the sensitivity summary.** Keep the sensitivity section compact: a 4x4 to 6x6 Data Table showing equity value (or the key output) across 2 variables. Label the row and column variables clearly. Highlight the base case cell with a dark border. Use the three-color conditional formatting scale (red to green). Include a one-sentence annotation below the table: "EV ranges from $X to $Y across the sensitivity range shown."

   Alternatively (for a cleaner layout): show a simpler 3-column summary: Metric | Bear | Base | Bull. This takes less space and is more readable for a non-technical audience.

6. **Design the assumption log.** The assumption log is a two-column table: Assumption Name | Base Value. List the 10-15 most important model inputs. Organize them by category (Revenue, Cost, Capital, Macro) with a bold section header row per category.

   All values in the assumption log are references to the Inputs tab: ='Inputs'!$D$5 (no formulas, only cell references to the specific input cells). If an assumption changes on the Inputs tab, the assumption log updates automatically.

   Include the assumption unit in the Value column (e.g., "15%" not "0.15"; "3.2x" not "3.2"). Use a custom number format or append the unit in a third narrow column.

7. **Apply professional formatting.** On the Outputs tab:
   - Remove gridlines: View > Gridlines (uncheck).
   - Set background: white or very light grey.
   - Use a consistent font family (Calibri or Arial, not Times New Roman).
   - KPI tile values: 18-24pt font. Tile labels: 8-9pt font.
   - Narrative body text: 10-11pt font.
   - Assumption log: 9-10pt font, condensed.
   - Section borders: medium border weight, dark grey or black.
   - Page title: 14-16pt bold.

8. **Set the print area.** Go to Page Layout > Print Area > Set Print Area. Select the entire Outputs tab grid (from cell A1 to the bottom-right of Zone E). Set page orientation to Landscape. Scale to fit one page (Page Layout > Scale to Fit: 1 page wide, 1 page tall). This ensures the tab prints cleanly on one A4 or Letter page.

9. **Add a model metadata footer.** At the very bottom of the Outputs tab (outside the print area or as a narrow footer strip within it), add: Model version, Last updated date (reference to a date cell on the Inputs tab), Prepared by, Confidential label. This is standard for client-facing models.

10. **Test the "screenshot test."** Take a screenshot of the Outputs tab. Can the key story be understood without opening any other tabs? Is the key metric visible at a glance? Is the narrative block readable? If not, the tab needs redesign. A well-designed Outputs tab is a self-contained document.

11. **Lock and protect the Outputs tab.** All cells on the Outputs tab should be locked (no editing). This prevents accidental overwriting of the carefully formatted cell references. Use sheet protection without a password, or with a shared password known to the team.

12. **Add a hyperlink navigation row.** At the top of the Outputs tab, add a row of text with hyperlinks to the key Calc tabs (Ctrl+K > Place in This Document > select tab). This allows a reviewer who wants to trace a number to navigate directly to the source without scrolling through all tabs.

## Inputs

- Completed model with key output cells identified
- List of the 4-6 KPIs to feature in the tiles
- The key narrative message (the main finding or recommendation)
- The key sensitivity variables (for the sensitivity summary)
- The 10-15 most important input assumptions to show in the assumption log

## Output format

A complete Outputs tab design specification with:

1. **Zone layout map**: ASCII-style description of the five zones and their dimensions.
2. **KPI tile designs**: Metric name, value cell reference, unit, and optional delta row for each of the 4-6 tiles.
3. **Narrative block content**: Structured text using Pyramid Principle format (answer + 3 supporting points).
4. **Sensitivity summary format**: Choice of compact Data Table or Bear/Base/Bull table with exact layout.
5. **Assumption log table**: All 10-15 assumptions listed with their Inputs tab cell reference.
6. **Formatting checklist**: Gridlines, fonts, borders, print area, scale to fit settings.
7. **Footer metadata content**: Version, date, author, confidential label.

## Example

**User says:** "I have a 5-year SaaS P&L model. Key outputs: ARR Year 5, EBITDA Margin Year 5, Customer Count Year 5, CAC Payback. Key finding: the business reaches profitability in Year 3 and generates $45M EBITDA in Year 5. Key assumptions: 35% ARR growth, 20% churn, $18K ACV."

**Claude responds:**

**Zone layout (A4 landscape):**
Row 1-3: Title bar [Model Name | Period | Version | Date]
Rows 4-22: [KPI Tiles (cols A-M)] | [Narrative Block (cols N-AH)]
Rows 23-40: [Sensitivity Summary (cols A-N)] | [Assumption Log (cols O-AH)]
Rows 41-43: [Footer metadata -- full width]

**KPI tiles:**
Tile 1: ARR Year 5 | reference: ='Rev - ARR'!G45 | unit: $M
Tile 2: EBITDA Margin Year 5 | reference: ='FS - P&L'!G18 | unit: % of revenue
Tile 3: Customer Count Year 5 | reference: ='Rev - ARR'!G12 | unit: Customers
Tile 4: CAC Payback | reference: ='Cost - Unit Econ'!D5 | unit: months
Tile 5: Breakeven Year | reference: ='FS - P&L'!D20 | unit: FY20XX

**Narrative block:**
Heading: "The business reaches EBITDA profitability in Year 3 and delivers $45M EBITDA by Year 5 at 22% margin."
Bullet 1: ARR grows from $18M today to $80M by Year 5, driven by 35% net new ARR growth and churn improvement from 20% to 15%.
Bullet 2: Gross margin expands from 68% to 76% as infrastructure costs scale more slowly than revenue.
Bullet 3: EBITDA turns positive in Year 3 as sales and marketing efficiency improves; CAC payback reaches 14 months by Year 4.
Implication: The model supports the growth investment thesis. Key risk: if churn remains at 20% throughout, Year 5 EBITDA falls to $31M (31% below base case).

**Assumption log (excerpt):**
| Assumption | Base Value |
| ARR Growth Rate | 35% |
| Annual Churn Rate | 20% -> 15% (improving) |
| Average Contract Value | $18,000 |
| Gross Margin | 68% -> 76% |
| CAC (blended) | $45,000 |
