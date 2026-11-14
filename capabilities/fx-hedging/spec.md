
# Scenario 33 — Mining Shovels FX Transaction Hedge Model

## Technical Specification

| Field | Value |
|---|---|
| **Created by** | Jazmyne Viloria |
| **Updated by** | Jazmyne Viloria |
| **Date Created** | 2026-11-05 |
| **Date Updated** | 2026-11-13 |
| **Version** | 0.4 |
| **LLM Used** | ChatGPT — specification review and AI-assisted workbook build |
| **Role** | Treasury Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | `model.xlsx` — Stage 3 build target |

---

## 1. Problem Statement

A U.S. mining-shovel manufacturer has an outstanding receivable of **EUR 14,500,000** from a Finnish nickel miner, payable in **182 days**. Because the receivable is denominated in euros while the manufacturer's reporting currency is U.S. dollars, a depreciation of the euro against the dollar would reduce the USD proceeds received at settlement.

The objective is to compare the unhedged exposure with forward, money-market, and option-based strategies. The model evaluates potential USD proceeds, downside protection, and upside participation to support the company's hedging decision.

**Exposure details:**

- Exposure type: Foreign-currency receivable
- Functional/reporting currency: USD
- Foreign currency: EUR
- Receivable amount: EUR 14,500,000
- Settlement horizon: 182 days
- Exchange-rate convention: USD per EUR
- Objective: Protect the USD value of the receivable while comparing the costs and benefits of available hedging strategies.

## 2. Inputs (Known Variables)

All ten required inputs must be exposed as standardized Excel named ranges. Each named range must refer to its corresponding input cell on the `Inputs` worksheet. Calculation formulas must reference these names rather than individual cell addresses.

Scenario-provided values are indicative assignment inputs. Spot and interest-rate inputs use the dated market observations recorded below. Sources and access dates must also be recorded in the `Notes & Assumptions` worksheet.

### 2.1 Named-Range Contract

All exchange rates are quoted in USD per EUR. Interest rates are entered as decimals in calculations. The model uses an ACT/360 day-count convention.

| Named Range | Description | Value | Unit | Source |
|---|---|---:|---|---|
| `FC_AMT` | Foreign-currency receivable | 14,500,000 | EUR | Scenario 33 instructions |
| `S0_in` | Spot exchange rate at inception | 1.1165 | USD per EUR | ECB euro foreign exchange reference rate, November 6, 2026 |
| `F0_in` | Assigned forward exchange rate | 1.1705 | USD per EUR | Scenario 33 instructions; assigned indicative forward rate |
| `R_USD` | USD annual interest rate | 3.88% | Annual % | Federal Reserve H.15, effective federal funds rate, November 6, 2026 |
| `R_FC` | EUR annual interest rate | 2.50% | Annual % | ECB deposit facility rate, effective September 16, 2026; in effect November 6, 2026 |
| `K_PUT` | EUR put option strike price | `=S0_in` | USD per EUR | Scenario 33 assumption: strike at inception spot |
| `K_CALL` | EUR call option strike price | `=S0_in` | USD per EUR | Scenario 33 assumption: strike at inception spot |
| `PREM_PUT` | EUR put option premium | 0.0194 | USD per EUR | Scenario 33 instructions; assigned indicative premium |
| `PREM_CALL` | EUR call option premium | 0.0296 | USD per EUR | Scenario 33 instructions; assigned indicative premium |
| `T_DAYS` | Days to settlement | 182 | Days | Scenario 33 instructions |

**Market Data Conventions and Sources**

- `S0_in` is the ECB euro foreign exchange reference rate for November 6, 2026. The ECB reported 1 EUR = 1.1165 USD.  
  Source: https://www.ecb.europa.eu/stats/shared/pdf/eurofxref.pdf
- `R_USD` is the Federal Reserve effective federal funds rate of 3.88% reported for November 6, 2026.  
  Source: https://www.federalreserve.gov/releases/H15/Current/
- `R_FC` is the ECB deposit facility rate of 2.50%, effective September 16, 2026, and in effect on November 6, 2026.  
  Source: https://www.ecb.europa.eu/stats/policy_and_exchange_rates/key_ecb_interest_rates/html/index.en.html
- The USD and EUR rates are overnight benchmark proxies, not transaction-specific borrowing or deposit quotes or matched 182-day market rates. They are used as annualized reference rates for the model's ACT/360 calculations.
- Market-data observation date: November 6, 2026.
- Market-data access date: November 9, 2026.

