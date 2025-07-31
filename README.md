# VWAP-Based Order Execution Strategy

This repository contains Python code and analysis for a trading assignment focused on minimizing temporary market impact when executing large orders. The strategy simulates Volume-Weighted Average Price (VWAP) execution using Level II order book data and formulates an allocation model to determine how to spread the purchase of a fixed number of shares throughout the trading day.

## 📌 Assignment Summary
You are given:
- A total order size \(S\)
- Minute-by-minute Level II data for three stock tickers
- A trading day broken into \(N = 390\) one-minute intervals

The goal is to compute an allocation vector \(x \in \mathbb{R}^N\) such that \(\sum x_i = S\), minimizing cumulative **slippage** — the difference between execution price and midpoint price.

## 🧠 Approach

1. **Data Preparation**
   - Clean and normalize price/size fields in Level II data
   - Construct minute-level snapshots by extracting the midpoint record from each minute

2. **Slippage Model**
   - Compute slippage \(g_t(x)\) as the difference between simulated VWAP and the midpoint price
   - Simulate VWAP using **10 levels** of order book data (ask price and size columns from level 1 to level 10)

3. **Greedy Allocation Strategy**
   - Allocate shares in small blocks to the minute with the lowest marginal slippage
   - Repeat until all \(S\) shares are assigned
   - This loop avoids high-slippage intervals by prioritizing cheaper liquidity early

4. **Output**
   - CSV files that log share allocations per minute for each ticker
   - Visualization of the piecewise temporary impact function \(g_t(x)\)

## 📂 Files
- `simulate_vwap.py`: Function for VWAP calculation using 10 ask levels
- `main_allocation.py`: Allocation logic and greedy loop
- `data/*.csv`: Pre-cleaned Level II data by symbol
- `outputs/*.csv`: Final allocations for each ticker

## 🧪 Requirements
- Python 3.8+
- pandas, matplotlib, numpy

## 🧪 Usage
```bash
# Upload CSV and run in Google Colab or local Python
python main_allocation.py --symbol FROG --shares 500
```
