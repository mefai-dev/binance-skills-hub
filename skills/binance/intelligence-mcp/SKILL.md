---
name: intelligence-mcp
description: |
  MCP server providing 8 computed intelligence tools for Binance futures markets.
  Unlike raw API wrappers, each tool combines multiple Binance public endpoints into
  derived analytics: smart accumulation detection, whale footprint scanning, market
  impact simulation, smart money radar (6-factor), candlestick pattern recognition,
  pair correlation matrix, market regime classification (ADX/ATR), and DCA backtesting.
  Install via pip, no API keys needed — all public endpoints.
metadata:
  author: mefai-dev
  version: "1.0.0"
license: MIT
---

# Binance Intelligence MCP Server

MCP (Model Context Protocol) server with 8 computed intelligence tools for Binance. Each tool combines 2-4 Binance public API endpoints into composite analytics that go beyond raw data forwarding.

## Installation

```bash
pip install binance-intelligence-mcp
```

Or run directly:
```bash
python -m binance_intelligence
```

## Architecture

```
User / LLM
    ↓  MCP (stdio)
binance-intelligence-mcp
    ↓  aiohttp (async)
Binance Public API (fapi.binance.com / api.binance.com)
```

- **Transport**: stdio (standard MCP pattern)
- **Dependencies**: `mcp>=1.0.0`, `aiohttp>=3.9.0`
- **Python**: >=3.10
- **API Keys**: None required (all public endpoints)

## 8 Intelligence Tools

### 1. `detect_accumulation` — Smart Accumulation Detector

Identifies stealth institutional buying before price moves.

| Sub-Score | Source Endpoint | Logic |
|-----------|----------------|-------|
| volume_surge | `GET /fapi/v1/klines` (4h, 30) | Current volume vs 20-period moving average |
| oi_buildup | `GET /futures/data/openInterestHist` (4h, 30) | Linear regression slope of OI, normalized by mean |
| stealth_mode | `GET /fapi/v1/premiumIndex` | Funding rate proximity to zero (quiet accumulation) |
| buyer_aggression | `GET /futures/data/takerlongshortRatio` (4h) | Taker buy ratio deviation from neutral 0.5 |

**Output**: Composite score 0-100 per symbol, signal: STRONG (≥70) / MODERATE (≥45) / WEAK

**Parameters**:
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| symbols | list[str] | No | Top 12 futures pairs | Symbols to analyze |

---

### 2. `scan_whale_trades` — Whale Footprint Scanner

Detects large trades and classifies them by size.

| Endpoint | Description |
|----------|-------------|
| `GET /fapi/v1/aggTrades` (500 per symbol) | Aggregate trade data |

**Classification**:
- DOLPHIN: $50K – $250K
- WHALE: $250K – $1M
- MEGA: > $1M

**Output**: Trade list with side (buy/sell), net pressure, biggest trade per symbol

**Parameters**:
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| symbols | list[str] | No | Top 12 | Symbols to scan |
| min_usd | float | No | 50000 | Minimum trade size in USD |

---

### 3. `simulate_market_impact` — Market Impact Simulator

Walks the order book to calculate real slippage for large orders.

| Endpoint | Description |
|----------|-------------|
| `GET /fapi/v1/depth` (1000 levels) | Full order book |

**Output**: Average fill price, worst fill price, slippage %, levels consumed, impact cost

**Parameters**:
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| symbol | str | No | BTCUSDT | Trading pair |
| side | str | No | BUY | BUY or SELL |
| amount_usd | float | No | 100000 | Order size in USD |

---

### 4. `smart_money_radar` — Smart Money Radar (6-Factor)

Composite positioning analysis from 6 independent signals.