**Scenario-Provided Values**

The assigned forward rate (`F0_in`) and option premiums (`PREM_PUT` and `PREM_CALL`) are scenario-provided indicative values. They remain unchanged unless the assignment specifies otherwise.

**Input Conventions**

- All exchange rates must use USD per EUR. A higher rate indicates euro appreciation.
- `K_PUT` and `K_CALL` must equal `S0_in`.
- Interest rates must be entered as annualized decimals.
- Use ACT/360 for both interest-rate calculations.
- The assigned forward rate and option premiums are indicative assignment values, not live market quotes.
- Do not use the template's example GBP values or one-year assumptions.

### 2.2 Derived Values

The following values must be calculated from the standardized input named ranges. They must be formula-driven and update when the inputs change.

| Name | Description | Calculation | Unit |
|---|---|---|---|
| `DF_USD` | USD accumulation factor | `1 + R_USD * T_DAYS / 360` | Factor |
| `DF_FC` | EUR accumulation factor | `1 + R_FC * T_DAYS / 360` | Factor |
| `S_T_grid` | Settlement spot-rate sensitivity grid | `S0_in * 0.95` through `S0_in * 1.05` in 1% increments | USD per EUR |
| `USD_NO_HEDGE` | Unhedged USD proceeds | `FC_AMT * S_T` | USD |
| `USD_FWD` | Forward hedge proceeds | `FC_AMT * F0_in` | USD |
| `MM_BORROW_FC` | EUR borrowed at inception | `FC_AMT / DF_FC` | EUR |
| `MM_USD_NOW` | USD received from converting borrowed EUR | `MM_BORROW_FC * S0_in` | USD |
| `USD_MM` | Money-market hedge proceeds | `MM_USD_NOW * DF_USD` | USD |
| `F_IMPLIED` | Forward rate implied by covered interest-rate parity | `S0_in * DF_USD / DF_FC` | USD per EUR |
| `PARITY_DIFF` | Difference between money-market proceeds and parity-implied proceeds | `USD_MM - (FC_AMT * F_IMPLIED)` | USD |
| `PREM_PUT_FV` | Put option premium carried to settlement | `FC_AMT * PREM_PUT * DF_USD` | USD |
| `PREM_CALL_FV` | Call option premium carried to settlement | `FC_AMT * PREM_CALL * DF_USD` | USD |
| `USD_PUT` | Put-based receivable proceeds after premium | `FC_AMT * MAX(S_T, K_PUT) - PREM_PUT_FV` | USD |
| `USD_FLOOR_PUT` | Minimum put-based proceeds after carried premium | `FC_AMT * K_PUT - PREM_PUT_FV` | USD |
| `CALL_PAYOFF` | Gross call option payoff | `FC_AMT * MAX(S_T - K_CALL, 0)` | USD |
| `USD_CALL_NET` | Call payoff after carried premium | `CALL_PAYOFF - PREM_CALL_FV` | USD |

**Calculation Notes**

- `S_T` represents the hypothetical EUR settlement exchange rate used in each sensitivity scenario.
- `S_T_grid` contains eleven settlement-rate scenarios from 95% to 105% of `S0_in` in 1% increments.
- `PREM_PUT_FV` and `PREM_CALL_FV` carry the upfront premiums to settlement using `DF_USD`.
- `PARITY_DIFF` should equal zero, subject to minor floating-point differences, when both calculations use consistent inputs and full precision.
- The parity validation tolerance is an absolute difference of no more than USD 0.01.
- `USD_FLOOR_PUT` represents the minimum modeled USD proceeds after deducting the carried put premium.
- `USD_CALL_NET` is a separate call payoff after its premium; it is not total receivable proceeds.

## 3. Assumptions & Constraints

