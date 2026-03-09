---
name: network-pulse
description: |
  Real-time BSC network health dashboard. Provides a pressure score (0-100), congestion
  recommendation (OPTIMAL/BUSY/CONGESTED), gas utilization, average block time, current TPS,
  gas price, and block statistics for network condition assessment.
metadata:
  author: mefai
  version: "1.0"
---

# Network Pulse Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Network Pulse | Network health monitor | Real-time BSC network condition and congestion assessment |

## Use Cases

1. **Transaction Timing**: Check network conditions before submitting transactions
2. **Gas Optimization**: Monitor gas prices to find optimal submission times
3. **Congestion Detection**: Get early warning of network congestion
4. **Network Health**: Monitor overall BSC network health metrics

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Network Pulse

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/network-pulse
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns current network status |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/network-pulse'
```

**Response Example**:
```json
{
  "pressureScore": 25,
  "recommendation": "OPTIMAL",
  "gasUtilization": 32.1,
  "avgBlockTime": 3.01,
  "currentTps": 42,
  "gasPriceGwei": 3.0,
  "blockStats": {
    "latestBlock": 35100050,
    "avgTxPerBlock": 126,
    "avgGasPerBlock": 44800000,
    "blocksAnalyzed": 20
  }
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| pressureScore | number | Network pressure from 0 (idle) to 100 (severely congested) |
| recommendation | string | Transaction timing recommendation: OPTIMAL, BUSY, or CONGESTED |
| gasUtilization | number | Average gas utilization percentage across recent blocks |
| avgBlockTime | number | Average block time in seconds |
| currentTps | number | Current transactions per second |
| gasPriceGwei | number | Current gas price in Gwei |
| blockStats | object | Block-level statistics |
| blockStats.latestBlock | number | Latest block number |
| blockStats.avgTxPerBlock | number | Average transactions per block |
| blockStats.avgGasPerBlock | number | Average gas used per block |
| blockStats.blocksAnalyzed | number | Number of blocks in the analysis window |

---

## Notes

1. Pressure score thresholds: 0-30 = OPTIMAL, 31-60 = BUSY, 61-100 = CONGESTED
2. Normal BSC gas price is 3 Gwei; higher values indicate congestion
3. BSC target block time is 3 seconds; deviations indicate validator issues
4. Normal TPS range for BSC is 30-60; spikes may indicate bot activity
5. Gas utilization above 70% sustained suggests increasing network load
6. Use OPTIMAL periods for cost-sensitive transactions; avoid CONGESTED periods for large swaps
