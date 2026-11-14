# FX Hedging — Scenario 33: Mining Shovels

This capability evaluates foreign-exchange hedging strategies for a U.S. mining-shovel manufacturer with a EUR 14,500,000 receivable due in 182 days from a Finnish nickel miner. The model compares the USD cash-flow outcomes of an unhedged position, a forward hedge, a money-market hedge, and option-based strategies.

## Project Stages

1. **Stage 1 — Executive Brief:** Define the currency exposure, quantify the risk, and establish assumptions and a hypothesis.
2. **Stage 2 — Model Specification:** Document the workbook structure, standardized named ranges, calculation formulas, sensitivity analysis, and validation requirements in `spec.md`.
3. **Stage 3 — AI-Assisted Build + Audit:** Build `model.xlsx` from the committed specification, test the workbook against the instructor's seven requirements, and document the build and audit process.
4. **Stage 4 — Market Data + Population:** Confirm or replace the model inputs with appropriate sourced, timestamped market data and populate the workbook.
5. **Stage 5 — Validation + Decision Memo:** Independently validate the model, compare the hedging strategies, and prepare the final hedge recommendation.

## Project Files

| File | Purpose |
|---|---|
| `README.md` | Project overview, stages, and current status |
| `spec.md` | Technical specification and workbook requirements |
| `prompt-log.md` | Record of AI prompts, feedback, revisions, and build activity |
| `model.xlsx` | Stage 3 Excel workbook |
| `analysis/2026-11-13-scenario-33-build-audit-analysis.md` | Stage 3 workbook build and audit record |

## Hedge Strategies

The model compares:

- **Unhedged exposure:** Converts the EUR receivable at the future settlement spot rate.
- **Forward hedge:** Locks in the assigned forward exchange rate.
- **Money-market hedge:** Borrows EUR, converts the borrowed amount into USD, and invests the USD until settlement.
- **EUR put option:** Provides a minimum modeled USD proceeds floor while retaining upside participation.
- **EUR call option:** Reports the separate call payoff after its premium; it is not presented as total receivable proceeds.

## Current Status

**Stage 3 — AI-Assisted Build + Audit

The Stage 2 technical specification defines the ten standardized input names, calculation formulas, hedge strategies, sensitivity analysis, and validation requirements. Stage 3 requires the Excel workbook to be built and tested against the instructor's seven requirements.

The workbook audit must verify the named ranges, formula integrity, worksheet structure and formatting, all three hedge families, sensitivity table and chart, and visible validation checks. Audit findings must be based on actual workbook inspection. Any defects and corrections must be documented before Stage 3 is considered complete.

Stage 4 market-data confirmation and population, followed by Stage 5 independent validation and the decision memo, remain subsequent project stages.
