---
layout: post
title: "Modeling Economic Phenomena: Interest Rate-Centered Analysis of Demand and Value"
date: 2025-11-13
categories: [CS, Economics, Analysis]
---

## Research Background: Phenomena and Problems

Countless reports and news articles deliver information in fragments. Companies like Samsung Electronics chase the AI boom, while biotech firms focus on technology exports.

This article focuses on two pillars of South Korea’s growth: semiconductors and biotech. In particular, Samsung Electronics (the largest KOSPI company by market cap) and Alteogen (the largest KOSDAQ company by market cap) symbolize the present and future of the Korean economy. While these topics appear distinct at first glance, they are interconnected through a single variable: the interest rate.

Interest rates are more than just percentages—they represent the price of time, a measure of risk, and the discount rate used to evaluate the value of all assets. This semester, while taking Econometrics and learning regression models and forecasting techniques, I became fascinated by the engineering problems hidden within economic phenomena while analyzing the KOSPI index using ARIMA and LSTM models in a student research group.

Centering on interest rates, this article proposes methods to analyze and address these problems using data, mathematical models, and predictive algorithms.

---

## Chapter 1. Interest Rates and the Economy: Multi-Variable Demand Forecasting – Samsung Electronics

### Phenomenon
Semiconductor companies like Samsung Electronics experience both cyclical demand influenced by interest rates (for PCs and smartphones) and structural growth driven by AI (HBM demand). These overlapping factors make it difficult to explain future demand using a single indicator.

### Problem Definition
A sophisticated time-series model is needed to forecast future semiconductor (DRAM/HBM) demand and prices, taking into account both traditional cyclical variables (interest rates, inventory) and structural growth variables (AI investment levels).

### Proposed Approach
* **Data Construction:** Include macro variables such as interest rates, exchange rates, and PC shipments, as well as server investments, HBM prices, and inventory data. Incorporate time-lagged features to capture the delayed effects of past values on future demand.
* **Model Application:** Use ARIMA and LSTM models to learn time-series patterns and predict future demand, capturing both short-term fluctuations and long-term trends.
* **Extension Ideas:** Machine learning models like Random Forest regression or XGBoost can further explore non-linear relationships and interaction effects between variables.

### Analytical Goals
Predict future demand, indirectly assess the influence of each variable, and compare results with traditional statistical models to gain deeper analytical insights.

---

## Chapter 2. Interest Rates and Time: Valuation Under Uncertainty – Alteogen

### Phenomenon
Alteogen does not develop new drugs but exports the ALT-B4 platform technology, which converts existing drugs from intravenous to subcutaneous delivery. Its value comes not from upfront contracts but from royalties received years later if partner companies successfully commercialize SC products. When interest rates—or discount rates—rise, the present value of 10-year future royalties worth 1 trillion KRW drops to 610 billion KRW.

### Problem Definition
The uncertainty lies in whether partner companies successfully commercialize SC products and the market penetration of those products. A valuation model is needed to reasonably account for both cash flow uncertainty and market interest rate uncertainty.

### Proposed Approach
* **Model Application:** Design a decision tree model dividing 10-year future cash flows into probabilistic scenarios: high market penetration (30%), medium (50%), low (20%). Since the platform technology is validated, modeling revenue uncertainty rather than complete failure is more realistic.

### Analytical Goals
Conduct sensitivity analysis to see how changes in SC product penetration and applied discount rates affect overall corporate value. This helps distinguish whether stock price fluctuations are due to partner sales performance or simply changes in market interest rates.

---

## Conclusion: Seeking Models Beyond Phenomena

The complexity of capital markets can ultimately be expressed through data, analyzed via models, and predicted with algorithms.

As a student pursuing multiple majors, I aim not only to interpret macroeconomic trends but also to define hidden engineering problems within them and apply learned knowledge to find optimal solutions through research.
