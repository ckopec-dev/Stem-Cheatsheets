# Financial Analysis Cheatsheet

## Trading

### Types of traders

- Day: holds positions for less than 48 hours
- Swing: holds positions for a few days to several weeks
- Position: holds positions for a few months to several years

### Indicator Types

- Trend: measures the strength and/or direction of a trend
- Momentum: measures the velocity of price movement
- Volatility: meantues the magnitude of price deviations

### Technical indicators

- Simple Moving Average (SMA): mean price over a specified n-period
  - Calculation: (P<sub>1</sub> + P<sub>2</sub> + ... + P<sub>n</sub>) / n
- Exponential Moving Average (EMA): exponentially weighted average
  - Calculation: EMA<sub>n</sub> = P<sub>n</sub> * m + previous EMA * (1 - m), where m = 2 / (n + 1)
  - Advantages: more sensitive to most recent price movement

### Signals

- Triggers to long or short assets based on criteria, constructed using indicators and/or market data.
- Examples:
  - Buy long when price rises above the SMA, and exit when the price lowers below the SMA.
  - Buy long when short-term EMA crosses above the long-term EMA.
  - Buy short when short-term EMA crosses below the long-term EMA.
  - Buy short when RSA > 70.
  - Buy long when RSA < 30.

### Strategy Types

- Trend-following: bets the price trend will continue
- Mean reversion: bets the price tends to reverse back towards the mean
