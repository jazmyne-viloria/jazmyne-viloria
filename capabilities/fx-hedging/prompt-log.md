# FX Hedging — AI Prompt Log

**Project:** Scenario 33 — Mining Shovels  
**Course:** FIN 321 — International Finance and Securities  
**Stages Documented:** Stage 2 — Model Specification; Stage 3 — AI-Assisted Build + Audit  
**Author:** Jazmyne Viloria

---

## 1. Initial Specification Review

**Purpose:** Review the first draft of the technical specification for gaps, calculation accuracy, and alignment with the Stage 2 assignment requirements.

**Prompt submitted:**

> Review my FX hedging model specification against the Stage 2 assignment requirements. My assigned scenario is Scenario 33 — Mining Shovels, involving a EUR 14,500,000 receivable due in 182 days.
>
> Check all eight required sections, the ten standardized input names, workbook tab architecture, calculation formulas, three-step money-market hedge, covered interest-rate parity, option payoffs, sensitivity analysis, validation rules, and required outputs.
>
> Identify missing requirements, incorrect calculations, inconsistent assumptions, and anything an AI workbook builder might misunderstand. Provide specific recommendations for improvement.

**AI feedback received:**

The initial review identified several areas where the specification could be more explicit:

- Clarify the sensitivity grid as eleven settlement-rate scenarios, ranging from 95% to 105% of the initial spot rate in 1% increments.
- Clarify that the put floor represents minimum modeled USD proceeds after deducting the option premium.
- Distinguish the assigned indicative forward rate from the forward rate implied by covered interest-rate parity.
- Make the treatment of option premiums consistent with the assignment's specified formulas.
- Identify which market inputs are provided by the scenario and which must be sourced during Stage 4.

**Changes made:**

- Specified the sensitivity grid as eleven scenarios from 95% through 105% of the initial spot rate.
- Defined `USD_FLOOR_PUT` as the minimum put-based proceeds after deducting the carried put premium.
- Added the `F_IMPLIED` and `PARITY_DIFF` calculations.
- Distinguished scenario-provided indicative values from sourced market inputs.
- Clarified the option premium treatment in the subsequent specification revision.

---

## 2. Revised Specification Review

**Purpose:** Verify that the revised specification addresses the initial feedback and provides sufficiently precise instructions for the Stage 3 workbook build.

**Prompt submitted:**

> Review my revised Scenario 33 FX hedging model specification. Compare it against the Stage 2 assignment requirements and identify any remaining gaps or inconsistencies.
>
> Specifically verify that:
> - All eight required sections are included.
> - All ten standardized named ranges have descriptions, values, units, and sources.
> - The workbook tab architecture is clearly defined.
> - The forward and three-step money-market calculations are correct.
> - The covered interest-rate parity check is clearly explained.
> - The put and call option payoffs are specified correctly.
> - The sensitivity analysis includes eleven settlement-rate scenarios from 95% to 105% of the initial spot rate.
> - The validation rules and required outputs are specific enough to guide the Stage 3 workbook build.

**AI feedback received:**

The revised specification addressed the main issues identified in the initial review. The follow-up review confirmed that:

- All ten named ranges include descriptions, values, units, and sources.
- The workbook tabs are individually named and their purposes are explained.
- The calculation flow uses named-range notation rather than Excel cell addresses.
- The money-market hedge is separated into three required steps.
- The parity calculation, option payoffs, and sensitivity plan are explicitly defined.
- The specification requires formula-driven calculated outputs and identifies formula errors and broken references as validation failures.

One additional recommendation was to make formula-error validation more specific by listing examples of Excel errors.

**Changes made:**

- Confirmed that the validation checklist covers the parity check, formula-driven outputs, and formula-error checks.
- Identified examples such as `#REF!`, `#VALUE!`, `#DIV/0!`, and `#NAME?` for workbook auditing.

---

## 3. Documented Draft-to-Revision Example

**Issue identified:**

The original specification did not explicitly distinguish between scenario-provided indicative values and market data that must be sourced during Stage 4.

**Original wording:**

> The assigned forward rate and option premiums are indicative, replaced with live data at Stage 4 if updated market quotes are required.

**Revised wording:**

> The assigned forward rate (`F0_in`) and option premiums (`PREM_PUT` and `PREM_CALL`) are scenario-provided indicative values. Spot (`S0_in`) and interest-rate inputs (`R_USD` and `R_FC`) use dated market observations recorded in the specification. Sources and access dates must be documented.

**Reason for revision:**

The revised wording identifies each input individually and distinguishes assigned scenario values from sourced market observations. This makes the Stage 4 data requirements clearer and reduces ambiguity for the workbook build.

---

## 4. Option Premium Treatment Revision

**Issue identified:**

An earlier specification draft described deducting the upfront option premiums directly from settlement proceeds. This did not account for the time value of premiums paid at the beginning of the hedge period.

**Initial treatment:**

> Option premiums are paid upfront but deducted directly from settlement proceeds.

