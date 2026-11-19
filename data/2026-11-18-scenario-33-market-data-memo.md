# Market-Data Memo — Scenario 33: Mining Shovels FX Transaction Hedge

**Prepared by:** Jazmyne Viloria, Treasury Analyst
**Audience:** CFO / Director of Treasury
**Data retrieved:** November 18, 2026 [add clock time and time zone of your own re-pull]
**Workbook:** `capabilities/fx-hedging/model.xlsx` (Stage 4 population)
**Exposure:** EUR 14,500,000 receivable, 182 days, ACT/360

## 1. Summary

Live market data replaced the placeholder spot and both interest rates. The bank's forward quote and the scenario option premiums were kept. The model recalculated with no broken formulas.

The main result is that **the bank's forward quote (1.1705) is far above the CIP-implied forward (1.10355539) on live data**. The gap is 0.06694461 USD per EUR, which is USD 970,696.89 on the receivable. The quote was set against the earlier spot of 1.1165; spot has since fallen to 1.0968. I have not changed the quote to close the gap. This is flagged for Stage 5 and for a requote (section 5).

## 2. Inputs, sources and timestamps

| Input | Value | Source | Timestamp | Proxy / computation |
|---|---|---|---|---|
| S0_in (EURUSD spot) | 1.0968 USD per EUR | ECB euro foreign exchange reference rate for Nov. 18, 2026 | Nov. 18, 2026 (ECB publishes about 14:15 CET) | None. Read via a republisher of the ECB fix (divisasboe.es/en/exchange-rate-EUR-USD-2026-11-18); re-pull from ecb.europa.eu to confirm. |
| Spot cross-check | 1.0969 to 1.0980 | Trading Economics (1.0969); Forbes Advisor (1.0977, 20:59 UTC); Moneyswapp (1.0980, 23:46 GMT+2) | Nov. 18, 2026 | Confirms the ECB fix is in line with market quotes; not used in the model. |
| R_USD | 4.60% | U.S. Treasury daily par yield curve, 1-year | Nov. 17, 2026 close (latest published at retrieval) | Government yield used as the USD reference rate. Cross-read on stockmarketwatch.com/bonds (4.60%). |
| R_FC | 3.361% | 12-month Euribor fixing (EMMI), as listed on euribor-rates.eu | Nov. 17, 2026 (latest published at retrieval) | Interbank reference rate used as the EUR rate. |
| F0_in | 1.1705 USD per EUR | Scenario 33 bank quote | Scenario input | **Retained.** Not replaced by the CIP-implied forward. |
| K_PUT, K_CALL | 1.0968 | Formula-linked to S0_in | Same as spot | At the money, per the scenario convention. |
| PREM_PUT | 0.0194 USD per EUR | Scenario 33 instructions | Scenario input | **Assumption.** Kept; retail option quotes are unreliable. |
| PREM_CALL | 0.0296 USD per EUR | Scenario 33 instructions | Scenario input | **Assumption.** Kept; same reason. |
| FC_AMT | EUR 14,500,000 | Scenario 33 instructions | Scenario input | None. |
| T_DAYS | 182 | Scenario 33 instructions | Scenario input | None. |

The spot is dated Nov. 18 and the rates Nov. 17 because the Nov. 18 rate fixings were not yet published at retrieval. The one-day difference is small relative to the quote gap below.

## 3. Rate choices

- **R_USD: 1-year Treasury par yield (4.60%).** Chosen because the Stage 4 specification calls for 1-year rates, and a government yield is a clean, published, risk-free USD reference.
- **R_FC: 12-month Euribor (3.361%).** Chosen for the same tenor, and because Euribor is a published ACT/360 euro money-market benchmark.
- **Limitations:** The scenario horizon is 182 days, so the 1-year rates are applied as annualized rates over 182 days, as in the CIP formula. The USD rate is a government yield and the EUR rate is an interbank rate, so they are not a like-for-like credit pair, and the Treasury yield is not quoted on an ACT/360 basis. Using 6-month rates instead (Treasury 4.41%, 6-month Euribor 3.007%) gives an implied forward of 1.10446 and a gap of USD 957,536, so the conclusion does not depend on the tenor.

## 4. CIP-implied forward and comparison with the quote

F = S0 × (1 + R_USD × T/360) / (1 + R_FC × T/360)

- DF_USD = 1 + 0.046 × 182/360 = 1.02325556
- DF_FC = 1 + 0.03361 × 182/360 = 1.01699172
- **CIP-implied forward = 1.0968 × 1.02325556 / 1.01699172 = 1.10355539**

