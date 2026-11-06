# Scenario 33 — Mining Shovels FX Transaction Hedge Model
## Technical Specification

| Field | Value |
|---|---|
| **Created by** | Jazmyne Viloria |
| **Updated by** | Jazmyne Viloria |
| **Date Created** | 2026-11-05 |
| **Date Updated** | 2026-11-05 |
| **Version** | 0.2 |
| **LLM Used** | ChatGPT — feedback and specification review |
| **Role** | Treasury Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | To be developed in Stage 3 |

---

## 1. Problem Statement

A U.S. mining-shovel manufacturer has an outstanding receivable of **EUR 14,500,000** from a Finnish nickel miner, payable in **182 days**. Because the receivable is denominated in euros while the manufacturer's reporting currency is U.S. dollars, a depreciation of the euro against the dollar would reduce the USD proceeds received at settlement. This specification establishes a model to compare the unhedged exposure with forward, money-market, and option-based strategies. The objective is to evaluate potential USD proceeds, compare downside protection and upside participation, and provide evidence to support the company's hedging decision.

**Exposure details:**

- Exposure type: Foreign-currency receivable
- Functional/reporting currency: USD
- Foreign currency: EUR
- Receivable amount: EUR 14,500,000
- Settlement horizon: 182 days
- Exchange-rate convention: USD per EUR
- Objective: Protect the USD value of the receivable while comparing the cost and benefits of available hedging strategies.

## 2. Inputs (Known Variables)

All ten required inputs must be exposed as standardized workbook named ranges. The calculation formulas must reference these names rather than individual Excel cell addresses. Scenario-provided values are **indicative, replaced with live data at Stage 4** where applicable. Spot and interest-rate placeholders must be populated with sourced market data during Stage 4. Sources and access dates must be recorded in the Notes tab.

### 2.1 Core Inputs

| Standardized Name | Description | Placeholder / Scenario Value | Unit | Stage 4 Data Source |
|---|---|---:|---|---|
| `FC_AMT` | Foreign-currency receivable | 14,500,000 | EUR | Assigned Scenario 33 instructions |
| `S0_in` | Spot exchange rate at inception | To be sourced | USD per EUR | ECB EUR/USD reference rate |
| `F0_in` | Forward rate to settlement | 1.1705 | USD per EUR | Assigned Scenario 33 instructions; indicative |
| `R_USD` | USD annual interest rate | To be sourced | Annual % | Federal Reserve H.15; document selected rate and tenor |
| `R_FC` | EUR annual interest rate | To be sourced | Annual % | ECB interest-rate data; document selected rate and tenor |
| `T_DAYS` | Days to settlement | 182 | Days | Assigned Scenario 33 instructions |
| `K_PUT` | EUR put option strike | Equal to `S0_in` | USD per EUR | Assigned Scenario 33 instructions; strike at spot |
| `K_CALL` | EUR call option strike | Equal to `S0_in` | USD per EUR | Assigned Scenario 33 instructions; strike at spot |
| `PREM_PUT` | Put premium per EUR | 0.0194 | USD per EUR | Assigned Scenario 33 instructions; indicative |
| `PREM_CALL` | Call premium per EUR | 0.0296 | USD per EUR | Assigned Scenario 33 instructions; indicative |

**Input conventions:**

- All exchange rates must use USD per EUR. A higher rate indicates euro appreciation.
- `K_PUT` and `K_CALL` must equal `S0_in`.
- Interest rates must be entered as annual percentages and converted to decimal form in calculations.
- Use ACT/360 for both interest-rate calculations.
- The assigned forward rate and option premiums are indicative, replaced with live data at Stage 4 if updated market quotes are required.
- Do not use the template's example GBP values or one-year assumptions.
- All scenario-provided market values (F0_in, PREM_PUT, and PREM_CALL) are indicative, replaced with live data at Stage 4. The spot rate (S0_in) and interest rates (R_USD and R_FC) are placeholders to be populated with sourced market data during Stage 4.

### 2.2 Derived Values

