# Trader_sentiment_analysis


> Uncovering how Bitcoin market sentiment shapes trader behaviour, surfaces behavioural archetypes, and identifies the true drivers of profitability — using Hyperliquid historical trade data merged with the Fear & Greed Index.

---

## Overview

This project combines **Hyperliquid on-chain trade history** with the **Bitcoin Fear & Greed Index** to answer a core question in quantitative trading: *does market sentiment actually determine who wins and loses, or do trader-specific behaviours matter more?*

The analysis spans exploratory data analysis, unsupervised clustering, a supervised classification model, SHAP explainability, and an LLM-powered insight layer.

---

## Key Questions Explored

- Do traders perform differently under Fear vs. Greed market conditions?
- Can traders be clustered into meaningful behavioural archetypes?
- What features most strongly predict whether a trader will be profitable?
- Do top performers rely on market timing, or on consistent discipline?

---

## Methodology

**1. Data Merging**
Hyperliquid trade records are joined to daily Fear & Greed classifications on trade date, creating a unified dataset with sentiment context for every closed position.

**2. Exploratory Analysis**
PnL distributions, win rates, and average returns are broken down by sentiment regime (Extreme Fear → Extreme Greed) and compared across trader performance tiers.

**3. Feature Engineering**
Trader-level features are derived from raw trades:
- `win_rate` — share of profitable trades
- `avg_pnl_per_size` — risk-adjusted return proxy
- `contrarian_score` — tendency to trade against prevailing sentiment
- `trader_consistency` — inverse of PnL standard deviation

**4. KMeans Clustering (k=4)**
Traders are segmented into four archetypes: *Consistent Performers*, *Contrarian Winners*, *Sentiment Followers*, and *Struggling Traders*.

**5. Random Forest + SHAP**
A classifier predicts trader profitability and SHAP values reveal which features drive the model's decisions — separating genuine skill signals from noise.

**6. LLM Insight Layer**
Aggregated statistics are passed to a Mistral model prompted as a quantitative analyst, surfacing non-obvious patterns, contradictions, and recommended follow-up analyses.

---

## Key Findings

- **Sentiment shapes distribution, not ranking.** PnL spreads widen significantly during Extreme Greed, but top traders maintain their edge across all regimes.
- **Discipline beats timing.** `win_rate` and `trader_consistency` are the strongest predictors of profitability — outweighing sentiment exposure.
- **Contrarian Winners exist but are rare.** A small cluster of traders systematically bets against crowd sentiment and remains profitable.
- **Fear periods offer higher baseline win rates** across the trader population, making them historically safer trading windows.

---

## Tech Stack

`Python` · `pandas` · `scikit-learn` · `SHAP` · `Matplotlib / Seaborn` · `Mistral AI`

---

## Data Sources

| Dataset | Description |
|---|---|
| `historical_data.csv` | Hyperliquid closed trade records with PnL, size, side, and timestamp |
| `fear_greed_index.csv` | Daily Bitcoin Fear & Greed Index values and classifications |
