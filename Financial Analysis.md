<!-- markdownlint-disable MD029 -->
<!-- markdownlint-disable MD033 -->

# Financial Analysis Cheatsheet

## Trading

### Trader Types

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
  

### Strategy Types

- Trend-following: bets the price trend will continue
- Mean reversion: bets the price tends to reverse back towards the mean

## Stock Trading Strategies

### 📈 By Time Horizon

1. **Day Trading**
   - Buy and sell within the same day.
   - Relies on intraday price movements.
   - Uses technical indicators, chart patterns, and news.

2. **Swing Trading**
   - Holds positions for days to weeks.
   - Captures medium-term trends.
   - Mix of technical + fundamental analysis.

3. **Position Trading**
   - Long-term approach (weeks to years).
   - Focus on fundamentals and market cycles.

4. **Scalping**
   - Very short-term (seconds to minutes).
   - Seeks small profits repeatedly.
   - High frequency + strict risk management.

---

### 📊 By Analytical Method

5. **Technical Analysis Strategies**
   - Trend Following
   - Breakout Trading
   - Reversal Trading
   - Momentum Trading

6. **Fundamental Analysis Strategies**
   - Value Investing
   - Growth Investing
   - Income Investing
   - Contrarian Investing

7. **Quantitative/Algorithmic Strategies**
   - Statistical Arbitrage
   - Mean Reversion
   - Pairs Trading
   - Machine Learning Models

---

### ⚡ By Market Condition

8. **Bull Market Strategies**
   - Buy the dips
   - Growth and momentum stocks
   - Leveraged trading

9. **Bear Market Strategies**
   - Short selling
   - Inverse ETFs
   - Hedging with options

10. **Sideways Market Strategies**
    - Range trading
    - Options strategies (iron condors, straddles)
    - Pairs/statistical arbitrage

---

### 🛠 By Tools Used

11. **Options Strategies**
    - Covered Calls, Protective Puts
    - Straddles & Strangles
    - Spread Strategies (Vertical, Butterfly, Iron Condor)

12. **Risk-Managed Approaches**
    - Stop-loss & Trailing Stops
    - Position sizing & Diversification
    - Hedging with Derivatives

---

### 🌍 Specialized Strategies

- Sector Rotation (economic cycles)
- Event-Driven Trading (earnings, mergers, news)
- Dividend Capture (buy before ex-dividend, sell after)
- Index Arbitrage (mispricing between futures and index)
- High-Frequency Trading (HFT)

### Stock Trading Strategies Comparison

| Strategy Type          | Example Strategies                            | Goal/Approach                                | Risk Level | Best Use Case |
|------------------------|-----------------------------------------------|----------------------------------------------|------------|---------------|
| **Day Trading**        | Intraday trades, news-based scalps            | Profit from small intraday price moves       | 🔴 High    | Volatile stocks, news catalysts |
| **Swing Trading**      | Trend reversals, channel trading              | Capture medium-term price swings             | 🟠 Medium  | Stocks with momentum over days/weeks |
| **Position Trading**   | Long-term holds, trend following              | Ride multi-month or year-long trends         | 🟢 Low-Med | Strong fundamentals, growth sectors |
| **Scalping**           | Ultra-fast buys/sells (seconds/minutes)       | Accumulate many small gains                  | 🔴 Very High | Highly liquid stocks/ETFs |
| **Technical Analysis** | Breakouts, reversals, momentum                | Predict price based on charts & indicators   | Varies     | Traders who rely on chart patterns |
| **Fundamental Analysis** | Value, Growth, Income, Contrarian          | Find under/over-valued companies             | 🟢 Low-Med | Long-term investing, dividend seekers |
| **Quant/Algo Trading** | Mean reversion, pairs trading, ML models      | Use math/statistics to exploit inefficiencies| 🔴 High    | Automated/quant funds, HFT |
| **Bull Market**        | Buy dips, momentum, leverage                  | Maximize upside in rising markets            | 🟠 Medium  | Expanding economy, bullish sectors |
| **Bear Market**        | Short selling, inverse ETFs, puts             | Profit/hedge during declines                 | 🔴 High    | Recessions, market crashes |
| **Sideways Market**    | Range trading, options spreads                | Profit when market is flat                   | 🟠 Medium  | Low-volatility environments |
| **Options Strategies** | Covered calls, straddles, condors             | Leverage, hedge, or generate income          | Varies     | Traders with options knowledge |
| **Risk Management**    | Stop-loss, diversification, hedging           | Protect capital, control losses              | 🟢 Low     | All market conditions |
| **Specialized**        | Sector rotation, event-driven, dividend capture | Exploit specific events or cycles           | Varies     | Earnings season, mergers, dividends |

### Swing Trading 

#### 1. Trend Following Swing Strategy