**Final treatment:**

> The put and call option premiums are paid upfront at the beginning of the 182-day hedge period. Each premium is carried to settlement using the USD accumulation factor (`DF_USD`) and deducted from its corresponding option outcome.

**Final formulas:**

- `PREM_PUT_FV = FC_AMT * PREM_PUT * DF_USD`
- `PREM_CALL_FV = FC_AMT * PREM_CALL * DF_USD`
- `USD_PUT(S_T) = FC_AMT * MAX(S_T, K_PUT) - PREM_PUT_FV`
- `USD_CALL_NET(S_T) = CALL_PAYOFF(S_T) - PREM_CALL_FV`

**Reason for revision:**

The revised treatment compares option outcomes at the same settlement date as the other hedge strategies. The earlier direct-deduction treatment was superseded and is not the final model specification.

---

## 5. Final Stage 2 Specification Review

**Final specification:** `capabilities/fx-hedging/spec.md`

**Review outcome:**

The revised specification defines the ten standardized named ranges, workbook architecture, calculation formulas, hedge strategies, sensitivity analysis, and validation requirements. It provides the calculation structure intended to guide the Stage 3 workbook build.

The Stage 2 specification is the source of truth for the workbook's inputs, formulas, and assumptions. Stage 3 audit findings may identify defects that require further specification or workbook corrections.

---

## 6. Stage 3 — AI-Assisted Workbook Build

**Date:** 2026-11-13  
**Project:** Scenario 33 — Mining Shovels  
**AI tool:** ChatGPT  
**Source specification:** `capabilities/fx-hedging/spec.md`  
**Required workbook:** `capabilities/fx-hedging/model.xlsx`

**Build prompt submitted:**

> Read my complete Stage 2 specification (`spec.md` attached).
>
> Build the Excel workbook for Scenario 33 — Mining Shovels exactly as specified.
>
> All requirements in my specification are binding.
>
> The workbook must include:
> 1. All ten standardized named ranges, each attached to the correct input cell.
> 2. Formulas in every calculated cell, referencing named ranges. No hard-coded calculated values.
> 3. A Cover worksheet with scenario, author, date, and data provenance.
> 4. A Legend-Key worksheet defining the workbook's formatting conventions.
> 5. All three hedge families, including the three explicit money-market steps and put and call option calculations.
> 6. A formula-driven sensitivity table and chart using the required settlement-rate scenarios.
> 7. Visible validation checks, including the parity check and specification check figures.

**Build requirements:**

The workbook must follow the committed specification and the instructor's seven Stage 3 requirements. The workbook must contain all eight required worksheets and retain formula-driven calculations.

**Build result:**

Record the actual workbook-generation result here after confirming that `model.xlsx` was produced and saved at the required repository path. Do not report successful generation until the file exists and has been checked.

---

## 7. Stage 3 — Workbook Audit

**Audit file:** `analysis/2026-11-13-scenario-33-build-audit-analysis.md`

**Audit standard:**

The workbook must be inspected against the seven Stage 3 requirements:

1. Ten standardized named ranges attached to the correct cells.
2. Formula integrity, with formulas in every calculated cell.
3. Cover worksheet with scenario, author, date, and data provenance.
4. Legend-Key worksheet with yellow inputs, blue assumptions, green formulas, and gray outputs.
5. Forward, money-market, and option hedge calculations, including all three money-market steps.
6. Formula-driven sensitivity table and linked chart covering ±5% in 1% increments.
7. Visible validation checks, including parity and specification check figures.

**Audit findings:**

The audit note must document at least three substantive findings. Each finding must identify what was checked, what the workbook showed, and what action was taken. A confirmed result may count as a finding; defects do not need to be invented.

**Audit status:**

Complete this section only after inspecting the actual workbook. Record the actual results, any defects, corrections, and retesting. Do not state that the workbook passed a test unless the test was performed.

---

## 8. Stage 3 — Corrections and Follow-Up Prompts

Every additional prompt used to correct or regenerate the workbook must be recorded below in the order submitted.

For each follow-up, document:

- **Prompt submitted:** The actual prompt used.
- **Reason:** The specific defect or requirement being addressed.
- **AI response or change:** What the tool changed or recommended.
- **Verification:** How the correction was checked in the workbook.

If no correction or regeneration was necessary, state that only after completing the audit and confirming the workbook's results.

---

## 9. Stage 3 Completion Review

Stage 3 is complete only when:

- `model.xlsx` is saved at `capabilities/fx-hedging/model.xlsx`.
- All seven instructor requirements have been audited.
- At least three substantive findings are documented in the audit note.
- Numerical validation results are compared with the specification's expected figures.
- Formula and sensitivity tests have been performed.
- Defects and corrections are documented.
- The audit note is saved at `analysis/2026-11-13-scenario-33-build-audit-analysis.md`.

Stage 4 market-data confirmation and population remain separate from the Stage 3 build and audit.
