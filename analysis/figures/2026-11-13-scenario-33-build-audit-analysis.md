
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

## 1. Build Overview

The Scenario 33 Excel workbook was generated using the Stage 2 specification in `spec.md`. The purpose of this audit is to evaluate the workbook against the seven requirements in the Stage 3 assignment and verify that its calculations, formulas, named ranges, sensitivity analysis, and validation checks follow the specification.

The audit focuses on the workbook itself rather than assuming that generated formulas or outputs are correct.

## 2. Workbook Structure

The specification requires the following eight worksheets:

| Worksheet | Required Contents | Audit Result |
|---|---|---|
| Cover | Scenario information, author, date, and data provenance | Pending audit |
| Legend-Key | Workbook color conventions | Pending audit |
| Inputs | Ten standardized named ranges and source assumptions | Pending audit |
| Forward Hedge | Forward contract settlement proceeds | Pending audit |
| Money Market Hedge | Foreign-currency borrowing, spot conversion, and USD investment | Pending audit |
| Option Hedge | Put and call payoffs, premiums, and net proceeds | Pending audit |
| Sensitivity | Formula-driven sensitivity table and chart | Pending audit |
| Notes & Assumptions | Methodology, limitations, and assumptions | Pending audit |

## 3. Audit Findings

### Finding 1 — Named Range Verification

**What was checked:**  
The workbook's Name Manager was checked against the ten standardized named ranges required by `spec.md`. Each name must refer to its correct input cell on the Inputs worksheet. The put and call strike prices must be formula-linked to `S0_in`.

**What was found:**  
Pending workbook inspection. The audit must confirm that all ten names exist, refer to the correct cells, and use the required formulas for `K_PUT` and `K_CALL`.

**Action taken:**  
Pending verification. Any missing, incorrectly assigned, or improperly defined named ranges must be corrected in the workbook.

### Finding 2 — Formula Integrity

**What was checked:**  
Calculated cells were checked across the hedge worksheets and sensitivity analysis. Formulas must reference standardized named ranges and scenario variables rather than hard-coded calculated values. The money market hedge must show all three calculation steps explicitly.

**What was found:**  
Pending workbook inspection. The audit must identify whether calculated values use formulas, whether formulas reference the required named ranges, and whether any calculated results were entered as fixed values.

**Action taken:**  
Pending verification. Any hard-coded calculated values, broken references, or incorrect formulas must be corrected and documented.

### Finding 3 — Sensitivity and Option Calculations

**What was checked:**  
The sensitivity table must calculate eleven exchange-rate scenarios from 95% to 105% of `S0_in`, in 1% increments. The chart must link to the sensitivity table. Put and call calculations must follow the premium treatment and payoff definitions in `spec.md`.

The audit must also verify that the put premium is carried to settlement using `DF_USD`, and that the call premium is shown separately from the receivable proceeds.

**What was found:**  
Pending workbook inspection. The audit must confirm that the sensitivity scenarios are formula-driven, the chart reflects the table, and the option calculations follow the specification.

**Action taken:**  
Pending verification. Any incorrect sensitivity formulas, chart references, option payoff calculations, or premium treatments must be corrected.

## 4. Numerical Validation

The following figures from `spec.md` are the expected results for checking the workbook. Actual workbook results must be recorded after inspection.

| Validation Item | Expected Result | Actual Workbook Result | Status |
|---|---:|---:|---|
| `DF_USD` | 1.01961556 | Pending audit | Pending |
| `DF_FC` | 1.01263889 | Pending audit | Pending |
| `MM_BORROW_FC` | €14,319,023.45 | Pending audit | Pending |
| `MM_USD_NOW` | $15,987,189.69 | Pending audit | Pending |
| `USD_MM` | $16,300,787.29 | Pending audit | Pending |
| `F_IMPLIED` | 1.12419223 | Pending audit | Pending |
| `PARITY_DIFF` | Approximately $0.00 | Pending audit | Pending |
| `PREM_PUT_FV` | $286,817.86 | Pending audit | Pending |
| `PREM_CALL_FV` | $437,619.00 | Pending audit | Pending |
| `USD_FLOOR_PUT` | $15,902,432.14 | Pending audit | Pending |

The parity check should be within the specification's $0.01 tolerance. Any failed validation must be reported and investigated rather than adjusted to force a passing result.

## 5. Formula and Sensitivity Testing

The following tests must be completed in Excel before the audit is finalized:

| Test | Expected Behavior | Result |
|---|---|---|
| Change `S0_in` | Strike prices and dependent calculations update | Pending |
| Check named ranges | All ten names refer to the correct input cells | Pending |
| Check forward hedge | Settlement proceeds equal `FC_AMT × F0_in` | Pending |
| Check money market hedge | All three steps calculate using the specified formulas | Pending |
| Check put option | Premium is carried to settlement and deducted from proceeds | Pending |
| Check call option | Net payoff reflects the call payoff less the carried premium | Pending |
| Check sensitivity table | Eleven scenarios recalculate from `S0_in` | Pending |
| Check sensitivity chart | Chart updates with the sensitivity table | Pending |

## 6. Defects and Corrections

No defects or corrections are recorded until the workbook has been inspected.

If a defect is identified, document:
- The worksheet and calculation affected.
- The incorrect result or formula.
- The correction made to the specification or workbook.
- Whether the workbook was regenerated after the correction.
- The result of the follow-up test.

If no specification changes are necessary, state that the workbook was corrected directly or that no defects were identified, as supported by the audit evidence.

## 7. Audit Conclusion

The Stage 3 audit is not complete until the workbook has been checked against all seven instructor requirements, the numerical validation results have been compared with the expected figures, and the sensitivity and formula tests have been completed.

The final conclusion will summarize the requirements that passed, any defects identified, the corrections made, and any remaining limitations. No requirement will be reported as passing without supporting workbook evidence.
