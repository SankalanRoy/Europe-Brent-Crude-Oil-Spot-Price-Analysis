# 🛢️ Europe Brent Crude Oil Spot Price Analysis (`RBRTE`)

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)
![Data Source](https://img.shields.io/badge/Data%20Source-U.S.%20EIA%20API%20v2-green)
![Status](https://img.shields.io/badge/Status-Active-success)

An automated data ingestion, exploratory data analysis (EDA), and trend-smoothing pipeline for daily **Europe Brent Crude Oil Spot Prices FOB (`RBRTE`)** measured in `$/BBL`, sourced directly from the **U.S. Energy Information Administration (EIA) API v2**.

---

## 📌 Table of Contents
- [Overview & Purpose](#-overview--purpose)
- [Real-World Market Usefulness](#-real-world-market-usefulness)
- [Dataset Snapshot](#-dataset-snapshot)
- [Methodology & Mathematical Formulas](#-methodology--mathematical-formulas)
- [Project Workflow](#-project-workflow)
- [Getting Started](#-getting-started)
- [Future Enhancements](#-future-enhancements)

---

## 🎯 Overview & Purpose

This project retrieves, validates, and analyzes daily Brent crude spot prices to uncover underlying market trends and price dispersion:

1. **Automated Data Ingestion & Quality Assurance:** Fetches recent daily spot price observations (`100` records covering **2026-06-30 to 2026-11-17**), enforces proper `float64` and `datetime64[ns]` data types, sorts chronologically, and verifies data completeness (`0` missing values).
2. **Price Behavior & Distribution Analysis:** Examines historical price trajectories and visualizes price concentration using time-series line plots and Kernel Density Estimate (KDE) histograms.
3. **Short- & Medium-Term Trend Detection:** Smooths daily market noise using **7-day** and **30-day Simple Moving Averages (`SMA_7`, `SMA_30`)** to distinguish broader directional trends from short-term price swings.

---

## 🌍 Real-World Market Usefulness

Brent crude serves as the primary global benchmark for pricing international oil trades. Insights from spot price tracking, distribution metrics, and moving averages support practical decision-making across several sectors:

- **Energy Trading & Risk Management:** Traders and commodity desks monitor trend crossovers and price dispersion to gauge momentum, set stop-loss/take-profit thresholds, and size positions relative to market risk.
- **Corporate Hedging & Procurement:** Airlines, maritime shipping companies, chemical manufacturers, and refiners rely on fuel and feedstock cost trends to time forward contracts, swaps, or options hedges to protect operating margins.
- **Macroeconomic & Inflation Forecasting:** Sustained shifts in crude oil prices pass through to transportation, manufacturing, and consumer energy costs, helping economists anticipate headline inflation pressures and trade balance shifts.
- **Budgeting & Capital Allocation:** Energy producers, governments, and fuel-intensive enterprises use historical ranges, central tendencies, and trend baselines to stress-test cash flows and capital expenditure plans.

---

## 📊 Dataset Snapshot

| Metric | Value |
| :--- | :--- |
| **Series ID** | `RBRTE` (Europe Brent Spot Price FOB) |
| **Unit** | Dollars per Barrel (`$/BBL`) |
| **Sample Size** | `100` trading days (`0` missing values) |
| **Date Range** | `2026-06-30` to `2026-11-17` |
| **Mean Price** | `$102.08 / BBL` |
| **Median Price** | `$96.94 / BBL` |
| **Min / Max Price** | `$68.53 / BBL` — `$135.51 / BBL` |
| **Price Std. Deviation** | `$17.68 / BBL` |

---

## 📐 Methodology & Mathematical Formulas

### 1. Simple Moving Average (SMA)
A **Simple Moving Average (SMA)** is an unweighted rolling mean of prices over a fixed lookback window of $n$ consecutive periods (such as $n = 7$ or $n = 30$ trading days). Giving equal weight to each observation filters out short-term noise and highlights the underlying market direction:
- **`SMA_7` (7-Day):** Reacts rapidly to recent price movements (short-term momentum).
- **`SMA_30` (30-Day):** Provides a smoother, slower-moving baseline (medium-term trend).

For a chronologically ordered price series $P_1, P_2, \dots, P_t$, the $n$-period SMA at time $t$ (where $t \ge n$) is defined as:

$$\text{SMA}_t(n) = \frac{1}{n} \sum_{i=t-n+1}^{t} P_i = \frac{P_{t-n+1} + P_{t-n+2} + \dots + P_t}{n}$$

Where:
- $\text{SMA}_t(n)$ = Simple Moving Average over $n$ periods ending at period $t$
- $P_i$ = Crude oil spot price (`value`, in `$/BBL`) at period $i$
- $n$ = Rolling window length ($7$ or $30$)

---

### 2. Volatility
**Volatility** measures the degree of variation or dispersion in prices (or returns) over time. High volatility reflects sharp, unpredictable price swings, while low volatility indicates stable price action.

In financial and commodity analytics, volatility is typically quantified using the standard deviation of period-over-period returns:

1. **Simple Return ($r_t$) and Log Return ($u_t$):**
   $$r_t = \frac{P_t - P_{t-1}}{P_{t-1}} = \frac{P_t}{P_{t-1}} - 1, \qquad u_t = \ln\!\left(\frac{P_t}{P_{t-1}}\right)$$

2. **Sample Historical Volatility (over $N$ return observations):**
   $$s = \sqrt{\frac{1}{N - 1} \sum_{t=1}^{N} \left(r_t - \bar{r}\right)^2}$$
   where $\bar{r} = \frac{1}{N} \sum_{t=1}^{N} r_t$ is the sample mean return.

3. **Rolling Volatility (Window of $w$ periods):**
   $$\sigma_{\text{rolling}, t}(w) = \sqrt{\frac{1}{w - 1} \sum_{i=t-w+1}^{t} \left(r_i - \bar{r}_i\right)^2}$$

4. **Annualized Volatility (assuming 252 trading days per year):**
   $$\sigma_{\text{annualized}} = s_{\text{daily}} \times \sqrt{252}$$

---

## ⚙️ Project Workflow

1. **Data Extraction & Cleaning (`oil_df`):** Queries `https://api.eia.gov/v2/petroleum/pri/spt/data/` for series `RBRTE`, casts `value` to `float64` and `period` to `datetime64[ns]`, and runs summary/null checks.
2. **Exploratory Visualization:** Generates a daily spot price line chart and a distribution histogram overlaid with a Kernel Density Estimate (KDE).
3. **Trend Smoothing (`oil_df_sorted`):** Sorts observations chronologically by `period` and computes `.rolling(window=7).mean()` (`SMA_7`) and `.rolling(window=30).mean()` (`SMA_30`) to compare short- and medium-term trends against daily prices.

---

## 🚀 Getting Started

### Prerequisites
Install the required Python libraries:
```bash
pip install requests pandas matplotlib seaborn
```

### API Key Configuration
1. Obtain a free API key from the [U.S. EIA Open Data Portal](https://www.eia.gov/opendata/).
2. Set your API key as an environment variable (recommended rather than hardcoding it in your notebook/script):
   ```bash
   export EIA_API_KEY="your_api_key_here"
   ```

### Running the Analysis
Open and run the Jupyter Notebook (`.ipynb`) or Python script to fetch the latest `RBRTE` spot prices and generate the summary statistics and visualizations.

---

## 🔮 Future Enhancements
- [ ] Add rolling return volatility (`Volatility_7` and `Volatility_30`) alongside `SMA_7` and `SMA_30`.
- [ ] Implement bullish/bearish SMA crossover signal detection (`SMA_7` vs. `SMA_30`).
- [ ] Compare Brent (`RBRTE`) against WTI (`RWTC`) to analyze the Brent–WTI spread.
