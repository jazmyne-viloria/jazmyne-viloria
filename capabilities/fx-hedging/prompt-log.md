# FX Hedging — AI Prompt Log

**Project:** Scenario 33 — Mining Shovels  
**Course:** FIN 321 — International Finance and Securities  
**Stage:** Stage 2 — Model Specification  
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

- Clarify the sensitivity grid as 11 settlement-rate scenarios, ranging from 95% to 105% of the initial spot rate in 1% increments.
- Clarify that the put floor represents minimum modeled USD proceeds after deducting the option premium.
- Distinguish the assigned indicative forward rate from the forward rate implied by covered interest-rate parity.
- Make the treatment of option premiums consistent with the assignment's specified formulas.
- Ensure the specification clearly identifies which market inputs are provided by the scenario and which must be sourced during Stage 4.

**Changes made:**

- Specified the sensitivity grid as 11 scenarios from 95% through 105% of the initial spot rate.
- Defined `USD_FLOOR_PUT` as the minimum put-based proceeds after deducting the put premium.
- Added the `F_IMPLIED` and `PARITY_DIFF` calculations to distinguish the implied forward rate from the assigned forward rate.
- Clarified that option premiums are paid upfront but deducted directly from settlement proceeds in the model, rather than compounded to maturity.
- Expanded the input conventions to distinguish indicative scenario values from market-data placeholders.

---

## 2. Revised Specification Review

**Purpose:** Verify that the revised specification addresses the initial feedback and provides sufficiently precise instructions for the Stage 3 workbook build.

**Prompt submitted:**

> Review my revised Scenario 33 FX hedging model specification. Compare it against the Stage 2 assignment requirements and identify any remaining gaps or inconsistencies.
>
> Specifically verify that:
> - All eight required sections are included.
> - All ten standardized named ranges have descriptions, values or placeholders, units, and Stage 4 data sources.
> - The workbook tab architecture is clearly defined.
> - The forward and three-step money-market calculations are correct.
> - The covered interest-rate parity check is clearly explained.
> - The put and call option payoffs are specified correctly.
> - The sensitivity analysis includes 11 settlement-rate scenarios from 95% to 105% of the initial spot rate.
> - The validation rules and required outputs are specific enough to guide the Stage 3 workbook build.

**AI feedback received:**

The revised specification addressed the main issues identified in the initial review. The follow-up review confirmed that:

- All ten named ranges include descriptions, values or placeholders, units, and Stage 4 sources.
- The workbook tabs are individually named and their purposes are explained.
- The calculation flow uses named-range notation without Excel cell addresses.
- The money-market hedge is separated into the three required steps.
- The parity calculation, option payoffs, and sensitivity plan are explicitly defined.
- The specification requires formula-driven calculated outputs and identifies formula errors and broken references as validation failures.

One additional recommendation was to make the formula-error validation rule more specific by listing examples of Excel errors.

**Changes made:**

- Confirmed that the existing validation checklist covers the required parity check, formula-driven outputs, and error checks.
- Recommended adding examples such as `#REF!`, `#VALUE!`, `#DIV/0!`, and `#NAME?` to make the error-validation rule easier to audit.

---

## 3. Documented Draft-to-Revision Example

**Issue identified:**

The original specification did not explicitly distinguish between all scenario-provided indicative market values and the market-data placeholders that must be populated during Stage 4.

**Original wording:**

> The assigned forward rate and option premiums are indicative, replaced with live data at Stage 4 if updated market quotes are required.

**Revised wording:**

> All scenario-provided market values (`F0_in`, `PREM_PUT`, and `PREM_CALL`) are indicative, replaced with live data at Stage 4. The spot rate (`S0_in`) and interest rates (`R_USD` and `R_FC`) are placeholders to be populated with sourced market data during Stage 4.

**Reason for revision:**

The revised wording identifies each input individually and distinguishes assigned scenario values from market data that still needs to be sourced. This makes the Stage 4 data requirements clearer and reduces ambiguity for the AI workbook builder in Stage 3.

---

## 4. Final Review

**Final specification:** `spec.md`

**Review outcome:**

The revised specification addresses the eight required sections and the Stage 2 checklist. It defines the ten named ranges, workbook architecture, calculation formulas, hedge strategies, sensitivity analysis, and validation requirements. The specification is ready to guide the Stage 3 workbook build.

Live market-data sourcing and population remain for Stage 4. Workbook construction, formula testing, and the final audit remain for Stage 3.

---

## 5. Instructor Feedback and Revisions

**Feedback received:** November 7, 2026

The instructor identified four areas requiring improvement:

1. The spot rate and USD/EUR interest rates needed actual market values, sources, and observation dates rather than placeholders.
2. The option-premium calculations needed to account for the time value of premiums paid upfront.
3. The specification needed numerical check figures for the money-market hedge, implied forward rate, and put minimum, along with a specific parity tolerance.
4. Section 2.1 contained both the original and revised placeholder wording.

**Revisions made:**

- Populated `S0_in`, `R_USD`, and `R_FC` with November 6, 2026 market observations and documented their sources and access date.
- Retained the assigned scenario forward rate and option premiums.
- Added `PREM_PUT_FV` and `PREM_CALL_FV` to carry the upfront premiums to settlement using `DF_USD`.
- Revised the put and call net payoff formulas to deduct the carried premiums.
- Added expected check figures for the money-market hedge, implied forward rate, parity difference, and put minimum.
- Specified a parity validation tolerance of ±$0.01.
- Reviewed the 11 sensitivity scenarios and documented whether carrying the premiums changes the highest-proceeds strategy.
- Removed the outdated original placeholder wording from Section 2.1.

**Final review outcome:**

The specification has been revised to address the instructor's November 7 feedback. The updated market inputs, premium treatment, expected check figures, and validation tolerance provide a more reproducible foundation for the Stage 3 workbook build.

---


## Stage 3 — AI-Assisted Build + Audit

**Date:** November 13, 2026

**Project:** Scenario 33 — Mining Shovels FX Hedging Model

**AI Tool:** ChatGPT

### 1. Workbook Build

**Source specification:** `capabilities/fx-hedging/spec.md`

**Build prompt submitted:**

[Paste the exact prompt submitted to generate the Excel workbook, including any additional instructions.]

**Required workbook output:** `capabilities/fx-hedging/model.xlsx`

### 2. Workbook Audit

The generated workbook was audited against the committed Stage 2 specification and the seven Stage 3 build requirements.

**Audit areas:**

- Ten named ranges and their cell references.
- Formula integrity and hard-coded calculated values.
- Cover worksheet and Legend-Key formatting.
- Forward hedge and three-step money-market hedge calculations.
- Put and call option payoffs and premium treatment.
- Formula-driven sensitivity table and linked chart.
- Parity validation and Section 7 numerical check figures.

**Audit findings:**

The audit results, including what was checked, what was found, and what was done, are documented in `analysis/2026-11-13-scenario-33-build-audit-analysis.md`.

### 3. Workbook Revisions and Final Output

**Defects identified:** [Summarize actual defects found, or state the substantive checks that were confirmed.]

**Specification changes:** [Identify any specification changes made in response to the audit. If none were necessary, state that.]

**Workbook regeneration:** [State whether the workbook was regenerated after specification changes.]

**Final workbook:** `capabilities/fx-hedging/model.xlsx`

