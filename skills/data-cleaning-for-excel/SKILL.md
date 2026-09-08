---
name: data-cleaning-for-excel
description: Turns pasted or exported data with mixed formats, blank rows, or duplicates into a clean, consistently formatted range ready for analysis. Use it right after pasting raw data into a worksheet.
---

# Data Cleaning for Excel

## When to use
Use this immediately after pasting or importing raw data, a CSV export, a copy from another
system, or a table copied out of a document, before you build a pivot table or a formula on
top of it.

## Instructions
1. Identify inconsistent formats within the same column (text numbers next to real numbers, mixed date formats, inconsistent casing).
2. Remove or flag exact duplicate rows, stating the rule used to define a duplicate.
3. Standardize blank cells: distinguish a true zero from a missing value and handle each consistently.
4. Trim stray whitespace and normalize inconsistent category labels that clearly refer to the same thing.
5. Convert text-formatted numbers and dates to their proper Excel types so formulas can use them.
6. Report what was changed, row by row where practical, so the cleaning is auditable.

## Example prompts
- "Use the data-cleaning-for-excel skill on this pasted CSV before I pivot it."
- "Clean up this exported customer list, the dates are in three different formats."

## Output
A cleaned, consistently formatted range, plus a short change log describing what was standardized, removed, or converted.
