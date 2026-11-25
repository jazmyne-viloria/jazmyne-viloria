# Scenario 33 — Independent Validation Analysis

**Project:** FX Hedging Analysis — Mining Shovels  
**Stage:** 5 — Independent Validation and CFO Decision  
**Prepared by:** Jazmyne Viloria  
**Date:** November 24, 2026  
**Exposure:** EUR 14,500,000 receivable  
**Settlement:** 182 days (ACT/360)

---

## 1. Purpose and Validation Scope

This analysis compares the independent LLM's calculations against the Stage 3 Excel hedging workbook using the updated market inputs documented in Stage 4.

The independent LLM received exactly two source documents:

1. `capabilities/fx-hedging/spec.md` — Stage 2 technical specification, Version 0.4.
2. `data/2026-11-18-scenario-33-market-data-memo.md` — Stage 4 market-data memo.

The independent LLM did not receive the Excel workbook and was instructed not to use outside market data, assume missing inputs, or revise its calculations based on feedback.

The validation covers:

- Unhedged exposure
- Forward hedge
- Money-market hedge
- EUR put option
- EUR call option
- Three settlement spot-rate scenarios
- Covered interest-rate parity (CIP)
- Option premium treatment
- Workbook calculations and visible validation
- Differences between the original Stage 2 check figures and the updated Stage 4 market inputs

The purpose is to determine whether the independent calculations agree with the recalculated workbook, identify remaining modeling and documentation issues, and establish whether the results support the provisional CFO recommendation.

## 2. Source Inputs and Assumptions

The analysis uses the following Stage 4 inputs while retaining the calculation methodology established in Stage 2 Version 0.4.

| Input | Value | Classification |
|---|---:|---|
| EUR receivable | €14,500,000 | Scenario input |
| Settlement horizon | 182 days | Scenario input |
| Initial spot rate | 1.0968 USD/EUR | ECB reference rate, November 18, 2026 |
| Assigned forward rate | 1.1705 USD/EUR | Scenario-provided indicative quote |
| USD interest rate | 4.60% | U.S. Treasury 1-year par yield |
| EUR interest rate | 3.361% | 12-month Euribor |
| Put strike | 1.0968 USD/EUR | Assumption: equal to initial spot |
| Call strike | 1.0968 USD/EUR | Assumption: equal to initial spot |
| Put premium | 0.0194 USD/EUR | Scenario-provided indicative premium |
| Call premium | 0.0296 USD/EUR | Scenario-provided indicative premium |
| Day-count convention | ACT/360 | Stage 2 specification |

The assigned forward rate and option premiums are not confirmed executable market quotes.

The USD and EUR benchmark rates are different instruments and are not transaction-specific 182-day borrowing or deposit quotes. The USD rate is a U.S. Treasury 1-year par yield, while the EUR rate is a 12-month Euribor fixing. Both are applied as annualized rates over the 182-day horizon.

The model therefore provides a benchmark comparison rather than a fully executable treasury quote comparison.

### 2.1 Derived accumulation factors

The workbook uses simple ACT/360 accumulation factors:

\[
DF_{USD}=1+R_{USD}\times\frac{T}{360}
\]

\[
DF_{EUR}=1+R_{EUR}\times\frac{T}{360}
\]

Substituting the inputs:

\[
DF_{USD}=1+0.046\times\frac{182}{360}
=1.02325556
\]

\[
DF_{EUR}=1+0.03361\times\frac{182}{360}
=1.01699172
\]

These factors are used in the money-market hedge, CIP calculation, and option premium treatment.

## 3. Independent LLM Versus Workbook Comparison

The independent LLM and recalculated workbook were compared at 95%, 100%, and 105% of the initial spot rate.

The future spot rates are:

| Scenario | Calculation | Future spot |
|---|---|---:|
| 95% | 1.0968 × 0.95 | 1.04196 |
| 100% | 1.0968 × 1.00 | 1.09680 |
| 105% | 1.0968 × 1.05 | 1.15164 |

### 3.1 Comparison of USD outcomes

All amounts are rounded to two decimal places.

| Strategy | 95% spot | 100% spot | 105% spot |
|---|---:|---:|---:|
| Unhedged | $15,108,420.00 | $15,903,600.00 | $16,698,780.00 |
| Forward | $16,972,250.00 | $16,972,250.00 | $16,972,250.00 |
| Money market | $16,001,553.11 | $16,001,553.11 | $16,001,553.11 |
| Put | $15,615,758.21 | $15,615,758.21 | $16,410,938.21 |
| Call gross payoff | $0.00 | $0.00 | $795,180.00 |
| Call net payoff | -$439,181.28 | -$439,181.28 | $355,998.72 |

