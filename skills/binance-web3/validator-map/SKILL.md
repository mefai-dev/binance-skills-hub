---
name: validator-map
description: |
  Map the BSC validator landscape by analyzing recent block production patterns. Shows which
  validators are producing blocks, their gas utilization, TPS metrics, block timing, rotation
  patterns, and MEV activity alerts. Provides a real-time view of network validator behavior.
metadata:
  author: mefai
  version: "1.0"
---

# Validator Map Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Validator Map | Validator analysis | Map BSC validator activity, block production, and MEV patterns |

## Use Cases

1. **Validator Monitoring**: Track which validators are actively producing blocks
2. **Performance Comparison**: Compare gas utilization and TPS across validators
3. **Rotation Analysis**: Understand the validator rotation pattern
4. **MEV Detection**: Identify potential MEV activity from validator behavior

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Validator Map

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/validator-map
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns current validator landscape |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/validator-map'
```

**Response Example**:
```json
{
  "validators": [
    {
      "address": "0xValidator1",
      "label": "BSC Validator #1",
      "blocksProduced": 5,
      "avgGasUtil": 35.2,
      "avgTps": 42,
      "timing": {
        "avgBlockTime": 3.01,
        "fastestBlock": 2.85,
        "slowestBlock": 3.22
      }
    },
    {
      "address": "0xValidator2",
      "label": "BSC Validator #2",
      "blocksProduced": 4,
      "avgGasUtil": 28.7,
      "avgTps": 38,
      "timing": {
        "avgBlockTime": 3.00,
        "fastestBlock": 2.92,
        "slowestBlock": 3.10
      }
    }
  ],
  "rotationPattern": "round-robin",
  "mevAlerts": [
    {
      "validator": "0xValidator1",
      "type": "high-gas-tx-reorder",
      "block": 35100050,
      "details": "Suspicious transaction ordering detected"
    }
  ],
  "networkStats": {
    "activeValidators": 21,
    "blocksAnalyzed": 63,
    "avgNetworkTps": 40,
    "avgGasUtilization": 32.5
  }
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| validators | array | List of active validators with metrics |
| validators[].address | string | Validator address |
| validators[].label | string | Validator name or label |
| validators[].blocksProduced | number | Blocks produced in the analysis window |
| validators[].avgGasUtil | number | Average gas utilization percentage |
| validators[].avgTps | number | Average transactions per second |
| validators[].timing | object | Block timing statistics |
| validators[].timing.avgBlockTime | number | Average block time in seconds |
| validators[].timing.fastestBlock | number | Fastest block time in seconds |
| validators[].timing.slowestBlock | number | Slowest block time in seconds |
| rotationPattern | string | Observed validator rotation pattern |
| mevAlerts | array | Potential MEV activity alerts |
| mevAlerts[].validator | string | Validator address involved |
| mevAlerts[].type | string | Type of MEV activity detected |
| mevAlerts[].block | number | Block number |
| mevAlerts[].details | string | Human-readable alert description |
| networkStats | object | Aggregated network statistics |
| networkStats.activeValidators | number | Number of active validators |
| networkStats.blocksAnalyzed | number | Total blocks in the analysis window |
| networkStats.avgNetworkTps | number | Network-wide average TPS |
| networkStats.avgGasUtilization | number | Network-wide average gas utilization |

---

## Notes

1. BSC uses a Proof of Staked Authority (PoSA) consensus with 21 active validators
2. Validators rotate block production in a deterministic pattern
3. MEV alerts flag suspicious transaction ordering patterns within blocks
4. Block timing close to 3.0 seconds indicates healthy validator operation
5. Gas utilization above 80% sustained across validators indicates network congestion
6. Analysis window covers the most recent 3 rotation cycles (approximately 63 blocks)