| Name | Description | Calculation |
|---|---|---|
| `DF_USD` | USD accumulation factor | `1 + R_USD × T_DAYS / 360` |
| `DF_FC` | EUR accumulation factor | `1 + R_FC × T_DAYS / 360` |
| `S_T_grid` | Settlement spot-rate sensitivity grid | `S0_in × 0.95` through `S0_in × 1.05` in 1% increments |
| `USD_NO_HEDGE` | Unhedged USD proceeds | `FC_AMT × S_T` |
| `USD_FWD` | Forward hedge proceeds | `FC_AMT × F0_in` |
| `MM_BORROW_FC` | EUR borrowed at inception | `FC_AMT / DF_FC` |
| `MM_USD_NOW` | USD received from converting borrowed EUR | `MM_BORROW_FC × S0_in` |
| `USD_MM` | Money-market hedge proceeds | `MM_USD_NOW × DF_USD` |
| `F_IMPLIED` | Forward rate implied by interest-rate parity | `S0_in × DF_USD / DF_FC` |
| `USD_FLOOR_PUT` | Minimum put-based proceeds | `FC_AMT × K_PUT − FC_AMT × PREM_PUT` |

## 3. Assumptions & Constraints

- **Quote convention:** All exchange rates are expressed as USD per EUR. A higher quote indicates euro appreciation against the U.S. dollar.
- **Horizon:** The model evaluates one EUR receivable settling in 182 days.
- **Day-count basis:** ACT/360 is used for both USD and EUR interest calculations. The accumulation factors use `T_DAYS / 360`.
- **Covered interest-rate parity:** The money-market hedge should approximate the forward hedge when the forward rate is consistent with the interest-rate differential. The assigned indicative forward rate may differ from the implied rate.
- **Option premiums:** Premiums are quoted in USD per EUR and paid upfront. For this model, the assignment's specified formula deducts the total premium directly from settlement proceeds. Premiums will not be separately compounded to maturity.
- **Option strikes:** Both `K_PUT` and `K_CALL` equal the initial spot rate, `S0_in`.
- **Transaction costs:** Bid-ask spreads, bank fees, and other transaction costs are excluded.
- **Counterparty risk:** Counterparty and settlement risks are excluded. Derivative contracts are assumed to be fulfilled as agreed.
- **Tax and accounting:** Tax effects and hedge-accounting treatment are excluded. The model reports pre-tax USD cash outcomes.
- **Scenario construction:** Future settlement spot rates are varied deterministically. No probability weights or implied-volatility distributions are applied.
- **Market data:** Spot and interest rates must be sourced during Stage 4. Record the source, selected rate or tenor, and access date in the Notes tab.

## 4. Calculation Flow

All calculations must use named-range notation rather than Excel cell addresses. Formulas are written for the company's EUR receivable.

### Step 1 — Derived Inputs

Calculate the USD and EUR accumulation factors:

1. `DF_USD` = `1 + R_USD × T_DAYS / 360`
2. `DF_FC` = `1 + R_FC × T_DAYS / 360`

These factors are used in the money-market hedge and covered interest-rate parity check.

### Step 2 — No-Hedge Benchmark

The company converts its EUR receivable into USD at the settlement spot rate.

`USD_NO_HEDGE(S_T)` = `FC_AMT × S_T`

Proceeds increase when the euro appreciates and decrease when the euro depreciates.

### Step 3 — Forward Hedge

The company locks in the indicative forward rate to convert its EUR receivable into USD at settlement.

`USD_FWD` = `FC_AMT × F0_in`

**Scenario check figure:**

EUR 14,500,000 × 1.1705 = **USD 16,972,250**

Forward proceeds remain constant across settlement spot-rate scenarios.

### Step 4 — Money-Market Hedge

The company borrows EUR today, converts the borrowed amount into USD at the current spot rate, and invests the USD until settlement. The EUR receivable repays the EUR borrowing at maturity.

1. **Borrow EUR today:**

   `MM_BORROW_FC` = `FC_AMT / DF_FC`

2. **Convert borrowed EUR into USD:**

   `MM_USD_NOW` = `MM_BORROW_FC × S0_in`

3. **Invest USD until settlement:**

   `USD_MM` = `MM_USD_NOW × DF_USD`

The EUR borrowing plus accrued interest must equal the original EUR receivable at settlement. The USD investment represents the locked-in USD proceeds.

### Step 5 — Covered Interest-Rate Parity Check

Calculate the forward rate implied by the spot rate and interest-rate differential.

