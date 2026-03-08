---
name: burn-tracker
description: |
  Track token burn events on BSC by monitoring transfers to known dead addresses
  (0x0, 0xdead). Returns recent burn transactions with token addresses, amounts,
  and estimated USD values for deflationary token analysis.
metadata:
  author: mefai
  version: "1.0"
---

# Burn Tracker Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Burn Tracker | Token burn monitoring | Track recent token burn events on BSC |

## Use Cases

1. **Deflationary Analysis**: Monitor burn activity for deflationary tokens
2. **Supply Reduction Tracking**: Quantify how much supply is being removed over time
3. **Burn Verification**: Verify claimed token burns with on-chain evidence
4. **Token Economics**: Assess the impact of burns on token supply and value

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Burn Tracker

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/burn-tracker
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns recent burn events |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/burn-tracker'
```

**Response Example**:
```json
{
  "blocksScanned": 100,
  "totalBurns": 12,
  "totalValueUsd": 45200,
  "burns": [
    {
      "txHash": "0xBurnTxHash1",
      "token": "0xTokenAddress1",
      "symbol": "TOKEN1",
      "from": "0xBurnerAddress",
      "burnAddress": "0x000000000000000000000000000000000000dEaD",
      "rawAmount": "1000000000000000000000000",
      "valueUsd": 25000,
      "block": 35100050
    },
    {
      "txHash": "0xBurnTxHash2",
      "token": "0xTokenAddress2",
      "symbol": "TOKEN2",
      "from": "0xAnotherBurner",
      "burnAddress": "0x0000000000000000000000000000000000000000",
      "rawAmount": "500000000000000000000",
      "valueUsd": 20200,
      "block": 35100035
    }
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| blocksScanned | number | Number of recent blocks scanned for burn events |
| totalBurns | number | Total number of burn transactions found |
| totalValueUsd | number | Total USD value of all burns in the scan window |
| burns | array | List of burn transactions |
| burns[].txHash | string | Transaction hash |
| burns[].token | string | Burned token contract address |
| burns[].symbol | string | Token symbol |
| burns[].from | string | Address that initiated the burn |
| burns[].burnAddress | string | Dead address the tokens were sent to |
| burns[].rawAmount | string | Raw amount of tokens burned |
| burns[].valueUsd | number | Estimated USD value of the burned tokens |
| burns[].block | number | Block number of the burn |

---

## Notes

1. Monitors transfers to standard dead addresses: 0x0 and 0xdead
2. USD values are estimated based on current token prices at scan time
3. Some tokens implement custom burn functions that may not transfer to dead addresses
4. Regular burns on a schedule may indicate a deflationary tokenomics model
5. Large single burns can significantly impact token supply and price
6. Scan window covers the most recent 100 blocks (approximately 5 minutes on BSC)
