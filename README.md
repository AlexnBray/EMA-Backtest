# EMA Crossover Backtester

A lightweight Python backtesting tool for Exponential Moving Average (EMA) crossover strategies. Pulls historical price data via `yfinance`, simulates trades with realistic transaction costs, and produces an annotated chart with a full trade log.

---

<img width="1338" height="669" alt="Image" src="https://github.com/user-attachments/assets/e48bca55-b2f3-4420-b0bf-5072616bf306" />

## How It Works

The strategy is simple:
- **Buy** when the fast EMA crosses **above** the slow EMA (bullish crossover)
- **Sell** when the fast EMA crosses **below** the slow EMA (bearish crossover)

Each trade accounts for:
- A **percentage-based commission** (e.g. broker fee as % of trade value)
- A **fixed fee per transaction** (e.g. platform flat charge)
- A **bid/ask spread** (half applied on entry, half on exit)

Positions are fully liquidated on each sell signal. If the backtest ends while still holding shares, the final position is valued at the last close price (minus spread) and included in the portfolio total.

---

## Requirements

```bash
pip install yfinance matplotlib pandas numpy python-dateutil
```

---

## Usage

Configure the parameters at the bottom of the script and run it:

```python
########### USER INPUTS ############
stock       = 'AAPL'   # Ticker symbol
months      = 40        # Lookback period in months
fast        = 20        # Fast EMA span (periods)
slow        = 100       # Slow EMA span (periods)
portfolioSize = 10000   # Starting capital ($)

txCommPct   = 0.001     # Commission as % of trade value (0.1%)
txFixed     = 1.0       # Fixed fee per transaction ($)
spread      = 0.05      # Bid/ask spread ($)
####################################
```

Then run the script. Results are printed to the console and a chart is displayed.

---

## Output

### Console
```
====== AAPL Backtest Results ======
Market Cost (initial): $10,000.00
Market Value (final):  $13,420.55
Percentage G/L:        34.21%
Trades Completed:      7

--- Trade History ---
2022-06-14 | SELL | Price: $134.51 | Shares: 62 | PnL: $-412.30 (-4.71%) | Fees: $18.24
...
```

### Chart
- **Black line** — closing price
- **Orange line** — fast EMA
- **Purple line** — slow EMA
- **Green dashed lines** — buy signals
- **Red dashed lines** — sell signals

---