The independent LLM's results match the recalculated workbook outputs at all three scenarios.

The call is displayed separately because it is an option payoff, not total proceeds from converting the EUR receivable.

### 3.2 Scenario interpretation

**At 95% of initial spot:** The euro depreciates to 1.04196 USD/EUR. The unhedged company receives $15,108,420. The put maintains a minimum modeled receivable outcome of $15,615,758.21 after the carried premium. The forward produces $16,972,250 under the assigned quote.

**At 100% of initial spot:** The settlement rate remains at 1.0968 USD/EUR. The forward produces $16,972,250, compared with $16,001,553.11 from the money-market hedge and $15,615,758.21 from the put.

**At 105% of initial spot:** The euro appreciates to 1.15164 USD/EUR. Unhedged proceeds increase to $16,698,780, while the put preserves upside participation and produces $16,410,938.21 after its premium. The forward produces $16,972,250 under the assigned quote.

The forward wins all three requested scenarios and all eleven scenarios in the workbook's 95%–105% sensitivity grid.

This result is conditional on the assigned 1.1705 forward rate. It does not establish that a bank will execute a transaction at that rate.

### 3.3 Call option treatment

The call option is a separate payoff comparison rather than a hedge of the EUR receivable.

Its strike equals the initial spot rate of 1.0968 USD/EUR.

At 95% and 100% spot, the call expires without gross payoff. The carried premium produces a net loss of $439,181.28.

At 105% spot:

\[
Call\ Gross\ Payoff =
14,500,000\times(1.15164-1.0968)
\]

\[
=\$795,180
\]

After deducting the carried premium:

\[
Call\ Net\ Payoff =
795,180-439,181.28
\]

\[
=\$355,998.72
\]

The independent LLM and workbook agree on the call's gross and net outcomes.

## 4. Hand Verification of Workbook Calculations

The following calculations provide the arithmetic required to independently check the workbook. The checks must be completed with a calculator rather than Excel. The calculations shown here are the verification trail; the analyst should mark the checks complete only after personally confirming the arithmetic.

### 4.1 Forward hedge

The company converts the full EUR receivable at the assigned indicative forward rate.

\[
USD_{FWD}=FC_{AMT}\times F_0
\]

\[
=14,500,000\times1.1705
\]

\[
=\boxed{\$16,972,250}
\]

The result is constant across settlement spot scenarios because the forward rate is locked in.

### 4.2 Money-market hedge

The money-market hedge has three steps.

**Step 1 — Borrow EUR today**

\[
MM\ Borrow_{EUR}=\frac{FC_{AMT}}{DF_{EUR}}
\]

\[
=\frac{14,500,000}{1.0169917222}
\]

\[
=\boxed{€14,257,736.50}
\]

The EUR borrowing accumulates to the receivable amount at maturity:

\[
14,257,736.50\times1.0169917222
=\boxed{€14,500,000}
\]

**Step 2 — Convert borrowed EUR to USD**

\[
MM\ USD_{Now}=MM\ Borrow_{EUR}\times S_0
\]

\[
=14,257,736.50\times1.0968
\]

\[
=\boxed{\$15,637,885.40}
\]

**Step 3 — Invest USD until settlement**

\[
USD_{MM}=MM\ USD_{Now}\times DF_{USD}
\]

\[
=15,637,885.40\times1.0232555556
\]

\[
=\boxed{\$16,001,553.11}
\]

### 4.3 Put option

The put premium is paid upfront and carried to settlement using the USD accumulation factor, as specified in Stage 2 Version 0.4.

Upfront premium:

\[
14,500,000\times0.0194
=\$281,300
\]

Carried premium:

\[
PREM_{PUT,FV}=281,300\times1.0232555556
\]

\[
=\boxed{\$287,841.79}
\]

The strike equals the initial spot rate:

\[
K_{PUT}=1.0968
\]

Minimum modeled proceeds:

\[
USD_{FLOOR}=FC_{AMT}\times K_{PUT}-PREM_{PUT,FV}
\]

\[
=14,500,000\times1.0968-287,841.79
\]

\[
=\boxed{\$15,615,758.21}
\]