- **Quote convention:** All exchange rates are expressed as USD per EUR. A higher quote indicates euro appreciation against the U.S. dollar.
- **Horizon:** The model evaluates one EUR receivable settling in 182 days.
- **Day-count basis:** ACT/360 is used for both USD and EUR interest calculations. Accumulation factors use `T_DAYS / 360`.
- **Covered interest-rate parity:** The money-market hedge should approximate the forward hedge when the forward rate is consistent with the interest-rate differential. The assigned indicative forward rate may differ from the implied rate.
- **Option premium timing:** Put and call premiums are paid upfront at the beginning of the 182-day hedge period. Each premium is carried to settlement using `DF_USD` and deducted from its corresponding option outcome.
- **Option strikes:** Both `K_PUT` and `K_CALL` equal the initial spot rate, `S0_in`.
- **Transaction costs:** Bid-ask spreads, bank fees, and other transaction costs are excluded.
- **Counterparty risk:** Counterparty and settlement risks are excluded. Derivative contracts are assumed to be fulfilled as agreed.
- **Tax and accounting:** Tax effects and hedge-accounting treatment are excluded. The model reports pre-tax USD cash outcomes.
- **Scenario construction:** Future settlement spot rates are varied deterministically. No probability weights or implied-volatility distributions are applied.
- **Market data:** Spot and interest rates use the dated observations in Section 2.1. The overnight benchmark rates are proxies and may differ from transaction-specific 182-day borrowing or deposit rates.

## 4. Calculation Flow

All calculations must use named-range notation rather than hard-coded Excel cell addresses. Full precision must be retained throughout calculations, with rounding applied only to displayed results.

### Step 1 — Derived Inputs

Calculate the USD and EUR accumulation factors:

1. `DF_USD = 1 + R_USD * T_DAYS / 360`
2. `DF_FC = 1 + R_FC * T_DAYS / 360`

### Step 2 — No-Hedge Benchmark

The company converts its EUR receivable into USD at the settlement spot rate.

`USD_NO_HEDGE(S_T) = FC_AMT * S_T`

### Step 3 — Forward Hedge

The company locks in the assigned indicative forward rate to convert its EUR receivable into USD at settlement.

`USD_FWD = FC_AMT * F0_in`

**Scenario check figure:**

EUR 14,500,000 × 1.1705 = **USD 16,972,250**

Forward proceeds remain constant across settlement spot-rate scenarios.

### Step 4 — Money-Market Hedge

The company borrows EUR today, converts the borrowed amount into USD at the current spot rate, and invests the USD until settlement. The EUR receivable repays the EUR borrowing at maturity.

1. **Borrow EUR today:**

   `MM_BORROW_FC = FC_AMT / DF_FC`

2. **Convert borrowed EUR into USD:**

   `MM_USD_NOW = MM_BORROW_FC * S0_in`

3. **Invest USD until settlement:**

   `USD_MM = MM_USD_NOW * DF_USD`

The EUR borrowing plus accrued interest must equal the original EUR receivable at settlement.

**Expected check figures:**

| Calculation | Expected Result |
|---|---:|
| `DF_FC` | 1.01263889 |
| `DF_USD` | 1.01961556 |
| `MM_BORROW_FC` | EUR 14,319,023.45 |
| `MM_USD_NOW` | USD 15,987,189.69 |
| `USD_MM` | **USD 16,300,787.29** |

### Step 5 — Covered Interest-Rate Parity Check

Calculate the forward rate implied by the spot rate and interest-rate differential.

`F_IMPLIED = S0_in * DF_USD / DF_FC`

Calculate the difference between money-market proceeds and parity-implied proceeds.

`PARITY_DIFF = USD_MM - (FC_AMT * F_IMPLIED)`

**Expected check figures:**

| Calculation | Expected Result |
|---|---:|
| `F_IMPLIED` | 1.12419223 USD per EUR |
| `FC_AMT * F_IMPLIED` | USD 16,300,787.29 |
| `PARITY_DIFF` | Approximately USD 0.00 |

**Validation tolerance:** `ABS(PARITY_DIFF) <= 0.01`

The workbook must calculate and visibly display the parity result and whether it passes the tolerance. Do not force a failed check to pass.

Separately compare `USD_MM` with `USD_FWD`. Any difference between the money-market and assigned forward proceeds must be reported and explained because the assigned forward rate is indicative and may not match the interest-rate-implied rate.

### Step 6 — Option Hedge

**EUR put option — receivable protection**

The company purchases a put option on EUR with a strike equal to the initial spot rate.

Upfront put premium:

`PUT_PREMIUM = FC_AMT * PREM_PUT`

Carry the premium to settlement:

`PREM_PUT_FV = FC_AMT * PREM_PUT * DF_USD`

Put-based receivable proceeds after the carried premium:

`USD_PUT(S_T) = FC_AMT * MAX(S_T, K_PUT) - PREM_PUT_FV`

