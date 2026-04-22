# 📊 Trader Performance vs Market Sentiment
### Primetrade.ai — Data Science Intern Assignment

> **Analyzing how Bitcoin Fear/Greed cycles shape trader behavior and profitability on Hyperliquid**

---

## 🗂️ Project Structure

```
primetrade_assignment/
│
├── 📓 primetrade_notebook.ipynb     # Main analysis notebook (run this)
├── 📄 README.md                     # You are here
│
├── 📁 data/
│   ├── fear_greed_index.csv         # Bitcoin Fear/Greed Index (2018–2025)
│   └── historical_data.csv          # Hyperliquid trade data (211K rows)
│
└── 📁 charts/
    ├── chart1_performance_by_sentiment.png
    ├── chart2_pnl_boxplot.png
    ├── chart3_behaviour_heatmap.png
    ├── chart4_longshort_bias.png
    ├── chart5_segment_pnl.png
    ├── chart6_cross_heatmap.png
    ├── chart7_scatter_freq_pnl.png
    ├── chart8_drawdown_timeline.png
    ├── chart9_model_results.png
    └── chart10_clusters.png
```

---

## ⚙️ Setup & Installation

### 1 — Clone or download the repo

```bash
git clone https://github.com/your-username/primetrade-assignment.git
cd primetrade-assignment
```

### 2 — Install dependencies

```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy jupyter nbformat
```

> Python 3.8+ recommended. No additional configuration needed.

### 3 — Place the data files

Make sure both CSV files are in the **same folder** as the notebook:
- `fear_greed_index.csv`
- `historical_data.csv`

### 4 — Run the notebook

```bash
jupyter notebook primetrade_notebook.ipynb
```

Or execute all cells at once headlessly:

```bash
jupyter nbconvert --to notebook --execute primetrade_notebook.ipynb --output executed.ipynb
```

---

## 📦 Datasets

| Dataset | Source | Rows | Key Columns |
|---|---|---|---|
| Bitcoin Fear/Greed Index | Alternative.me | 2,644 | `date`, `value`, `classification` |
| Hyperliquid Trader Data | Hyperliquid DEX | 211,224 | `Account`, `Coin`, `Closed PnL`, `Side`, `Size USD`, `Timestamp IST` |

**Date overlap used for analysis:** January 2024 – December 2024

**Sentiment classes:** Extreme Fear · Fear · Neutral · Greed · Extreme Greed

---

## 🔬 Methodology

### Part A — Data Preparation
- Loaded and documented shape, missing values, and duplicates for both datasets
- Parsed `Timestamp IST` (dayfirst format) and normalized to daily granularity
- Merged on `date` with an inner join to ensure clean sentiment coverage
- Engineered daily per-trader metrics: PnL, win rate, trade count, position size, long/short ratio

### Part B — Analysis
- Compared PnL, win rate, and drawdown across all 5 sentiment classes
- Used Welch's t-test to validate statistical significance of Fear vs Greed PnL differences
- Built 3 trader segmentation schemes (frequency, performance tier, position size) using tertile splits
- Cross-analyzed segment × sentiment to uncover interaction effects

### Part C — Strategy Output
- Derived 2 actionable trading rules directly from segment × sentiment findings
- Quantified the PnL impact of each recommended behavioral change

### Bonus
- **Random Forest classifier** (5 features, 300 trees) predicting daily profitability — evaluated with AUC-ROC and 5-fold cross-validation
- **K-Means clustering** (k=4) on standardized trader-level features to identify behavioral archetypes

---

## 💡 Key Insights

### 1 · Extreme Fear = Highest Trader PnL
Counterintuitively, days labeled **Extreme Fear** produced the highest mean daily PnL per trader. Panic-driven selling creates mispricings that disciplined contrarian traders exploit. Greed days show elevated win rates but significantly thinner per-trade margins — crowded longs compress returns.

### 2 · Frequent Traders Are Hurt Most on Fear Days
High-frequency traders (top 33% by trade count) show **negative average PnL on Fear days**. Overtrading during volatile, sentiment-driven markets causes repeated small losses that compound quickly. Infrequent, selective traders — who wait for high-conviction setups — outperform significantly on the same days.

### 3 · Long Bias Spikes on Greed Days — a Fade Signal
Traders open significantly more BUY positions on Greed and Extreme Greed days, reflecting herd behavior. This creates a systematic fade opportunity: when the crowd is maximum long, short setups have asymmetric upside. The long/short ratio is the clearest behavioral signal tied to sentiment.

### 4 · 4 Distinct Trader Archetypes Exist
K-Means clustering identifies four behavioral profiles regardless of market conditions:
- 🏆 **Disciplined Winners** — moderate frequency, controlled sizing, consistent PnL
- ⚡ **High-Freq Scalpers** — very high trade count, small sizes, marginal profitability
- 💸 **Reckless Losers** — large positions, low win rate, significant negative PnL
- 🎲 **Casual Traders** — low frequency, inconsistent behavior, near-zero net PnL

---

## 🎯 Strategy Recommendations

### Strategy 1 — "Contrarian Caution" *(for Frequent Traders)*

| Sentiment | Action |
|---|---|
| **Extreme Fear / Fear** | Cut trade count by ≥ 40% · Shift to SHORT (SELL) bias · Wait for high-conviction setups only |
| **Greed / Extreme Greed** | Trim winning long positions early · Avoid momentum chasing · Reduce position size by 20–30% |
| **Neutral** | Maintain baseline frequency · No directional bias adjustment needed |

**Evidence:** Frequent traders on Fear days averaged negative PnL; infrequent traders on the same days averaged positive PnL.

---

### Strategy 2 — "Sentiment-Aware Sizing" *(for Break-Even Traders)*

| Sentiment | Action |
|---|---|
| **Extreme Fear / Fear** | Smaller position sizes · Short-biased setups only · Strict stop-losses |
| **Greed / Extreme Greed** | Increase size on SHORT fade setups · Fade the crowd when long ratio > 60% |
| **Neutral** | Reduce overall exposure — no clear edge, transaction fees erode PnL |

**Evidence:** Break-even traders show the widest PnL variance across sentiment regimes — sizing discipline is the primary lever for improvement.

---

## 📈 Charts Overview

| Chart | Description | Assignment Section |
|---|---|---|
| `chart1` | Mean PnL & Win Rate by all 5 sentiment classes | Part B, Q1 |
| `chart2` | Full PnL distribution boxplots by sentiment | Part B, Q1 |
| `chart3` | Behavioural metrics heatmap (trades, size, long ratio) | Part B, Q2 |
| `chart4` | Long vs Short directional bias by sentiment | Part B, Q2 |
| `chart5` | Mean PnL across frequency / performance / size segments | Part B, Q3 |
| `chart6` | Segment × Sentiment cross-analysis heatmap | Part B, Q3 |
| `chart7` | Trade frequency vs total PnL scatter (bubble = avg size) | Part B, insight |
| `chart8` | Cumulative PnL + drawdown proxy with sentiment overlay | Part B, drawdown |
| `chart9` | Random Forest feature importance + confusion matrix | Bonus model |
| `chart10` | K-Means trader behavioral cluster scatter | Bonus clustering |

---

## 🧰 Tech Stack

| Library | Purpose |
|---|---|
| `pandas` | Data loading, cleaning, merging, groupby aggregations |
| `numpy` | Numerical operations, random seed |
| `matplotlib` | All chart rendering |
| `seaborn` | Heatmaps, distribution plots |
| `scipy.stats` | Welch's t-test for statistical significance |
| `scikit-learn` | Random Forest, K-Means, StandardScaler, cross-validation |

---
