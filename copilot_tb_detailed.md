# Trial Balance Adjustment with Calculation Breakdown - Copilot Prompt

## Production-Grade Prompt for Copilot

```
I need you to process a Trial Balance Excel file and create a comprehensive adjusted trial balance with detailed calculation breakdowns. Follow these instructions precisely:

**SOURCE FILE SPECIFICATIONS:**
- File name: TB_final_extracted.xlsx
- Sheet name: TB_final
- Row 1: Metadata (skip this row)
- Row 2: Actual column headers (use this as header)
- Data: Rows 3 onwards

**STEP 1: DATA LOADING & PREPARATION**

Load the file with the following parameters:
- Skip Row 1 (metadata)
- Use Row 2 as header row
- Preserve all columns
- Load all data rows

**STEP 2: IDENTIFY & VALIDATE REQUIRED COLUMNS**

Locate these exact columns (or close variations):
1. "Mar'25" – Original trial balance (base amount)
2. "Audit Adjustment Entries 22-23" – FY 2022-23 adjustments
3. "Audit Adjustment Entries 23-24" – FY 2023-24 adjustments
4. "Audit Adjustment Entries" – General audit adjustments

Also identify:
- "GL Code" (or similar: Account Code, Account Number)
- "GL Code Description" (or similar: Account Name, Description)

**Critical:** If any column name has slight variations (spaces, quotes, case), use fuzzy matching to identify it. Report any columns that cannot be found.

**STEP 3: DATA TYPE CONVERSION**

For the four adjustment columns:
- Convert to numeric data type
- Replace any missing, blank, or non-numeric values with 0 (zero)
- Preserve negative values as-is
- Maintain full decimal precision (no rounding)

**STEP 4: CALCULATE ADJUSTED TRIAL BALANCE**

Formula:
```
Adjusted Balance GAAP India = Mar'25 + Audit Adjustment Entries 22-23 + Audit Adjustment Entries 23-24 + Audit Adjustment Entries
```

Note: Do NOT include "Reclass entries for Mar'25" in this calculation.

**STEP 5: CREATE CALCULATION FORMULA TEXT**

For each row, create a text formula showing the calculation breakdown:

Format:
```
Mar'25(actual_value) + Audit Adjustment Entries 22-23(actual_value) + Audit Adjustment Entries 23-24(actual_value) + Audit Adjustment Entries(actual_value)
```

Examples:
- `Mar'25(10000) + Audit Adjustment Entries 22-23(500) + Audit Adjustment Entries 23-24(-200) + Audit Adjustment Entries(0)`
- `Mar'25(50000.50) + Audit Adjustment Entries 22-23(0) + Audit Adjustment Entries 23-24(0) + Audit Adjustment Entries(1500.25)`

**STEP 6: CREATE OUTPUT FILE WITH THREE SHEETS**

Generate a new Excel file with the following three sheets:

---

### **SHEET 1: "Calculation"**

Columns in exact order:
1. GL Code
2. GL Code Description
3. Mar'25
4. Audit Adjustment Entries 22-23
5. Audit Adjustment Entries 23-24
6. Audit Adjustment Entries
7. Adjusted Balance GAAP India
8. Calculation Formula

**Sheet Requirements:**
- Include all rows from source data
- Format numeric columns with appropriate decimal places
- Make "Calculation Formula" column wide enough to display full text
- Apply borders and basic formatting for readability
- Add a header row with bold formatting
- Consider adding conditional formatting to highlight negative values

---

### **SHEET 2: "Original TB"**

Content:
- Complete original data from TB_final sheet
- Include Row 1 (metadata)
- Include Row 2 (headers)
- Include all data rows
- Preserve all original columns
- No modifications to data

---

### **SHEET 3: "Adjusted TB GAAP India"**

Content:
- All original columns from the source data
- PLUS the new "Adjusted Balance GAAP India" column (as last column)
- Skip Row 1 (metadata) - start directly with headers
- Include all data rows

---

**STEP 7: OUTPUT FILE SPECIFICATIONS**

- File format: .xlsx (Excel 2010+)
- File name: TB_Adjusted_GAAP_India_Detailed_[YYYYMMDD_HHMMSS].xlsx
- Sheet order: Calculation, Original TB, Adjusted TB GAAP India
- Apply auto-fit to all columns for readability
- Freeze header rows in all sheets (Row 1 in Calculation and Adjusted TB, Row 2 in Original TB)
- Apply number formatting: #,##0.00 for all numeric columns

**STEP 8: VALIDATION & REPORTING**

After processing, provide a summary report:

1. **Data Statistics:**
   - Total rows processed: [number]
   - Rows with adjustments: [count of rows where sum of adjustments ≠ 0]
   - Rows with negative adjusted balance: [count]

2. **Column Verification:**
   - List all four source columns found (confirm exact names)
   - Report any missing or substituted columns

3. **Calculation Samples:**
   Show first 5 rows with:
   - GL Code
   - Each component value
   - Final adjusted balance
   - Formula text

4. **Data Quality Checks:**
   - Any rows with all zeros in adjustment columns
   - Any unusually large adjustments (>1,000,000 or <-1,000,000)
   - Any rows where adjusted balance is significantly different from Mar'25 (>50% change)

5. **File Confirmation:**
   - Confirm all three sheets created successfully
   - Confirm file is ready for download
   - Provide file size

**ERROR HANDLING RULES:**

1. If GL Code or GL Code Description columns are missing:
   - Use the first two columns as substitutes
   - Report this substitution clearly

2. If any of the four calculation columns are missing:
   - Proceed with available columns
   - Set missing columns to 0
   - Report which columns were missing

3. If column names don't match exactly:
   - Attempt fuzzy matching (ignore spaces, case, quotes)
   - Report the actual column names used

4. If data type conversion fails for specific cells:
   - Convert those cells to 0
   - Log row numbers where this occurred

5. If original file cannot be loaded:
   - Report specific error message
   - Suggest troubleshooting steps

**QUALITY ASSURANCE:**

Before finalizing, verify:
- [ ] All three sheets present and correctly named
- [ ] Calculation sheet has exactly 8 columns
- [ ] Formula text correctly shows all four components with values
- [ ] Adjusted Balance equals sum of four components (spot check 10 random rows)
- [ ] Original TB preserves metadata row
- [ ] Adjusted TB GAAP India has new column as last column
- [ ] No #DIV/0!, #VALUE!, or #REF! errors in any cell
- [ ] File size is reasonable (< 50MB typically)
- [ ] File downloads without errors

Please proceed with the processing and provide the detailed validation report along with the downloadable file.
```