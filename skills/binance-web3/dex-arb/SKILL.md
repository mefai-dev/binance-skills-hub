---
name: dex-arb
description: |
  Scan for arbitrage opportunities across BSC decentralized exchanges. Compares token prices
  across multiple DEXs to identify price spreads and estimate potential profit. Supports
  scanning all tracked tokens or a specific token address.
metadata:
  author: mefai
  version: "1.0"
---

# DEX Arbitrage Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| DEX Arb | Cross-DEX arbitrage scanner | Find price differences for the same token across BSC DEXs |

## Use Cases

1. **Arbitrage Discovery**: Find tokens with significant price spreads across DEXs
2. **Profit Estimation**: Calculate estimated profit from cross-DEX arbitrage
3. **Market Efficiency**: Monitor how quickly prices converge across exchanges
4. **Token-Specific Analysis**: Check arbitrage opportunities for a specific token

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: DEX Arbitrage Scanner

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/dex-arb
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | No | Token contract address (optional; scans all tracked tokens if omitted) |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/dex-arb'
```

```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/dex-arb?address=0xTokenAddress'
```

**Response Example**:
```json
{
  "opportunities": [
    {
      "token": "0xTokenAddress",
      "symbol": "TOKEN",
      "cheapestDex": "BiSwap",
      "cheapestPrice": 1.2345,
      "expensiveDex": "PancakeSwap V2",
      "expensivePrice": 1.2567,
      "spreadPct": 1.8,
      "estimatedProfitUsd": 22.2
    },
    {
      "token": "0xAnotherToken",
      "symbol": "ATOKEN",
      "cheapestDex": "ApeSwap",
      "cheapestPrice": 0.00456,
      "expensiveDex": "PancakeSwap V3",
      "expensivePrice": 0.00478,
      "spreadPct": 4.82,
      "estimatedProfitUsd": 48.2
    }
  ],
  "dexesScanned": 5,
  "tokensScanned": 25,
  "timestamp": "2026-03-08T12:00:00Z"
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| opportunities | array | List of arbitrage opportunities sorted by spread |
| opportunities[].token | string | Token contract address |
| opportunities[].symbol | string | Token symbol |
| opportunities[].cheapestDex | string | DEX with the lowest price |
| opportunities[].cheapestPrice | number | Lowest price in USD |
| opportunities[].expensiveDex | string | DEX with the highest price |
| opportunities[].expensivePrice | number | Highest price in USD |
| opportunities[].spreadPct | number | Price spread as a percentage |
| opportunities[].estimatedProfitUsd | number | Estimated profit in USD (before gas) |
| dexesScanned | number | Number of DEXs compared |
| tokensScanned | number | Number of tokens scanned |
| timestamp | string | Scan timestamp in ISO 8601 format |

---

## Notes

1. Profit estimates are before gas costs; actual profit depends on gas fees and slippage
2. High spreads may indicate low liquidity on one side, making execution difficult
3. Opportunities are time-sensitive and may disappear within seconds
4. DEXs scanned include PancakeSwap V2/V3, BiSwap, ApeSwap, BabySwap, and others
5. When no address parameter is provided, scans the top 25 tracked BSC tokens
6. Price data is fetched from on-chain router quotes for accuracy
