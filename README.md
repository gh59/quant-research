# 📊 QuantResearch

> A systematic quantitative strategy research project covering A-share factor mining, crypto timing strategies, and machine learning signals. Continuously updated.

🌐 [中文版](./README_CN.md)

---

## 🏆 Current Best Strategy（BTC/USDT Daily）

**Strategy: MACD(16,35,9) + Fear & Greed Filter(<80) + 3% Stop Loss**

| Metric | Value |
|--------|-------|
| 📅 Period | 2023.01 — 2026.03 |
| 💰 Cumulative Return (after fees) | **+384%** |
| 📈 Benchmark (Buy & Hold) | +127% |
| ⚡ Sharpe Ratio | **1.70** |
| 🛡️ Max Drawdown | **-16%** |
| 🔁 Avg Trades / Year | 21 |

---

## 🗺️ Strategy Evolution Map

```mermaid
graph TD
    A["📊 Quant Strategy Overview"] --> B["🇨🇳 A-Share Strategy"]
    A --> C["₿ Crypto Strategy"]
    A --> D["🤖 ML Strategy"]

    B --> B1["Data\nakshare CSI300"]
    B1 --> B2["3-Factor IC Test"]
    B2 --> B2a["Momentum\nIC=-0.0023 ❌"]
    B2 --> B2b["Reversal\nIC=0.0062 ❌"]
    B2 --> B2c["Turnover\nIC=0.0290 weak"]
    B2 --> B2d["Multi-Factor\nIC=0.0328 ICIR=0.22 weak"]
    B2d --> B3["Backtest v1\nSharpe0.24 ❌"]
    B3 --> B4["Add Risk Control\nMA200 + 3% Stop Loss"]
    B4 --> B4a["Return+16.75% Sharpe0.43\nBeats CSI300 ✅"]
    B4a --> B5["Return Attribution"]
    B5 --> B5a["Timing +65% ✅\nStock Selection ≈0 ❌"]
    B4a --> B6["Position Sizing\n5%→100%"]
    B6 --> B6a["100% Position Sharpe1.03\nAlpha from Timing ✅"]

    C --> C1["Data\nOKX/Binance/CoinMetrics"]
    C1 --> C2["MACD Timing\nDefault 12,26,9"]
    C2 --> C2a["Return+135% Sharpe1.00\nDD-32%"]
    C2 --> C3["Parameter Optimization"]
    C3 --> C3a["Best 16,35,9\nReturn+339% Sharpe1.61\nDD-21% ✅"]
    C3a --> C4["Walk-Forward\nAvg Val Sharpe0.62 ⚠️"]
    C3a --> C5["Add Signal Filters"]
    C5 --> C5a["Funding Rate\n+310% Sharpe1.17"]
    C5 --> C5b["Fear & Greed <80\n+423% Sharpe1.70 ✅"]
    C5 --> C5c["Volume Factor\nInvalid ❌"]
    C5 --> C5d["On-chain Addr\nIC=-0.01 ❌"]
    C5b --> C6["Add 3% Stop Loss\n+443% Sharpe1.70 DD-15% ✅"]
    C6 --> C6a["🏆 After Fees\n+384% 21 trades/yr\nFinal Best Strategy"]
    C3a --> C7["Hourly MACD\n150,400,20"]
    C7 --> C7a["Before fees+682%\nAfter fees+439% ≈"]
    C3a --> C8["Advanced Research"]
    C8 --> C8a["Multi-Timeframe\n+199% ❌ Over-filtered"]
    C8 --> C8b["Short Selling\n+216% ❌ BTC upward bias"]
    C8 --> C8c["Position Sizing\n+153~215% ❌\nFull position better for BTC"]

    D --> D1["Daily XGBoost\n1183 samples"]
    D1 --> D1a["Price Direction\n51% acc ❌"]
    D1 --> D1b["Volatility Direction\n45% acc ❌ too few samples"]
    D1 --> D2["Hourly XGBoost\n28217 samples"]
    D2 --> D2a["Price Direction\n47% acc ❌"]
    D2 --> D2b["Volatility Level\n67% acc ✅ clustering effect"]
    D2b --> D3["ML Risk Strategy\n+200% Sharpe1.10 ✅"]
    D3 --> D4["MACD+ML\nBefore fees +178% Sharpe1.34"]
    D4 --> D4a["After fees +48%\n158 trades/yr fees 45% ❌"]
    D4a --> D5["Conclusion: ML suits daily vol prediction\nnot hourly timing ⚠️"]
```

