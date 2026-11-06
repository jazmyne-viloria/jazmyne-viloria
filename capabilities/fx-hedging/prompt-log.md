# FX Hedging — Prompt Log

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

- Specified the sensitivity grid as 11 scenarios
