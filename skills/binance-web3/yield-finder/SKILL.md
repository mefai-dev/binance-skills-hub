---
name: yield-finder
description: |
  Discover yield opportunities on BSC by analyzing trading fee APY across major DEX liquidity
  pools. Scans PancakeSwap and other BSC DEXs to find the most profitable pools based on
  real trading volume and liquidity depth.
metadata:
  author: mefai
  version: "1.0"
---

# Yield Finder Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Yield Finder | Yield opportunity scanner | Find the best yield opportunities across BSC DEX liquidity pools |

## Use Cases

1. **Yield Optimization**: Find the highest-yielding liquidity pools on BSC
2. **Pool Discovery**: Discover new pools with attractive fee revenue
3. **APY Comparison**: Compare yields across different DEXs and pool types
4. **Liquidity Provisioning**: Make data-driven decisions on where to provide liquidity

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Yield Finder

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/yield-finder
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns current yield opportunities |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/yield-finder'
```

**Response Example**:
```json
{
  "opportunities": [
    {
      "pool": "CAKE/WBNB",
      "dex": "PancakeSwap V3",
      "pairAddress": "0xPairAddress1",
      "token0": "CAKE",
      "token1": "WBNB",
      "liquidity": 8500000,
      "volume24h": 32000000,
      "feeRate": 0.25,
      "estimatedApyPct": 34.5,
      "feeRevenue24h": 80000
    },
    {
      "pool": "USDT/WBNB",
      "dex": "PancakeSwap V2",
      "pairAddress": "0xPairAddress2",
      "token0": "USDT",
      "token1": "WBNB",
      "liquidity": 35000000,
      "volume24h": 85000000,
      "feeRate": 0.25,
      "estimatedApyPct": 22.2,
      "feeRevenue24h": 212500
    },
    {
      "pool": "USDC/USDT",
      "dex": "PancakeSwap V3",
      "pairAddress": "0xPairAddress3",
      "token0": "USDC",
      "token1": "USDT",
      "liquidity": 15000000,
      "volume24h": 45000000,
      "feeRate": 0.01,
      "estimatedApyPct": 10.9,
      "feeRevenue24h": 4500
    }
  ],
  "totalPoolsScanned": 50,
  "timestamp": "2026-03-08T12:00:00Z"
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| opportunities | array | List of yield opportunities sorted by estimated APY |
| opportunities[].pool | string | Pool name (token pair) |
| opportunities[].dex | string | DEX name and version |
| opportunities[].pairAddress | string | Pool/pair contract address |
| opportunities[].token0 | string | First token symbol |
| opportunities[].token1 | string | Second token symbol |
| opportunities[].liquidity | number | Total pool liquidity in USD |
| opportunities[].volume24h | number | 24-hour trading volume in USD |
| opportunities[].feeRate | number | Fee rate as a percentage |
| opportunities[].estimatedApyPct | number | Estimated annual percentage yield from trading fees |
| opportunities[].feeRevenue24h | number | 24-hour fee revenue in USD |
| totalPoolsScanned | number | Total number of pools analyzed |
| timestamp | string | Scan timestamp in ISO 8601 format |

---

## Notes

1. APY is estimated from 24-hour trading fee revenue annualized; actual returns will vary
2. Higher volume relative to liquidity produces higher fee APY
3. Stablecoin pairs typically have lower fee rates (0.01-0.05%) but steadier returns
4. Impermanent loss risk is not factored into the APY estimate
5. Very high APY (>100%) on new pools may be unsustainable as more liquidity enters
6. Pools are ranked by estimated APY descending; consider liquidity depth for risk assessment
