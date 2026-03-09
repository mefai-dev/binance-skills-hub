---
name: liquidity-pulse
description: |
  Monitor liquidity health across major BSC trading pairs. Analyzes liquidity depth, stability,
  concentration, and recent changes for the most active DEX pools. Provides a liquidity health
  score and alerts for significant liquidity events.
metadata:
  author: mefai
  version: "1.0"
---

# Liquidity Pulse Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Liquidity Pulse | Liquidity health monitor | Analyze liquidity depth and stability across major BSC DEX pairs |

## Use Cases

1. **Liquidity Monitoring**: Track liquidity health across the BSC DEX ecosystem
2. **Rug Pull Detection**: Get alerts when significant liquidity is removed from pools
3. **Trading Conditions**: Assess liquidity depth before executing large trades
4. **Pool Health**: Monitor liquidity stability and concentration risks

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Liquidity Pulse

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/liquidity-pulse
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns current liquidity analysis |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/liquidity-pulse'
```

**Response Example**:
```json
{
  "overallHealth": 82,
  "totalLiquidityUsd": 850000000,
  "liquidityChange24h": 1.5,
  "pairs": [
    {
      "pair": "CAKE/WBNB",
      "dex": "PancakeSwap V2",
      "liquidity": 8500000,
      "change1h": 0.2,
      "change24h": 3.5,
      "healthScore": 92,
      "concentration": "LOW",
      "topLpPct": 15.2
    },
    {
      "pair": "USDT/WBNB",
      "dex": "PancakeSwap V2",
      "liquidity": 35000000,
      "change1h": -0.1,
      "change24h": -0.5,
      "healthScore": 95,
      "concentration": "LOW",
      "topLpPct": 8.5
    }
  ],
  "alerts": [
    {
      "type": "LIQUIDITY_DROP",
      "pair": "TOKEN/WBNB",
      "dex": "PancakeSwap V2",
      "changePct": -35.2,
      "timeframe": "1h",
      "severity": "high"
    }
  ],
  "timestamp": "2026-03-08T12:00:00Z"
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| overallHealth | number | Overall BSC liquidity health score (0-100) |
| totalLiquidityUsd | number | Total tracked liquidity in USD |
| liquidityChange24h | number | 24-hour change in total liquidity percentage |
| pairs | array | List of monitored pairs with liquidity data |
| pairs[].pair | string | Trading pair name |
| pairs[].dex | string | DEX name |
| pairs[].liquidity | number | Pool liquidity in USD |
| pairs[].change1h | number | 1-hour liquidity change percentage |
| pairs[].change24h | number | 24-hour liquidity change percentage |
| pairs[].healthScore | number | Individual pair health score (0-100) |
| pairs[].concentration | string | LP concentration risk: LOW, MEDIUM, HIGH |
| pairs[].topLpPct | number | Percentage of liquidity held by the largest LP |
| alerts | array | Significant liquidity events |
| alerts[].type | string | Alert type (LIQUIDITY_DROP, LIQUIDITY_SURGE, etc.) |
| alerts[].pair | string | Affected pair name |
| alerts[].dex | string | DEX where the event occurred |
| alerts[].changePct | number | Liquidity change percentage |
| alerts[].timeframe | string | Timeframe of the change |
| alerts[].severity | string | Alert severity: low, medium, high, critical |
| timestamp | string | Analysis timestamp in ISO 8601 format |

---

## Notes

1. Health score factors: liquidity depth, stability, concentration, and volume-to-liquidity ratio
2. Concentration risk is HIGH when a single LP controls >50% of pool liquidity
3. LIQUIDITY_DROP alerts with >30% change in 1 hour may indicate potential rug pulls
4. Healthy pools maintain stable liquidity with low concentration and consistent volume
5. Stablecoin pairs typically show the highest health scores due to low volatility
6. Alerts are generated in real-time and should be investigated promptly
