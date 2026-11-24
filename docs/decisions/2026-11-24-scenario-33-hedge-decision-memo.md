# Hedge Decision Memo — Scenario 33: Mining Shovels

**To:** Chief Financial Officer
**From:** Jazmyne Viloria, Treasury Analyst
**Date:** November 24, 2026
**Re:** Hedging the EUR 14,500,000 receivable due in 182 days

## A. Exposure summary

We will receive EUR 14,500,000 in 182 days. At today's spot of 1.0968 USD per EUR (ECB reference rate, Nov. 18, 2026) the receivable is worth USD 15,903,600. Every one-cent move in EURUSD changes our proceeds by USD 145,000. A ±5% move in the euro puts proceeds anywhere between USD 15.11 million and USD 16.70 million if we leave the exposure unhedged.

## B. Hedge outcomes

| Strategy | USD proceeds | What it means for the decision |
|---|---|---|
| Unhedged | 15,108,420 to 16,698,780 (±5% range) | A USD 1.59 million swing and no protection. |
| Forward at the bank's quote (1.1705) | 16,972,250, fixed | Beats the best unhedged outcome in the range by USD 273,470. |
| Money market (borrow EUR, convert, invest USD) | 16,001,553, fixed | Equals a forward at the parity rate of 1.1036. Uses balance sheet and no counterparty quote. |
| Put at spot strike, premium carried | Floor 15,615,758; USD 16,410,938 at +5% | Insurance with upside, but the floor sits USD 1.36 million below the quoted forward. |
| Call (for comparison only) | −439,181 to +355,999 | Pays only if the euro rises. It adds risk to a receivable and is not a hedge. |

The central finding is that the quoted forward beats every other strategy at every point in the ±5% range. That result comes from the quote, not from the market: it sits USD 970,697 above what the money-market route delivers (see D).

## C. Sensitivity interpretation

**If the euro weakens.** The forward and money market are unaffected. The put floors proceeds at USD 15.62 million, which is USD 287,842 below today's value; that is the premium we pay for the protection. Without a hedge, a 5% euro decline costs USD 795,180 against today's value.

**If the euro strengthens.** The forward and money market give up all gains. The unhedged position and the put gain, but not enough to matter in this range: at +5% the put pays USD 16.41 million and the unhedged position USD 16.70 million, both below the forward's 16.97 million. The put overtakes the forward only if the euro rises 8.5% above spot, to 1.1904.

**Cost.** The forward has no upfront cost. The put costs USD 281,300 up front (USD 287,842 carried to settlement, 1.8% of notional). The money market needs EUR 14.26 million of borrowing and is the least convenient route.

## D. Recommendation

**Hedge the full EUR 14,500,000 with a forward, but obtain a requote before we execute.**

**The quote and parity disagree.** The bank's 1.1705 compares with a CIP-implied forward of 1.10356, built from live spot (1.0968), the 1-year Treasury yield (4.60%) and the 12-month Euribor fixing (3.361%). The gap is 0.06694 USD per EUR, or USD 970,697 on the receivable (6.1%).

**What explains it.** The quote appears to be stale. It was set when spot was 1.1165; spot has since fallen 1.8% and the quote has not moved. Even then it sat above parity: with the Stage 3 placeholder inputs the implied forward was 1.1242 and the gap USD 671,463. The rate inputs do not explain it. Using 6-month rates instead moves the implied forward by about 0.001, and mixing a government yield with an interbank rate cannot account for 0.067.

**Does it change the recommendation?** It changes what we do, not which instrument.
- If the bank confirms 1.1705, we should execute at once. That locks in USD 970,697 more than a market-priced hedge, and it beats every other strategy.
- If the requote comes back near parity (about 1.1036), the forward delivers about USD 16.00 million and equals the money market. The put then costs USD 385,795 of certain proceeds (floor 15.62 million) and beats the forward only if the euro rises more than 2.4% to 1.1234. I would still choose the forward unless the board holds a strong view that such a rally is likely.

This recommendation assumes the firm's priority is budget certainty over upside.

## E. Executive justification

- **Cash-flow stability and budget certainty.** The forward fixes USD proceeds today; the money market does the same at a lower level.
- **Liquidity.** The forward needs no upfront cash. The put needs USD 281,300 now, and the money market ties up EUR 14.26 million of borrowing capacity.
- **Optionality.** We give up the chance to benefit from a stronger euro. In this range the cost is small: the put does not beat the forward until the euro is 8.5% above spot at the quoted rate.
- **Premium cost.** The put premium is 1.8% of notional, and carried to settlement it is USD 287,842. That is the price of keeping upside.
- **Next step.** Ask the bank to confirm or requote the forward today; the recommendation and its USD impact depend on that answer.

*Figures come from `capabilities/fx-hedging/model.xlsx` and were verified by hand and compared against an independent LLM run; see `analysis/2026-11-24-scenario-33-validation-analysis.md`.*
