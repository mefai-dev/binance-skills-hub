---
name: token-battle
description: |
  Compare up to 4 BSC tokens side by side across key metrics: price, 24h change, volume,
  liquidity, pair count, and burned supply percentage. Provides a structured comparison
  table for investment decision-making.
metadata:
  author: mefai
  version: "1.0"
---

# Token Battle Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Token Battle | Multi-token comparison | Compare up to 4 BSC tokens across price, volume, liquidity, and other key metrics |

## Use Cases

1. **Investment Comparison**: Compare competing tokens before making an investment decision
2. **Sector Analysis**: Compare tokens within the same sector (e.g., DEX tokens, meme coins)
3. **Performance Benchmarking**: See which token is outperforming on volume, liquidity, or price change
4. **Burn Analysis**: Compare token burn percentages to assess deflationary pressure

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Token Battle

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/token-battle
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| tokens | string | Yes | Comma-separated list of 2-4 token contract addresses |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/token-battle?tokens=0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82,0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d'
```

**Response Example**:
```json
{
  "tokens": [
    {
      "address": "0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82",
      "symbol": "CAKE",
      "name": "PancakeSwap Token",
      "price": 2.45,
      "change24h": 3.2,
      "volume24h": 45000000,
      "liquidity": 12500000,
      "pairCount": 85,
      "burnedPct": 78.5
    },
    {
      "address": "0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d",
      "symbol": "USDC",
      "name": "USD Coin",
      "price": 1.0,
      "change24h": 0.01,
      "volume24h": 85000000,
      "liquidity": 35000000,
      "pairCount": 120,
      "burnedPct": 0
    }
  ],
  "comparedAt": "2026-03-08T12:00:00Z"
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| tokens | array | List of compared token data |
| tokens[].address | string | Token contract address |
| tokens[].symbol | string | Token symbol |
| tokens[].name | string | Token full name |
| tokens[].price | number | Current price in USD |
| tokens[].change24h | number | 24-hour price change percentage |
| tokens[].volume24h | number | 24-hour trading volume in USD |
| tokens[].liquidity | number | Total DEX liquidity in USD |
| tokens[].pairCount | number | Number of active trading pairs |
| tokens[].burnedPct | number | Percentage of total supply that has been burned |
| comparedAt | string | Comparison timestamp in ISO 8601 format |

---

## Notes

1. Accepts 2 to 4 token addresses separated by commas
2. Price and volume data are sourced from DexScreener for real-time accuracy
3. Burned percentage is calculated by checking dead address and burn address balances
4. Pair count reflects active pairs across all BSC DEXs
5. Tokens not found on any DEX will return null values for market fields
6. Use this skill to make data-driven comparisons rather than relying on hype