| Factor | Source Endpoint | Metric |
|--------|----------------|--------|
| Top Position L/S | `GET /futures/data/topLongShortPositionRatio` | Position ratio trend |
| Top Account L/S | `GET /futures/data/topLongShortAccountRatio` | Account ratio trend |
| Global L/S | `GET /futures/data/globalLongShortAccountRatio` | Global sentiment |
| Taker Ratio | `GET /futures/data/takerlongshortRatio` | Buyer vs seller aggression |
| OI Trend | `GET /futures/data/openInterestHist` | Open interest momentum |
| Price Momentum | `GET /fapi/v1/klines` | Price direction alignment |

**Output**: 6 factor scores (-1 to +1), composite 0-100, signal: STRONG_BULLISH / BULLISH / NEUTRAL / BEARISH / STRONG_BEARISH

**Parameters**:
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| symbols | list[str] | No | Top 12 | Symbols to analyze |

---

### 5. `scan_candlestick_patterns` — Candlestick Pattern Scanner

Detects classic candlestick patterns with confidence scores.

| Endpoint | Description |
|----------|-------------|
| `GET /fapi/v1/klines` (50 candles) | OHLCV data |

**Detected Patterns**: Hammer, Inverted Hammer, Bullish Engulfing, Bearish Engulfing, Doji, Morning Star, Evening Star, Three White Soldiers, Three Black Crows

**Output**: Pattern name, direction (BULLISH/BEARISH), confidence 0-100

**Parameters**:
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| symbols | list[str] | No | Top 12 | Symbols to scan |
| interval | str | No | 4h | Kline interval (1h/4h) |

---

### 6. `compute_correlation_matrix` — Pair Correlation Matrix

Calculates Pearson correlation between trading pairs from close prices.

| Endpoint | Description |
|----------|-------------|
| `GET /fapi/v1/klines` (4h, 90 candles per symbol) | Close price series |

**Output**: NxN correlation matrix (-1 to +1), strongest/weakest pairs

**Parameters**:
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| symbols | list[str] | No | Top 12 | 4-12 symbols |
| interval | str | No | 4h | Kline interval |
| limit | int | No | 90 | Number of candles |

---

### 7. `classify_market_regime` — Market Regime Classifier

Classifies regime using ADX, ATR, and volume analysis.

| Endpoint | Description |
|----------|-------------|
| `GET /fapi/v1/klines` (1h, 168) | 7 days of hourly data |
| `GET /fapi/v1/premiumIndex` | Funding rate context |

**Regimes**:
| Regime | Condition |
|--------|-----------|
| TRENDING | ADX ≥ 25 |
| VOLATILE_BREAKOUT | ADX ≥ 40 and ATR% ≥ 1.5% |
| LOW_ACTIVITY | ATR% < 0.5% and volume change < 20% |
| RANGING | Default (low ADX, moderate ATR) |

**Output**: Regime label, direction (UP/DOWN for trending), ADX, ATR, volume change %, funding rate

**Parameters**:
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| symbols | list[str] | No | Top 12 | Symbols to classify |

---

### 8. `backtest_dca` — DCA Backtester

Compares Dollar-Cost Averaging vs lump-sum investment over historical data.

| Endpoint | Description |
|----------|-------------|
| `GET /fapi/v1/klines` (1d, up to 365) | Daily close prices |

**Output**: Total invested, DCA final value, DCA ROI%, lump-sum value, lump-sum ROI%, number of buys, average cost

**Parameters**:
| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| symbol | str | No | BTCUSDT | Trading pair |
| amount | float | No | 100 | USD per interval |
| interval_days | int | No | 7 | Days between buys |
| total_days | int | No | 365 | Backtest period |

---

## Binance API Endpoints Used

All endpoints are **public** and require **no authentication**.

| Endpoint | Type | Used By |
|----------|------|---------|
| `GET /fapi/v1/klines` | Futures | accumulation, patterns, correlation, regime, smart_money, dca |
| `GET /fapi/v1/aggTrades` | Futures | whale |
| `GET /fapi/v1/depth` | Futures | impact |
| `GET /fapi/v1/premiumIndex` | Futures | accumulation, regime |
| `GET /futures/data/openInterestHist` | Futures | accumulation, smart_money |
| `GET /futures/data/topLongShortPositionRatio` | Futures | smart_money |
| `GET /futures/data/topLongShortAccountRatio` | Futures | smart_money |
| `GET /futures/data/globalLongShortAccountRatio` | Futures | smart_money |
| `GET /futures/data/takerlongshortRatio` | Futures | accumulation, smart_money |