### 4.4 Call option

The call premium is also paid upfront and carried to settlement.

Upfront premium:

\[
14,500,000\times0.0296
=\$429,200
\]

Carried premium:

\[
PREM_{CALL,FV}=429,200\times1.0232555556
\]

\[
=\boxed{\$439,181.28}
\]

At 105% of initial spot, the gross call payoff is $795,180 and the net payoff is $355,998.72.

### 4.5 Calculator verification record

Complete these checks before submitting the validation analysis:

- [ ] Forward proceeds independently checked with a calculator.
- [ ] EUR borrowing independently checked with a calculator.
- [ ] USD conversion independently checked with a calculator.
- [ ] USD investment proceeds independently checked with a calculator.
- [ ] Put carried premium and minimum proceeds independently checked with a calculator.

The arithmetic above should be retained in the final submission. Do not mark a check complete unless you personally performed it.

## 5. Covered Interest-Rate Parity Analysis

The CIP-implied forward rate is calculated using the spot rate and the USD and EUR accumulation factors.

\[
F_{IMPLIED}=S_0\times\frac{DF_{USD}}{DF_{EUR}}
\]

\[
=1.0968\times\frac{1.0232555556}{1.0169917222}
\]

\[
=\boxed{1.10355539\ USD/EUR}
\]

The parity-implied USD proceeds are:

\[
14,500,000\times1.1035553867
=\boxed{\$16,001,553.11}
\]

The money-market hedge also produces $16,001,553.11.

\[
PARITY\ Difference=USD_{MM}-USD_{CIP}
\]

\[
=16,001,553.11-16,001,553.11
\]

\[
=\boxed{\$0.00}
\]

The workbook's parity validation passes its $0.01 tolerance.

### 5.1 Assigned forward versus CIP-implied forward

The assigned forward rate is 1.1705 USD/EUR, compared with the CIP-implied rate of 1.10355539 USD/EUR.

\[
Rate\ Difference=1.1705-1.10355539
\]

\[
=\boxed{0.06694461\ USD/EUR}
\]

The corresponding modeled USD difference on the receivable is:

\[
14,500,000\times0.0669446133
\]

\[
=\boxed{\$970,696.89}
\]

The assigned quote is approximately 6.07% above the CIP-implied rate.

This is a modeled comparison, not guaranteed savings. The CIP rate is calculated using benchmark interest rates rather than transaction-specific borrowing and deposit quotes. The USD Treasury yield and EUR Euribor fixing also differ in instrument and credit basis.

The assigned 1.1705 rate must be treated as an indicative scenario quote unless the bank confirms that it is currently executable for the full EUR 14.5 million notional and 182-day maturity.

## 6. Discrepancy Diagnosis

This section classifies the differences and validation issues using the required categories: LLM error, workbook error, or specification ambiguity. A difference caused by updated inputs is not automatically a calculation error.

### 6.1 Independent LLM versus recalculated workbook

No numerical discrepancies were identified in the independent LLM's forward, money-market, put, call, or CIP calculations at the three requested settlement scenarios.

| Item reviewed | Finding | Classification |
|---|---|---|
| Unhedged proceeds | Independent results agree with recalculated workbook outputs at all three scenarios. | No discrepancy identified |
| Forward proceeds | Both calculate $16,972,250 using the assigned 1.1705 rate. | No discrepancy identified |
| Money-market hedge | All three steps and final proceeds agree. | No discrepancy identified |
| Put option | Carried premium and minimum proceeds agree. | No discrepancy identified |
| Call option | Gross payoff, carried premium, and net payoff agree. | No discrepancy identified |
| CIP calculation | Money-market proceeds and CIP-implied proceeds agree within the $0.01 tolerance. | No discrepancy identified |

The recalculated workbook produced the same numerical outcomes as the independent calculations. This numerical agreement does not, by itself, establish that every Stage 3 build and presentation requirement has passed.

### 6.2 Difference from original Stage 2 sensitivity expectations

Stage 2 Version 0.4 includes expected sensitivity results using its original placeholder market inputs. Those inputs included an initial spot rate of 1.1165 USD/EUR and different benchmark interest rates.

Stage 4 replaced the spot and interest-rate inputs with updated November 17–18 observations.

Consequently, the original Stage 2 sensitivity expectation—that the forward wins from 95% through 104% and unhedged exposure wins at 105%—does not describe the updated Stage 4 results.

