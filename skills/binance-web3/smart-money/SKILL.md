---
name: smart-money
description: |
  Track large-value transactions from whale wallets on BSC in real-time. Scans recent blocks
  for high-value transfers, DEX swaps, and token movements from known whale addresses.
  Provides labeled whale activity with transaction details and directional signals.
metadata:
  author: mefai
  version: "1.0"
---

# Smart Money Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Smart Money | Whale transaction tracker | Monitor large-value transactions from known whale wallets on BSC |

## Use Cases

1. **Whale Alert**: Get notified of large-value movements on BSC
2. **Market Direction**: Gauge market sentiment from whale buying and selling patterns
3. **Accumulation Detection**: Spot whales accumulating specific tokens
4. **Institutional Flow**: Track smart money movements across DeFi protocols

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Smart Money

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/smart-money
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns latest whale activity |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/smart-money'
```

**Response Example**:
```json
{
  "blocksScanned": 20,
  "totalValue": 4250000,
  "whales": [
    {
      "address": "0xWhaleAddress1",
      "label": "Binance Hot Wallet",
      "value": 2500000,
      "txHash": "0xTransactionHash1",
      "direction": "OUT",
      "block": 35100050
    },
    {
      "address": "0xWhaleAddress2",
      "label": "DeFi Fund #12",
      "value": 1750000,
      "txHash": "0xTransactionHash2",
      "direction": "IN",
      "block": 35100045
    }
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| blocksScanned | number | Number of recent blocks scanned |
| totalValue | number | Total USD value of all whale transactions found |
| whales | array | List of whale transactions |
| whales[].address | string | Whale wallet address |
| whales[].label | string | Known wallet label (e.g., "Binance Hot Wallet", "DeFi Fund") |
| whales[].value | number | Transaction value in USD |
| whales[].txHash | string | Transaction hash |
| whales[].direction | string | Fund direction: IN (receiving) or OUT (sending) |
| whales[].block | number | Block number of the transaction |

---

## Notes

1. Scans the most recent 20 blocks for large-value transactions
2. Known whale addresses are labeled from a maintained database of BSC entities
3. Value threshold for whale classification adjusts based on current market conditions
4. Direction indicates fund flow relative to the whale address (IN = receiving, OUT = sending)
5. Large outflows from exchange wallets often signal OTC deals or institutional movements
6. Multiple whales moving in the same direction can signal strong market conviction
