# Scenario 33 — Mining Shovels FX Transaction Hedge Model
## Technical Specification

| Field | Value |
|------|------|
| **Created by** | Jazmyne Viloria |
| **Updated by** | Jazmyne Viloria |
| **Date Created** | 2026-11-05 |
| **Date Updated** | 2026-11-05 |
| **Version** | 0.1 |
| **LLM Used** | ChatGPT — feedback and specification review |
| **Role** | Treasury Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | To be developed in Stage 3 |

---

## 1. Problem Statement

A U.S. mining-shovel manufacturer has an outstanding receivable of **EUR 14,500,000** from a Finnish nickel miner, payable in **182 days**. Because the receivable is denominated in euros while the manufacturer's reporting currency is U.S. dollars, a depreciation of the euro against the dollar would reduce the amount of USD received at settlement. This specification establishes a model to compare the company's unhedged exposure with forward, money-market, and option-based strategies. The objective is to evaluate potential USD proceeds, understand the trade-off between certainty and upside potential, and provide evidence to support the company's hedging decision.

**Exposure details:**

- Exposure type: Foreign-currency receivable
- Functional/reporting currency: USD
- Foreign currency: EUR
- Receivable amount: EUR 14,500,000
- Settlement horizon: 182 days
- Exchange-rate convention: USD per EUR
- Objective: Compare hedging strategies and evaluate their effect on USD proceeds.

## 2. Inputs (Known Variables)

All inputs must be exposed as workbook **named ranges** so the calculation flow uses consistent variable names throughout the model. Scenario-provided values are indicative and will be replaced or confirmed with sourced market data during Stage 4 where applicable. Market-data sources and access dates must be documented in the Notes & Assumptions tab.

### 2.1 Core Inputs

| Standardized Name | Description | Placeholder / Scenario Value | Unit | Stage 4 Data Source |
|-------------------|-------------|------------------------------:|------|--------------------|
| `FC_AMT` | Foreign-currency receivable | 14,500,000 | EUR | Assigned Scenario 33 instructions |
| `S0_in` | Spot exchange rate at inception | To be sourced | USD per EUR | European Central Bank (ECB) EUR/USD reference rate |
| `F0_in` | Forward rate to settlement | 1.1705 | USD per EUR | Assigned Scenario 33 instructions; indicative rate |
| `R_USD` | USD annual interest rate | To be sourced | Annual % | Federal Reserve H.15 interest-rate data; document selected rate and tenor |
| `R_FC` | EUR annual interest rate | To be sourced | Annual % | ECB interest-rate data; document selected rate and tenor |
| `T_DAYS` | Days to settlement | 182 | Days | Assigned Scenario 33 instructions |
| `K_PUT` | Put option strike | Equal to `S0_in` | USD per EUR | Assigned Scenario 33 instructions; strike set at spot |
| `K_CALL` | Call option strike | Equal to `S0_in` | USD per EUR | Assigned Scenario 33 instructions; strike set at spot |
| `PREM_PUT` | Put premium per unit of foreign currency | 0.0194 | USD per EUR | Assigned Scenario 33 instructions; indicative premium |
| `PREM_CALL` | Call premium per unit of foreign currency | 0.0296 | USD per EUR | Assigned Scenario 33 instructions; indicative premium |

**Input conventions:**

- All exchange rates must be expressed as USD per EUR.
- `K_PUT` and `K_CALL` must equal `S0_in`.
- Interest rates must be entered as annual percentages and used consistently in decimal form within calculations.
- Scenario-provided forward rates and option premiums are indicative and must be identified as such until confirmed or replaced during Stage 4.
- Do not use the template's example GBP values or one-year assumptions.

### 2.2 Derived / Intermediate Values

| Name | Description | Calculation / Source |
|------|-------------|----------------------|
| `S_T_grid` | Settlement spot-rate sensitivity grid | `S0_in` varied from 95% to 105% in 1% increments |
| `USD_NO_HEDGE` | USD proceeds without a hedge | `FC_AMT × S_T` |
| `MM_BORROW_FC` | EUR borrowed at inception | `FC_AMT / (1 + R_FC × T_DAYS/360)` |
| `MM_USD_NOW` | USD received from converting borrowed EUR | `MM_BORROW_FC × S0_in` |
| `F_IMPLIED` | Forward rate implied by covered interest-rate parity | `S0_in × (1 + R_USD × T_DAYS/360) / (1 + R_FC × T_DAYS/360)` |
| `USD_FWD` | Forward hedge proceeds | `FC_AMT × F0_in` |
| `USD_MM` | Money-market hedge proceeds | `MM_USD_NOW × (1 + R_USD × T_DAYS/360)` |

