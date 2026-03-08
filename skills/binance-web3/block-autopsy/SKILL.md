---
name: block-autopsy
description: |
  Dissect any BSC block to reveal its internal composition: transaction count, gas usage,
  total BNB value transferred, transaction type breakdown (swap, transfer, approve, other),
  and top gas consumers. Analyze the latest block or a specific block number.
metadata:
  author: mefai
  version: "1.0"
---

# Block Autopsy Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Block Autopsy | Block analysis | Dissect BSC blocks to reveal transaction composition and gas usage patterns |

## Use Cases

1. **Block Analysis**: Understand the composition and activity within a specific block
2. **Gas Usage Patterns**: Identify which contracts consumed the most gas in a block
3. **Activity Breakdown**: See the distribution of transaction types (swaps, transfers, approvals)
4. **Network Monitoring**: Analyze block fullness and activity levels

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Block Autopsy

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/block-autopsy
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| number | number | No | Block number to analyze (defaults to latest block if omitted) |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/block-autopsy'
```

```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/block-autopsy?number=35100050'
```

**Response Example**:
```json
{
  "blockNumber": 35100050,
  "timestamp": "2026-03-08T12:00:00Z",
  "txCount": 185,
  "gasUsed": 45000000,
  "gasLimit": 140000000,
  "gasUtilization": 32.1,
  "totalValue": 125.5,
  "txTypes": {
    "swap": 78,
    "transfer": 52,
    "approve": 31,
    "other": 24
  },
  "topGasConsumers": [
    {
      "address": "0x10ED43C718714eb63d5aA57B78B54704E256024E",
      "label": "PancakeSwap Router V2",
      "gasUsed": 12500000,
      "txCount": 45
    },
    {
      "address": "0x13f4EA83D0bd40E75C8222255bc855a974568Dd4",
      "label": "PancakeSwap Router V3",
      "gasUsed": 8200000,
      "txCount": 28
    },
    {
      "address": "0xUnknownContract",
      "label": "Unknown",
      "gasUsed": 5600000,
      "txCount": 12
    }
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| blockNumber | number | Analyzed block number |
| timestamp | string | Block timestamp in ISO 8601 format |
| txCount | number | Total transaction count in the block |
| gasUsed | number | Total gas consumed in the block |
| gasLimit | number | Block gas limit |
| gasUtilization | number | Gas utilization percentage |
| totalValue | number | Total BNB value transferred in the block |
| txTypes | object | Transaction type breakdown |
| txTypes.swap | number | Number of DEX swap transactions |
| txTypes.transfer | number | Number of token/BNB transfer transactions |
| txTypes.approve | number | Number of approval transactions |
| txTypes.other | number | Number of other transaction types |
| topGasConsumers | array | Top contracts by gas consumption |
| topGasConsumers[].address | string | Contract address |
| topGasConsumers[].label | string | Known contract label |
| topGasConsumers[].gasUsed | number | Gas consumed by this contract |
| topGasConsumers[].txCount | number | Number of transactions to this contract |

---

## Notes

1. When no block number is specified, the latest finalized block is analyzed
2. Transaction types are classified by function selector matching
3. Gas utilization above 80% indicates a congested block
4. Top gas consumers typically include major DEX routers and bridge contracts
5. High "other" transaction counts may indicate MEV bot activity
6. BSC produces blocks every 3 seconds, each with a 140M gas limit
