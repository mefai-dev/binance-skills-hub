---
name: gas-calculator
description: |
  Calculate and compare gas costs for common blockchain operations on BSC vs Ethereum.
  Shows costs in native tokens and USD for transfers, swaps, approvals, contract deployments,
  and other operations. Quantifies exact savings from using BSC.
metadata:
  author: mefai
  version: "1.0"
---

# Gas Calculator Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Gas Calculator | Gas cost comparison | Calculate operation costs on BSC vs Ethereum with savings breakdown |

## Use Cases

1. **Cost Planning**: Estimate gas costs before executing operations on BSC
2. **Savings Calculation**: Quantify exactly how much you save using BSC vs Ethereum
3. **Operation Comparison**: Compare costs across different operation types
4. **Budget Estimation**: Plan transaction budgets for DeFi strategies

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |
| Ethereum | 1 |

---

## API: Gas Calculator

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/gas-calculator
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns current gas cost comparisons |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/gas-calculator'
```

**Response Example**:
```json
{
  "bscGasGwei": 3.0,
  "bnbPrice": 590,
  "ethGasGwei": 25.0,
  "ethPrice": 3200,
  "operations": [
    {
      "operation": "BNB/ETH Transfer",
      "gasUnits": 21000,
      "bsc": {
        "costBnb": 0.000063,
        "costUsd": 0.037
      },
      "eth": {
        "costEth": 0.000525,
        "costUsd": 1.68
      },
      "savingsUsd": 1.643
    },
    {
      "operation": "Token Transfer",
      "gasUnits": 65000,
      "bsc": {
        "costBnb": 0.000195,
        "costUsd": 0.115
      },
      "eth": {
        "costEth": 0.001625,
        "costUsd": 5.20
      },
      "savingsUsd": 5.085
    },
    {
      "operation": "DEX Swap",
      "gasUnits": 150000,
      "bsc": {
        "costBnb": 0.00045,
        "costUsd": 0.266
      },
      "eth": {
        "costEth": 0.00375,
        "costUsd": 12.00
      },
      "savingsUsd": 11.734
    },
    {
      "operation": "Token Approval",
      "gasUnits": 46000,
      "bsc": {
        "costBnb": 0.000138,
        "costUsd": 0.081
      },
      "eth": {
        "costEth": 0.00115,
        "costUsd": 3.68
      },
      "savingsUsd": 3.599
    },
    {
      "operation": "Contract Deployment",
      "gasUnits": 3000000,
      "bsc": {
        "costBnb": 0.009,
        "costUsd": 5.31
      },
      "eth": {
        "costEth": 0.075,
        "costUsd": 240.00
      },
      "savingsUsd": 234.69
    }
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| bscGasGwei | number | Current BSC gas price in Gwei |
| bnbPrice | number | Current BNB price in USD |
| ethGasGwei | number | Current Ethereum gas price in Gwei |
| ethPrice | number | Current ETH price in USD |
| operations | array | List of operation cost comparisons |
| operations[].operation | string | Operation name |
| operations[].gasUnits | number | Gas units consumed by the operation |
| operations[].bsc | object | BSC cost breakdown |
| operations[].bsc.costBnb | number | Cost in BNB |
| operations[].bsc.costUsd | number | Cost in USD |
| operations[].eth | object | Ethereum cost breakdown |
| operations[].eth.costEth | number | Cost in ETH |
| operations[].eth.costUsd | number | Cost in USD |
| operations[].savingsUsd | number | USD saved by using BSC instead of Ethereum |

---

## Notes

1. Gas prices and token prices are fetched in real-time for accurate cost calculation
2. Gas unit estimates use standard values; actual costs may vary based on contract complexity
3. BSC standard gas price is 3 Gwei; Ethereum gas varies significantly with demand
4. Savings calculation: savingsUsd = eth.costUsd - bsc.costUsd
5. Contract deployment costs can vary widely based on contract size and complexity
6. During Ethereum gas spikes, the savings from using BSC can be 100x or more
