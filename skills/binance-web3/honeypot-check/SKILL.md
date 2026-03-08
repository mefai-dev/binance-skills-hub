---
name: honeypot-check
description: |
  Detect honeypot tokens on BSC by analyzing contract bytecode for dangerous selectors (blacklist, pause, maxTxAmount),
  checking owner token concentration, and comparing buy/sell ratios from DEX data. Returns a trap score (0-100) with
  verdict (SAFE/CAUTION/DANGER), detailed risk factors, and token metadata.
metadata:
  author: mefai
  version: "1.0"
---

# Honeypot Check Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Honeypot Check | Detect honeypot tokens | Scan BSC tokens for buy/sell traps, dangerous contract patterns, and owner concentration risks |

## Use Cases

1. **Pre-Trade Safety**: Check any BSC token before buying to avoid honeypot scams
2. **Risk Assessment**: Get a quantified trap score (0-100) for objective risk evaluation
3. **Contract Analysis**: Identify dangerous function selectors hidden in bytecode
4. **DEX Liquidity Check**: Verify buy/sell ratios to detect one-sided trading traps

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Honeypot Check

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/honeypot-check
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BEP-20 token contract address to analyze |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/honeypot-check?address=0x55d398326f99059ff775485246999027b3197955'
```

**Response Example**:
```json
{
  "address": "0x55d398326f99059ff775485246999027b3197955",
  "trapScore": 12,
  "verdict": "SAFE",
  "riskFactors": [
    {
      "name": "Owner Concentration",
      "value": "8.2%",
      "severity": "low"
    }
  ],
  "tokenInfo": {
    "name": "Tether USD",
    "symbol": "USDT",
    "decimals": 18,
    "totalSupply": "3380000000000000000000000"
  },
  "dexData": {
    "pairCount": 5,
    "totalLiquidity": 13400000,
    "buyTxCount": 19850,
    "sellTxCount": 20019,
    "buySellRatio": 0.99
  }
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Analyzed contract address |
| trapScore | number | Risk score from 0 (safe) to 100 (definite honeypot) |
| verdict | string | Overall verdict: SAFE (0-30), CAUTION (31-60), DANGER (61-100) |
| riskFactors | array | List of identified risk factors |
| riskFactors[].name | string | Risk factor name (e.g., "Blacklist Function", "Pause Function") |
| riskFactors[].value | string | Risk factor value or measurement |
| riskFactors[].severity | string | Severity level: low, medium, high, critical |
| tokenInfo | object | Basic token metadata |
| tokenInfo.name | string | Token name |
| tokenInfo.symbol | string | Token symbol |
| tokenInfo.decimals | number | Token decimal places |
| tokenInfo.totalSupply | string | Total token supply (raw) |
| dexData | object | DEX trading data |
| dexData.pairCount | number | Number of active trading pairs |
| dexData.totalLiquidity | number | Total liquidity in USD |
| dexData.buyTxCount | number | Number of buy transactions |
| dexData.sellTxCount | number | Number of sell transactions |
| dexData.buySellRatio | number | Ratio of buys to sells (healthy > 0.8) |

---

## Notes

1. Trap score thresholds: 0-30 = SAFE, 31-60 = CAUTION, 61-100 = DANGER
2. Bytecode analysis checks for dangerous selectors: blacklist, pause, maxTxAmount, transfer restrictions
3. Owner concentration above 50% significantly increases the trap score
4. A buy/sell ratio significantly below 1.0 suggests sell restrictions may be present
5. DEX data is sourced from DexScreener for real-time accuracy
6. This tool provides risk indicators only and is not financial advice