| Measure | Value |
|---|---|
| Bank quote (F0_in) | 1.17050000 |
| CIP-implied forward | 1.10355539 |
| Gap (quote less implied), rate | 0.06694461 USD per EUR (quote is about 6.1% above implied) |
| Gap, USD on the receivable | USD 970,696.89 |
| Quote premium over spot | 6.72% (implied: 0.62%) |
| Forward proceeds (FC_AMT × F0_in) | USD 16,972,250.00 |
| Money-market proceeds | USD 16,001,553.11 |

## 5. Requote

The gap is large enough that I think the quote is stale rather than a pricing difference. Per the Stage 4 instructions the quote stays in the workbook and I should ask for a requote. [YOU: record here whether you asked your instructor for a requote, the date, and any new quote. If a requote arrives, replace F0_in with it and re-run the checks.]

## 6. Populating the workbook and structural fixes

Live values were entered in `S0_in`, `R_USD` and `R_FC` (Inputs C6, C8, C9). All hedge formulas, the strikes, the sensitivity table and the chart recalculated with no errors. The structural points found:

1. **Hard-coded expectation on the Sensitivity sheet (B39).** It said "Forward wins 95%–104%; unhedged wins at 105%," which was true only for the placeholder inputs. On live data the forward wins all 11 scenarios because 1.1705 is above every grid rate (the highest is 1.15164). I did not change the inputs to make it true. I relabelled B39 as the Stage 3 placeholder expectation and added B41, a formula that reports the live winner count.
2. **No gap reporting.** The workbook could show the gap only indirectly (Money Market Hedge B26). I added rows 30–32 on that sheet: gap as a rate, gap in USD, and the quote's premium over spot.
3. **Parity check cannot fail.** The parity difference compares the money-market result with FC_AMT × F_IMPLIED, and both come from the same inputs, so it is zero by algebra. It confirms the arithmetic but does not test the data. The meaningful test is the quote against the implied forward in section 4. I left the check as it is and noted this.
4. **Display only:** R_FC now shows three decimals (3.361%) so the Euribor value is visible.
5. **Documentation updated:** provenance text on Inputs, Cover and Notes, the build and access dates, and the expected check figures on the Notes sheet (now Stage 4 values).

## 7. Re-run checks

| Check | Result |
|---|---|
| Parity (Money Market Hedge B23, B24) | Difference USD 0.00; PASS (tolerance USD 0.01). See point 3 above on what this does and does not test. |
| Quote vs CIP-implied forward | Gap of 0.06694461 (USD 970,696.89). Not a pass or fail; recorded for Stage 5. |
| Put floor (Sensitivity B21) | PASS. Floor USD 15,615,758.21. |
| Sensitivity grid | Re-centred on the new spot: 1.04196 to 1.15164. Forward is the highest-proceeds strategy in all 11 scenarios. |
| Errors | None on any sheet. |

Other outputs: put premium carried USD 287,841.79; call premium carried USD 439,181.28; money-market borrowing EUR 14,257,736.50; USD converted today USD 15,637,885.40.

## 8. FX Hedging Lab cross-check

[YOU: enter the live inputs below into the FX Hedging Lab and record its outputs here. If any differ from the workbook, find which one is wrong and record the resolution.]

Inputs to enter: spot 1.0968; R_USD 4.60%; R_FC 3.361%; forward quote 1.1705; T = 182 days; EUR 14,500,000; strikes 1.0968; premiums 0.0194 and 0.0296.

| Output | Workbook | FX Lab | Match? |
|---|---|---|---|
| Forward proceeds | USD 16,972,250.00 | [YOU] | [YOU] |
| Money-market proceeds | USD 16,001,553.11 | [YOU] | [YOU] |
| CIP-implied forward | 1.10355539 | [YOU] | [YOU] |
| Put floor | USD 15,615,758.21 | [YOU] | [YOU] |

I also recomputed these values independently outside the workbook, and they agree with it to the cent. That is not a substitute for the Lab comparison.

## 9. Assumptions and limitations

- Option premiums are the scenario's indicative values, set when spot was 1.1165. They were not re-priced for the new spot and are not live quotes.
- Transaction costs, counterparty risk and taxes remain excluded, as in earlier stages.
- Sources outside the ECB, Treasury and EMMI sites were used to read the values; every number should be re-pulled from the primary source by anyone auditing this memo.

## 10. AI assistance

Claude (Anthropic) helped locate the market data, compute the CIP-implied forward, update the workbook and draft this memo. Logged in `prompt-log.md`.
