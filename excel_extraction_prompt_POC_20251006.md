# Financial Data Extraction Prompt for Microsoft Copilot

## Objective
Extract financial data from specified Excel tab with **100% fidelity** - NO rounding, truncation, or data modification. **Create a downloadable Excel file** as output containing the extracted data.

---

## Input Requirements

**Source File**: "Draft IND AS  Financials_ FY 24-25.xlsx"  
**Tab Name**: "TB 25"  
**Header Row**: "4"

### Column Headers to Extract (in order):
1. G/L Acct/BP Code
2. Name
3. Local Currency - Indian Rupee - OB
4. Local Currency - Indian Rupee - Debit
5. Local Currency - Indian Rupee - Credit
6. Local Currency - Indian Rupee - Balance
7. Jeev TB
8. Elimination entry
9. Closing Balance
10. Audit entries
11. Net Closing Balance
12. Function
13. Grouping
14. Sub-Grouping

**Note**: If duplicate column names exist, rename as [ColumnName]_1, [ColumnName]_2

---

## CRITICAL Extraction Rules - NO EXCEPTIONS

### Data Fidelity Requirements
- **NO rounding** of numeric values
- **NO truncation** of decimal places
- **NO data type conversions**
- **NO removal** of leading zeros
- **NO format standardization**
- Preserve exact precision as stored in Excel (not just displayed values)
- If source has 15 decimals, output must have 15 decimals
- Extract formula results as values, preserve exact numbers

### Complete Data Extraction
- Extract ALL rows from header+1 until first completely empty row
- Include rows with partial data (some empty cells)
- Preserve empty cells as empty (NOT as 0, N/A, or null)
- Do NOT skip, filter, or exclude any data rows

### Format Preservation
- Preserve currency symbols, thousand separators if present
- Preserve date formats exactly as they appear
- Maintain all significant digits and decimal precision
- Keep leading/trailing spaces in text fields if present

---

## Output Specifications

**IMPORTANT**: Generate and provide a **downloadable Excel file** (.xlsx format) as the final output.

**Output File Name**: [SourceFileName]_Extracted_[YYYYMMDD_HHMMSS].xlsx

### Tab 1: "Extracted_Data"
- Column headers in Row 1 (exactly as specified)
- All extracted data from Row 2 onwards
- Formatting:
  - Bold header row, freeze panes at Row 2
  - Auto-fit columns, enable filters
  - **CRITICAL**: Set numeric columns to "Number" format with 30 decimal places
  - Right-align numbers, left-align text
  - Format G/L Acct/BP Code as TEXT to preserve leading zeros

**This tab must contain the actual extracted data ready for use.**

### Tab 2: "Validation_Report"

**Extraction Summary:**
- Source File, Tab Name, Timestamp
- Total Rows Extracted (excluding header)
- Total Columns Extracted
- Processing Status (Success/Failed)

**Column Validation Table:**
| Column Name | Found | Position | Data Type | Sample Value | Empty Count | Notes |
|-------------|-------|----------|-----------|--------------|-------------|-------|

**Data Quality Checks:**
- Duplicate headers detected: [Yes/No - list if any]
- Non-numeric values in currency columns: [Count and row numbers]
- Rows with all empty cells: [Count - these were skipped]
- Formula cells detected: [Count - extracted as values]
- Excel errors (#N/A, #VALUE!, etc.): [Count and cell references]
- Scientific notation detected: [Cell references if any]

**Other Findings:**
- Extra columns not in specification: [List]
- Footer/summary rows detected: [Row numbers]
- Missing required columns: [List]

---

## Error Handling

**Tab Not Found**: Stop processing. Report available tab names.

**Missing Columns**: Continue with available columns. List missing ones in validation report.

**No Data Rows**: Create output with headers only. Report warning.

**Excel Errors Present**: Extract error as-is. Document in validation report with cell references.

**Precision Loss Risk**: Flag in validation report with affected cell references.

---

## Data Type Expectations

- **G/L Acct/BP Code**: Text (preserve leading zeros)
- **Name, Function, Grouping, Sub-Grouping**: Text
- **All Currency Columns**: Numeric with full decimal precision
- **Empty Cells**: Keep empty (not 0 or null)

---

## Quality Checklist

Before completion, verify:
- All specified headers present or documented as missing
- Row count matches extraction range
- NO numeric rounding or truncation occurred
- Decimal precision matches source exactly
- Empty cells are truly empty
- NO data type conversions
- Column order matches specification
- Validation report is complete

---

## Special Instructions

1. **Currency Columns**: Use "Number" format (15+ decimals), NOT "Currency" format which may round
2. **Code Columns**: Use "Text" format to prevent Excel auto-formatting
3. **Large Numbers**: If exceeding Excel's 15 significant digit limit, document in validation
4. **Preserve Source Precision**: Extract exact values, not display-rounded values

---

## Example

**Input**: File: FinancialData_FY2024.xlsx, Tab: "Trial Balance - March", Header: Row 1

**Output**: FinancialData_FY2024_Extracted_20250106_153045.xlsx
- Tab 1: 1546 data rows with all 14 columns, exact precision preserved
- Tab 2: Validation report showing all columns found, 0 errors, 12 rows with empty Sub-Grouping

---

## Execute

Extract data per above requirements and **generate a downloadable Excel file** with the extracted data. Ensure:
1. **Zero modification** to values - exact precision
2. **Complete extraction** - all rows and columns
3. **Comprehensive validation** - document everything
4. **Downloadable output** - provide Excel file (.xlsx) for download

After extraction, provide:
- Download link for the output Excel file
- Summary: total rows and columns extracted
- Any warnings or issues encountered