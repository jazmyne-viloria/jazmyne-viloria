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
- **Covered interest-rate parity:** The money-market hedge should approximate the forward hedge when the forward rate is consistent with the interest-rate differential