`F_IMPLIED` = `S0_in × DF_USD / DF_FC`

Calculate the difference between the money-market proceeds and the proceeds implied by the interest-rate parity calculation:

`PARITY_DIFF` = `USD_MM − (FC_AMT × F_IMPLIED)`

The parity difference should be approximately zero, subject to rounding. Separately compare `USD_MM` with `USD_FWD`. Any difference between the money-market and assigned forward proceeds must be reported and explained because the assigned forward rate is indicative and may not match the interest-rate-implied rate.

### Step 6 — Option Hedge

**EUR put option — receivable protection**

The company purchases a put option on EUR with a strike equal to the initial spot rate. The put protects against euro depreciation while allowing participation in euro appreciation.

`USD_PUT(S_T)` = `FC_AMT × MAX(S_T, K_PUT) − FC_AMT × PREM_PUT`

**Put premium check figure:**

EUR 14,500,000 × USD 0.0194 = **USD 281,300**

The minimum put-based proceeds are:

`USD_FLOOR_PUT` = `FC_AMT × K_PUT − FC_AMT × PREM_PUT`

**EUR call option — separate payoff comparison**

Calculate the call option payoff separately. It is not the protective option strategy for the EUR receivable and must not be presented as total receivable proceeds.

`CALL_PAYOFF(S_T)` = `FC_AMT × MAX(S_T − K_CALL, 0)`

`USD_CALL_NET(S_T)` = `CALL_PAYOFF(S_T) − FC_AMT × PREM_CALL`

**Call premium check figure:**

EUR 14,500,000 × USD 0.0296 = **USD 429,200**

The call's net payoff may be negative when the gross payoff is less than the premium.

### Step 7 — Sensitivity Calculations

For each settlement rate in `S_T_grid`, calculate:

| Strategy | Output | Formula |
|---|---|---|
| No hedge | `USD_NO_HEDGE(S_T)` | `FC_AMT × S_T` |
| Forward | `USD_FWD` | `FC_AMT × F0_in` |
| Money market | `USD_MM` | `MM_USD_NOW × DF_USD` |
| Put option | `USD_PUT(S_T)` | `FC_AMT × MAX(S_T, K_PUT) − FC_AMT × PREM_PUT` |
| Call option | `USD_CALL_NET(S_T)` | `FC_AMT × MAX(S_T − K_CALL, 0) − FC_AMT × PREM_CALL` |

The call option must be displayed separately from total receivable proceeds.

## 5. Outputs

The workbook must contain the following tabs, with each tab clearly named and used for its stated purpose.

| Tab Name | Purpose |
|---|---|
| `Cover` | Identifies Scenario 33, the receivable exposure, model objective, and key results. |
| `Legend-Key` | Explains input, assumption, formula, and cross-tab formatting. |
| `Inputs` | Contains all ten named-range inputs, units, values, sources, and access dates. |
| `Forward Hedge` | Calculates locked-in USD proceeds using the forward rate. |
| `Money Market Hedge` | Displays the three borrowing, conversion, and investment steps. |
| `Option Hedge` | Calculates put proceeds and the put floor; reports the call payoff separately. |
| `Sensitivity` | Compares strategy outcomes across all settlement spot-rate scenarios and displays the line chart. |
| `Notes & Assumptions` | Records market-data sources, access dates, calculation conventions, assumptions, and limitations. |

**Required workbook outputs:**

| Output | Description | Purpose |
|---|---|---|
| Input panel | Ten standardized inputs with units and sources | Establishes the model's source of truth |
| Forward proceeds | `USD_FWD` | Shows locked-in forward proceeds |
| Money-market proceeds | `USD_MM` | Shows proceeds from the money-market hedge |
| Implied forward rate | `F_IMPLIED` | Supports the parity check |
| Put proceeds | `USD_PUT(S_T)` | Shows downside protection and upside participation |
| Put floor | `USD_FLOOR_PUT` | Shows minimum put-based proceeds |
| Call net payoff | `USD_CALL_NET(S_T)` | Shows the separate call payoff after its premium |
| Sensitivity table and chart | Strategy outcomes across `S_T_grid` | Compares outcomes under different exchange rates |
| Strategy summary | Key proceeds, premiums, and parity difference | Provides an at-a-glance comparison for the CFO |