## 3. Tab Architecture

The workbook will contain the following tabs:

| Tab Name | Purpose |
|----------|---------|
| `Cover` | Introduces the Scenario 33 exposure, model purpose, and key results. |
| `Legend-Key` | Explains workbook formatting, input cells, assumptions, formulas, and cross-tab references. |
| `Inputs` | Contains all ten standardized named-range inputs, units, sources, and access dates. |
| `Forward Hedge` | Calculates USD proceeds from the forward contract. |
| `Money Market Hedge` | Displays the three-step borrowing, currency conversion, and investment process. |
| `Option Hedge` | Calculates the put hedge's minimum proceeds and upside participation, and separately reports the call option payoff. |
| `Sensitivity` | Compares USD proceeds across the settlement exchange-rate scenarios and displays the comparison chart. |
| `Notes & Assumptions` | Records data sources, access dates, calculation conventions, assumptions, and model limitations. |

## 4. Assumptions & Constraints

The model uses the following assumptions and conventions to keep calculations consistent and reproducible.

- **Quote convention:** All exchange rates are expressed as USD per EUR. A higher quote indicates euro appreciation against the U.S. dollar.
- **Horizon:** The model evaluates a single receivable settling in 182 days.
- **Day-count basis:** Use ACT/360 for both USD and EUR interest calculations. The annual interest rates are applied using `T_DAYS/360`.
- **Covered interest-rate parity:** The money-market hedge should produce approximately the same USD proceeds as the forward hedge when the forward rate and interest rates are consistent with covered interest-rate parity.
- **Option premiums:** Premiums are quoted in USD per EUR and paid upfront. For this model, the assignment's specified option formula deducts the premium amount directly from the settlement proceeds. The model will not separately compound the premium to maturity.
- **Option strikes:** Both `K_PUT` and `K_CALL` are set equal to the initial spot rate, `S0_in`.
- **Transaction costs:** Bid-ask spreads, bank fees, and other transaction costs are excluded from the base case.
- **Counterparty risk:** Counterparty and settlement risks are excluded. Derivative contracts are assumed to be fulfilled as agreed.
- **Tax and accounting treatment:** Tax effects and hedge-accounting treatment are excluded. The model reports pre-tax USD cash outcomes.
- **Scenario construction:** Future settlement spot rates are varied deterministically using the sensitivity grid. No probability weights or implied-volatility distributions are applied.
- **Market data:** Spot and interest rates must be sourced and documented during Stage 4. Scenario-provided forward and premium values must remain identified as indicative until updated or confirmed.

## 5. Calculation Flow

All calculations must use named-range notation rather than Excel cell references. The formulas below describe the model logic for a foreign-currency receivable.

### Step 1 — Derived Inputs

Calculate the USD and EUR accumulation factors:

1. `DF_USD` = `1 + R_USD × T_DAYS / 360`
2. `DF_FC` = `1 + R_FC × T_DAYS / 360`

These factors are used in the money-market hedge and covered interest-rate parity check.

### Step 2 — No-Hedge Benchmark

The company receives the EUR receivable and converts it to USD at the settlement spot rate.

- `USD_NO_HEDGE(S_T)` = `FC_AMT × S_T`

The resulting USD proceeds vary directly with the settlement exchange rate.

### Step 3 — Forward Hedge

The company locks in the indicative forward exchange rate to convert the EUR receivable into USD at settlement.

- `USD_FWD` = `FC_AMT × F0_in`

**Scenario check figure:**

EUR 14,500,000 × 1.1705 = **USD 16,972,250**

Forward proceeds remain constant across all settlement spot-rate scenarios.

### Step 4 — Money-Market Hedge

The company borrows EUR today, converts the borrowed amount into USD at the current spot rate, and invests the USD until settlement. The EUR receivable repays the foreign-currency borrowing at maturity.

1. **Borrow EUR today:**

   `MM_BORROW_FC` = `FC_AMT / DF_FC`

2. **Convert borrowed EUR into USD:**

   `MM_USD_NOW` = `MM_BORROW_FC × S0_in`

3. **Invest USD until settlement:**

   `USD_MM` = `MM_USD_NOW × DF_USD`

At settlement, the EUR receivable repays the EUR borrowing. The USD investment represents the locked-in USD proceeds.

### Step 5 — Covered Interest-Rate Parity Check

