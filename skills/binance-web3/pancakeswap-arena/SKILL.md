---
name: pancakeswap-arena
description: |
  Live leaderboard of top-performing PancakeSwap trading pairs on BSC. Ranks pairs by
  volume, liquidity, and price changes to surface trending tokens and active markets.
  Provides a snapshot of PancakeSwap activity across V2 and V3 pools.
metadata:
  author: mefai
  version: "1.0"
---

# PancakeSwap Arena Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| PancakeSwap Arena | PancakeSwap leaderboard | Rank top PancakeSwap pairs by volume, liquidity, and price performance |

## Use Cases

1. **Trend Discovery**: Find the most active and trending tokens on PancakeSwap
2. **Volume Leaders**: Identify which pairs are generating the most trading volume
3. **Liquidity Rankings**: Find the deepest liquidity pools for optimal trade execution
4. **Market Momentum**: Spot tokens with the strongest price movements on PancakeSwap

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: PancakeSwap Arena

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/pancakeswap-arena
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns current PancakeSwap leaderboard |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/pancakeswap-arena'
```

**Response Example**:
```json
{
  "pairs": [
    {
      "rank": 1,
      "pairAddress": "0xPairAddress1",
      "baseToken": "CAKE",
      "quoteToken": "WBNB",
      "version": "V2",
      "price": 2.45,
      "change1h": 1.2,
      "change24h": 5.8,
      "volume24h": 32000000,
      "liquidity": 8500000,
      "txCount24h": 30000
    },
    {
      "rank": 2,
      "pairAddress": "0xPairAddress2",
      "baseToken": "USDT",
      "quoteToken": "WBNB",
      "version": "V3",
      "price": 1.0,
      "change1h": 0.01,
      "change24h": 0.02,
      "volume24h": 85000000,
      "liquidity": 35000000,
      "txCount24h": 45000
    }
  ],
  "totalPairsTracked": 500,
  "pcsVolume24h": 450000000,
  "timestamp": "2026-03-08T12:00:00Z"
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| pairs | array | Ranked list of top PancakeSwap pairs |
| pairs[].rank | number | Leaderboard position |
| pairs[].pairAddress | string | Pair contract address |
| pairs[].baseToken | string | Base token symbol |
| pairs[].quoteToken | string | Quote token symbol |
| pairs[].version | string | PancakeSwap version (V2 or V3) |
| pairs[].price | number | Current base token price in USD |
| pairs[].change1h | number | 1-hour price change percentage |
| pairs[].change24h | number | 24-hour price change percentage |
| pairs[].volume24h | number | 24-hour trading volume in USD |
| pairs[].liquidity | number | Pool liquidity in USD |
| pairs[].txCount24h | number | Number of transactions in 24h |
| totalPairsTracked | number | Total number of PancakeSwap pairs tracked |
| pcsVolume24h | number | Total PancakeSwap 24h volume in USD |
| timestamp | string | Data timestamp in ISO 8601 format |

---

## Notes

1. Rankings include both PancakeSwap V2 and V3 pools
2. Pairs are ranked by a composite score of volume, liquidity, and price momentum
3. Top pairs are refreshed in near real-time from on-chain and DexScreener data
4. High volume with decreasing liquidity may indicate liquidity withdrawal risk
5. New pairs with sudden volume spikes should be investigated for bot activity
6. This skill focuses exclusively on PancakeSwap, the largest BSC DEX