## How It Works

```
┌─────────────────────────────────────────────────┐
│              MCP Server (stdio)                  │
│  8 @mcp.tool() functions                         │
│                                                   │
│  detect_accumulation  ─┐                          │
│  scan_whale_trades    ─┤                          │
│  simulate_market_impact┤   BinanceClient          │
│  smart_money_radar    ─┤   (async, aiohttp)       │
│  scan_candlestick     ─┤   - rate limiting        │
│  compute_correlation  ─┤   - connection pooling    │
│  classify_market_regime┤   - no API keys           │
│  backtest_dca         ─┘                          │
│                                                   │
│  Each tool: validate → fetch 2-4 endpoints →      │
│  compute derived scores → return structured JSON   │
└─────────────────────────────────────────────────┘
```

## Example Usage

### MCP Configuration

```json
{
  "mcpServers": {
    "binance-intelligence": {
      "command": "binance-intelligence-mcp"
    }
  }
}
```

### Example: Accumulation Detection

**Request** (via MCP tool call):
```json
{
  "name": "detect_accumulation",
  "arguments": {
    "symbols": ["BTCUSDT", "ETHUSDT", "SOLUSDT"]
  }
}
```

**Response**:
```json
{
  "tool": "detect_accumulation",
  "count": 3,
  "results": [
    {
      "symbol": "SOLUSDT",
      "scores": {
        "volume_surge": 72.5,
        "oi_buildup": 68.3,
        "stealth_mode": 85.1,
        "buyer_aggression": 61.2
      },
      "composite": 71.4,
      "signal": "STRONG"
    },
    {
      "symbol": "BTCUSDT",
      "scores": {
        "volume_surge": 45.2,
        "oi_buildup": 52.1,
        "stealth_mode": 78.4,
        "buyer_aggression": 48.7
      },
      "composite": 55.8,
      "signal": "MODERATE"
    }
  ]
}
```

### Example: Market Regime Classification

**Request**:
```json
{
  "name": "classify_market_regime",
  "arguments": {
    "symbols": ["BTCUSDT", "ETHUSDT"]
  }
}
```

**Response**:
```json
{
  "tool": "classify_market_regime",
  "count": 2,
  "regime_summary": {
    "TRENDING": ["BTCUSDT"],
    "RANGING": ["ETHUSDT"]
  },
  "results": [
    {
      "symbol": "BTCUSDT",
      "regime": "TRENDING",
      "direction": "UP",
      "indicators": {
        "adx": 35.72,
        "plus_di": 28.41,
        "minus_di": 12.53,
        "atr": 1250.45,
        "atr_pct": 1.823,
        "volume_change_pct": 15.34,
        "funding_rate": 0.000125
      }
    }
  ]
}
```

## Testing

```bash
# Clone and install
git clone https://github.com/mefai-dev/binance-intelligence-mcp.git
cd binance-intelligence-mcp
pip install -e ".[dev]"

# Run 142 tests (all mock-based, no API keys needed)
pytest tests/ -v
```

## Key Differentiators

| Feature | Raw API Wrappers | This Package |
|---------|-----------------|--------------|
| Data | Pass-through | Computed intelligence |
| Endpoints per tool | 1 | 2-6 combined |
| Output | Raw JSON | Scores, signals, classifications |
| Algorithms | None | ADX, ATR, Pearson, linear regression, pattern detection |
| API Keys | Often required | Never needed |
| Rate Limiting | Manual | Built-in semaphore |

## Source Code

Full source: [github.com/mefai-dev/binance-intelligence-mcp](https://github.com/mefai-dev/binance-intelligence-mcp)

## License

MIT
