# Trader Performance vs Market Sentiment
### Primetrade.ai — Data Science Intern Assignment

---

## Overview

This project investigates how Bitcoin market sentiment (Fear/Greed Index) relates to trader behavior and performance on the Hyperliquid perpetuals exchange. The analysis spans trade-level data merged with daily sentiment classifications to surface actionable patterns for smarter trading strategies.

---

## Repository Structure

```
primetrade-assignment/
│
├── Complete Notebook.ipynb          # Main analysis notebook (end-to-end)
│
├── Datasets/
│   ├── fear_greed_index.csv    # Daily market sentiment — is the market panicking (Fear) 
│   │                             or euphoric (Greed)?
│   └── historical_data.csv     # Every trade made — who traded, what, how big, at what
│                                leverage, and whether they made money
│
├── Outputs/
│   ├── Confusion Matrix.png
│   ├── Drawdown Proxy by Sentiment.png
│   ├── Feature Importance.png
│   ├── Long-Short Bias by Sentiment.png
│   └── etc
│
├── README.md                   # This file
└── Insites_And_Strategies.pdf                  # 1-page summary of insites and strategies
```

---

## Datasets

| Dataset | Source 
|---|---|
| Fear/Greed Index | https://drive.google.com/file/d/1PgQC0tO8XN-wqkNyghWc_-mnrYv_nhSf/view?usp=sharing |
| historical_data.csv | https://drive.google.com/file/d/1IAfLZwu6rJzyWKgBToqwSmmVYU6VbjVs/view |

---

## Setup Instructions

### 1. Clone the repository

```bash
git clone https://github.com/your-username/primetrade-assignment.git
cd primetrade-assignment
```

### 2. Create a virtual environment (recommended)

```bash
python -m venv venv
source venv/bin/activate        # Mac/Linux
venv\Scripts\activate           # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

**Required packages:**

```
pandas
numpy
matplotlib
seaborn
scikit-learn
jupyter
```

Or install directly:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### 4. Add the data files

Download both CSV files from the provided Google Drive links and place them inside the correct folder.

### 5. Launch the notebook

```bash
jupyter notebook notebooks/Complete Notebook.ipynb
```

Run all cells top to bottom (`Kernel → Restart & Run All`).

---

## How to Run

The notebook is self-contained and runs in five sequential sections:

| Section | What it does |
|---|---|
| **Part A** | Loads, cleans, and merges both datasets; engineers key features |
| **Part B-1** | Sentiment vs Performance analysis (PnL, win rate, drawdown) |
| **Part B-2** | Sentiment vs Behavior analysis (leverage, trade size, long/short ratio) |
| **Part B-3** | Trader segmentation (high vs low leverage, frequent vs infrequent, winners vs losers) |
| **Part C** | Strategy recommendations with supporting evidence |

All charts are automatically saved to the `charts/` directory on run.

---
## Methodology

### Data Preparation
Only **closed trades** (`event = CLOSE`, `closedPnL != 0`) were used for performance analysis to ensure all PnL figures represent realized outcomes. Trades on dates outside the sentiment dataset's range were excluded.

### Feature Engineering
The following metrics were computed per trader per day:

- **Daily PnL** — sum of `closedPnL`
- **Win Rate** — share of trades with `closedPnL > 0`
- **Average Trade Size** — mean `size` in USD
- **Leverage Tier** — binned as Low (≤3x), Medium (4–10x), High (>10x)
- **Long/Short Ratio** — count of long trades ÷ total trades
- **Trade Frequency** — number of trades per day per account

Sentiment was simplified into two groups for most comparisons: **Fear** (Extreme Fear + Fear) vs **Greed** (Greed + Extreme Greed), with Neutral excluded from binary comparisons.

## Notes

- Timestamp alignment is done at the **daily level** (UTC)
- Trades without a corresponding sentiment date are dropped after logging
- `closedPnL` is used as the primary performance metric (realized PnL only)
- Leverage is treated as a categorical bin (Low: ≤3x, Medium: 4–10x, High: >10x) for segmentation
