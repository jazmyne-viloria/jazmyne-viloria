# Scenario 33 — FX Hedging Workbook Build Audit

| Field | Details |
|---|---|
| Author | Jazmyne Viloria |
| Course | FIN 321 — International Finance and Securities |
| Stage | Stage 3 — AI-Assisted Build + Audit |
| Scenario | Scenario 33 — Mining Shovels |
| Audit Date | November 13, 2026 |
| Workbook | `capabilities/fx-hedging/model.xlsx` |
| Specification | `capabilities/fx-hedging/spec.md` |

---

## 1. Build Overview

The Stage 3 workbook was generated using the Stage 2 technical specification for Scenario 33 — Mining Shovels.

The workbook compares the unhedged EUR receivable with forward, money-market, put-option, and call-option hedging strategies.

The generated workbook was reviewed against the specification's requirements for named ranges, formula-driven calculations, worksheet architecture, formatting, sensitivity analysis, and validation checks.

## 2. Workbook Structure

| Worksheet | Purpose | Audit Result |
|---|---|---|
| Cover | Scenario overview and data provenance | Pending audit |
| Legend-Key | Workbook formatting conventions | Pending audit |
| Inputs | Ten standardized named-range inputs | Pending audit |
| Forward Hedge | Forward hedge proceeds | Pending audit |
| Money Market Hedge | Three-step money-market hedge | Pending audit |
| Option Hedge | Put and call calculations | Pending audit |
| Sensitivity | Sensitivity table and chart | Pending audit |
| Notes & Assumptions | Sources, assumptions, and limitations | Pending audit |

## 3. Audit Findings

### Finding 1 — Named Range Verification

**What I checked:**

[Document whether all ten named ranges exist and refer to their correct input cells.]

**What I found:**

[Document the actual result.]

**Action taken:**

[Document any corrections or explain how the correct result was verified.]

### Finding 2 — Formula Integrity

**What I checked:**

[Inspect calculated cells to determine whether formulas reference named ranges instead of using hard-coded calculated values.]

**What I found:**

[Document the actual result and any formula defects.]

**Action taken:**

[Document any corrections or explain how the correct result was verified.]

### Finding 3 — Sensitivity and Option Calculations

**What I checked:**

[Check whether the sensitivity table recalculates when S0_in changes and whether the put and call calculations correctly account for option premiums.]

**What I found:**

[Document the actual result.]

**Action taken:**

[Document any corrections or explain how the correct result was verified.]

## 4. Numerical Validation

| Calculation | Expected Result | Actual Workbook Result | Status |
|---|---:|---:|---|
| DF_USD | 1.01961556 | [Enter result] | [Pass/Fail] |
| DF_FC | 1.01263889 | [Enter result] | [Pass/Fail] |
| MM_BORROW_FC | €14,319,023.45 | [Enter result] | [Pass/Fail] |
| MM_USD_NOW | $15,987,189.69 | [Enter result] | [Pass/Fail] |
| USD_MM | $16,300,787.29 | [Enter result] | [Pass/Fail] |
| F_IMPLIED | 1.12419223 | [Enter result] | [Pass/Fail] |
| PARITY_DIFF | $0.00 | [Enter result] | [Pass/Fail] |
| PREM_PUT_FV | $286,817.86 | [Enter result] | [Pass/Fail] |
| PREM_CALL_FV | $437,619.00 | [Enter result] | [Pass/Fail] |
| USD_FLOOR_PUT | $15,902,432.14 | [Enter result] | [Pass/Fail] |

The parity validation tolerance is:

`ABS(PARITY_DIFF) <= $0.01`

Any failed validation must be reported and explained rather than forced to pass.

## 5. Formula and Sensitivity Testing

The following tests were performed:

- [Document whether all ten named ranges exist and reference the correct cells.]
- [Document whether calculated cells contain formulas rather than pasted values.]
- [Document whether changing S0_in updates the sensitivity table and chart.]
- [Document whether the sensitivity table contains 11 scenarios from 95% to 105% of S0_in.]
- [Document whether the money-market hedge contains three explicit steps.]
- [Document whether the put premium is carried to settlement.]
- [Document whether the call payoff is separate from the receivable proceeds.]
- [Document whether the chart matches the sensitivity table.]

## 6. Defects and Corrections

[Describe any defects discovered, their corresponding specification changes, and whether the workbook was regenerated.]

## 7. Audit Conclusion

[Summarize whether the workbook satisfies the Stage 3 requirements, identify remaining limitations, and state whether it is ready for Stage 4 market-data population.]
