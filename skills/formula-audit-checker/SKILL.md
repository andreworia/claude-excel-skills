---
name: formula-audit-checker
description: Provides a systematic audit checklist for Excel model formula logic, hardcoded values, circular references, and error handling, with step-by-step instructions for using Excel's native auditing tools.
---

# Formula Audit Checker

## When to use
Use this skill when reviewing an Excel model that was built by someone else, or verifying your own model before sharing it with a client or senior stakeholder. Trigger it when a model has been flagged for quality review, when a version handover is happening, or before a model is used to make a significant financial decision. It is most valuable as a pre-submission quality gate: running this audit before a model is presented to a board, an investment committee, or a client.

## What it does
Provides a systematic, step-by-step model audit process covering: Excel's native formula auditing tools (Trace Precedents, Trace Dependents, Error Checking), detection and correction of hardcoded values in formulas, identification of circular references, consistent IFERROR wrapping, and documentation of audit findings in a review log. The output is a completed audit log and a list of prioritized issues to fix.

## Method

1. **Save a working copy before auditing.** Before making any changes, save the model as a new file with "_Audit_YYYY-MM-DD" appended to the name. All edits during the audit should be made on this copy. This preserves the original state for comparison.

2. **Run the Error Checking tool.** Go to Formulas > Error Checking. Excel will step through all cells that contain errors (#N/A, #REF!, #DIV/0!, #VALUE!, #NAME?) and display the error and the formula. For each error: diagnose the cause (missing lookup key, deleted source cell, wrong data type, missing named range), fix it, and document it in the audit log with: cell reference, error type, cause, fix applied, severity (Critical, Major, Minor).

3. **Find and resolve circular references.** Go to Formulas > Error Checking > Circular References. Excel will list any cells with circular dependencies. Circular references are almost always an error in a financial model (except in iterative calculation scenarios, which are rare and must be explicitly intentional). For each circular reference: trace the dependency chain, identify the loop, and break it by restructuring the formula logic. Document each circular reference found.

4. **Use Trace Precedents to verify formula logic.** For every major calculation row (revenue, EBITDA, FCF, equity value, IRR), click the cell and press Ctrl+[ or go to Formulas > Trace Precedents. This draws arrows showing which cells feed this formula. Verify: (a) the formula is pulling from the correct tabs (Inputs tab for assumptions, Data tab for references, correct Calc tab for intermediate results), (b) no unexpected cells are being referenced, (c) the formula does not reference cells that should be on a different tab.

5. **Use Trace Dependents to check what a cell feeds.** For key output cells on Calc tabs, use Formulas > Trace Dependents to see which downstream cells reference this output. This confirms: (a) that output cells are being picked up by the Outputs tab correctly, (b) that no unexpected downstream formulas are referencing a cell that should be a local intermediate.

6. **Find all hardcoded numbers in formula cells.** This is the most important audit step for model integrity. All hardcoded numbers (except on the Inputs tab) are model errors because they cannot be easily changed without opening each formula.

   Step: Press Ctrl+H (Find and Replace). Click Options. Set "Look in" to Formulas. Search for specific numbers to check (try "0.2", "0.3", "0.15", "1000", "12"). Any formula containing these numbers outside of the Inputs tab should be investigated.

   Better approach: Use Excel's Go To Special feature (Ctrl+G > Special > Constants > Numbers). This selects all cells containing hardcoded numbers. Review: any such cell on a Calc tab that is not on the Inputs tab should be flagged.

   For each hardcoded number found in a Calc formula: move the assumption to the Inputs tab, give it a label, apply blue fill, and replace the hardcode with a cell reference to the Inputs tab.

7. **Audit for IFERROR consistency.** Review all lookup formulas (VLOOKUP, INDEX-MATCH, XLOOKUP, MATCH) to verify they are wrapped in IFERROR. An unwrapped lookup that returns #N/A can propagate through downstream formulas silently, producing incorrect results.

   To find unprotected lookups: Ctrl+F > search for "=VLOOKUP", "=INDEX(", "=XLOOKUP" without the "IFERROR(" prefix. For each unprotected lookup: wrap in IFERROR(formula, "Check: [description] not found") or IFERROR(formula, 0) depending on the context. Document each lookup that was wrapped.

8. **Check named range integrity.** Go to Formulas > Name Manager. For every named range: verify it refers to the correct cell or range (no #REF! in the "Refers to" column), verify the name is still used in the model (search for the name in formulas using Ctrl+F), and delete any orphaned named ranges (names that no longer have a valid reference or are no longer used).

9. **Verify the sign convention.** A common model error is an inconsistent sign convention -- some items positive, some negative, with no stated rule. Audit: all income statement items (revenue, gross profit, EBITDA) should be positive numbers. Costs may be positive (with subtraction in the formula) or negative (with addition). Pick one convention and apply it consistently. Flag any row where the sign appears inconsistent with surrounding rows.

10. **Check the sensitivity tables for correct input cell references.** In any Excel Data Table, the Row input cell and Column input cell must reference the exact cells on the Inputs tab that are being varied. A common error: the input cells reference a different cell than the one actually used in the model calculations. To verify: manually change the Row input cell value and check that the model output changes as expected. Then run the Data Table and verify the results change accordingly.

11. **Audit the formula complexity.** Long nested IF formulas (more than 2 levels) are an audit risk because they are hard to read, hard to debug, and prone to logic errors. Flag any formula longer than 150 characters or containing more than 2 levels of nesting. For each flagged formula: break it into intermediate calculation rows, name each intermediate step clearly, and replace the nested formula with a cleaner step-by-step sequence.

12. **Complete the audit log.** Record all findings in a structured audit log table on the Documentation tab or in a separate Audit Log tab:

    Finding ID | Tab | Cell | Issue Type | Severity | Description | Status (Open/Fixed) | Fixed by | Date fixed.
    
    Issue types: Hardcoded value, Circular reference, Formula error, IFERROR missing, Sign inconsistency, Named range broken, Sensitivity table error, Formula complexity.
    
    Severity: Critical (will produce wrong results in current state), Major (will produce wrong results in certain conditions), Minor (does not affect results but violates conventions).
    
    Summarize at the top of the log: total findings, critical count, major count, minor count, and overall audit status (Pass if zero Critical, Conditional Pass if only Minor, Fail if any Critical or Major unresolved).

## Inputs

- The Excel model to be audited (tab names, description of purpose, and where key outputs live)
- Any known issues or concerns flagged by the model builder
- The audience for the model (determines how rigorous the audit needs to be)
- The deadline for completing the audit

## Output format

A complete audit specification with:

1. **Audit checklist**: All 12 audit steps as a verifiable checklist with the Excel navigation path for each.
2. **Hardcode detection instructions**: Step-by-step Ctrl+H and Go To Special approach.
3. **IFERROR audit instructions**: The search string to find unprotected lookups.
4. **Circular reference resolution steps**: How to trace and break a circular dependency chain.
5. **Audit log table structure**: Column headers and definitions for the audit finding log.
6. **Severity classification guide**: What constitutes Critical, Major, and Minor findings.
7. **Audit summary template**: The summary block for the top of the audit log.

## Example

**User says:** "I've inherited a 15-tab DCF model from a departing analyst. The model was used to value an acquisition target. I need to audit it before presenting to the investment committee next week."

**Claude responds:**

**Priority audit sequence for an investment committee deadline:**

Step 1 (30 minutes): Run Error Checking (Formulas > Error Checking). Any #REF! in the equity bridge or WACC calculation is Critical -- fix before proceeding. Document all errors found.

Step 2 (20 minutes): Check for circular references (Formulas > Error Checking > Circular References). A circular reference in a DCF or LBO model will produce wrong output silently if iterative calculations are enabled. If found: flag as Critical, diagnose, and fix.

Step 3 (45 minutes): Hardcode hunt across all Calc tabs. Use Go To Special > Constants > Numbers on each of the 10 Calc tabs. Flag every number on a Calc tab that is not on the Inputs tab. Common critical hardcodes in DCF models: tax rate embedded in the NOPAT formula, WACC embedded in the discount factor formula, number of shares hardcoded in the equity bridge.

Step 4 (20 minutes): Verify sensitivity table input cell references. Open each Data Table, manually change the row and column input cells, and confirm the output changes appropriately.

Step 5 (30 minutes): Review all INDEX, VLOOKUP, and MATCH formulas for IFERROR wrapping. Search: Ctrl+F for "=INDEX(" and "=VLOOKUP(" -- scan results for those lacking IFERROR.

**Audit log structure:**
| ID | Tab | Cell | Type | Severity | Description | Status |
| A001 | Val - DCF | F22 | Hardcode | Critical | Tax rate 0.25 hardcoded in NOPAT formula | Open |
| A002 | Val - Sensitivity | B2 | Sensitivity error | Major | Row input cell references wrong WACC cell | Open |
| A003 | Inputs | B14 | Sign | Minor | Working capital increase shown as positive (should be negative cash impact) | Open |

**Audit summary:** 3 findings (1 Critical, 1 Major, 1 Minor). Audit Status: FAIL. Do not present to investment committee until A001 and A002 are resolved.