---

## 📁 Project Structure

```
QuantResearch/
├── 01_data/                    # 🔧 Data Engineering
│   └── 01_data_cleaning.ipynb  # Data acquisition, adjustment, quality check
├── 02_factors/                 # 🔬 Factor Research
│   ├── 02_factor_ictest.ipynb  # Cross-sectional IC testing
│   └── 04_ml_factor.ipynb      # Machine learning factors
├── 03_backtest/                # 📈 Strategy Backtesting
│   ├── 03_backtest.ipynb       # Backtest framework, risk control, attribution
│   └── 03_roe_factor.ipynb     # Fundamental factor research
└── 04_crypto/                  # ₿ Crypto Strategies
    ├── 01_crypto_data.ipynb    # Multi-exchange data ingestion
    └── 02_live_trading.ipynb   # Live execution framework
```

---

## 🇨🇳 Research Track 1: A-Share Factor Research

### Data Engineering

- Source: akshare, CSI300 constituents, forward-adjusted daily OHLCV
- Cleaning: dividend/split adjustment, suspension filling, MAD winsorization, survivorship-bias-free universe

### Factor IC Test Results

| Factor | IC Mean | ICIR | IC>0 Rate | Verdict |
|--------|---------|------|-----------|---------|
| Momentum (20D) | -0.0023 | -0.011 | 50.6% | ❌ Invalid |
| Reversal (5D) | 0.0062 | 0.032 | 49.6% | ❌ Invalid |
| Turnover Rate | 0.0290 | 0.109 | 55.1% | ⚠️ Weak |
| 3-Factor Composite | 0.0328 | 0.224 | 58.3% | ⚠️ Weak |

> Passing bar: IC Mean > 0.05, ICIR > 0.5, IC>0 > 55%

### Backtest & Return Attribution

| Strategy | Cumulative Return | Sharpe | Max DD |
|----------|------------------|--------|--------|
| 3-Factor Composite (base) | +8.56% | 0.24 | -22.73% |
| + MA200 Timing + Stop Loss | +16.75% | 0.43 | -15.06% |
| CSI300 Benchmark | +10.17% | — | — |

**💡 Attribution Findings**
- Pure Timing (MA200, full position): +65%, Sharpe 1.15 ✅
- Pure Stock Selection (3-factor, no risk control): +8.56%, Sharpe 0.24 ❌
- **All alpha originates from the timing module; factor selection contributes zero**

---

## ₿ Research Track 2: Crypto Timing Strategy

### Data Infrastructure

| Data Type | Source | Coverage |
|-----------|--------|---------|
| BTC/USDT Daily | OKX API | 2023.01–now, 1,183 bars |
| BTC/USDT Hourly | OKX API | 2023.01–now, 28,385 bars |
| Funding Rate | Binance API | 2019.09–now, 7,174 records |
| Fear & Greed Index | Alternative.me | 2020.10–now, 2,000 records |
| On-chain Active Addresses | CoinMetrics | 2020.10–now |

### Strategy Iteration

| Strategy Version | Cum. Return | Sharpe | Max DD |
|-----------------|-------------|--------|--------|
| MACD Default (12,26,9) | +135% | 1.00 | -32% |
| MACD Optimized (16,35,9) | +339% | 1.61 | -21% |
| + Funding Rate Filter | +310% | 1.17 | -49% |
| + Fear & Greed Filter (<80) | +423% | 1.70 | -15% |
| + 3% Stop Loss | +443% | 1.70 | -15% |
| **🏆 After Transaction Costs** | **+384%** | **1.70** | **-16%** |

### Walk-Forward Validation

| Fold | Best Params | Train Sharpe | Val Sharpe |
|------|------------|-------------|-----------|
| 1 | (16,35,9) | 2.99 | -0.65 |
| 2 | (16,35,12) | 2.90 | -0.18 |
| 3 | (20,26,12) | 1.73 | 3.22 |
| 4 | (8,21,7) | 0.42 | 0.09 |
| **Average** | — | — | **0.62** |

