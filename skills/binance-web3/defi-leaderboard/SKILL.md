---
name: defi-leaderboard
description: |
  Rank the top DeFi protocols on BSC by Total Value Locked (TVL), trading volume, and
  user activity. Provides a leaderboard view of the BSC DeFi ecosystem with protocol-level
  metrics for ecosystem analysis.
metadata:
  author: mefai
  version: "1.0"
---

# DeFi Leaderboard Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| DeFi Leaderboard | DeFi protocol ranking | Rank BSC DeFi protocols by TVL, volume, and activity |

## Use Cases

1. **Ecosystem Overview**: Get a snapshot of the BSC DeFi landscape
2. **Protocol Comparison**: Compare DeFi protocols by TVL and volume
3. **Trend Tracking**: Monitor which protocols are gaining or losing TVL
4. **Investment Research**: Identify the largest and most active BSC DeFi platforms

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: DeFi Leaderboard

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/defi-leaderboard
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns current DeFi rankings |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/defi-leaderboard'
```

**Response Example**:
```json
{
  "protocols": [
    {
      "rank": 1,
      "name": "PancakeSwap",
      "category": "DEX",
      "tvl": 2500000000,
      "tvlChange24h": 2.3,
      "volume24h": 450000000,
      "users24h": 125000,
      "dominancePct": 45.2
    },
    {
      "rank": 2,
      "name": "Venus",
      "category": "Lending",
      "tvl": 1200000000,
      "tvlChange24h": -0.5,
      "volume24h": 85000000,
      "users24h": 15000,
      "dominancePct": 21.7
    },
    {
      "rank": 3,
      "name": "Alpaca Finance",
      "category": "Yield",
      "tvl": 450000000,
      "tvlChange24h": 1.2,
      "volume24h": 32000000,
      "users24h": 8500,
      "dominancePct": 8.1
    }
  ],
  "totalBscTvl": 5530000000,
  "totalProtocols": 15,
  "timestamp": "2026-03-08T12:00:00Z"
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| protocols | array | Ranked list of DeFi protocols |
| protocols[].rank | number | Leaderboard position |
| protocols[].name | string | Protocol name |
| protocols[].category | string | Protocol category (DEX, Lending, Yield, Bridge, etc.) |
| protocols[].tvl | number | Total Value Locked in USD |
| protocols[].tvlChange24h | number | 24-hour TVL change percentage |
| protocols[].volume24h | number | 24-hour trading/lending volume in USD |
| protocols[].users24h | number | Unique users in 24 hours |
| protocols[].dominancePct | number | Percentage of total BSC DeFi TVL |
| totalBscTvl | number | Total BSC DeFi ecosystem TVL in USD |
| totalProtocols | number | Number of protocols tracked |
| timestamp | string | Data timestamp in ISO 8601 format |

---

## Notes

1. TVL includes all assets deposited in the protocol's smart contracts
2. Protocols are ranked by TVL descending as the primary metric
3. Categories include DEX, Lending, Yield, Bridge, Derivatives, and others
4. Dominance percentage shows each protocol's share of total BSC DeFi TVL
5. Negative TVL change may indicate withdrawals or asset price depreciation
6. User counts are based on unique wallet addresses interacting with the protocol
