---
name: assumption-registry-builder
description: Creates a structured assumption register for any Excel model with columns for name, category, base value, source, Bull/Bear values, sensitivity rank, and owner, plus a formula for computing sensitivity rank from Data Table outputs.
---

# Assumption Registry Builder

## When to use
Use this skill when a model is complete and needs to be documented for handover, sign-off, or recurring use. Trigger it when a client or reviewer asks "where do your assumptions come from?", when a model needs to be formally approved before a capital allocation decision, when the model will be updated by multiple people over time, or when a business case requires a formal assumption sign-off log. It converts a working model into a fully documented, auditable asset.

## What it does
Produces a structured assumption register: a dedicated Assumptions tab listing every material assumption in the model, with columns for assumption name, category, base value, source and rationale, Bull and Bear values, sensitivity rank (impact on key output), and owner. Includes the formula for computing sensitivity rank using Data Table outputs, and a flag system for assumptions that require active monitoring.

## Method

1. **Define what constitutes a "material assumption."** Not every input in a model needs to be in the assumption register. Material assumptions are: (a) inputs with high sensitivity (top 50% of the tornado chart), (b) inputs that are genuinely uncertain (not based on contractual or regulatory certainty), and (c) inputs that, if wrong, would change the recommendation or decision. Assumptions that are highly certain (e.g., statutory tax rates, known contract prices) still belong in the register but with a "Low" sensitivity rank and "Regulatory/Contractual" source.

