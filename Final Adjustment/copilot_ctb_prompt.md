# Consolidated Trial Balance Generator - Production Prompt

## Objective
Generate a downloadable consolidated trial balance Excel file combining 6 entity adjusted trial balances with the master unadjusted trial balance file.

---

## Input Files (7 files total)

### Master File (1 file)
**Filename:** `Trial_Balance_Cleaned.xlsx`

**Columns Required:**
- `G/L Acct/BP Code` - GL account code (primary key)
- `Name` - Account name
- `Local Currency - Ringgit Malaysia - Balance` - Opening balance in MYR

### Entity Adjusted Files (6 files)
User will upload these specific files:
1. `Adjusted_Trial_Balance_ENCICP_20251015_161817.xlsx`
2. `Adjusted_Trial_Balance_ICIAEA_20251015_164741.xlsx`
3. `Adjusted_Trial_Balance_MANA_20251015_181245.xlsx`
4. `Adjusted_Trial_Balance_RBK_20251015_173230.xlsx`
5. `Adjusted_Trial_Balance_RECA_20251015_170255.xlsx`
6. `Adjusted_Trial_Balance_RFWD_20251015_174257.xlsx`

**Each file must contain:**
- Sheet name: "Adjusted Trial Balance"
- Columns: `GL Code`, `Account Name`, `Balance Before Adjustment (MYR)`, `Net Adjustment`, `Adjusted Balance (MYR)`

---

## Processing Steps

### Step 1: Load Files
1. Load `Trial_Balance_Cleaned.xlsx` as master source
2. Load all 6 adjusted trial balance files using exact filenames provided
3. Access "Adjusted Trial Balance" sheet in each adjusted file
4. Validate all required columns exist

**Error Handling:** If any file missing or columns not found, report specific error and halt processing.

### Step 2: Extract Entity Codes
Extract entity codes from filenames for column headers:
- ENCICP → "Net Adjustment-ENCICP"
- ICIAEA → "Net Adjustment-ICIAEA"  
- MANA → "Net Adjustment-MANA"
- RBK → "Net Adjustment-RBK"
- RECA → "Net Adjustment-RECA"
- RFWD → "Net Adjustment-RFWD"

### Step 3: Match GL Codes and Extract Adjustments
1. Create master GL code list from `Trial_Balance_Cleaned.xlsx`
2. For each GL code in master:
   - Match `G/L Acct/BP Code` with `GL Code` in each entity file
   - Extract `Net Adjustment` value (use 0.00 if GL code not found in entity file)
3. Track unmatched GL codes (present in entity files but not in master)

### Step 4: Calculate Consolidated Balances
For each GL Code:
```
Adjusted Balance (MYR) = Balance Before Adjustment
                       + Net Adjustment-ENCICP
                       + Net Adjustment-ICIAEA
                       + Net Adjustment-MANA
                       + Net Adjustment-RBK
                       + Net Adjustment-RECA
                       + Net Adjustment-RFWD
```

**Rules:**
- Round all values to 2 decimal places
- Default missing adjustments to 0.00
- Preserve debit/credit signs

---

## Output File Generation

### Filename Format
`Consolidated_Trial_Balance_YYYYMMDD_HHMMSS.xlsx`

Use current system date/time. Example: `Consolidated_Trial_Balance_20251016_143052.xlsx`

### Sheet 1: "Consolidated Trial Balance"

**Column Structure:**
| A: GL Code | B: Account Name | C: Balance Before Adjustment (MYR) | D: Net Adjustment-ENCICP | E: Net Adjustment-ICIAEA | F: Net Adjustment-MANA | G: Net Adjustment-RBK | H: Net Adjustment-RECA | I: Net Adjustment-RFWD | J: Adjusted Balance (MYR) |

**Formatting Requirements:**
- Row 1 (Headers): Bold, background RGB(68,114,196), white text, AutoFilter enabled
- Freeze top row (Freeze Panes at A2)
- Monetary columns (C-J): Number format `#,##0.00`
- Text columns (A-B): Left-aligned
- Last row: TOTAL with SUM formulas for columns C-J, bold formatting
- Auto-fit all column widths
- Add borders to all cells

