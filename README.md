# Sodium-ion Battery Project (all Na-ion work in one place)

This folder holds every sodium-ion step of the MTP, plus a combined step-by-step report with a comparison and
the final results.

## Start here
**[`Sodium_ion_Full_Report/Sodium_ion_Full_Report.pdf`](Sodium_ion_Full_Report/Sodium_ion_Full_Report.pdf)** is the
complete step-by-step report (data → cleaning and the data error found → features → models → testing →
results → comparison → final result). The same report is also available as a runnable notebook,
`Sodium_ion_Full_Report.ipynb`, and every result is collected in one spreadsheet, `Sodium_ion_All_Results.csv`.

## What is in this folder

| Folder | Project | Data | Approach |
|---|---|---|---|
| [`Sodium_ion_new/`](Sodium_ion_new) | **1. dQ/dV features + 8 ML models** (replicates Qian et al. 2026) | BatteryLife, 59 cells | Machine learning |
| [`Sodium_ion/`](Sodium_ion) | **2. BiTCN-MHA-BiGRU hybrid network** (from the Li-ion work) | Wenzhou, 7 half-cells | Deep learning |
| [`Sodium_ion_life_2026-09-24/`](Sodium_ion_life_2026-09-24) | **3. Early-life prediction + Bayesian life forecast** | BatteryLife, 59 cells (cleaned) | ML + Bayesian statistics |
| [`Sodium_ion_Full_Report/`](Sodium_ion_Full_Report) | Combined report, notebook, all-results CSV | all of the above | |

`Sodium_ion_new` covers both batteries, so it exists in both project folders:
- **This copy** holds the Na-ion raw data (`data/raw/naion`).
- **The copy in `../Lithium_ion_Project/Sodium_ion_new`** holds the Tongji Li-ion raw data (5.2 GB).
- Each links to the other's data, so nothing large is stored twice.
- Their code and results are identical.

## Data

| Data set | Cells | Cycles | Cell type | Test | Life threshold |
|---|---|---|---|---|---|
| BatteryLife Na-ion (Zenodo 17960956) | 59 (+10 excluded) | 11,879 after cleaning (11,938 raw) | 18650, 0.71–0.94 Ah | constant current 2–6 A, 2.0–4.0 V | 90% retention (only 2 cells reach 80%) |
| Wenzhou (He et al. 2026, *EES Batteries*) | 7 half-cells | 10,990 | H, BY families | constant current | 85% SOH (source paper) |

**Data error found and fixed:** every BatteryLife file ends with a partial final cycle. In 8 cells it looked
like an 80% end of life, which made Project 1's Na-ion RUL results invalid. Project 3 removes those cycles.

## Comparison of the projects

| | 1. dQ/dV + ML | 2. Deep learning | 3. Early life + Bayesian |
|---|---|---|---|
| Input | one partial charging curve | 32 past cycles | first 50 or 100 cycles |
| Best model | SVR / XGBoost / Ridge | BiGRU (best network) | Elastic net; Bayesian power law |
| Accuracy | SOH median R² 0.49–0.62 | SOH R² 0.60 (persistence 0.999) | retention at cycle 150: R² 0.93 (50 cycles), 0.99 (100 cycles) |
| Life prediction | invalid (data error) | networks 86–100% off; straight line 12% | cycle of reaching 90% within 6–8 cycles from cycle 100, calibrated 90% range |
| Verdict | moderate | poor | **best** |

## Final result

- **Best data set:** BatteryLife, cleaned (59 cells).
- **Best capacity prediction:** elastic net. Retention at cycle 150 is predicted to 0.77 pp (R² 0.93) from 50 cycles
  and 0.25 pp (R² 0.99) from 100 cycles.
- **Best life prediction:** a straight line or the Bayesian power law predicts the cycle of reaching 90% to within
  6–8 cycles from cycle 100. The Bayesian 90% range contains the truth for 97–100% of cells.
- **Deep learning** was the weakest approach: with 7 cells, no network beat simple baselines.
- **Overall winner:** Project 3 (`Sodium_ion_life_2026-09-24`).

## Rebuild the report

```bash
python3 /Users/vikram/MTP/MTP_Final_Report_2026-09-24/build_battery_reports.py na
```

# MTP_Na
