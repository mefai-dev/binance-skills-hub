---
name: portfolio-heatmap
description: |
  Generate a portfolio heatmap for any BSC wallet showing holdings weighted by value,
  24-hour performance per asset, concentration risk assessment, and an overall portfolio
  health score. Visualizes portfolio composition and risk distribution.
metadata:
  author: mefai
  version: "1.0"
---

# Portfolio Heatmap Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Portfolio Heatmap | Portfolio analysis | Generate a weighted portfolio heatmap with performance, risk, and health metrics |

## Use Cases

1. **Portfolio Visualization**: See holdings weighted by value with 24h performance
2. **Risk Assessment**: Identify concentration risk from over-allocation to single assets
3. **Health Monitoring**: Get an overall portfolio health score based on diversification and performance
4. **Rebalancing Signals**: Identify assets that dominate the portfolio and may need rebalancing

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Portfolio Heatmap

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/portfolio-heatmap
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BSC wallet address to analyze |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/portfolio-heatmap?address=0xWalletAddress'
```

**Response Example**:
```json
{
  "address": "0xWalletAddress",
  "totalValueUsd": 15420.50,
  "change24hPct": 2.3,
  "holdings": [
    {
      "symbol": "BNB",
      "balance": 12.5,
      "valueUsd": 7375.00,
      "change24h": 3.5,
      "pctOfPortfolio": 47.8
    },
    {
      "symbol": "USDT",
      "balance": 5000,
      "valueUsd": 5000.00,
      "change24h": 0.01,
      "pctOfPortfolio": 32.4
    },
    {
      "symbol": "CAKE",
      "balance": 1245.5,
      "valueUsd": 3045.50,
      "change24h": 5.2,
      "pctOfPortfolio": 19.8
    }
  ],
  "healthScore": 72,
  "concentrationRisk": "MEDIUM",
  "riskFactors": [
    "BNB accounts for 47.8% of portfolio (>40% threshold)",
    "Only 3 assets held (low diversification)"
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Analyzed wallet address |
| totalValueUsd | number | Total portfolio value in USD |
| change24hPct | number | Overall portfolio 24-hour change percentage |
| holdings | array | List of holdings sorted by value |
| holdings[].symbol | string | Token symbol |
| holdings[].balance | number | Token balance |
| holdings[].valueUsd | number | Holding value in USD |
| holdings[].change24h | number | 24-hour price change percentage |
| holdings[].pctOfPortfolio | number | Percentage of total portfolio value |
| healthScore | number | Portfolio health score from 0 (poor) to 100 (excellent) |
| concentrationRisk | string | Concentration risk level: LOW, MEDIUM, HIGH, CRITICAL |
| riskFactors | array | List of identified portfolio risk factors |

---

## Notes

1. Health score factors: diversification, concentration, volatility exposure, stablecoin ratio
2. Concentration risk thresholds: LOW (<30% in one asset), MEDIUM (30-50%), HIGH (50-70%), CRITICAL (>70%)
3. Holdings are sorted by value descending (largest position first)
4. 24-hour change is calculated using weighted average of individual asset changes
5. Portfolios heavily weighted toward stablecoins score lower on growth potential but higher on stability
6. A minimum of 4-5 different assets is generally recommended for healthy diversification
