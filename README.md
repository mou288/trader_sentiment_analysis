# Trader Performance & Market Sentiment Analysis

> Uncovering how Bitcoin market sentiment shapes trader behaviour, surfaces behavioural archetypes, and identifies the true drivers of profitability — using Hyperliquid historical trade data merged with the Fear & Greed Index.


---
## If the ipynb file does not render or shows error on github, you can download and view it locally
---

## Overview

This project combines **Hyperliquid on-chain trade history** with the **Bitcoin Fear & Greed Index** to answer a core question in quantitative trading: *does market sentiment actually determine who wins and loses, or do trader-specific behaviours matter more?*

The analysis spans exploratory data analysis, unsupervised clustering, a supervised classification model, SHAP explainability, and an LLM-powered insight layer (Mistral).

---

## Key Questions Explored

- Do traders perform differently under Fear vs. Greed market conditions?
- Can traders be clustered into meaningful behavioural archetypes?
- What features most strongly predict whether a trader will be profitable?
- Do top performers rely on market timing, or on consistent discipline?

---

## Methodology

**1. Data Merging**
Hyperliquid trade records (211,224 raw → 35,864 matched) are joined to daily Fear & Greed classifications, covering 32 unique traders from January 2023 to May 2025.

**2. Exploratory Analysis**
PnL distributions, win rates, and average returns broken down by sentiment regime and compared across trader performance tiers.

**3. Feature Engineering**
Trader-level features derived from raw trades: `win_rate`, `avg_pnl_per_size` (risk-adjusted return proxy), `contrarian_score` (% of trades against prevailing sentiment), and `trader_consistency` (inverse PnL std dev).

**4. KMeans Clustering (k=4)**
Traders segmented into two dominant archetypes: *Contrarian Winners* and *Sentiment Followers*.

**5. Random Forest + SHAP**
A classifier predicts trader profitability. SHAP values identify which features drive decisions — separating genuine skill signals from noise.

**6. LLM Insight Layer**
Aggregated statistics passed to Mistral, prompted as a quantitative analyst, surfacing non-obvious patterns, contradictions, and recommended follow-up analyses.

---

## Key Findings

**1. Extreme Greed is the highest-EV regime, despite not having the highest win rate**
Average PnL under Extreme Greed is **$205.82 per trade** — the highest of any sentiment regime — yet win rate is only 55.3%. In contrast, Extreme Fear trades win only 29.3% of the time with a near-zero avg PnL of $1.89. The real alpha is not in trade frequency but in bet sizing during euphoric conditions.

**2. Contrarian Winners sacrifice win rate for outsized gains**
The Contrarian Winners archetype wins just **26.1% of trades** but generates an average PnL of **$137.68**, versus Sentiment Followers who win 43.4% of trades but average only **$62.00**. Fewer, higher-conviction bets against the crowd dramatically outperform high-frequency trend-following.

**3. Risk-adjusted return per size, not win rate, is the #1 profitability driver**
The Random Forest ranked `avg_pnl_per_size` (importance: **0.272**) above `win_rate` (0.251) and `trade_count` (0.165). Capital efficiency — earning more per dollar deployed — matters more than being right more often.

**4. Profitable traders are meaningfully more contrarian**
Profitable traders carry a contrarian score of **0.57** versus **0.365** for unprofitable ones — a ~56% gap. Trading against prevailing sentiment is a consistent, statistically separable trait of the winning population.

**5. Fear dominates trade volume but destroys returns**
Fear-regime trades account for the largest slice of the dataset (**13,869 trades**, 38.7% of total) but deliver only **$128.29 avg PnL** with a 38.2% win rate. Traders are most active precisely when market conditions are worst for them.

**6. The model achieves perfect classification on this dataset — flagging a survivorship risk**
The Random Forest scores **1.00 precision, recall, and F1** on the test set. With only 32 traders and 87.5% of them profitable, the dataset likely suffers from survivorship bias — unprofitable traders may have exited the platform before the observation window.

---

## Tech Stack

`Python` · `pandas` · `scikit-learn` · `SHAP` · `Matplotlib / Seaborn` · `Mistral AI`

---

## Data Sources

| Dataset | Description |
|---|---|
| `historical_data.csv` | Hyperliquid closed trade records — 211,224 rows across 32 traders |
| `fear_greed_index.csv` | Daily Bitcoin Fear & Greed Index values and classifications (2018–2025) |
