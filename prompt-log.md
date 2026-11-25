# Prompt Log

## 2026-09-22

- **What I asked:** Set up a public graduate business course portfolio repository with a defined file structure, tailored AI instructions, a resume, a prompt log, capability folders, documentation folders, data and analysis folders, and a Git ignore file; show every file before committing anything.
- **What you produced:** A complete proposed repository structure and draft contents for all requested files, including tailored `AGENTS.md`, `CLAUDE.md`, `RESUME.md`, `README.md`, `prompt-log.md`, `.gitignore`, capability documentation, and directory placeholder files.
- **What was wrong:** Nothing identified yet; the files had not been committed or tested.
- **How it was caught:** Human review was required before repository changes were committed.

---

## 2026-11-24 — Stage 5: Independent Validation and Decision Memo

- **What I asked:** Use a fresh, independent LLM conversation to calculate and compare all five FX hedging outcomes, evaluate three future spot-rate scenarios, calculate the CIP-implied forward rate, and develop a hedge recommendation using only the Stage 2 technical specification and Stage 4 market-data memo.
- **What I provided:** Exactly two source documents: `capabilities/fx-hedging/spec.md` and `data/2026-11-18-scenario-33-market-data-memo.md`. I did not provide the Excel workbook or coach the model during the independent analysis.
- **Independent prompt:** The exact prompt submitted in the fresh independent LLM conversation is reproduced below.
- **What the LLM produced:** An independent analysis of the unhedged exposure, forward hedge, money-market hedge, put option, call option, sensitivity scenarios, CIP-implied forward rate, and hedge recommendation. The output identified missing information and ambiguities rather than assuming unavailable inputs.
- **What I checked:** I compared the independent LLM's calculations against the recalculated Stage 3 workbook at 95%, 100%, and 105% of the initial spot rate. The five strategy outcomes, including the separate call option payoff and premium, agreed across the three scenarios. The CIP-implied forward rate and money-market hedge proceeds also reconciled. The workbook's parity check returned PASS, and its put floor validation returned PASS. Personal calculator verification of the forward, all three money-market steps, and the put calculation remains a separate completion item.
- **What was wrong or unclear:** No numerical discrepancies were identified between the independent LLM and the recalculated workbook for the reviewed scenarios. However, the workbook Cover worksheet still identified the specification as Version 0.3 dated November 9, 2026, while the repository's governing specification is Version 0.4 dated November 13, 2026. The workbook also does not visibly provide a separate formula-driven PASS/FAIL comparison for every numerical check figure. The original Stage 2 sensitivity expectations use earlier market inputs, so the updated Stage 4 results must be interpreted using the revised inputs. The assigned 1.1705 forward rate is not confirmed as executable, and the benchmark interest rates are not transaction-specific financing quotes.
- **What I changed:** I documented the independent LLM output and completed the Stage 5 validation analysis. The validation analysis compares the LLM results with the workbook, shows the hand-calculation arithmetic, evaluates CIP, classifies the identified documentation and input-version issues, and includes a specification retrospective with recommended Version 2 improvements. It distinguishes calculation agreement from completion of every Stage 3 build and documentation requirement.
- **Final result:** The forward hedge produces the highest modeled proceeds across all eleven workbook sensitivity scenarios using the updated Stage 4 inputs. The provisional recommendation is to obtain a current executable bank quote for the full EUR 14.5 million receivable and 182-day maturity. If the bank confirms the assigned 1.1705 rate, the forward is the strongest modeled choice. The $970,696.89 difference from the CIP-implied comparison is a modeled difference, not guaranteed savings. The recommendation remains conditional on quote confirmation and completion of the remaining workbook documentation and verification checks.

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
3. Comparison of outcomes at the selected spot-rate scenarios
4. CIP-implied forward versus bank quote
5. Recommendation and rationale
6. Missing information, ambiguities, or assumptions you had to make

Show formulas and intermediate arithmetic so the calculations can be independently checked. Clearly label any result that cannot be calculated from the two documents.



Do not ask me questions or revise your calculations based on feedback. Complete the analysis independently using only the two documents.
```
