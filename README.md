## Executive Summary

This project develops an automated time-series analytics pipeline to track, validate, and model daily **Europe Brent Crude Oil Spot Prices (`RBRTE`)** sourced directly from the **U.S. Energy Information Administration (EIA) API v2**. Analyzing 100 trading observations (June 30, 2026 – November 17, 2026), the workflow transforms raw API payloads into validated statistical distributions and trend indicators (`SMA_7` and `SMA_30`). The results reveal significant commodity price dispersion—spanning a **$66.98/BBL** range ($68.53 to $135.51/BBL) with a mean of **$102.08/BBL** and a standard deviation of **$17.68/BBL**—demonstrating how moving-average smoothing isolates actionable short- and medium-term market momentum from high-frequency daily price noise.

---

## Key Business Problem

Global crude oil markets are subject to rapid, sharp price swings driven by geopolitical events, supply-demand imbalances, and macroeconomic shifts. For energy-intensive enterprises (airlines, logistics providers, refiners, and manufacturers) as well as commodity trading desks, relying on unsmoothed daily spot prices creates three critical operational risks:

* **Signal vs. Noise Distortion:** Daily spot fluctuations often mask underlying directional shifts, leading procurement and trading teams to react prematurely to temporary price spikes or dips.
* **Margin & Budget Exposure:** Wide price dispersion (such as a nearly 2x swing between period lows and highs) directly threatens operating margins and complicates fuel cost forecasting if volatility and trend baselines are not systematically tracked.
* **Manual Reporting Bottlenecks:** Relying on static spreadsheets or manual data pulls delays risk assessment and prevents timely hedging or inventory allocation decisions.

This pipeline addresses these challenges by automating daily price ingestion, enforcing zero-missing-value data quality checks, quantifying price dispersion, and generating dual-horizon Simple Moving Averages (`SMA_7` and `SMA_30`) to support disciplined, data-driven hedging, budgeting, and risk management.

---