2. **Build the assumption register structure.** The Assumptions tab has the following column layout:

   Column A: Assumption ID (A001, A002, ... for easy cross-reference).
   Column B: Assumption Name (clear label, consistent with the Inputs tab label).
   Column C: Category (Revenue, Cost, Capital, Macro/Market, Operational, Strategic).
   Column D: Base Value (reference to the Inputs tab cell, not a hardcoded repeat).
   Column E: Unit (%, $M, x, #, days, etc.).
   Column F: Source/Rationale (text: where did this number come from? Management guidance, industry report, historical average, analyst consensus, benchmarking).
   Column G: Bull Value (the favorable assumption value in the Bull scenario).
   Column H: Bear Value (the conservative assumption value in the Bear scenario).
   Column I: Sensitivity Rank (High/Medium/Low or 1-10 numeric rank based on impact on key output).
   Column J: Impact on Key Output (the change in the key output when this assumption swings from Bear to Bull -- computed from the tornado analysis).
   Column K: Monitoring Flag (Active/Passive -- Active means this assumption should be revisited at each model refresh).
   Column L: Owner (the person responsible for validating and updating this assumption).
   Column M: Last Verified Date (when was this assumption last confirmed against reality).
   Column N: Notes (any additional context, planned changes, uncertainty flags).

3. **Populate the assumption register from the Inputs tab.** Go through the Inputs tab systematically. For each blue-fill input cell, create a corresponding row in the Assumptions tab. The Base Value column (D) should be a cell reference to the Inputs tab: ='Inputs'!$D$5. This ensures the register always shows the current model value, not a static copy.

4. **Write meaningful Source/Rationale entries.** This is the most important column for audit and sign-off purposes. Acceptable sources: "Management guidance, FY2024 budget presentation", "3-year historical average from audited financials (FY2021-FY2023)", "Industry benchmark: [sector] median from [data type]", "Analyst consensus from comparable public companies", "Contractual: supplier agreement dated [date]", "Estimated based on comparable transaction data." Unacceptable: "Assumed", "Estimate", or blank.

5. **Define the Bull and Bear values.** If a three-scenario model has been built, pull the Bull and Bear values directly from the Scenario Inputs tab: ='Scenario Inputs'!$E$5 (Bull) and ='Scenario Inputs'!$F$5 (Bear). If no scenario model exists, set Bull and Bear values based on the plus/minus range from the tornado analysis. Document the rationale for the range in the Notes column.

6. **Compute the sensitivity rank using Data Table outputs.** The sensitivity rank for each assumption measures its impact on the key model output when it swings from Bear to Bull value.

   If a tornado chart Data Table has been built (using the sensitivity-tornado skill), reference the impact values directly from that Data Table.

   Impact on Key Output formula: ='Tornado - Data Table'!F_row - 'Tornado - Data Table'!E_row (High impact minus Low impact for this variable).

   Sensitivity Rank (1-10, where 1 is most impactful): Use RANK function on the Impact column: =RANK(J5, $J$5:$J$50, 0) where J5 is the Impact column cell for this assumption. A rank of 1 means this is the single most impactful assumption.

   Sensitivity Category (High/Medium/Low): =IF(I5<=3,"High",IF(I5<=7,"Medium","Low")) where I5 is the numeric rank.

7. **Sort and organize the register.** Sort by Sensitivity Rank (ascending) so the most impactful assumptions are at the top. Within each rank tier, sort by Category. Add a frozen header row so the column names remain visible when scrolling. Add Excel AutoFilter so users can filter by Category, Owner, or Sensitivity rank.

8. **Add a summary section at the top.** Above the register body, add a summary block:

   Total assumptions in register: =COUNTA(A_range)-header rows.
   High sensitivity assumptions: =COUNTIF(SensitivityCategoryRange,"High").
   Active monitoring assumptions: =COUNTIF(MonitoringFlagRange,"Active").
   Assumptions with no source: =COUNTBLANK(SourceRange).
   Last full review date: [manually entered date cell].
   Reviewed by: [manually entered name cell].

9. **Apply formatting to aid readability.** Apply row banding (alternating light grey and white rows). Apply conditional formatting to the Sensitivity Category column: High = orange fill, Medium = yellow fill, Low = no fill. Apply conditional formatting to the Monitoring Flag column: Active = blue fill. Apply conditional formatting to the Last Verified Date column: red fill if date is more than 90 days ago (=TODAY()-M_cell>90).

10. **Define the monitoring protocol.** For each "Active" monitoring assumption, document: who is responsible for updating it, how often it should be reviewed, and what external data source should be checked. This converts the register into a living document. Add a scheduled review date column: the date by which this assumption should next be checked.

11. **Link the register to the model sign-off process.** Add a "Signed off by" column and a "Sign-off date" column. For a formal approval process, each assumption owner should sign off on their assumptions before the model is presented. The summary block at the top should flag any unsigned assumptions: =COUNTBLANK(SignOffRange). The model should not be presented to a board or investment committee if this count is greater than zero.

12. **Maintain version history of the register.** When the model is updated, update the assumption register. Add a "Previous Base Value" column and a "Change vs. Prior Version" column. This creates an audit trail showing how assumptions have evolved across model versions. The Documentation tab should record each version's change summary.

## Inputs

- List of all model inputs from the Inputs tab (names, values, units)
- Source or rationale for each assumption (management guidance, historical data, benchmarks)
- Bull and Bear values for each assumption (from scenario model or tornado analysis)
- Impact on key output for each assumption (from tornado chart, if available)
- Owner for each assumption (person responsible for validating it)
- Key output metric for sensitivity ranking (equity value, EBITDA, IRR, etc.)

## Output format

A complete assumption register specification with:

1. **Register column structure**: Full column list (A through N) with data type, formula vs. manual entry, and width.
2. **Source/Rationale quality guide**: Examples of acceptable and unacceptable source entries.
3. **Sensitivity rank formula**: RANK function formula with cell references explained.
4. **Sensitivity category formula**: IF formula converting numeric rank to High/Medium/Low.
5. **Summary block formulas**: COUNTA, COUNTIF, COUNTBLANK formulas for the summary section.
6. **Conditional formatting rules**: Sensitivity, Monitoring Flag, and Last Verified Date rules.
7. **Monitoring protocol template**: Fields for each Active assumption.
8. **Sign-off process**: How the register is used for formal model approval.

## Example

**User says:** "I have a SaaS revenue model with the following key assumptions: ARR growth rate (25%, base), churn rate (15%, base), average contract value ($22K), sales headcount additions (8/year), CAC ($35K). Key output: Year 5 ARR. I need to build the assumption register."

**Claude responds:**

**Assumption register (excerpt):**

| ID | Assumption | Category | Base Value | Unit | Source / Rationale | Bull | Bear | Rank | Impact on ARR Y5 | Monitor | Owner |
| A001 | ARR Growth Rate | Revenue | 25% | % | Historical 3-year CAGR (FY22-FY24): 27%; slightly discounted for maturation | 32% | 18% | 1 | +$18M / -$14M | Active | VP Sales |
| A002 | Annual Churn Rate | Revenue | 15% | % | FY2024 actual churn: 16.2%; improving to 15% reflects CSM investment program | 10% | 22% | 2 | -$12M / +$9M | Active | VP Customer Success |
| A003 | Average Contract Value | Revenue | $22K | $ | Average of last 24 months closed-won deals (CRM export); excludes 2 outlier enterprise deals | $26K | $18K | 3 | +$8M / -$8M | Active | VP Sales |
| A004 | Sales Headcount Additions | Cost | 8 | FTEs/year | Board-approved headcount plan (FY2025 budget) | 10 | 5 | 5 | +$3M / -$4M | Passive | CFO |
| A005 | CAC (blended) | Cost | $35K | $ | FY2024 blended CAC (total sales & marketing spend / new logos): $33.2K; slightly increased for wage inflation | $28K | $45K | 4 | +$5M / -$6M | Active | CMO |

**Sensitivity rank formula:**
=RANK(J5,$J$5:$J$20,0) -- ranks assumptions by absolute impact on Year 5 ARR, with 1 = highest impact.

**Sensitivity category:**
=IF(I5<=2,"High",IF(I5<=5,"Medium","Low"))
Result: A001, A002 = High; A003, A004, A005 = Medium.

**Summary block:**
Total assumptions: 5
High sensitivity: 2 (ARR growth rate, churn rate)
Active monitoring: 4
Assumptions with no source: =COUNTBLANK($F$5:$F$20) = 0 (all sourced)

**Monitoring note for A002 (Churn Rate):**
Owner: VP Customer Success. Review frequency: Monthly (updated from CRM cohort report). External benchmark: Track against SaaS Capital industry churn benchmarks quarterly. Next review: end of current month. Flag if actual churn exceeds 17% for 2 consecutive months -- triggers Bear scenario re-assessment.
