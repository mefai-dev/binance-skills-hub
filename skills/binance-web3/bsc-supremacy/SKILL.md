---
name: bsc-supremacy
description: |
  Compare BSC against Ethereum across key performance metrics: block time, transactions per
  second, gas costs, finality time, and daily transaction volume. Demonstrates BSC advantages
  with real-time data from both networks.
metadata:
  author: mefai
  version: "1.0"
---

# BSC Supremacy Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| BSC Supremacy | BSC vs ETH comparison | Compare real-time performance metrics between BSC and Ethereum |

## Use Cases

1. **Chain Comparison**: Objective comparison of BSC vs Ethereum performance
2. **Cost Analysis**: Compare transaction costs between the two chains
3. **Speed Benchmarking**: Compare block times, TPS, and finality
4. **Migration Decision**: Data-driven evaluation for choosing between BSC and Ethereum

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |
| Ethereum | 1 |

---

## API: BSC Supremacy

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/bsc-supremacy
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns current comparison data |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/bsc-supremacy'
```

**Response Example**:
```json
{
  "comparison": {
    "blockTime": {
      "bsc": 3.0,
      "eth": 12.1,
      "unit": "seconds",
      "bscAdvantage": "4x faster"
    },
    "tps": {
      "bsc": 42,
      "eth": 15,
      "unit": "tx/second",
      "bscAdvantage": "2.8x higher"
    },
    "gasCost": {
      "bsc": 0.08,
      "eth": 2.45,
      "unit": "USD (simple transfer)",
      "bscAdvantage": "30x cheaper"
    },
    "swapCost": {
      "bsc": 0.25,
      "eth": 8.50,
      "unit": "USD (DEX swap)",
      "bscAdvantage": "34x cheaper"
    },
    "finality": {
      "bsc": 7.5,
      "eth": 384,
      "unit": "seconds",
      "bscAdvantage": "51x faster"
    }
  },
  "bscStats": {
    "gasPrice": 3.0,
    "bnbPrice": 590,
    "latestBlock": 35100050,
    "dailyTx": 4500000
  },
  "ethStats": {
    "gasPrice": 25.0,
    "ethPrice": 3200,
    "latestBlock": 19500000,
    "dailyTx": 1200000
  }
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| comparison | object | Side-by-side metric comparisons |
| comparison.blockTime | object | Block time comparison |
| comparison.tps | object | Transactions per second comparison |
| comparison.gasCost | object | Simple transfer gas cost comparison in USD |
| comparison.swapCost | object | DEX swap gas cost comparison in USD |
| comparison.finality | object | Transaction finality time comparison |
| comparison.*.bsc | number | BSC metric value |
| comparison.*.eth | number | Ethereum metric value |
| comparison.*.unit | string | Unit of measurement |
| comparison.*.bscAdvantage | string | Human-readable BSC advantage |
| bscStats | object | Current BSC network statistics |
| bscStats.gasPrice | number | BSC gas price in Gwei |
| bscStats.bnbPrice | number | Current BNB price in USD |
| bscStats.latestBlock | number | BSC latest block number |
| bscStats.dailyTx | number | BSC daily transaction count |
| ethStats | object | Current Ethereum network statistics |
| ethStats.gasPrice | number | ETH gas price in Gwei |
| ethStats.ethPrice | number | Current ETH price in USD |
| ethStats.latestBlock | number | ETH latest block number |
| ethStats.dailyTx | number | ETH daily transaction count |

---

## Notes

1. All metrics use real-time data from both BSC and Ethereum RPC endpoints
2. Gas costs are calculated using current gas prices and native token prices
3. BSC finality is based on 2/3 validator confirmation (~7.5s with 21 validators)
4. Ethereum finality accounts for 2 epoch finalization (~384 seconds)
5. TPS values reflect current throughput, not theoretical maximum
6. Cost comparisons use standard gas units: 21,000 for transfer, ~150,000 for swap
