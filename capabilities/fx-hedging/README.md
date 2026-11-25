# FX Hedging — Scenario 33: Mining Shovels

This project evaluates foreign-exchange hedging strategies for a U.S. mining-shovel manufacturer with a EUR 14,500,000 receivable due in 182 days from a Finnish nickel miner. The analysis compares the USD cash-flow outcomes of an unhedged position, a forward hedge, a money-market hedge, and option-based strategies.

## Project Stages

1. **Stage 1 — Executive Brief:** Define the currency exposure, quantify the risk, and establish assumptions and a hypothesis.
2. **Stage 2 — Model Specification:** Document the workbook structure, standardized named ranges, calculation formulas, sensitivity analysis, and validation requirements.
3. **Stage 3 — AI-Assisted Build + Audit:** Build the Excel model and evaluate it against the instructor's seven requirements.
4. **Stage 4 — Market Data + Population:** Research, document, and apply sourced market observations to the model.
5. **Stage 5 — Independent Validation + Decision Memo:** Independently validate the calculations, compare the hedging strategies, assess covered interest-rate parity, and prepare the CFO recommendation.

## Project Files

| File | Purpose |
|---|---|
| [`spec.md`](spec.md) | Version 0.4 technical specification and seven Stage 3 requirements |
| [`model.xlsx`](model.xlsx) | Excel hedging model with live Stage 4 inputs |
| [`Stage 4 market-data memo`](../../data/2026-11-18-scenario-33-market-data-memo.md) | Sources, observations, and limitations for the updated market inputs |
| [`Independent LLM output`](../../analysis/figures/2026-11-24-scenario-33-independent-llm-output) | Independent analysis produced using the two specified source documents |
| [`Stage 5 validation analysis`](../../analysis/2026-11-24-scenario-33-validation-analysis.md) | Comparison of independent results with the workbook, hand calculations, and validation findings |
| [`CFO decision memo`](../../docs/decisions/2026-11-24-scenario-33-hedge-decision-memo.md) | Final modeled hedge recommendation and decision request |
| [`Prompt log`](../../prompt-log.md) | Record of project prompts, AI assistance, revisions, and validation activity |

## Hedge Strategies

- **Unhedged exposure:** Converts the EUR receivable at the future settlement spot rate.
- **Forward hedge:** Locks in the assigned forward exchange rate, subject to confirmation that the rate is current and executable.
- **Money-market hedge:** Borrows EUR, converts the borrowed amount into USD, and invests the USD until settlement.
- **EUR put option:** Provides a minimum modeled USD proceeds floor while retaining upside participation.
- **EUR call option:** Reports the separate call payoff after its premium; it is not presented as total receivable proceeds.

## Current Status

Stage 5 independent validation and the CFO decision memo have been prepared. The workbook's calculations have been recalculated and reconciled, while the remaining Stage 3 audit corrections—particularly the Cover specification metadata and complete visible numerical validation checks—must be addressed before the workbook can be marked fully compliant.

The recommendation is conditional on obtaining a current, executable forward quote for the full EUR 14.5 million receivable and 182-day maturity. The assigned 1.1705 USD/EUR rate is a scenario input, not a confirmed executable quote.

## Tools and Methods

Excel financial modeling, standardized named ranges, market-data research, covered interest-rate parity, sensitivity analysis, GitHub documentation, and independent AI-assisted validation.