Calculate the forward rate implied by the spot rate and interest-rate differential.

- `F_IMPLIED` = `S0_in × DF_USD / DF_FC`

**Parity check:** `USD_MM` should approximately equal `USD_FWD` when the interest rates and forward rate are consistent with covered interest-rate parity. Any difference must be identified and explained rather than automatically treated as a calculation error.

### Step 6 — Option Hedge

**Put option on EUR — receivable protection**

The company purchases a put option on EUR with a strike equal to the initial spot rate. The put provides a minimum conversion rate while allowing the company to benefit from euro appreciation.

- `USD_PUT(S_T)` = `FC_AMT × MAX(S_T, K_PUT) − FC_AMT × PREM_PUT`

**Scenario premium check figure:**

EUR 14,500,000 × USD 0.0194 = **USD 281,300**

The put's minimum proceeds before any excluded transaction costs are determined by the strike and premium:

- `USD_FLOOR_PUT` = `FC_AMT × K_PUT − FC_AMT × PREM_PUT`

**Call option on EUR — participation payoff**

The call option payoff is calculated separately to show the value of the call contract when the euro appreciates above its strike.

- `CALL_PAYOFF(S_T)` = `FC_AMT × MAX(S_T − K_CALL, 0)`
- `USD_CALL_NET(S_T)` = `CALL_PAYOFF(S_T) − FC_AMT × PREM_CALL`

**Scenario premium check figure:**

EUR 14,500,000 × USD 0.0296 = **USD 429,200**

The call payoff is reported separately from the proceeds generated by converting the receivable. It must not be presented as a standalone protective hedge for the EUR receivable.

### Step 7 — Sensitivity Table

For each settlement exchange rate in `S_T_grid`, calculate and compare:

| Strategy | Output | Formula |
|----------|--------|---------|
| No hedge | `USD_NO_HEDGE(S_T)` | `FC_AMT × S_T` |
| Forward | `USD_FWD` | `FC_AMT × F0_in` |
| Money market | `USD_MM` | `MM_USD_NOW × DF_USD` |
| Put option | `USD_PUT(S_T)` | `FC_AMT × MAX(S_T, K_PUT) − FC_AMT × PREM_PUT` |
| Call option | `USD_CALL_NET(S_T)` | `FC_AMT × MAX(S_T − K_CALL, 0) − FC_AMT × PREM_CALL` |

The sensitivity table must distinguish between total receivable proceeds and the standalone call option payoff.

## 6. Sensitivity Plan

- **Grid:** `S_T_grid` ranges from `S0_in × 0.95` to `S0_in × 1.05` in 1% increments, producing 11 settlement-rate scenarios.
- **Strategies compared:** No hedge, forward hedge, money-market hedge, and put hedge. The call option's net payoff is displayed separately.
- **Primary chart:** A line chart showing settlement spot rate (`S_T`) on the x-axis and USD proceeds on the y-axis. The no-hedge proceeds vary with the settlement rate, while forward and money-market proceeds remain constant. The put hedge provides a floor while retaining upside participation.
- **Call option:** Display its net payoff separately so it is not confused with total receivable proceeds.
- **Purpose:** Show how changes in the EUR/USD exchange rate affect the company's USD proceeds and highlight the trade-offs between certainty, downside protection, and upside participation.

## 7. Validation Rules & Check Figures

The workbook must include checks that confirm the formulas and outputs are functioning correctly.

### 7.1 Scenario Input Checks

- `FC_AMT` equals EUR 14,500,000.
- `F0_in` equals 1.1705 before any Stage 4 replacement.
- `T_DAYS` equals 182.
- `PREM_PUT` equals USD 0.0194 per EUR before any Stage 4 replacement.
- `PREM_CALL` equals USD 0.0296 per EUR before any Stage 4 replacement.
- `K_PUT` and `K_CALL` both equal `S0_in`.

### 7.2 Calculation Checks

- Forward proceeds equal USD 16,972,250 using the assigned indicative forward rate.
- Total put premium equals USD 281,300.
- Total call premium equals USD 429,200.
- The EUR borrowing plus accrued interest equals the original EUR receivable at settlement.
- The money-market proceeds approximately equal forward proceeds when the inputs are parity-consistent.
- The put option's proceeds do not fall below its strike-based floor under the specified formula.
- The call option's net payoff equals zero or a positive amount after accounting for its premium, depending on the settlement spot rate.

### 7.3 Workbook Audit Checks