- **Approach:** Ride the medium-term trend (up or down) by entering after confirmation of direction.  
- **Indicators Used:**  
  - Moving Averages (20-day, 50-day, 200-day)  
  - ADX (Average Directional Index) for trend strength  
  - Trendlines/Channels  
- **Example:**  
  - Buy when price bounces off the 50-day moving average in an uptrend.  
  - Sell when price closes below it.  
- ✅ Pros: Simple, works in trending markets.  
- ❌ Cons: Fails in choppy/sideways markets.  

---

#### 2. Breakout Trading

- **Approach:** Enter when a stock breaks above resistance (or below support for shorts).  
- **Indicators Used:**  
  - Support & Resistance levels  
  - Bollinger Bands (to catch volatility expansion)  
  - Volume spikes for confirmation  
- **Example:**  
  - A stock consolidates between $45–$50 for 2 weeks.  
  - Buy if it breaks $50 on high volume.  
- ✅ Pros: Can catch big moves early.  
- ❌ Cons: Many false breakouts ("fakeouts").  

---

#### 3. Pullback (Retracement) Trading

- **Approach:** Enter after a short-term correction in the direction of the larger trend.  
- **Indicators Used:**  
  - Fibonacci retracement levels (38.2%, 50%, 61.8%)  
  - RSI (for short-term oversold in uptrend)  
  - EMA (9-day, 21-day for timing entries)  
- **Example:**  
  - Stock in uptrend from $100 → $120.  
  - Wait for pullback to ~$112 (38.2% retracement).  
  - Enter long when it shows reversal signals.  
- ✅ Pros: Better risk/reward than chasing breakouts.  
- ❌ Cons: Pullback may turn into full trend reversal.  

---

#### 4. Momentum Swing Trading

- **Approach:** Ride strong price moves with heavy volume.  
- **Indicators Used:**  
  - RSI (above 60 shows bullish momentum)  
  - MACD crossover  
  - High Relative Volume (RVOL > 2)  
- **Example:**  
  - Buy a stock that gaps up 5%+ on strong earnings and continues higher on above-average volume.  
- ✅ Pros: Quick profits during strong moves.  
- ❌ Cons: Requires fast reaction; moves can reverse suddenly.  

---

#### 5. Range Trading (Channel Swing Strategy)

- **Approach:** Buy at support and sell at resistance within a sideways channel.  
- **Indicators Used:**  
  - Horizontal support/resistance  
  - Stochastic oscillator or RSI for overbought/oversold levels  
  - Bollinger Bands (price bouncing within bands)  
- **Example:**  
  - Stock bounces between $60 (support) and $70 (resistance).  
  - Buy near $60, sell near $70, repeat until breakout.  
- ✅ Pros: Consistent in flat markets.  
- ❌ Cons: Losses if stock breaks out strongly.  

---

#### 6. Earnings Swing Trading

- **Approach:** Trade around earnings reports or other major news events.  
- **Indicators Used:**  
  - Implied Volatility (IV) from options market  
  - Support/resistance zones before earnings  
  - Post-earnings price gap direction  
- **Example:**  
  - Stock historically rallies after earnings.  
  - Buy 1 week before and sell after the gap up.  
- ✅ Pros: Big moves possible in short time.  
- ❌ Cons: Very high risk, unpredictable.  

---

### 📌 Swing Trading Best Practices

- Use **stop-loss orders** (below support or recent swing low).  
- Risk **1–2% of account per trade** (position sizing).  
- Confirm entries with **multiple indicators** (trend + volume + oscillator).  
- Avoid overtrading — fewer, high-quality setups beat constant trading.  


## Swing Trading Strategies Comparison

| Strategy               | Tools / Indicators                          | Entry Rules                                         | Exit Rules                               | Risk Level |
|------------------------|---------------------------------------------|----------------------------------------------------|------------------------------------------|------------|
| **Trend Following**    | 20/50/200 MA, ADX, Trendlines               | Enter when price confirms uptrend (above MA)        | Exit if price closes below trendline/MA  | 🟠 Medium  |
| **Breakout Trading**   | Support/Resistance, Bollinger Bands, Volume | Buy when price breaks above resistance on volume    | Exit if breakout fails (returns inside)  | 🔴 High    |
| **Pullback Trading**   | Fibonacci, RSI, EMA (9/21)                  | Enter after retracement to 38–61% with reversal     | Exit if retracement breaks deeper levels | 🟠 Medium  |
| **Momentum Trading**   | RSI, MACD, Relative Volume                  | Buy when RSI > 60 and volume spikes on strong move  | Exit when momentum fades / RSI < 50      | 🔴 High    |
| **Range Trading**      | Support/Resistance, Stochastic, Bollinger   | Buy near support, sell near resistance              | Exit if price breaks out of range        | 🟢 Low-Med |
| **Earnings Trading**   | IV (Implied Volatility), Support/Resistance | Buy before earnings if historical trend supports    | Exit after post-earnings move/gap        | 🔴 Very High |
