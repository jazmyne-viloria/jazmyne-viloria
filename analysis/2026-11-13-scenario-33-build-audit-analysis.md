# Audit Note — Scenario 33 FX Hedge Model (Stage 3)

**Prepared by:** Jazmyne Viloria
**Workbook audited:** `capabilities/fx-hedging/model.xlsx`
**Method:** I recalculated the full workbook and read each cell named in the test plan, then compared it to the expected check figures on the `Notes & Assumptions` sheet. The recalculation was run with AI assistance (Claude) outside Excel; the results below are the values read from the recalculated cells.

## Test plan and results

| # | Test | Location | Expected | Actual | Result |
|---|---|---|---|---|---|
| 1 | DF_FC | `Money Market Hedge!B16` | 1.01263889 | 1.01263889 | Pass |
| 2 | DF_USD | `Money Market Hedge!B15` | 1.01961556 | 1.01961556 | Pass |
| 3 | USD_MM | `Money Market Hedge!B12` | USD 16,300,787.29 | USD 16,300,787.29 | Pass |
| 4 | PREM_PUT_FV | `Option Hedge!B7` | USD 286,817.86 | USD 286,817.86 | Pass |
| 5 | USD_FLOOR_PUT | `Option Hedge!B13` | USD 15,902,432.14 | USD 15,902,432.14 | Pass |
| 6 | Put floor check | `Sensitivity!B21` | PASS | PASS | Pass |
| 7 | Winner labels | `Sensitivity!B25:B35` | Forward at 95%–104%; No hedge at 105% | Forward in rows 25–34; No hedge in row 35 | Pass |
| 8 | Scenario grid | `Sensitivity!A25:A35` | 95% to 105% of 1.1165 | 1.060675 to 1.172325 in 1% steps | Pass |
| 9 | Parity difference | `Money Market Hedge!B23` | 0 (tolerance USD 0.01) | 0 | Pass (after fix, Finding 1) |
| 10 | Parity validation | `Money Market Hedge!B24` | PASS | PASS | Pass (after fix, Finding 1) |
| 11 | Cover key validation | `Cover!B20`, `Cover!C20` | A number and PASS | 0 and PASS | Pass (after fix, Finding 1) |
| 12 | Parity under changed input | R_USD set to 0.05 on a temporary copy | Still PASS | Difference 0; PASS | Pass |
| 13 | Workbook errors | All sheets | None | No error values | Pass |

## Findings

### Finding 1 — `PARITY_DIFF` referred to itself

- **Checked:** The parity cell `Money Market Hedge!B23`, the parity validation in `B24`, and the Cover's "Key validation."
- **Found:** All three showed `#VALUE!`. The defined name `PARITY_DIFF` points at B23, and the formula in B23 was `=PARITY_DIFF`, so the cell referred to itself.
- **Did:** Replaced the formula with `=USD_MM-(FC_AMT*F_IMPLIED)`, which is the definition given in the Notes sheet. B23 now shows 0, B24 shows PASS, and the Cover shows PASS. The check also stayed PASS when R_USD was changed to 0.05 on a temporary copy, so it follows the inputs.
- **Commit:** "Fix PARITY_DIFF self-reference in Money Market Hedge B23"

### Finding 2 — Output "Gray" fill read as the Blue assumption fill

- **Checked:** The Legend-Key against the fills on the output cells and on the assumption cells.
- **Found:** The Gray output fill was `D9E1F2`, a pale blue that is hard to tell apart from the Blue assumption fill (`DDEBF7`).
- **Did:** Changed the output fill to `D9D9D9` on every output cell and on the Legend's Gray row. No values or formulas changed. Afterward, outputs are `D9D9D9` and assumptions remain `DDEBF7`.
- **Commit:** "Change output fill to true gray D9D9D9 and update Legend"

### Finding 3 — Assigned forward exceeds the money-market result (checked, no change)

- **Checked:** `Sensitivity!B22` against the parity-implied forward in `Money Market Hedge!B20`.
- **Found:** The assigned forward proceeds exceed the money-market proceeds by USD 671,462.71. The assigned forward is 1.1705 and the implied forward is 1.12419223, a gap of 0.04630777 USD per EUR on EUR 14,500,000. This accounts for the whole difference. The Notes sheet says the assigned forward is indicative and need not equal the implied forward.
- **Did:** No change. This is consistent with the model's stated design and is recorded as a checked result, not a defect. (`Money Market Hedge!B26` shows the same amount as −671,462.71 because it is money market less forward.)

## Status

All tests above pass after Findings 1 and 2 were fixed. No open defects remain from this audit.
