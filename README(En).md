# UIP Puzzle & Dollar Accumulation Strategy: Empirical Evidence from Korea–US Markets (2010–2026)

An empirical econometrics and portfolio backtesting project investigating whether **Uncovered Interest Parity (UIP)** holds between South Korea and the United States—and translating the findings into an optimal dollar-accumulation strategy for individuals with future USD liabilities (e.g., studying abroad or building USD reserves).

---

## 1. Motivation

For Korean residents saving in KRW for future USD expenses, a practical macroeconomic question arises:

> *"When US short-term interest rates exceed Korean rates, should I convert KRW into USD Treasury bills every month, or save in KRW deposits and exchange all at once later?"*

Standard international finance theory—**Uncovered Interest Parity (UIP)**—predicts that higher foreign interest rates should be offset by subsequent depreciation of the foreign currency, leaving expected returns equal across currencies. This project tests whether UIP holds in the KRW/USD market over the past 16 years and evaluates how deviations from theory impact real-world household accumulation strategies.

---

## 2. Data & Methodology

All macroeconomic time-series data are sourced directly from **FRED (Federal Reserve Economic Data)** at monthly end-of-period frequency (January 2010 – October 2026):

| Variable | FRED Series ID | Description |
| :--- | :--- | :--- |
| US Short-Term Rate ($i^{US}_t$) | `TB3MS` | 3-Month US Treasury Bill Secondary Market Rate (%) |
| Korea Short-Term Rate ($i^{KR}_t$) | `IR3TIB01KRM156N` | 3-Month Interbank Rate for South Korea (%) |
| Exchange Rate ($S_t$) | `DEXKOUS` | South Korean Won to US Dollar Spot Exchange Rate |

### Step 1: Econometric Test of UIP (Fama Regression)
We estimate the 12-month overlapping UIP regression using OLS:

$$\Delta s_{t+12} = \alpha + \beta \left(i^{KR}_t - i^{US}_t\right) + \varepsilon_{t+12}$$

where $\Delta s_{t+12} = \ln(S_{t+12}) - \ln(S_t)$ is the 12-month log change in USD/KRW. Under UIP, the joint null hypothesis is $\alpha = 0$ and $\beta = 1$. Because 12-month overlapping horizons induce strong serial correlation in residuals, standard errors are adjusted using the **Newey–West (HAC) estimator** with `maxlags = 12`.

### Step 2: Dollar Accumulation Backtest
Using the exact same 3-month interest rate series (proxy for MMF/CMA in KRW and T-Bill ETFs such as `SGOV`/`BIL` in USD) with a realistic 0.1% FX conversion spread, we simulate monthly contributions of **1,000,000 KRW** across three strategies:
1. **KRW Deposit $\rightarrow$ Lump-Sum FX:** Accumulate in Korean 3-month rates and convert the entire balance to USD at the target date.
2. **USD T-Bill Monthly DCA:** Convert monthly contributions immediately into USD and compound at US 3-month T-Bill rates.
3. **Rate-Spread Dynamic DCA:** Direct new monthly contributions to USD when $i^{US}_t > i^{KR}_t$, and to KRW otherwise.

To eliminate start-date bias, we additionally run **rolling 36-month (3-year) backtests** across all overlapping windows from 2010 to 2026 and decompose outcomes by interest-rate regime (Rate Inversion vs. Normal).

---

## 3. Empirical Results

### 3.1 UIP Regression Output ($N = 190$, HAC Standard Errors)

| Parameter | Coefficient | HAC Std. Error | $z$-stat | $p$-value | 95% Confidence Interval |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Intercept ($\alpha$)** | **+0.0222** | 0.010 | 2.183 | **0.029** | `[+0.002, +0.042]` |
| **Rate Differential ($\beta$)** | **-0.8717** | 0.446 | -1.953 | **0.051** | `[-1.746, +0.003]` |

* **$R^2$:** `0.048` (Adj. $R^2$: `0.042`) | **F-statistic:** `3.814` ($p = 0.0523$) | **Durbin–Watson:** `0.363`

![UIP Regression Result](images/uip_test_result.png)

