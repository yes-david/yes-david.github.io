---
layout: post
title: "Modeling Economic Phenomena: Interest Rate-Centered Analysis of Demand and Value"
date: 2025-11-13
categories: [Economics]
---

## Research Background: Phenomena and Problems

Countless reports and news articles deliver information in fragments. Companies like Samsung Electronics chase the AI boom, while biotech firms focus on technology exports.

This article focuses on two pillars of South Korea's growth, semiconductors and biotech. Samsung Electronics is the largest company on the KOSPI by market value and Alteogen is the largest on the KOSDAQ, and together they stand for the present and the future of the Korean economy. The two topics look separate at first glance, but a single variable connects them: the interest rate.

Interest rates are more than just percentages. They are the price of time, a measure of risk, and the discount rate used to value every asset. This semester, while taking Econometrics and learning regression models and forecasting techniques, I became fascinated by the engineering problems hidden inside economic phenomena, and by analyzing the KOSPI index with ARIMA and LSTM models in a student research group.

Centering on interest rates, this article proposes ways to analyze and address these problems using data, mathematical models, and predictive algorithms.

---

## Chapter 1. Interest Rates and the Economy: Multi-Variable Demand Forecasting for Samsung Electronics

### Phenomenon

Semiconductor companies like Samsung Electronics face two kinds of demand at the same time. One is cyclical demand for PCs and smartphones, which rises and falls with interest rates. The other is structural growth in HBM, driven by AI. Because the two overlap, no single indicator explains where demand is headed.

### Problem Definition

Forecasting future demand and prices for DRAM and HBM calls for a time series model that can hold both sides at once. Traditional cyclical variables such as interest rates and inventory belong in it, and so do structural growth variables such as the level of AI investment.

### Proposed Approach

The data should combine macro variables such as interest rates, exchange rates, and PC shipments with industry variables such as server investment, HBM prices, and inventory. Time-lagged features matter here, since past values affect demand only after a delay.

ARIMA and LSTM models can then learn the patterns in that series and forecast demand, the first capturing short-term movement and the second capturing longer trends.

Beyond that, machine learning models such as Random Forest regression or XGBoost can explore non-linear relationships and interaction effects between variables.

### Analytical Goals

Predict future demand, assess indirectly how much each variable contributes, and compare the results against traditional statistical models to sharpen the analysis.

---

## Chapter 2. Interest Rates and Time: Valuation Under Uncertainty at Alteogen

### Phenomenon

Alteogen does not develop new drugs. It exports the ALT-B4 platform technology, which converts existing drugs from intravenous to subcutaneous delivery. Its value comes not from upfront contracts but from royalties that arrive years later, and only if partner companies succeed in commercializing SC products.

A higher interest rate means a higher discount rate. Raise it, and royalties worth 1 trillion KRW ten years from now are worth 610 billion KRW today.

### Problem Definition

The uncertainty sits in two places: whether partner companies commercialize SC products successfully, and how far those products penetrate the market. A valuation model has to account for uncertainty in the cash flows and uncertainty in market interest rates at the same time.

### Proposed Approach

A decision tree can split ten years of future cash flows into weighted scenarios, with high market penetration at 30%, medium at 50%, and low at 20%. Since the platform technology is already validated, modeling uncertainty in revenue is more realistic than modeling outright failure.

### Analytical Goals

Run a sensitivity analysis to see how changes in SC product penetration and in the applied discount rate move overall corporate value. This makes it possible to tell whether a swing in the stock price came from partner sales performance or simply from a change in market interest rates.

---

## Conclusion: Seeking Models Beyond Phenomena

The complexity of capital markets can ultimately be expressed through data, analyzed with models, and predicted with algorithms.

As a student pursuing multiple majors, I aim not only to interpret macroeconomic trends but also to define the hidden engineering problems inside them, and to apply what I have learned to find optimal solutions through research.
