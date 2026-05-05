# Financial Network & Energy Crisis in the S&P 500

## Introduction

Escalated conflicts in Iran led to increasing oil prices and affected many sectors, raising the question of how geopolitical conflict affects financial market structures.

This project explores the overall network dynamics and movement of S&P 500 stocks, with the main objective of applying new machine learning methods to provide meaningful insights in financial settings. Data is sourced from Yahoo Finance for daily stock prices. By using only correlations of returns, the analysis offers a quick way to assess the co-movement of stocks — and when combined with network analysis techniques, it reveals how the overall market network behaves following shifts in international politics.

***Key finding:*** Stocks are less distinctive during crisis times, as sectors cluster more closely together. Across all three time periods, the energy sector remains the most separated from the rest of the market.

---

## Data & Methods

### Time Periods

The start of the Iran conflict (**February 28, 2026**) is used as the crisis breakpoint, dividing the analysis into three periods:

| Period | Date Range |
|--------|-----------|
| 1-Year Pre-Crisis |    Feb 27, 2025 to Feb 27, 2026 |
| Pre-Crisis (1 month) |    Jan 1, 2026 to Feb 27, 2026 |
| Post-Crisis |    Feb 28, 2026 to Apr 24, 2026 |

**Data sources:** `yfinance` and [S&P 500 tickers/sectors via DataHub](https://datahub.io/core/s-and-p-500-companies)

### Data Processing

1. Downloaded daily closing prices from `yfinance` (Feb 27, 2025 - Apr 24, 2026)
2. Calculated 1-year returns to select the **top 5 stocks per sector**, yielding a final set of **55 stocks**
3. Monitored those stocks across all three periods to track structural changes after the conflict

### Network Analysis

- Pairwise correlations between stock returns are used to construct the network
- `NetworkX` is used for graph construction and visualization
- Stocks with a correlation **> 0.5** are considered connected, and an edge is drawn between them
- **Node size** reflects the number of connections (degree) each stock has

### Community Detection (Unsupervised ML)

The **Louvain method** is applied to the 1-month pre-conflict and post-conflict networks to detect optimal community structure. The algorithm works in two steps:

1. **Local optimization phase**: each node is initialized as its own community
2. **Aggregation phase**: nodes are grouped into "super-nodes" to maximize modularity

---

## Network Analysis (click each visual for an interactive version with more info)

### 1 Year Before the Conflict
[![1-Year Preview](https://github.com/tu-tong/Financial-Network-Analysis/raw/main/Network%20results/pre-1yr.png)](https://tu-tong.github.io/Financial-Network-Analysis/Network%20results/network_1yr.html)

### 1 Month Before the Conflict
[![1-Month Preview](https://github.com/tu-tong/Financial-Network-Analysis/raw/main/Network%20results/pre-1m.png)](https://tu-tong.github.io/Financial-Network-Analysis/Network%20results/network_1m.html)

### Post-Conflict
[![Post-Conflict Preview](https://github.com/tu-tong/Financial-Network-Analysis/raw/main/Network%20results/post-conflict.png)](https://tu-tong.github.io/Financial-Network-Analysis/Network%20results/network_post.html)

---

## Community Detection Results

The total number of communities **fell from 25 to 10** after the conflict, indicating substantial consolidation in market structure.

| Community | Stocks |
|-----------|--------|
| 1 | AES |
| 2 | APA, CF, VLO, BKR, HAL, MPC |
| 3 | BG, ALB, ADM |
| 4 | DG |
| 5 | CVNA, GOOGL, HCA, EBAY, DLR, IRM, HOOD, MRNA |
| 6 | SATS |
| 7 | EA |
| 8 | NEE, WELL, JNJ, CAH, EIX, ETR |
| 9 | PWR, STX, NRG, CIEN, WDC, FIX, SNDK, VRT, GEV, EQIX, LITE |
| 10 | NEM, GS, ROST, WBD, GM, C, MNST, STT, TPR, TKO, HST, BK, VTRS, STLD, CASY, CAT, FCX |

*Note*: Communities 5 and 10 are notably cross-sector — Community 5 spans Healthcare, Tech, and Real Estate, while Community 10 consolidates an even broader mix of industries.

---

## Conclusions

Comparing the 1-year and 1-month pre-conflict periods reveals the baseline market structure: **stocks separate clearly by sector** when conditions are calm.

After the conflict:

- The network **clusters together**, with energy remaining in its own isolated group
- **Node sizes grow**, indicating each stock forms more connections with others
- **Consumer Discretionary** and **Real Estate** join the market-wide movement rather than behaving independently as before

**Resistant stocks** (those that maintained distinct behavior post-conflict):

- Consumer Staples: `SATS`, `DG`
- Energy sector stocks
- Utilities: `EIX`
