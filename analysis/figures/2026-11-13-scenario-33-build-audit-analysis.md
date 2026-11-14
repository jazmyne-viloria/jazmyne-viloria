# Scenario 33 — Mining Shovels: Stage 3 Build Audit

| Item | Details |
|---|---|
| Author | Jazmyne Viloria |
| Course | FIN 321 |
| Stage | Stage 3 — AI-Assisted Build + Audit |
| Scenario | 33 — Mining Shovels |
| Audit Date | November 13, 2026 |
| Workbook | `capabilities/fx-hedging/model.xlsx` |
| Specification | `capabilities/fx-hedging/spec.md` |

**Audit status:** In progress — workbook inspection and testing required.

## 1. Build Overview

The Scenario 33 Excel workbook is intended to be generated from the Stage 2 specification in `capabilities/fx-hedging/spec.md`.

This audit evaluates the workbook against the seven requirements in the Stage 3 assignment. The review covers named ranges, formula integrity, workbook structure and formatting, hedge calculations, sensitivity analysis, and visible validation checks.

The audit must be based on inspection and testing of the actual Excel workbook. Expected values in the specification are comparison targets, not evidence that the workbook passed.

## 2. Workbook Structure

The specification requires the following eight worksheets.

| Worksheet | Required Contents | Audit Result |
|---|---|---|
| Cover | Scenario, author, date, exposure, and data provenance | Pending inspection |
| Legend-Key | Yellow inputs, blue assumptions, green formulas, gray outputs | Pending inspection |
| Inputs | Ten standardized named ranges, values, units, and sources | Pending inspection |
| Forward Hedge | Forward contract settlement proceeds | Pending inspection |
| Money Market Hedge | Borrowing, conversion, and investment steps | Pending inspection |
| Option Hedge | Put and call calculations, premiums, and net payoffs | Pending inspection |
| Sensitivity | Formula-driven sensitivity table and linked chart | Pending inspection |
| Notes & Assumptions | Sources, access dates, methodology, and limitations | Pending inspection |

## 3. Audit Findings

At least three substantive findings must be documented after the workbook is inspected. A finding may confirm that a requirement was correctly implemented or identify a defect and its correction.

### Finding 1 — Named Range Verification

**What was checked:**

The Excel Name Manager was checked against the ten standardized named ranges in `spec.md`. Each name must refer to its correct input cell on the `Inputs` worksheet. The `K_PUT` and `K_CALL` names must be formula-linked to `S0_in`.

**What was found:**

Record the actual result after inspecting Name Manager. Identify any missing names, incorrect cell references, or incorrect strike-price formulas.

**Action taken:**

Record whether the named ranges were correct or document the specific corrections made. Include the final verification result.

### Finding 2 — Formula Integrity

**What was checked:**

Calculated cells were inspected across the hedge worksheets and sensitivity analysis. Formulas must reference standardized named ranges and scenario variables rather than hard-coded calculated values. The money-market hedge must show all three calculation steps.

**What was found:**

Record the actual formulas inspected, any hard-coded calculated values, broken references, or formula errors found, and the affected worksheets.

**Action taken:**

Record the corrections made and how the affected formulas were retested. If no defects were found, identify the formulas and areas inspected to support that conclusion.

### Finding 3 — Sensitivity and Option Calculations

**What was checked:**

The sensitivity table must calculate eleven exchange-rate scenarios from 95% to 105% of `S0_in`, in 1% increments. The chart must link to the sensitivity table.

The put premium must be carried to settlement using `DF_USD`. The call payoff must be reported separately from receivable proceeds, with its premium carried to settlement.

**What was found:**

Record the actual sensitivity and option calculation results. Identify whether the scenario rows and chart update correctly and whether the option calculations follow the specification.

**Action taken:**

Record any corrections and the results of retesting. If the calculations were correct, identify the tests that confirmed the result.

## 4. Numerical Validation

The following figures from `spec.md` are the expected results for checking the workbook. Enter the actual workbook results only after inspecting the calculations.

| Validation Item | Expected Result | Actual Workbook Result | Status |
|---|---:|---:|---|
| `DF_USD` | 1.01961556 | Not yet recorded | Pending |
| `DF_FC` | 1.01263889 | Not yet recorded | Pending |
| `MM_BORROW_FC` | EUR 14,319,023.45 | Not yet recorded | Pending |
| `MM_USD_NOW` | USD 15,987,189.69 | Not yet recorded | Pending |
| `USD_MM` | USD 16,300,787.29 | Not yet recorded | Pending |
| `F_IMPLIED` | 1.12419223 | Not yet recorded | Pending |
| `PARITY_DIFF` | Approximately USD 0.00 | Not yet recorded | Pending |
| `PREM_PUT_FV` | USD 286,817.86 | Not yet recorded | Pending |
| `PREM_CALL_FV` | USD 437,619.00 | Not yet recorded | Pending |
| `USD_FLOOR_PUT` | USD 15,902,432.14 | Not yet recorded | Pending |

The parity check must pass when:

`ABS(PARITY_DIFF) <= 0.01`

A failed check must be reported and investigated. Do not adjust formulas or expected values simply to force a passing result.

## 5. Formula and Sensitivity Testing

Complete these tests in Excel and record the actual results.

| Test | Expected Behavior | Result |
|---|---|---|
| Named ranges | All ten names refer to the correct input cells | Pending |
| Strike prices | `K_PUT` and `K_CALL` update with `S0_in` | Pending |
| Formula integrity | Calculated cells contain formulas and no hard-coded calculated results | Pending |
| Forward hedge | Proceeds equal `FC_AMT * F0_in` | Pending |
| Money-market hedge | All three steps calculate using the specified formulas | Pending |
| Put option | Premium is carried to settlement and deducted from proceeds | Pending |
| Call option | Net payoff reflects the call payoff less the carried premium | Pending |
| Sensitivity table | Eleven scenarios recalculate from `S0_in` | Pending |
| Sensitivity chart | Chart reflects the sensitivity table | Pending |
| Validation checks | Parity and specification check figures are visible and calculated | Pending |

For the sensitivity test, change `S0_in` to a temporary test value in Excel and confirm that the strike prices, sensitivity rates, and dependent calculations update. Restore the original input after testing.

## 6. Defects and Corrections

Record each defect found during the audit. Do not leave this section as a generic statement that everything worked.

| Defect | Worksheet / Cell | Correction | Retest Result |
|---|---|---|---|
| Record actual defect or state none identified after inspection | Record location | Record correction or verification | Record result |

If a defect requires a specification change, document the change in `spec.md` and update its change log. If the workbook is regenerated, record the regeneration prompt and result in `capabilities/fx-hedging/prompt-log.md`.

## 7. Audit Conclusion

Complete this section after all seven instructor requirements have been tested.

Summarize:

- Which requirements passed and the evidence supporting each result.
- Any defects identified and the corrections made.
- Whether the numerical validation figures matched the specification.
- Whether the sensitivity table and chart recalculated correctly.
- Any remaining limitations or failed checks.

Stage 3 should be marked complete only after the workbook and audit findings have been verified. Any failed validation must remain disclosed in the audit conclusion.
