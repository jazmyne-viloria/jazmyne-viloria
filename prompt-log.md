# Prompt Log

## 2026-09-22

- **What I asked:** Set up a public graduate business course portfolio repository with a defined file structure, tailored AI instructions, a resume, a prompt log, capability folders, documentation folders, data and analysis folders, and a Git ignore file; show every file before committing anything.
- **What you produced:** A complete proposed repository structure and draft contents for all requested files, including tailored `AGENTS.md`, `CLAUDE.md`, `RESUME.md`, `README.md`, `prompt-log.md`, `.gitignore`, capability documentation, and directory placeholder files.
- **What was wrong:** Nothing identified yet; the files have not been committed or tested.
- **How it was caught:** Human review is required before any repository write operation.

---

## 2026-11-24 — Stage 5: Independent Validation and Decision Memo

- **What I asked:** I used a fresh LLM conversation to independently calculate and compare all five FX hedging outcomes, evaluate three future spot-rate scenarios, calculate the CIP-implied forward rate, and develop a hedge recommendation using only the Stage 2 technical specification and Stage 4 market-data memo.
- **What I provided:** Exactly two source documents: the Stage 2 project specification and the Stage 4 market-data memo. I did not provide the Excel workbook or coach the model during the analysis.
- **Independent prompt:** The exact prompt submitted in the fresh LLM conversation is reproduced below.
- **What the LLM produced:** An independent analysis of the unhedged exposure, forward hedge, money-market hedge, put option, call option, sensitivity scenarios, CIP-implied forward rate, and hedge recommendation. The output identifies missing information and ambiguities rather than assuming unavailable inputs.
- **What I checked:** I independently recomputed the forward proceeds, all three money-market hedge steps, and the put option floor. These calculations provide an independent arithmetic check. The complete LLM-versus-workbook reconciliation, including the call option payoff, remains outstanding and must not be described as completed until the workbook results have been checked.
- **What was unclear:** The independent analysis identified the assigned 1.1705 forward rate as potentially stale relative to the updated November 18 spot rate. The benchmark interest rates also differ in instrument and tenor from transaction-specific borrowing and investment rates. The input hierarchy between the Stage 2 assumptions and Stage 4 market updates should be stated more explicitly. The documents do not establish that the assigned forward quote is currently executable.
- **What I changed:** I revised the independent-analysis documentation to distinguish the assigned forward quote from a confirmed executable quote, describe the $970,696.89 CIP comparison as a modeled difference rather than guaranteed savings, and identify workbook reconciliation as outstanding. The workbook has not been represented as validated.
- **Final result:** The independent analysis provisionally favors the forward hedge if the bank confirms that the assigned 1.1705 rate is currently executable for the full EUR 14.5 million receivable and 182-day maturity. The recommendation remains conditional until the workbook reconciliation and current quote verification are complete.

### Exact independent prompt

The following is the exact prompt submitted in the fresh independent LLM conversation:

```text
I am completing an independent validation of an FX hedging analysis for a corporate EUR receivable.

Use only the two documents I provided:

1. The Stage 2 project specification.
2. The Stage 4 market-data memo.

Treat these documents as the complete source of information. Do not assume missing inputs, introduce outside market data, or rely on information that is not stated in the documents. If something is missing or ambiguous, identify it rather than guessing.

Independently calculate and compare all five outcomes:

- Unhedged exposure
- Forward hedge
- Money-market hedge
- Put option hedge
- Call option hedge

Show the results at three future spot-rate scenarios: 95%, 100%, and 105% of the initial spot rate, unless the documents specify a different required set of scenarios.

For each scenario:

- Show the calculation and resulting USD proceeds for the unhedged exposure, forward, money-market hedge, and put.
- Show the call option payoff and premium separately, and explain its net result.
- Identify which strategy produces the highest proceeds.
- Explain the trade-offs between certainty, flexibility, premium cost, and participation in favorable exchange-rate movements.

For the money-market hedge, explicitly show all three steps and the intermediate calculations.

For the forward, calculate the contracted USD proceeds using the forward rate in the documents.

For the put, account for the premium using the timing and treatment specified in the documents.

Also calculate the CIP-implied forward rate using the market data and conventions provided. Compare it with the bank's quoted forward rate. Quantify the difference in the exchange rate and the USD value of the receivable. Do not assume that the quoted and implied rates should match.

Conclude with a clear hedge recommendation for the company. Support the recommendation using the document's actual inputs and calculated results. Explain the business reasoning behind the choice, including cash-flow certainty, liquidity, optionality, and cost.

Organize the response into:

1. Source inputs and assumptions
2. Hedge calculations
3. Comparison of outcomes at the