All calculated outputs must be formula-driven rather than manually entered values, excluding designated input cells. The `Legend-Key` tab must identify the workbook's formatting conventions.

## 6. Model Review — What Worked & What to Improve

This specification establishes the calculation structure before the Excel workbook is built. The following checks and improvements must be addressed during the Stage 3 build.

### 6.1 What the Model Is Designed to Do

- Compare the company's unhedged exposure with forward, money-market, and put-option strategies.
- Use standardized named ranges to make inputs and formulas consistent.
- Display all three money-market hedge steps separately.
- Test covered interest-rate parity using the implied forward rate.
- Show the effects of changing settlement exchange rates through a sensitivity table and chart.
- Report the call option payoff separately from receivable proceeds.

### 6.2 What to Improve / Review During the Build

- Confirm that the selected USD and EUR interest rates use compatible tenors and the specified ACT/360 convention.
- Confirm that all exchange rates use USD per EUR and that the spot and forward maturities are consistent.
- Clearly distinguish indicative scenario values from sourced Stage 4 market data.
- Apply the assignment's specified premium deduction consistently in the put and call calculations.
- Ensure that all formulas, tables, and charts update when inputs change.
- Explain any difference between the assigned forward rate and the interest-rate-implied forward rate.

### 6.3 Auditability Checklist

- [ ] All ten required inputs have standardized named ranges.
- [ ] Every calculation formula uses named ranges rather than hard-coded cell addresses.
- [ ] The money-market hedge displays all three steps.
- [ ] The EUR borrowing plus accrued interest equals `FC_AMT`.
- [ ] `PARITY_DIFF` is approximately zero when calculated using `F_IMPLIED`.
- [ ] The comparison between `USD_MM` and `USD_FWD` is reported and explained.
- [ ] Put proceeds do not fall below `USD_FLOOR_PUT`.
- [ ] Call net payoff is reported separately and correctly accounts for its premium.
- [ ] All 11 sensitivity scenarios calculate correctly.
- [ ] Every calculated output is formula-driven.
- [ ] No formula errors, broken references, or missing outputs are present.
- [ ] The sensitivity chart matches the underlying table.
- [ ] Market-data sources and access dates are recorded in `Notes & Assumptions`.

## 7. Sensitivity Plan

- **Grid:** `S_T_grid` ranges from `S0_in × 0.95` to `S0_in × 1.05` in 1% increments, producing 11 settlement-rate scenarios.
- **Strategies compared:** No hedge, forward, money market, and put option. The call option's net payoff is displayed separately.
- **Primary chart:** A line chart with settlement spot rate (`S_T`) on the x-axis and USD proceeds on the y-axis.
- **Expected results:** No-hedge proceeds change directly with the settlement rate. Forward and money-market proceeds remain constant. Put proceeds provide a floor while retaining upside participation.
- **Call option:** Display the call's net payoff separately to avoid confusing it with the total proceeds from the EUR receivable.
- **Purpose:** Demonstrate how changes in the EUR/USD exchange rate affect the company's USD proceeds and compare the trade-offs between certainty, downside protection, and upside participation.

## 8. Limitations & Next Steps

**Limitations.** This model does not incorporate:

- Partial, layered, or dynamic hedging strategies.
- Counterparty credit risk or settlement risk.
- Transaction costs, bank fees, or bid-ask spreads.
- Implied-volatility-based option pricing. Option premiums are assigned scenario inputs rather than calculated option prices.
- Tax effects or hedge-accounting treatment.
- Multiple currencies or multiple settlement dates.

**Next steps:**

1. **Stage 3 — AI-Assisted Build + Audit:** Build `model.xlsx` using this specification and document the build audit.
2. **Stage 4 — Market Data + Population:** Replace or confirm placeholders with sourced, timestamped market data and populate the workbook.
3. **Stage 5 — Validation + Decision Memo:** Independently validate the calculations, compare the hedging strategies, and prepare a recommendation for the CFO.

---

## Appendix A — Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | 2026-11-05 | Jazmyne Viloria | Initial Scenario 33 specification draft. |
| 0.2 | 2026-11-05 | Jazmyne Viloria | Revised to align with the professor's template and Stage 2 checklist; clarified named ranges, calculation flow, sensitivity, and validation requirements. |
