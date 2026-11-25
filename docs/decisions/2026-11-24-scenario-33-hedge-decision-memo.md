# FX Hedging Decision Memo

**To:** Chief Financial Officer  
**From:** Jazmyne Viloria, Treasury Analyst  
**Date:** November 24, 2026  
**Subject:** Hedging Recommendation for EUR 14.5 Million Receivable — Scenario 33

## A. Exposure Summary

The company expects to receive EUR 14.5 million in 182 days from its mining-shovel transaction. Because the company reports in U.S. dollars, the receivable's value depends on the EUR/USD exchange rate at settlement. A weaker euro would reduce the USD proceeds, while a stronger euro would increase them.

The objective is to protect the receivable's USD value while considering the cost, liquidity requirements, and flexibility of each available hedge.

The analysis uses the November 18, 2026 initial spot rate of 1.0968 USD/EUR, the assigned forward rate of 1.1705, and the Stage 4 benchmark interest rates. The assigned option premiums are retained from the scenario assumptions. These are modeled inputs, not confirmed current transaction quotes.

## B. Hedge Outcomes

The forward produces the highest modeled receivable proceeds across the three selected scenarios. At 1.1705 USD/EUR, it locks in $16,972,250 regardless of the settlement spot rate. This provides the strongest cash-flow certainty in the model and exceeds the modeled money-market proceeds by $970,696.89.

The money-market hedge produces approximately $16,001,553.11. It creates a fixed USD outcome by borrowing the present value of the EUR receivable, converting the borrowed euros at spot, and investing the resulting dollars until settlement. Although it provides certainty, it requires immediate borrowing and investment transactions. Actual borrowing and deposit rates could change the result.

The put option establishes a modeled minimum of $15,615,758.21 after carrying the premium to settlement. It protects against euro depreciation while preserving participation in euro appreciation. That flexibility comes at a cost: the carried premium is $287,841.79.

The unhedged position provides full participation in exchange-rate movements but exposes the company to downside risk. At the 95% scenario, proceeds fall to $15,108,420. At the 105% scenario, proceeds increase to $16,698,780.

The call option is a separate derivative position rather than a hedge of the EUR receivable. Its net payoff is negative $439,181.28 at the 95% and 100% scenarios and positive $355,998.72 at the 105% scenario. It does not establish a minimum USD value for the receivable and should not be treated as a substitute for a protective hedge.

## C. Sensitivity Interpretation

The sensitivity analysis illustrates the trade-off between certainty and participation in favorable exchange-rate movements.

If the euro depreciates, the unhedged position loses value as the EUR converts into fewer dollars. The put limits this downside through its strike, although the premium reduces the minimum proceeds. The forward and money-market hedge each lock in a fixed USD outcome.

If the euro appreciates, the unhedged position benefits fully, and the put preserves upside participation above its strike. The forward and money-market hedge do not benefit from further favorable movements because their USD outcomes are fixed.

Under the selected 95%, 100%, and 105% scenarios, the assigned forward rate produces the highest modeled proceeds in every case. The forward's advantage is unusually large because the assigned 1.1705 rate is materially higher than the CIP-implied forward calculated using the updated market inputs. This result should therefore be treated cautiously until the bank confirms the rate.

## D. Recommendation

**Recommendation: Request a current, executable forward quote before committing to the hedge. If the bank confirms that 1.1705 USD/EUR is firm and available for the full EUR 14.5 million receivable and 182-day maturity, the forward is the strongest modeled choice.**

The independent analysis calculates a CIP-implied forward rate of 1.10355539 USD/EUR. Compared with the assigned forward rate of 1.1705, the difference is 0.06694461 USD/EUR. Applied to the receivable, this represents a modeled difference of approximately $970,696.89.

The money-market hedge produces approximately $16,001,553.11, matching the proceeds implied by the modeled CIP rate. This confirms the internal mathematical relationship between the money-market calculation and the CIP-implied forward under the selected inputs. It does not confirm that the assigned forward rate is executable or that the company can borrow and invest at the benchmark rates used.

The difference requires investigation rather than an assumption that the company can capture the full modeled advantage. The Stage 4 analysis identifies the assigned rate as potentially stale and notes that it was established against an earlier spot rate. The USD Treasury yield and EUR Euribor inputs are also different benchmark instruments and tenors, not transaction-specific rates for this company.

The company should request a firm bank requote for the full notional and exact maturity. If the bank confirms the assigned rate, the forward offers the strongest modeled proceeds and certainty. If the bank does not confirm it, Treasury should compare the new executable forward rate with an updated money-market calculation using actual borrowing and investment rates. The put remains an alternative if management values upside participation enough to accept its premium and lower minimum proceeds.

## E. Executive Justification

The forward is the preferred modeled strategy because it establishes a known USD amount for a large receivable and removes uncertainty from the settlement exchange rate. This supports cash-flow planning, budget certainty, and financial forecasting.

The money-market hedge provides a similar fixed outcome but requires immediate access to borrowing capacity and the ability to invest the converted funds. That may affect liquidity and credit availability. Its modeled proceeds should not be relied on until the company confirms the actual rates and financing terms available to it.

The put provides more flexibility by protecting against depreciation while retaining upside participation. However, the premium reduces the minimum USD proceeds, making it less attractive when the primary objective is to maximize a certain settlement amount.

The recommendation is therefore conditional. The assigned forward rate must be verified as current, firm, and executable before the company enters the transaction. Treasury should confirm the rate, maturity, notional, transaction costs, collateral requirements, counterparty exposure, and settlement terms. Until those items are confirmed, the modeled forward advantage should not be treated as a guaranteed commercial benefit.

**Decision requested:** Authorize Treasury to obtain and verify an executable forward quote for the full EUR 14.5 million receivable and return with the confirmed terms before execution.

---

**Basis of analysis:** Stage 2 technical specification, Stage 4 market-data memo, Stage 5 independent LLM analysis, and the recalculated Stage 4 workbook. The modeled calculations have been reconciled to the workbook; completion of the remaining Stage 3 audit corrections and confirmation of transaction-specific market terms remain outstanding.
