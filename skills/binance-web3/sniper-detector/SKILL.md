---
name: sniper-detector
description: |
  Detect sniper bot activity on BSC tokens by scanning the first blocks after pair creation.
  Identifies early buyers, measures their holding status, and produces a sniper score (0-100)
  indicating the likelihood of coordinated bot activity at token launch.
metadata:
  author: mefai
  version: "1.0"
---

# Sniper Detector Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Sniper Detector | Detect sniper bots | Analyze early trading blocks of a BSC token to identify bot sniping activity |

## Use Cases

1. **Launch Analysis**: Evaluate whether a token launch was dominated by sniper bots
2. **Bot Identification**: Identify specific addresses that sniped a token in the first blocks
3. **Holding Pattern Analysis**: Check if early snipers still hold or have dumped
4. **Fair Launch Verification**: Assess if a token had a fair launch vs coordinated bot manipulation

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Sniper Detector

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/sniper-detector
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BEP-20 token contract address to analyze |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/sniper-detector?address=0xTokenAddress'
```

**Response Example**:
```json
{
  "address": "0xTokenAddress",
  "sniperScore": 72,
  "earlyBuyers": [
    {
      "address": "0xBotAddress1",
      "block": 35000001,
      "amount": "500000000000000000000000",
      "stillHolds": false,
      "label": "Known MEV Bot"
    },
    {
      "address": "0xBotAddress2",
      "block": 35000001,
      "amount": "250000000000000000000000",
      "stillHolds": true,
      "label": "Unlabeled"
    }
  ],
  "pairInfo": {
    "pairAddress": "0xPairAddress",
    "creationBlock": 35000000,
    "dex": "PancakeSwap V2",
    "token0": "0xTokenAddress",
    "token1": "0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c"
  }
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Analyzed token address |
| sniperScore | number | Sniper activity score from 0 (clean) to 100 (heavy sniping) |
| earlyBuyers | array | List of addresses that bought in the first blocks |
| earlyBuyers[].address | string | Buyer wallet address |
| earlyBuyers[].block | number | Block number of the purchase |
| earlyBuyers[].amount | string | Raw token amount purchased |
| earlyBuyers[].stillHolds | boolean | Whether the address still holds the tokens |
| earlyBuyers[].label | string | Address label if identified (e.g., "Known MEV Bot") |
| pairInfo | object | Trading pair information |
| pairInfo.pairAddress | string | DEX pair contract address |
| pairInfo.creationBlock | number | Block number when the pair was created |
| pairInfo.dex | string | DEX name where the pair was created |
| pairInfo.token0 | string | Token0 address in the pair |
| pairInfo.token1 | string | Token1 address in the pair |

---

## Notes

1. Sniper score above 60 indicates significant bot activity at launch
2. Analysis scans the first 5-10 blocks after pair creation for rapid buy transactions
3. Known bot addresses are labeled from a maintained database of BSC MEV bots
4. Early buyers who have already sold (stillHolds: false) often indicate pump-and-dump patterns
5. Multiple purchases in the same block from different wallets may indicate a single operator
6. Tokens without DEX pairs or with very old launches may return limited data