### Sheet 2: "Adjustment Summary"

**Section 1: Entity Statistics** (Start Row 1)
| Entity | GL Codes Adjusted | Total Net Adjustment (MYR) | Unmatched GL Codes |
|--------|------------------|----------------------------|-------------------|
| ENCICP | [count non-zero] | [sum] | [list if any] |
| ICIAEA | [count non-zero] | [sum] | [list if any] |
| MANA | [count non-zero] | [sum] | [list if any] |
| RBK | [count non-zero] | [sum] | [list if any] |
| RECA | [count non-zero] | [sum] | [list if any] |
| RFWD | [count non-zero] | [sum] | [list if any] |
| **GRAND TOTAL** | **[total]** | **[sum all]** | |

**Section 2: Validation Results** (Start Row 12)
```
Total GL Codes Processed: [count]
Balance Before Adjustment: [sum] MYR
Total Net Adjustments: [sum] MYR
Adjusted Balance: [sum] MYR
Debit-Credit Variance: [difference] MYR ✓
```

**Section 3: Unmatched GL Codes Detail** (Start Row 20, if any exist)
| Entity | GL Code | Account Name | Net Adjustment (MYR) |

---

## Validation Checks

### Critical Validations (Halt if Failed)
✓ All 7 files loaded successfully  
✓ "Adjusted Trial Balance" sheet exists in all 6 entity files  
✓ Required columns present in all files  
✓ No duplicate GL codes within master file  
✓ Debit-Credit balance variance ≤ 0.01 MYR  
✓ All master GL codes present in output  
✓ Calculation accuracy verified for each GL code  

### Warning Validations (Report but Continue)
⚠ GL codes in entity files not in master (orphaned adjustments)  
⚠ GL codes with zero adjustments across all entities  
⚠ Balance changes > 50% variance  

---

## Output Delivery

**CRITICAL:** Generate the Excel file and make it downloadable immediately.

### Success Report Format
```
✓ Consolidated Trial Balance Generated Successfully

📊 File: Consolidated_Trial_Balance_20251016_143052.xlsx
📥 Status: Ready for Download

Processing Summary:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Files Processed:
  ✓ Trial_Balance_Cleaned.xlsx
  ✓ Adjusted_Trial_Balance_ENCICP_20251015_161817.xlsx
  ✓ Adjusted_Trial_Balance_ICIAEA_20251015_164741.xlsx
  ✓ Adjusted_Trial_Balance_MANA_20251015_181245.xlsx
  ✓ Adjusted_Trial_Balance_RBK_20251015_173230.xlsx
  ✓ Adjusted_Trial_Balance_RECA_20251015_170255.xlsx
  ✓ Adjusted_Trial_Balance_RFWD_20251015_174257.xlsx

Financial Summary:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total GL Codes: [count]
Balance Before Adjustment: [amount] MYR
Total Adjustments: [amount] MYR
Adjusted Balance: [amount] MYR
Variance: [amount] MYR ✓

Entity Adjustments:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ENCICP: [count] codes, [amount] MYR
ICIAEA: [count] codes, [amount] MYR
MANA:   [count] codes, [amount] MYR
RBK:    [count] codes, [amount] MYR
RECA:   [count] codes, [amount] MYR
RFWD:   [count] codes, [amount] MYR

Validation: ✓ All checks passed
Warnings: [count] (see Adjustment Summary sheet)

🔽 Download your consolidated trial balance file now
```

### Error Report Format (if validation fails)
```
❌ Consolidation Failed

Error Details:
[Specific error message with GL codes/filenames]

Action Required:
[Specific fix instructions]

No output file generated.
```

---

## Technical Specifications

- **Processing Time:** 30-60 seconds for typical datasets (5,000-10,000 GL codes)
- **Numeric Precision:** Use decimal type to prevent floating-point errors
- **Date Format:** YYYYMMDD_HHMMSS (e.g., 20251016_143052)
- **Excel Version:** Compatible with Excel 2016 and later
- **File Size:** Optimized for datasets up to 20,000 GL codes

**Version:** 1.0 | Last Updated: 2025-10-16