### Factor Validity Summary

| Factor | Test Result | Included |
|--------|-------------|---------|
| MACD Trend | Sharpe 1.61, core signal | ✅ Yes |
| Fear & Greed | Composite Sharpe 1.70 | ✅ Filter |
| Funding Rate | Standalone Sharpe 1.17 | Candidate |
| Volume Factor | Invalid, over-filters in combo | ❌ No |
| On-chain Addresses | IC=-0.01, ETF-cycle failure | ❌ No |

### Advanced Explorations

| Direction | Result | Root Cause |
|-----------|--------|-----------|
| Multi-timeframe Confluence | +199% ❌ | Holding time drops to 28%, misses rallies |
| Short Selling | +216% ❌ | BTC long-term upward bias |
| Position Sizing | +153–215% ❌ | Strong trend asset; full position dominates |
| Hourly MACD (after fees) | +439% ≈ | 102 trades/yr; fee drag significant |

---

## 🤖 Research Track 3: Machine Learning Signals

### Experiment Results

| Target | Frequency | Samples | Accuracy | Verdict |
|--------|-----------|---------|----------|---------|
| 5D Price Direction | Daily | 1,183 | 51% | ❌ Near-random |
| 10D Price Direction | Daily | 1,183 | 45% | ❌ Below random |
| 24H Price Direction | Hourly | 28,217 | 47% | ❌ Near-random |
| **24H Volatility Level** | **Hourly** | **28,217** | **67%** | **✅ Effective** |

### Strategy Performance

| Strategy | Cum. Return | Sharpe | Note |
|----------|-------------|--------|------|
| ML Risk Prediction | +200% | 1.10 | Beats buy & hold ✅ |
| MACD + ML (pre-fee) | +178% | 1.34 | — |
| MACD + ML (post-fee) | +48% | — | 45% eaten by fees ❌ |

**💡 Conclusion: ML volatility prediction adds value at daily frequency; hourly frequency is not deployable due to transaction costs**

---

## 📌 Key Research Findings

| # | Finding |
|---|---------|
| 1 | 🎯 **Simple strategies are more robust**: MACD + sentiment filter + stop loss beats all complex attempts |
| 2 | 🔍 **Return attribution is essential**: 100% of A-share alpha comes from timing; stock selection contributes zero |
| 3 | 💸 **Transaction costs set the minimum viable frequency**: 21 trades/yr (daily) is negligible; 100+/yr (hourly) eliminates alpha |
| 4 | ⚠️ **Walk-Forward is mandatory against overfitting**: In-sample Sharpe peaks at 2.99; out-of-sample averages 0.62 |
| 5 | 📐 **Asset characteristics determine strategy shape**: BTC suits full-position trend following; A-shares suit cross-sectional factor selection |

---

## 🛠️ Tech Stack

```
Python 3.10+
pandas / numpy          Time series & data processing
akshare                 A-share market data
ccxt                    Multi-exchange crypto API
requests                On-chain & sentiment data APIs
xgboost / scikit-learn  ML modeling & evaluation
matplotlib              Visualization
```

---

## 🗂️ Data Files

| File | Description |
|------|-------------|
| `03_backtest/close_data.csv` | CSI300 daily close prices |
| `03_backtest/volume_data.csv` | CSI300 turnover rates |
| `04_crypto/btc_daily.csv` | BTC/USDT daily OHLCV |
| `04_crypto/btc_1h.csv` | BTC/USDT hourly OHLCV |
| `04_crypto/btc_funding_full.csv` | BTC perpetual funding rates |
| `04_crypto/btc_fng.csv` | Crypto Fear & Greed Index |
| `04_crypto/crypto_close.csv` | Top 10 crypto close prices |

---

## 🔭 Roadmap

- [ ] Multi-coin cross-sectional factor selection (20+ coins)
- [ ] Dynamic position sizing via daily volatility prediction
- [ ] Live execution module (OKX API)
- [ ] A-share fundamental factors (ROE/TTM): pending stable data source
- [ ] Cross-asset correlation factor research

---

*🔄 Research in progress — Last updated: 2026.03*
