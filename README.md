# VaR Model Validation Suite — 2019–2025
### SR 11-7 Compliant · Four-Method Market Risk Framework

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/)
[![Framework: SR 11-7](https://img.shields.io/badge/Framework-SR%2011--7-red.svg)](https://www.federalreserve.gov/supervisionreg/srletters/sr1107.htm)
[![Basel: III FRTB](https://img.shields.io/badge/Basel-III%20FRTB-orange.svg)](https://www.bis.org/bcbs/publ/d457.htm)
[![Style: Wall Street](https://img.shields.io/badge/Style-Wall%20Street-navy.svg)](#)

---

## Overview

This project implements and **independently validates** a four-method Value-at-Risk (VaR) model suite for a multi-asset equity portfolio over the **2019–2025 period**, spanning five distinct market regimes including the COVID crash, the 2022 rate-shock bear market, and the AI-driven bull market.

The project is structured as a formal **Second-Line MRM validation exercise** following the SR 11-7 framework, and styled to match professional Wall Street and econometrics presentation standards (white background, professional color palette, regime-shaded panels, monospace annotation boxes).

---

## Data Period & Market Regimes

| Period | Regime | Ann. Vol | Characteristics |
|---|---|---|---|
| 2019 | Bull Phase 1 | ~15% | Low vol, trend-following |
| Feb–Mar 2020 | COVID Crash | ~70% | Fastest 30% S&P 500 decline in history |
| Apr 2020–Dec 2021 | Recovery | ~20% | Zero rates, fiscal stimulus |
| Jan–Oct 2022 | Rate-Shock Bear | ~35% | Fed 425bps hiking cycle |
| Nov 2022–2025 | AI Bull Phase 2 | ~18% | Tech-led; AI multiple expansion |

> **Note on data:** yfinance is network-restricted in this environment. Returns are simulated using a regime-switching model calibrated to actual 2019–2025 sector ETF statistics. Replace `simulate_portfolio()` with `yfinance.download()` in production.

---

## Repository Structure

```
var-validation-suite/
├── notebooks/
│   └── var_model_validation.ipynb   Main analysis notebook (self-contained)
├── outputs/
│   ├── fig1_portfolio.png           Portfolio construction & data integrity
│   ├── fig2_var_comparison.png      Conceptual soundness — method comparison
│   ├── fig3_es.png                  ES vs VaR (Basel III FRTB)
│   ├── fig4_backtesting.png         1,574-day rolling backtest exception report
│   ├── fig5_regime.png              Regime-specific breach analysis
│   ├── fig6_cusum.png               CUSUM ongoing monitoring dashboard
│   └── fig7_summary.png             SR 11-7 validation summary scorecard
├── requirements.txt
└── README.md
```

---

## SR 11-7 Alignment

| SR 11-7 Pillar | Coverage | Module |
|---|---|---|
| **Conceptual Soundness** | Assumption testing (JB, ADF+KPSS, Ljung-Box); sensitivity analysis (nu, window); fat-tail quantification | 2, 3 |
| **Outcome Analysis** | 1,574-day pseudo-OOS backtest; Kupiec POF; Christoffersen independence; Basel TL; regime-specific sub-analysis | 4 |
| **Ongoing Monitoring** | CUSUM (ARL0/ARL1 documented); stress scenario analysis; regime-conditional thresholds | 5 |
| **Data Integrity** | ADF + KPSS stationarity; EWMA covariance (lambda=0.94); normality test | 1 |

---

## Backtesting Results (2020–2025, 1,574 Days)

| Method | Breach Rate | Expected | Kupiec POF | Christoffersen | Basel Zone |
|---|---|---|---|---|---|
| Parametric | ~1.9% | 1.00% | **FAIL** | **FAIL** (Clustering) | Green |
| Historical | ~2.1% | 1.00% | **FAIL** | **PASS** | Green |
| MC Normal | ~1.9% | 1.00% | **FAIL** | **FAIL** (Clustering) | Green |
| MC Student-t | ~1.2% | 1.00% | **PASS** | **PASS** | Green |

### Regime-Specific Breach Rates (%)

| Regime | Parametric | Historical | MC Normal | MC Student-t | Expected |
|---|---|---|---|---|---|
| 2019 Bull | ~0.4% | ~0.8% | ~0.4% | ~0.4% | 1.0% |
| 2020 COVID Crash | ~18% | ~14% | ~18% | ~6% | 1.0% |
| 2020-21 Recovery | ~0.8% | ~1.0% | ~0.8% | ~0.8% | 1.0% |
| 2022 Rate-Shock Bear | ~15% | ~11% | ~15% | ~5% | 1.0% |
| 2022-25 AI Bull | ~0.9% | ~1.0% | ~0.9% | ~1.0% | 1.0% |

> **Key finding:** Kupiec's global test sees a breach rate of ~1.9% and calls it "slightly above target." The regime analysis reveals crash periods generating 15-18x the expected breach rate. These are the same number — one hides the structure, the other exposes it.

---

## Model Disposition

| Model | Verdict | Rationale |
|---|---|---|
| **Parametric** | CONDITIONAL REJECT | Kupiec FAIL + Christoffersen FAIL (clustering). Gaussian assumption empirically rejected. Consecutive stress failures. |
| **Historical** | CONDITIONAL APPROVE | Christoffersen PASS (breaches are independent). Primary method with Stressed VaR overlay. |
| **MC Normal** | CONDITIONAL REJECT | Same Gaussian tail failure as Parametric. No incremental benefit. |
| **MC Student-t** | APPROVE AS CHALLENGER | Only method to pass both Kupiec and Christoffersen. Best stress-period calibration. Require dynamic nu estimation. |


---

## References

- Kupiec, P. (1995). *Techniques for verifying the accuracy of risk measurement models.* Journal of Derivatives.
- Christoffersen, P. (1998). *Evaluating interval forecasts.* International Economic Review.
- Basel Committee on Banking Supervision (2019). *FRTB — Minimum capital requirements for market risk.*
- Federal Reserve (2011). *SR 11-7: Guidance on Model Risk Management.*
- RiskMetrics Group / J.P. Morgan (1994). *RiskMetrics Technical Document.*

---

**Author:** Kai Chen, PhD, CFA | [LinkedIn](https://linkedin.com/in/kaichen0515) | [GitHub](https://github.com/kchen0788)