**Put premium check figures:**

| Calculation | Expected Result |
|---|---:|
| Upfront put premium | USD 281,300.00 |
| Put premium carried to settlement | USD 286,817.86 |

Minimum put-based proceeds:

`USD_FLOOR_PUT = FC_AMT * K_PUT - PREM_PUT_FV`

**Expected put minimum:** USD 15,902,432.14

**EUR call option — separate payoff comparison**

The call payoff must be reported separately. It is not the protective option strategy for the EUR receivable and must not be presented as total receivable proceeds.

Upfront call premium:

`CALL_PREMIUM = FC_AMT * PREM_CALL`

Carry the premium to settlement:

`PREM_CALL_FV = FC_AMT * PREM_CALL * DF_USD`

Gross call payoff:

`CALL_PAYOFF(S_T) = FC_AMT * MAX(S_T - K_CALL, 0)`

Net call payoff after the carried premium:

`USD_CALL_NET(S_T) = CALL_PAYOFF(S_T) - PREM_CALL_FV`

**Call premium check figures:**

| Calculation | Expected Result |
|---|---:|
| Upfront call premium | USD 429,200.00 |
| Call premium carried to settlement | USD 437,619.00 |

The call's net payoff may be negative when the gross payoff is less than the premium.

### Step 7 — Sensitivity Calculations

The sensitivity grid must contain eleven settlement rates, from 95% through 105% of `S0_in`, in 1% increments.

For each settlement rate, calculate:

| Strategy | Output | Formula |
|---|---|---|
| No hedge | `USD_NO_HEDGE(S_T)` | `FC_AMT * S_T` |
| Forward | `USD_FWD` | `FC_AMT * F0_in` |
| Money market | `USD_MM` | `MM_USD_NOW * DF_USD` |
| Put option | `USD_PUT(S_T)` | `FC_AMT * MAX(S_T, K_PUT) - PREM_PUT_FV` |
| Call option | `USD_CALL_NET(S_T)` | `FC_AMT * MAX(S_T - K_CALL, 0) - PREM_CALL_FV` |

The call payoff must be displayed separately from total receivable proceeds.

**Sensitivity comparison check**

Using the specified inputs and carried option premiums:

- The forward hedge produces the highest modeled receivable proceeds from 95% through 104% of the initial spot rate.
- The unhedged strategy produces the highest modeled receivable proceeds at 105%.
- The put option provides downside protection but does not produce the highest proceeds at any of the eleven sensitivity points.
- The call option is a separate payoff comparison, not total receivable proceeds.

## 5. Outputs and Workbook Construction Requirements

The workbook must contain the following eight worksheets:

| Worksheet | Purpose |
|---|---|
| `Cover` | Identifies Scenario 33, author, date, receivable exposure, model objective, and data provenance. |
| `Legend-Key` | Explains input, assumption, formula, and output formatting conventions. |
| `Inputs` | Contains all ten named-range inputs, units, values, sources, and access dates. |
| `Forward Hedge` | Calculates locked-in USD proceeds using the forward rate. |
| `Money Market Hedge` | Displays the three borrowing, conversion, and investment steps. |
| `Option Hedge` | Calculates put proceeds and the put floor; reports the call payoff separately. |
| `Sensitivity` | Compares strategy outcomes across all settlement spot-rate scenarios and displays the chart. |
| `Notes & Assumptions` | Records market-data sources, access dates, calculation conventions, assumptions, and limitations. |

### 5.1 Required Workbook Outputs

| Output | Description |
|---|---|
| Input panel | Ten standardized inputs with units and sources |
| Forward proceeds | `USD_FWD` |
| Money-market proceeds | `USD_MM` |
| Implied forward rate | `F_IMPLIED` |
| Parity difference | `PARITY_DIFF` |
| Put proceeds | `USD_PUT(S_T)` |
| Put floor | `USD_FLOOR_PUT` |
| Call net payoff | `USD_CALL_NET(S_T)` |
| Sensitivity table and chart | Strategy outcomes across `S_T_grid` |
| Strategy summary | Key proceeds, premiums, and parity difference |

### 5.2 Stage 3 — Seven Required Build and Audit Checks

The Stage 3 workbook must satisfy all seven instructor requirements.

**1. Ten standardized named ranges**