- All ten required inputs have standardized named ranges.
- All calculation formulas use named ranges rather than hard-coded cell references.
- All 11 sensitivity scenarios calculate correctly.
- The sensitivity chart reflects the values in the sensitivity table.
- No formula errors or broken references are present.
- Output tables update when input assumptions change.
- Market-data sources and access dates are documented in the Notes & Assumptions tab.

## 8. Outputs

The workbook must produce the following outputs for review and comparison.

| Output | Description | Format | Purpose |
|--------|-------------|--------|---------|
| Input panel | Ten standardized inputs, units, sources, and access dates | Table | Establishes the model's source of truth |
| Forward proceeds | `USD_FWD` | USD amount | Shows the locked-in forward proceeds |
| Money-market proceeds | `USD_MM` | USD amount | Shows proceeds from the money-market hedge |
| Implied forward rate | `F_IMPLIED` | Exchange rate | Supports the parity check |
| Put proceeds | `USD_PUT` | USD amount by settlement scenario | Shows downside protection and upside participation |
| Put floor | `USD_FLOOR_PUT` | USD amount | Shows the minimum put-based proceeds |
| Call net payoff | `USD_CALL_NET` | USD amount by settlement scenario | Shows the separate call option payoff after its premium |
| Sensitivity table | `S_T_grid` and strategy proceeds | Table | Compares outcomes across exchange-rate scenarios |
| Sensitivity chart | USD proceeds against settlement spot rate | Line chart | Communicates the trade-offs among strategies |
| Strategy summary | Key USD proceeds and option costs | Summary table | Provides an at-a-glance comparison for the CFO |

**Workbook output formatting:** All calculated output cells and summary tables must be clearly identified in the `Legend-Key` tab. Input cells, assumptions, formulas, and cross-tab references must use consistent formatting.

---

## 9. Model Review — Known Risks & Auditability

Because this specification is being prepared before the Excel workbook is built, this section identifies issues the Stage 3 build must address rather than claiming that the model has already been tested.

### 9.1 What the Model Is Designed to Do

- Compare the USD proceeds from the no-hedge, forward, money-market, and put strategies.
- Use standardized named ranges to keep inputs and formulas consistent.
- Display each money-market calculation step separately.
- Test whether money-market proceeds and forward proceeds align under covered interest-rate parity.
- Show the effects of changing the settlement spot rate through a sensitivity table and chart.

### 9.2 Known Risks & Items to Review

- Confirm that the selected USD and EUR interest rates use compatible tenors and the specified ACT/360 convention.
- Confirm that the sourced spot rate and indicative forward rate are consistent in quote convention and maturity.
- Clearly distinguish scenario-provided indicative values from live market data.
- Ensure the option premium is treated consistently with the assignment's specified formula.
- Keep the call option payoff separate from total proceeds generated by the EUR receivable.
- Verify that all formulas, tables, and charts update when inputs change.

### 9.3 Auditability Checklist

- [ ] All ten required inputs have standardized named ranges.
- [ ] Each input includes a description, unit, placeholder or scenario value, and Stage 4 source.
- [ ] All formulas use named ranges rather than cell references.
- [ ] The three money-market steps are separately visible.
- [ ] The covered interest-rate parity check is included.
- [ ] The put option provides the specified strike-based floor.
- [ ] The call option payoff is reported separately.
- [ ] The sensitivity grid contains 11 scenarios from 95% to 105% of `S0_in`.
- [ ] The sensitivity chart matches the underlying table.
- [ ] The Notes & Assumptions tab records market-data sources and access dates.

## 10. Limitations & Next Steps

**Limitations.** This specification does not incorporate:

- Partial, layered, or dynamic hedging strategies.
- Counterparty credit risk or settlement risk.
- Transaction costs, bank fees, or bid-ask spreads.
- Implied-volatility-based option pricing. Option premiums are scenario inputs rather than calculated option prices.
- Tax effects or hedge-accounting treatment.
- Multiple currencies or multiple settlement dates.

**Next steps — later project stages will:**

1. **Stage 3 — AI-Assisted Build + Audit:** Build the Excel workbook using this specification and document the build audit.
2. **Stage 4 — Market Data + Population:** Replace or confirm placeholders with sourced, timestamped market data and populate the workbook.
3. **Stage 5 — Validation + Decision Memo:** Independently validate the calculations, compare the hedging strategies, and prepare a recommendation for the CFO.

---

## Appendix A — Change Log

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 0.1 | 2026-11-05 | Jazmyne Viloria | Initial Scenario 33 specification draft adapted from the professor's template. |
