---
name: token-flow
description: |
  Track the flow of a specific BEP-20 token across BSC by scanning recent blocks for
  Transfer events. Returns transfer details including sender, recipient, amount, and
  aggregated statistics like unique addresses and largest transfer.
metadata:
  author: mefai
  version: "1.0"
---

# Token Flow Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Token Flow | Token transfer tracking | Monitor recent transfer events for any BEP-20 token on BSC |

## Use Cases

1. **Transfer Monitoring**: Track where tokens are moving in real-time
2. **Whale Alerts**: Identify unusually large token transfers
3. **Exchange Flow**: Detect tokens flowing to or from exchange wallets
4. **Distribution Analysis**: Understand how a token is being distributed across addresses

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Token Flow

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/token-flow
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| contract | string | Yes | BEP-20 token contract address |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/token-flow?contract=0x55d398326f99059ff775485246999027b3197955'
```

**Response Example**:
```json
{
  "contract": "0x55d398326f99059ff775485246999027b3197955",
  "symbol": "USDT",
  "blocksScanned": 20,
  "totalTransfers": 145,
  "uniqueAddresses": 230,
  "largestTransfer": {
    "txHash": "0xLargestTxHash",
    "from": "0xSenderAddress",
    "to": "0xReceiverAddress",
    "rawAmount": "5000000000000000000000000",
    "block": 35100048
  },
  "transfers": [
    {
      "txHash": "0xTxHash1",
      "block": 35100050,
      "from": "0xAddress1",
      "to": "0xAddress2",
      "rawAmount": "1000000000000000000000"
    },
    {
      "txHash": "0xTxHash2",
      "block": 35100049,
      "from": "0xAddress3",
      "to": "0xAddress4",
      "rawAmount": "500000000000000000000"
    }
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| contract | string | Token contract address |
| symbol | string | Token symbol |
| blocksScanned | number | Number of recent blocks scanned |
| totalTransfers | number | Total number of transfers found |
| uniqueAddresses | number | Number of unique addresses involved |
| largestTransfer | object | The largest transfer found in the scan window |
| largestTransfer.txHash | string | Transaction hash |
| largestTransfer.from | string | Sender address |
| largestTransfer.to | string | Receiver address |
| largestTransfer.rawAmount | string | Raw transfer amount |
| largestTransfer.block | number | Block number |
| transfers | array | List of recent transfers |
| transfers[].txHash | string | Transaction hash |
| transfers[].block | number | Block number |
| transfers[].from | string | Sender address |
| transfers[].to | string | Receiver address |
| transfers[].rawAmount | string | Raw transfer amount |

---

## Notes

1. Scans the most recent 20 blocks for Transfer events from the specified token
2. Raw amounts need to be divided by 10^decimals for human-readable values
3. Transfers to/from known exchange addresses are particularly noteworthy
4. High transfer counts in a short window may indicate automated trading or distribution
5. The largest transfer highlights the most significant movement in the scan window
6. Mint and burn events also emit Transfer events (from/to zero address)