With the updated initial spot of 1.0968, the 105% settlement spot is 1.15164, which remains below the assigned forward rate of 1.1705. The forward therefore wins all eleven modeled sensitivity scenarios.

**Discrepancy classification: Specification/input-version ambiguity — not an LLM or workbook calculation error.** The original Stage 2 sensitivity expectations were based on the original market inputs, while the Stage 4 analysis uses updated spot and interest-rate observations. The difference in the winning strategy at the 105% scenario results from those changed inputs. The independent LLM and recalculated workbook agree when both use the Stage 4 inputs. Future specification versions should explicitly state that updated Stage 4 observations supersede the corresponding Stage 2 market inputs and that sensitivity outcomes must be recalculated accordingly.

### 6.3 Workbook metadata issue

The uploaded workbook's Cover worksheet identifies the specification as Version 0.3, dated November 9, 2026.

The repository's Stage 2 technical specification is Version 0.4, dated November 13, 2026.

**Discrepancy classification: Workbook documentation error.** The Cover worksheet metadata does not identify the governing specification version used for the updated analysis. Update the workbook Cover metadata to Version 0.4 and November 13, 2026, then confirm that the corrected workbook is the version committed to the repository.

This metadata issue does not change the numerical calculations, but it weakens the audit trail between the specification and workbook.

### 6.4 Visible validation gap

The reviewed workbook displays the parity check and put floor validation. It also displays numerical check figures in Notes & Assumptions. However, it does not provide a separate formula-driven PASS/FAIL comparison for every numerical check figure listed in the Stage 2 build requirements.

**Discrepancy classification: Workbook validation-presentation gap.** The missing individual comparisons do not establish that the underlying calculations are wrong. They mean that the workbook does not visibly demonstrate every required validation result. The Stage 3 build audit should document which comparisons were manually verified, or the workbook should be expanded to display the individual checks.

## 7. Workbook Structure and Stage 3 Build Audit

The uploaded workbook was reviewed for its structure and calculation outputs. It contains the following eight worksheets:

1. Cover
2. Legend-Key
3. Inputs
4. Forward Hedge
5. Money Market Hedge
6. Option Hedge
7. Sensitivity
8. Notes & Assumptions

The ten standardized input named ranges were present in the reviewed workbook. The strike assumptions were formula-linked to the initial spot rate.

The Sensitivity worksheet contains eleven settlement scenarios from 95% through 105% of initial spot in 1% increments. Its line chart is linked to the sensitivity table.

The recalculated workbook produced no formula errors. The parity check returned PASS, and the put floor validation returned PASS.

These findings apply to the uploaded workbook reviewed for this analysis. The workbook must still be committed at its canonical repository path and checked to ensure the committed copy is the same version.

### 7.1 Seven Stage 3 build requirements

| Stage 3 requirement | Review finding | Status |
|---|---|---|
| 1. Named ranges | Ten standardized input names were present in the uploaded workbook. | Confirm in committed copy |
| 2. Formula integrity | Recalculated workbook produced no formula errors. | Confirm in committed copy |
| 3. Cover | Scenario and workbook metadata require review; specification version and date need correction. | Revision required |
| 4. Legend-Key | Worksheet exists. Confirm the required input, assumption, formula, and output colors are consistently applied. | Visual confirmation required |
| 5. Hedge families | Forward, three money-market steps, put, and separate call payoff were included in the reviewed calculations. | Confirm in committed copy |
| 6. Sensitivity | Eleven scenarios and a linked chart were present in the uploaded workbook. | Confirm in committed copy |
| 7. Visible validation | Parity and put floor checks displayed PASS, but individual formula-driven comparisons for every numerical check figure were not separately displayed. | Additional validation documentation or workbook revision required |

The seven requirements should not be described as all passing. The uploaded workbook's core calculations reconcile with the independent results, but the Cover metadata, Legend-Key visual review, committed-copy verification, and complete visible validation evidence remain outstanding.

## 8. Specification Retrospective

### 8.1 What the specification communicated clearly

The Stage 2 Version 0.4 specification provided enough detail for the independent LLM to calculate the five outcomes without receiving the workbook.

It clearly defined:

- The EUR receivable and settlement horizon.
- Exchange-rate convention.
- The standardized model inputs.
- ACT/360 accumulation factors.
- All three money-market hedge steps.
- The forward calculation.
- Put and call payoff formulas.
- Option premium timing.
- The sensitivity range.
- The CIP calculation and validation tolerance.
- The distinction between the call payoff and total receivable proceeds.

