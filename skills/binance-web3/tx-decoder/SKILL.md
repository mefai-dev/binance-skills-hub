---
name: tx-decoder
description: |
  Decode any BSC transaction into human-readable components. Extracts function calls, event logs,
  token transfers, gas costs, and execution status. Translates raw transaction data into structured,
  understandable output for transaction analysis and debugging.
metadata:
  author: mefai
  version: "1.0"
---

# Transaction Decoder Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| TX Decoder | Transaction decoding | Decode BSC transactions into readable function calls, events, and token transfers |

## Use Cases

1. **Transaction Analysis**: Understand what a complex transaction actually did
2. **Token Transfer Tracking**: Extract all token transfers within a transaction
3. **Gas Cost Analysis**: Calculate the exact BNB cost of a transaction
4. **Debug Failed Transactions**: Identify why a transaction failed and what function was called

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Transaction Decoder

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/tx-decoder
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| hash | string | Yes | BSC transaction hash |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/tx-decoder?hash=0xTransactionHash'
```

**Response Example**:
```json
{
  "hash": "0xTransactionHash",
  "from": "0xSenderAddress",
  "to": "0xReceiverAddress",
  "value": "0",
  "function": "swapExactTokensForTokens",
  "functionSelector": "0x38ed1739",
  "status": "success",
  "gasUsed": 152340,
  "gasPrice": "3000000000",
  "gasCostBnb": 0.00045702,
  "blockNumber": 35100050,
  "events": [
    {
      "event": "Swap",
      "address": "0xPairAddress"
    },
    {
      "event": "Transfer",
      "address": "0xTokenAddress"
    }
  ],
  "tokenTransfers": [
    {
      "token": "0xTokenA",
      "from": "0xSenderAddress",
      "to": "0xPairAddress",
      "rawAmount": "1000000000000000000"
    },
    {
      "token": "0xTokenB",
      "from": "0xPairAddress",
      "to": "0xSenderAddress",
      "rawAmount": "5000000000000000000000"
    }
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| hash | string | Transaction hash |
| from | string | Sender address |
| to | string | Recipient/contract address |
| value | string | BNB value sent (in wei) |
| function | string | Decoded function name |
| functionSelector | string | 4-byte function selector |
| status | string | Execution status: "success" or "failed" |
| gasUsed | number | Gas units consumed |
| gasPrice | string | Gas price in wei |
| gasCostBnb | number | Total gas cost in BNB |
| blockNumber | number | Block number containing the transaction |
| events | array | List of emitted events |
| events[].event | string | Event name |
| events[].address | string | Contract that emitted the event |
| tokenTransfers | array | List of token transfers within the transaction |
| tokenTransfers[].token | string | Token contract address |
| tokenTransfers[].from | string | Transfer sender |
| tokenTransfers[].to | string | Transfer recipient |
| tokenTransfers[].rawAmount | string | Raw transfer amount |

---

## Notes

1. Function names are decoded from a database of known function signatures
2. Unknown function selectors will show the raw 4-byte selector without a name
3. Token transfers are extracted from Transfer event logs
4. Gas cost in BNB is calculated as: gasUsed * gasPrice / 1e18
5. Failed transactions still consume gas; the status field indicates execution result
6. Complex transactions (e.g., flash loans) may contain many events and token transfers