- Include all ten named ranges from Section 2.1.
- Each name must refer to the correct input cell on the `Inputs` worksheet.
- `K_PUT` and `K_CALL` must be formula-linked to `S0_in`.
- Verify the names and cell references using Excel Name Manager.

**2. Formula integrity**

- Every calculated cell must contain an Excel formula.
- Formulas must reference the standardized named ranges and relevant scenario variables rather than hard-coded input-cell addresses.
- Do not type calculated outputs as fixed values.
- Designated input cells are the only cells intended for manual input.
- Check for broken references and formula errors, including `#REF!`, `#VALUE!`, `#DIV/0!`, and `#NAME?`.

**3. Cover worksheet**

The `Cover` worksheet must include:
- Scenario number and scenario name.
- Author.
- Date.
- Currency exposure and settlement horizon.
- Model purpose.
- Data-provenance block identifying which values are scenario-provided, which are sourced market observations, and which are assumptions or indicative values.

**4. Legend-Key worksheet and formatting**

The `Legend-Key` worksheet must define and the workbook must consistently apply:

| Color | Meaning |
|---|---|
| Yellow | Inputs |
| Blue | Assumptions |
| Green | Formulas |
| Gray | Outputs |

Apply the conventions throughout the workbook. The legend must make the meaning of each color clear.

**5. Three hedge families**

The workbook must include:
- Forward hedge calculation.
- Money-market hedge shown in three explicit steps: borrow EUR, convert EUR to USD, and invest USD.
- Put and call option calculations, with premium cost in USD and proceeds/payoff shown as a function of `S_T`.

The call payoff must remain separate from total receivable proceeds.

**6. Sensitivity table and chart**

- Include eleven settlement-rate scenarios from -5% to +5% relative to `S0_in`, in 1% increments.
- Generate the settlement-rate rows with formulas, not hand-typed scenario values.
- Calculate the no-hedge, forward, money-market, and put proceeds for every scenario.
- Display the call net payoff separately.
- Link the chart to the sensitivity table so it reflects the calculated values.
- The sensitivity grid must update when `S0_in` changes.

**7. Visible validation checks**

The workbook must calculate and visibly display:
- The covered interest-rate parity check and its USD 0.01 tolerance.
- The specification's numerical check figures for the money-market hedge, implied forward rate, option premiums, and put floor.
- A clear pass/fail or comparison result for each validation.
- Failed checks must remain visible and be explained in the audit note. Never change formulas or expected values solely to force a passing result.

### 5.3 Formula and Formatting Rules

- All calculated outputs, summary values, validation results, and sensitivity results must be formula-driven.
- Use the ten standardized input named ranges wherever applicable.
- Use formulas for the sensitivity grid and scenario calculations.
- Do not hard-code calculated values into cells where formulas belong.
- Retain full precision in calculations and round only displayed values.
- Apply the Legend-Key color conventions consistently.
- The workbook must contain all eight required worksheets.

## 6. Model Review — What Worked & What to Improve

This specification establishes the calculation structure before the Excel workbook is audited.

### 6.1 What the Model Is Designed to Do

- Compare the company's unhedged exposure with forward, money-market, and put-option strategies.
- Use standardized named ranges to make inputs and formulas consistent.
- Display all three money-market hedge steps separately.
- Test covered interest-rate parity using the implied forward rate.
- Show the effects of changing settlement exchange rates through a sensitivity table and chart.
- Report the call option payoff separately from receivable proceeds.
- Carry upfront option premiums to settlement so that all strategy outcomes are compared at the same date.

### 6.2 What to Review During the Build

- Confirm that all ten named ranges refer to the correct input cells.
- Confirm that all formulas use the standardized names and scenario variables.
- Confirm that the selected USD and EUR interest rates use the specified ACT/360 convention.
- Confirm that all exchange rates use USD per EUR.
- Distinguish indicative scenario values from sourced market data.
- Carry option premiums to settlement using `DF_USD`.
- Ensure formulas, tables, and charts update when inputs change.
- Explain any difference between the assigned forward rate and the interest-rate-implied forward rate.
- Recalculate expected check figures if any input values are changed.
- Verify all seven Stage 3 requirements before marking the audit complete.

### 6.3 Auditability Checklist

