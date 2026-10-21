---
type: brief
title: EUR Receivable FX Hedging Executive Brief
scenario: eur-receivable
date: 2026-10-21
hypothesis: "I expect a forward hedge to provide the most predictable USD proceeds because it locks in the EUR/USD conversion rate without an option premium."
llm_used: "ChatGPT (GPT-6)"
---

# EUR Receivable FX Hedging Executive Brief

**Created by:** [Your Name]  
**Updated by:** [Your Name]  
**Date Created:** 2026-10-21  
**Date Updated:** 2026-10-21  
**Version:** 1.0  
**LLM Used:** ChatGPT (GPT-6)

---

## Executive Summary (≤150 words)

Our company expects to receive EUR 4,500,000 in one year and convert the funds into U.S. dollars. Although the euro amount is fixed, the dollar proceeds depend on the exchange rate at settlement. At EUR/USD 1.10, the receipt equals $4,950,000; at 1.00, it equals $4,500,000, a $450,000 reduction. This exposure could create a significant budget shortfall. This project will compare forward or futures contracts, money-market hedges, and currency options to assess their costs, protection, and potential upside. I expect a forward hedge to provide the most predictable dollar proceeds because it locks in a conversion rate without an option premium. The analysis will test this hypothesis using a specified model, sourced market data, independent validation, and documented calculations before a final recommendation is made.

## Background & Objectives

Foreign exchange risk arises because the company measures its financial results in U.S. dollars but expects to receive euros in the future. If the euro weakens against the dollar, the receivable converts into fewer dollars. A decline from EUR/USD 1.10 to 1.00 reduces proceeds by $450,000, approximately 9.1% of the original dollar value. This difference could affect cash-flow planning and the company's ability to meet its budget.

The objective is to compare three approaches to managing the receivable while considering certainty, cost, and participation in favorable exchange-rate movements. A forward or futures contract locks in a conversion rate, providing certainty but sacrificing additional gains if the euro strengthens. A money-market hedge borrows euros, converts them into dollars, and invests the dollar proceeds; it can provide similar certainty but uses financing capacity and depends on interest rates. A euro put option establishes a minimum conversion rate while preserving upside potential, but requires an upfront premium.

## Methods

The analysis will proceed in five stages:

- **Stage 1 — Executive brief:** Define the exposure, assumptions, and hypothesis before modeling.
- **Stage 2 — Model specification:** Design the workbook structure, named ranges, formula logic, and validation checks.
- **Stage 3 — AI-assisted build and audit:** Generate the workbook from the specification, then inspect and correct formulas and outputs.
- **Stage 4 — Market data and population:** Replace illustrative inputs with sourced, timestamped exchange rates, interest rates, forward rates, and option pricing inputs.
- **Stage 5 — Validation and decision memo:** Independently review the model, verify calculations, compare hedge outcomes, and document the final recommendation.

The model will compare the dollar proceeds and costs of each hedge under consistent assumptions and exchange-rate scenarios.

## Limitations & Next Steps

The initial analysis assumes a EUR 4,500,000 receivable due in one year and a USD reporting currency. The exchange rates above are illustrative, not forecasts. Actual market rates, financing costs, option premiums, transaction costs, and contract terms remain to be established. The comparison must also account for differences in hedge structure and execution requirements.

I expect the forward hedge to provide the most predictable dollar proceeds because it fixes the conversion rate. This hypothesis would be challenged if the money-market hedge secures equivalent protection at a lower total cost, or if an option delivers sufficiently greater net proceeds under plausible exchange-rate outcomes to justify its premium. Stage 2 will formalize these assumptions and calculations; later stages will populate, audit, and validate the model before any hedge is recommended.

## References

- International Finance and Securities course materials. (2026). *FX Hedging — Stage 1: Executive Brief*. Course assignment instructions.
- Stauffer, A. (n.d.). *Template decision memo*. Shidler course repository. https://github.com/adamwstauffer/shidler/blob/main/courses/International-Finance-And-Securities/projects/fx-hedging/_templates/template-decision-memo.md