The independent LLM's calculations matched the recalculated workbook outputs using the Stage 4 inputs.

### 8.2 What required interpretation or qualification

The main area requiring careful interpretation was the relationship between the Stage 2 assumptions and Stage 4 market updates. The independent LLM needed to use the updated Stage 4 spot and benchmark rates while retaining the scenario-provided forward quote and option premiums.

The Stage 4 data also required a distinction between benchmark rates and transaction-specific financing terms. The model uses a U.S. Treasury yield and a Euribor fixing as reference inputs, not executable borrowing and investment quotes.

The 1.1705 forward quote required further qualification because the documents do not establish that it is currently executable. The original Stage 2 sensitivity expectations also needed to be interpreted in light of the updated Stage 4 inputs. Without a clear input hierarchy, a reader could mistake a changed sensitivity outcome for a calculation error.

Finally, although the specification provides numerical check figures, the workbook's visible validation does not separately compare every check figure with a formula-driven PASS/FAIL result. This makes it harder for another reviewer to verify the model's integrity directly from the workbook.

### 8.3 Recommended Version 2 improvements

Future versions should:

1. State the input hierarchy explicitly: Stage 4 replaces only the dated market observations, while scenario-provided values remain unchanged unless the assignment directs otherwise.
2. Require the model to distinguish assigned indicative quotes from confirmed executable market quotes.
3. Require a clear label whenever benchmark rates are used as proxies for transaction-specific financing rates.
4. Identify expected sensitivity results as dependent on the original placeholder inputs, so later market-data updates do not appear to create unexplained errors.
5. Require workbook Cover metadata to match the exact specification version used to build the workbook.
6. Require formula-driven PASS/FAIL comparisons for each numerical check figure, with stated tolerances.
7. Require the workbook audit to distinguish numerical formula validation from visual, documentation, and repository checks.

These changes would make the specification easier to interpret independently and make the workbook's validation evidence more transparent.

## 9. Recommendation and Validation Status

The forward hedge produces the highest modeled receivable proceeds in all eleven settlement scenarios using the Stage 4 inputs.

It also provides certainty over the USD amount received at maturity if executed at the modeled rate, avoiding the downside exposure associated with an unhedged receivable and the premium cost associated with the put option.

The money-market hedge produces $16,001,553.11, approximately $970,696.89 below the proceeds modeled using the assigned forward quote. However, this difference should not be interpreted as guaranteed savings because the assigned forward is not confirmed executable and the benchmark rates are not transaction-specific financing quotes.

The provisional recommendation is to request a current executable forward quote from the bank for the full EUR 14.5 million receivable and 182-day maturity.

If the bank confirms that the 1.1705 rate is executable for the required notional and maturity, the forward is the strongest modeled choice based on proceeds certainty and the sensitivity results.

If the bank cannot confirm that rate, the company should reassess the forward against updated executable market quotes and actual borrowing and deposit terms.

### Validation status

The independent LLM calculations reconcile with the recalculated workbook outputs for the five strategy outcomes at the three requested spot scenarios.

The reviewed calculations, money-market steps, option premium treatment, and CIP parity check agree.

The following items remain before the project can be considered fully documented and ready for submission:

- Complete the calculator verification record in Section 4.5.
- Correct the workbook Cover worksheet's specification version and date.
- Confirm that the committed workbook is the same version that was recalculated and reviewed.
- Visually confirm the Legend-Key color conventions in the committed workbook.
- Confirm that the Stage 3 build audit records all seven instructor checks without claiming unsupported PASS results.
- Add or document the individual numerical validation comparisons required by the Stage 2 specification.
- Confirm that the Stage 4 source memo matches the inputs and provenance recorded in the workbook.
- Obtain a current executable forward quote before treating the recommendation as an executable treasury decision.

The independent calculation comparison is complete for the reviewed workbook and stated scenarios. The full Stage 3 build audit and documentation checks are not yet all confirmed. The recommendation remains conditional on market quote confirmation.

---

**Source Documents:**

1. Stage 2 — Scenario 33 Technical Specification, Version 0.4.
2. Stage 4 — Market-Data Memo, November 18, 2026.
3. Stage 3 — Recalculated FX hedging workbook.

**End of Stage 5 Independent Validation Analysis**