- [ ] All ten required inputs have standardized named ranges attached to the correct input cells.
- [ ] `K_PUT` and `K_CALL` are formula-linked to `S0_in`.
- [ ] Every calculated cell uses a formula rather than a hard-coded calculated value.
- [ ] The Cover worksheet contains scenario, author, date, and data provenance.
- [ ] The Legend-Key defines yellow inputs, blue assumptions, green formulas, and gray outputs.
- [ ] The three money-market hedge steps are displayed separately.
- [ ] The EUR borrowing plus accrued interest equals `FC_AMT`.
- [ ] `USD_MM` matches the expected check figure of USD 16,300,787.29.
- [ ] `F_IMPLIED` matches the expected check figure of 1.12419223 USD per EUR.
- [ ] `ABS(PARITY_DIFF)` is no greater than USD 0.01.
- [ ] The comparison between `USD_MM` and `USD_FWD` is reported and explained.
- [ ] Put proceeds do not fall below `USD_FLOOR_PUT`, with an expected minimum of USD 15,902,432.14.
- [ ] Put and call premiums are carried to settlement using `DF_USD`.
- [ ] Call net payoff is reported separately and accounts for its premium.
- [ ] All eleven sensitivity scenarios calculate correctly.
- [ ] The sensitivity chart is linked to the sensitivity table.
- [ ] Sensitivity calculations update when `S0_in` changes.
- [ ] All required validation checks are calculated and visible.
- [ ] No formula errors, broken references, or missing required outputs remain.

## 7. Sensitivity Plan

- **Grid:** `S_T_grid` ranges from `S0_in * 0.95` to `S0_in * 1.05` in 1% increments, producing eleven settlement-rate scenarios.
- **Strategies compared:** No hedge, forward, money market, and put option. The call option's net payoff is displayed separately.
- **Primary chart:** A line chart with settlement spot rate (`S_T`) on the x-axis and USD proceeds on the y-axis.
- **Expected results:** No-hedge proceeds change directly with the settlement rate. Forward and money-market proceeds remain constant. Put proceeds provide a floor while retaining upside participation.
- **Call option:** Display the call's net payoff separately to avoid confusing it with total proceeds from the EUR receivable.
- **Premium treatment:** Deduct the future value of the upfront put and call premiums using `DF_USD`.
- **Expected strategy comparison:** With the specified inputs, the forward hedge produces the highest modeled receivable proceeds from 95% through 104% of the initial spot rate. The unhedged strategy produces the highest proceeds at 105%.
- **Purpose:** Demonstrate how changes in the EUR/USD exchange rate affect the company's USD proceeds and compare the trade-offs between certainty, downside protection, and upside participation.

## 8. Limitations & Next Steps

**Limitations.** This model does not incorporate:

- Partial, layered, or dynamic hedging strategies.
- Counterparty credit risk or settlement risk.
- Transaction costs, bank fees, or bid-ask spreads.
- Implied-volatility-based option pricing. Option premiums are assigned scenario inputs rather than calculated option prices.
- Tax effects or hedge-accounting treatment.
- Multiple currencies or multiple settlement dates.
- Transaction-specific 182-day borrowing or deposit rates. The model uses overnight benchmark proxies for its interest-rate calculations.

**Next steps:**

1. **Stage 3 — AI-Assisted Build + Audit:** Build `model.xlsx` using this specification and document the build audit.
2. **Stage 4 — Market Data + Population:** Confirm or replace the inputs with appropriate sourced, timestamped market data and populate the workbook.
3. **Stage 5 — Validation + Decision Memo:** Independently validate the calculations, compare the hedging strategies, and prepare a recommendation for the CFO.

---

## Appendix A — Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | 2026-11-05 | Jazmyne Viloria | Initial Scenario 33 specification draft. |
| 0.2 | 2026-11-05 | Jazmyne Viloria | Revised to align with the professor's template and Stage 2 checklist; clarified named ranges, calculation flow, sensitivity, and validation requirements. |
| 0.3 | 2026-11-09 | Jazmyne Viloria | Incorporated instructor feedback; populated spot and interest-rate inputs with dated sources; revised option premium timing to carry premiums to settlement; added expected money-market, implied-forward, and put-floor check figures; established a USD 0.01 parity tolerance; updated sensitivity results and audit checks. |
| 0.4 | 2026-11-13 | Jazmyne Viloria | Updated specification metadata and made the seven Stage 3 build and audit requirements explicit, including cover-page provenance, workbook color conventions, formula integrity, sensitivity testing, and visible validation checks. |
