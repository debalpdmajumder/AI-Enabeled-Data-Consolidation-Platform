# Trial Balance Adjustment Process - Copilot Prompt

## Overview
Process unadjusted trial balance data and apply adjustments to generate an accurate adjusted trial balance report with proper GL Code formatting.

---

## Required Inputs

1. **Unadjusted Trial Balance File:** "Trial_Balance_Cleaned.xlsx"
2. **Adjustment File:** "Reclassification_Entry.xlsx"
3. **Adjustment Type:** "Reclassification Entries – GAAP-compliant adjustments to correct account classification for accurate financial reporting"
4. **Output File Name:** "Adjusted_Trial_Balance_RECA_YYYYMMDD_HHMMSS.xlsx"
   - Format: Year(4) Month(2) Day(2) underscore Hour(2) Minute(2) Second(2)
   - Example: "Adjusted_Trial_Balance_RECA_20241015_143052.xlsx"

---

## Processing Steps

### Step 1: Data Validation and GL Code Format Enforcement

**CRITICAL GL CODE HANDLING:**
- **GL Codes MUST be TEXT/STRING throughout entire process**
- Convert to string immediately: `df['GL Code'].astype(str).str.strip()`
- Preserve leading zeros (e.g., "0001234" must stay "0001234", not "1234")
- Prevent Excel auto-conversion to numeric (no "1000001.0")
- Strip whitespace from both ends

**Actions:**
1. Open both files, verify GL Code columns exist
2. **Convert all GL Codes to string format immediately**
3. Check format consistency across files
4. Confirm Debit and Credit columns exist in Adjustment file
5. Validate no GL Codes stored as float (e.g., no ".0" endings)

**Report:**
- Total GL Codes in each file
- Total adjustment entries
- Format inconsistencies found
- Confirmation: "All GL Codes converted to string format"

---

### Step 2: Match GL Codes (String-Based)

**Actions:**
1. Create master GL Code list from Unadjusted Trial Balance (as strings)
2. Ensure both source GL Codes are strings before matching
3. Perform exact string match: `unadjusted_gl.isin(adjustment_gl)`
4. Strip whitespace before comparison
5. Flag unmatched GL Codes

**Report:**
- Matched GL Codes count
- Unmatched GL Codes list with recommendations

---

### Step 3: Filter Non-Zero Adjustments

**Actions:**
1. Extract rows where Debit OR Credit is non-zero
2. Exclude zero/blank entries
3. Keep GL Code (as string), Name, Debit, Credit columns
4. Verify GL Code remains string after filtering

**Report:**
- Entries before/after filtering
- Sample of 3-5 filtered entries

---

### Step 4: Apply Adjustments

**Formula:**
```
Adjusted Balance = Original Balance + Total Debits - Total Credits
```

**Process:**
1. Group by GL Code (string-based grouping)
2. Sum Debit adjustments per GL Code
3. Sum Credit adjustments per GL Code
4. Calculate adjusted balance, round to 2 decimals
5. Maintain GL Code as string throughout

**Report per GL Code:**
- GL Code (text with leading zeros)
- Account Name
- Original Balance
- Total Debits/Credits
- Adjusted Balance

---

### Step 5: Generate Downloadable Excel File

**CRITICAL: Create actual downloadable .xlsx file**

**File Naming:**
- "Adjusted_Trial_Balance_RECA_YYYYMMDD_HHMMSS.xlsx"
- Use current system date/time

**GL CODE EXCEL FORMATTING (MANDATORY):**
When writing Excel file:
1. **Format GL Code column as TEXT (@) before writing**
2. Prevent numeric conversion in Excel
3. Methods:
   - Set column format: `worksheet.column_dimensions['A'].number_format = '@'`
   - OR prepend quote: `df['GL Code'] = "'" + df['GL Code'].astype(str)`

**Main Worksheet: "Adjusted Trial Balance"**

