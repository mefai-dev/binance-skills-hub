---
name: pair-analytics
description: |
  Detailed analytics for BSC trading pairs including liquidity depth, volume trends,
  price impact simulation, and trading activity metrics. Provides comprehensive pair
  data from DexScreener for informed trading decisions.
metadata:
  author: mefai
  version: "1.0"
---

# Pair Analytics Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Pair Analytics | DEX pair analysis | Get detailed liquidity, volume, and price impact data for BSC trading pairs |

## Use Cases

1. **Liquidity Assessment**: Evaluate the depth and stability of a pair's liquidity
2. **Volume Analysis**: Understand trading volume trends and buy/sell ratios
3. **Price Impact Estimation**: Estimate the price impact of trades at various sizes
4. **Pair Selection**: Compare multiple pairs for the same token to find the best execution

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Pair Analytics

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/pair-analytics
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BEP-20 token contract address |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/pair-analytics?address=0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82'
```

**Response Example**:
```json
{
  "token": "0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82",
  "symbol": "CAKE",
  "pairs": [
    {
      "pairAddress": "0xPairAddress",
      "dex": "PancakeSwap V2",
      "baseToken": "CAKE",
      "quoteToken": "WBNB",
      "price": 2.45,
      "liquidity": 8500000,
      "volume24h": 32000000,
      "buyCount24h": 15200,
      "sellCount24h": 14800,
      "priceImpact1k": 0.02,
      "priceImpact10k": 0.18,
      "priceImpact100k": 1.85
    }
  ],
  "totalLiquidity": 12500000,
  "totalVolume24h": 45000000,
  "totalPairs": 3
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| token | string | Token contract address |
| symbol | string | Token symbol |
| pairs | array | List of trading pairs |
| pairs[].pairAddress | string | Pair contract address |
| pairs[].dex | string | DEX name |
| pairs[].baseToken | string | Base token symbol |
| pairs[].quoteToken | string | Quote token symbol |
| pairs[].price | number | Current price in USD |
| pairs[].liquidity | number | Pair liquidity in USD |
| pairs[].volume24h | number | 24-hour volume in USD |
| pairs[].buyCount24h | number | Number of buy transactions in 24h |
| pairs[].sellCount24h | number | Number of sell transactions in 24h |
| pairs[].priceImpact1k | number | Estimated price impact for a $1,000 trade (%) |
| pairs[].priceImpact10k | number | Estimated price impact for a $10,000 trade (%) |
| pairs[].priceImpact100k | number | Estimated price impact for a $100,000 trade (%) |
| totalLiquidity | number | Total liquidity across all pairs |
| totalVolume24h | number | Total 24h volume across all pairs |
| totalPairs | number | Number of active trading pairs |

---

## Notes

1. Price impact values are estimates based on current liquidity depth
2. Pairs are sorted by liquidity (highest first) for easy comparison
3. Buy/sell count ratio can indicate directional market pressure
4. Low liquidity pairs may show high price impact even for small trades
5. Data is sourced from on-chain pair contracts and DexScreener
6. Price impact above 5% for your intended trade size suggests finding a more liquid pair
