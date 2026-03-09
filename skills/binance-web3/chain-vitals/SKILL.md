---
name: chain-vitals
description: |
  Quick health check for BSC node connectivity and chain status. Returns the latest block
  number, current gas price, peer count, and sync status. A lightweight endpoint for
  monitoring BSC availability and basic chain state.
metadata:
  author: mefai
  version: "1.0"
---

# Chain Vitals Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Chain Vitals | Chain health check | Quick BSC node status and chain state check |

## Use Cases

1. **Health Check**: Verify BSC node connectivity and responsiveness
2. **Block Tracking**: Get the latest block number for transaction confirmation tracking
3. **Gas Monitoring**: Check current gas price before submitting transactions
4. **Sync Status**: Verify if the connected node is fully synced

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Chain Vitals

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/chain-vitals
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns current chain status |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/chain-vitals'
```

**Response Example**:
```json
{
  "blockNumber": 35100050,
  "gasPrice": "3000000000",
  "gasPriceGwei": 3.0,
  "peerCount": 25,
  "syncing": false,
  "chainId": 56,
  "networkName": "BSC Mainnet",
  "timestamp": "2026-03-08T12:00:00Z"
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| blockNumber | number | Latest block number |
| gasPrice | string | Current gas price in wei |
| gasPriceGwei | number | Current gas price in Gwei |
| peerCount | number | Number of connected peers |
| syncing | boolean | Whether the node is currently syncing |
| chainId | number | Chain ID (56 for BSC Mainnet) |
| networkName | string | Human-readable network name |
| timestamp | string | Response timestamp in ISO 8601 format |

---

## Notes

1. This is the lightest-weight BSC status check, suitable for health monitoring
2. A syncing status of true indicates the node is not yet up to date
3. Low peer count (<5) may indicate network connectivity issues
4. Standard BSC gas price is 3 Gwei (3000000000 wei)
5. Block numbers increase every ~3 seconds on BSC
6. Use this endpoint for heartbeat monitoring in automated systems