| Column | Header | Format |
|--------|--------|--------|
| A | GL Code | TEXT (@) - MANDATORY |
| B | Account Name | Text |
| C | Balance Before Adjustment (MYR) | #,##0.00 |
| D | [Adjustment Type] - Debit | #,##0.00 |
| E | [Adjustment Type] - Credit | #,##0.00 |
| F | Net Adjustment | #,##0.00 |
| G | Adjusted Balance (MYR) | #,##0.00 |

**Formatting Requirements:**
- Column A (GL Code): TEXT format, left-aligned
- Monetary columns: Thousand separators, 2 decimals
- Headers: Bold, white background, black text
- Enable AutoFilter on header row
- Freeze top row
- Add TOTAL row at bottom (SUM for monetary columns)
- Auto-fit column widths

**Second Worksheet: "Adjustment Summary"**

Include:
- Adjustment Type
- Date processed (with timestamp)
- Total GL Codes adjusted
- Net adjustment amount
- Unmatched GL Codes list (as text)

**Completion:**
- Create Excel file with datetime stamp
- Verify GL Code column formatted as TEXT
- Provide download link/attachment
- Confirm: "✓ Excel file created: [filename] - ready for download"
- Confirm: "✓ GL Codes preserved as text with leading zeros intact"

---

## Validation Checks

**Check 0: GL Code Format**
- ✓ All GL Codes stored as string
- ✓ No ".0" endings (float indicators)
- ✓ Leading zeros preserved
- ✓ Excel output column A formatted as TEXT

**Check 1: Debit-Credit Balance**
- ✓ Total Debits = Total Credits (before/after)

**Check 2: Adjustment Sum Match**
- ✓ Sum of adjustments matches source

**Check 3: Data Completeness**
- ✓ All original GL Codes present
- ✓ No duplicate GL Codes

**Check 4: Cell Formatting**
- ✓ No blank critical cells
- ✓ Proper number formatting

**Validation Report:**
- ✓ Pass or ✗ Fail for each check
- Explanations if failed
- Corrective actions

---

## Error Handling

**GL Code Format Errors:**
- Detect float format (e.g., "1000001.0")
- Auto-convert to string, strip decimal
- Report conversions made

**Missing GL Codes:**
- List unmatched codes
- Options: skip, create new rows, or abort

**Format Inconsistencies:**
- Show examples
- Suggest standardization

**Imbalanced Adjustments:**
- Report imbalance amount
- Ask to proceed or abort

---

## Final Deliverable

**1. Downloadable Excel File:**
- Filename: "Adjusted_Trial_Balance_RECA_YYYYMMDD_HHMMSS.xlsx"
- GL Code column formatted as TEXT (@)
- Both worksheets included
- Download link provided

**2. Processing Summary:**
```
✓ Trial Balance Adjustment Complete

📊 File: Adjusted_Trial_Balance_RBK_20241015_143052.xlsx
📥 Status: Ready for Download

Summary:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total GL Codes Processed: [count]
GL Codes Adjusted: [count]
Net Adjustment Impact: [amount] MYR
Original Balance: [amount] MYR
Adjusted Balance: [amount] MYR

Validation Results:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✓ GL Codes preserved as text
✓ Debit-Credit balance verified
✓ All adjustments applied
✓ No duplicate entries
[Additional checks...]

🔽 Download your file now
```

**3. Next Steps Recommendations**

---

## Technical Notes

- **GL Code Handling:** Always string/text type, never numeric
- **Processing Time:** 30-60 seconds typical
- **Excel Compatibility:** 2016 and later
- **Numeric Precision:** Decimal type for calculations, round to 2 places
- **Date Format:** YYYYMMDD_HHMMSS

---

## Start Confirmation

Respond: "Ready to process trial balance adjustments. GL Codes will be preserved as text format throughout. Proceeding with Step 1: Data Validation and GL Code Format Enforcement."

If inputs missing, list required items.

**Version:** 1.0 | Updated: 2025-10-16