**Key Econometric Findings:**
* **Strong Rejection of UIP (Forward Premium Puzzle):** The theoretical UIP slope ($\beta = +1.0$) lies far outside the 95% confidence interval (`[-1.746, 0.003]`). Contrary to textbook theory, periods where US rates exceeded Korean rates were followed by **USD appreciation**, not depreciation.
* **Structural KRW Depreciation Drift:** A statistically significant positive intercept ($\alpha = +2.22\%$, $p = 0.029$) indicates an average annual upward drift in USD/KRW even when interest rates are equalized.
* **Low Short-Horizon Explanatory Power:** An $R^2$ of 4.8% is consistent with the classic Meese–Rogoff exchange rate disconnect puzzle, while the Durbin–Watson statistic (`0.363`) confirms that HAC correction was essential for valid inference.

---

### 3.2 Dollar Accumulation Backtest Results

![Dollar Backtest Summary](images/dollar_backtest_summary.png)

#### Full-Period Accumulation (2010–2026, 1,000,000 KRW/month)
| Strategy | Final USD Value | Diff vs. KRW Lump-Sum ($) | Diff vs. KRW Lump-Sum (%) |
| :--- | :---: | :---: | :---: |
| **1. KRW Deposit $\rightarrow$ Lump-Sum FX** | `$[180,XXX]` | `$0.00` | `0.00%` |
| **2. USD T-Bill Monthly DCA** | `$[209,XXX]` | `+$[29,XXX]` | `+[XX.X]%` |
| **3. Rate-Spread Dynamic DCA** | `$[187,XXX]` | `+$[7,XXX]` | `+[X.X]%` |

#### Rolling 36-Month (3-Year) Risk & Return Summary
* **USD T-Bill DCA Win Rate:** `[XX.X]%` of all rolling 3-year windows (Mean outperformance: `[+X.XX]%`)
* **Dramatic FX Volatility Reduction:** Across all rolling 3-year horizons, monthly USD T-Bill accumulation reduced the standard deviation of final dollar wealth by **46.9%**—from **`$3,024`** (KRW Lump-Sum) down to **`$1,607`** (USD DCA).
* **Downside Protection for Risk-Averse Savers:** While KRW Lump-Sum achieved higher extreme right-tail outcomes during rare KRW rallies (~$37k), it exposed savers to severe left-tail FX shocks (~$24.2k). USD DCA tightly anchored final wealth between ~$27.2k and ~$33.2k, offering superior downside protection for fixed future USD obligations.

#### Performance by Interest-Rate Regime (Rolling 3-Year)
| Regime | Windows | USD DCA Win Rate (%) | Avg. USD Outperf. (%) | Avg. Dynamic Outperf. (%) |
| :--- | :---: | :---: | :---: | :---: |
| **US Rate > KR Rate (Inversion)** | `[XX]` | `[XX.X]%` | `[+X.XX]%` | `[+X.XX]%` |
| **KR Rate >= US Rate (Normal)** | `[XX]` | `[XX.X]%` | `[±X.XX]%` | `[±X.XX]%` |

---

## 4. Discussion & Limitations

1. **Why Pure USD DCA Outperformed the Dynamic Strategy:** In Strategy 3 (Dynamic DCA), switching only *new* monthly cash flows left previously accumulated KRW balances from 2010–2021 exposed to the sharp USD surge of 2022–2026. This highlights that **stock (existing balance) currency exposure dominates flow (new contribution) timing** during regime shifts.
2. **Real-World Frictions Not Modeled:** The baseline simulation incorporates FX spreads (with sensitivity tests from `0.0%` to `1.0%`), but abstracts away from domestic dividend/interest withholding taxes (15.4% in Korea / 15% US dividend tax) and ETF expense ratios (~0.09% for `SGOV`).
3. **Duration Isolation:** To stay consistent with the 3-month UIP regression and isolate FX/rate effects from bond price volatility, longer-duration Treasuries (e.g., 10Y `IEF`) were excluded from the baseline model.

---

## 5. Repository Structure & Reproduction

```text
uip-dollar-strategy/
├── images/
│   ├── uip_test_result.png          # Step 1: UIP OLS vs. Theoretical Line
│   └── dollar_backtest_summary.png  # Step 2: Cumulative, Rolling & Risk Histograms
├── notebooks/
│   ├── 01_uip_test.ipynb            # FRED ingestion & HAC OLS regression
│   └── 02_dollar_backtest.ipynb     # 3-strategy, rolling 3Y & spread sensitivity
├── requirements.txt
└── README.md
