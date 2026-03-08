---
name: risk-radar
description: |
  Comprehensive risk assessment for BSC tokens. Combines bytecode analysis, ownership checks,
  liquidity evaluation, and holder distribution to produce a risk score (0-100) with letter
  grade (A-F). Identifies both safe indicators and risk factors for balanced evaluation.
metadata:
  author: mefai
  version: "1.0"
---

# Risk Radar Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Risk Radar | Token risk assessment | Generate a comprehensive risk score and grade for any BSC token |

## Use Cases

1. **Investment Screening**: Quick risk assessment before investing in a BSC token
2. **Portfolio Review**: Evaluate risk levels of tokens already held
3. **Due Diligence**: Get a breakdown of safe indicators vs risk factors
4. **Comparative Analysis**: Compare risk grades across multiple tokens

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Risk Radar

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/risk-radar
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BEP-20 token contract address to assess |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/risk-radar?address=0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82'
```

**Response Example**:
```json
{
  "address": "0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82",
  "riskScore": 22,
  "grade": "A",
  "safeIndicators": [
    "Verified source code on BscScan",
    "Healthy buy/sell ratio (0.98)",
    "Liquidity above $10M",
    "No blacklist function detected",
    "Owner renounced or multi-sig"
  ],
  "riskFactors": [
    "Mintable token (new supply can be created)",
    "Top 10 holders control 45% of supply"
  ],
  "tokenInfo": {
    "name": "PancakeSwap Token",
    "symbol": "CAKE",
    "decimals": 18,
    "liquidity": 12500000,
    "holders": 450000
  }
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Assessed token contract address |
| riskScore | number | Risk score from 0 (lowest risk) to 100 (highest risk) |
| grade | string | Letter grade: A (0-20), B (21-40), C (41-60), D (61-80), F (81-100) |
| safeIndicators | array | List of positive safety signals identified |
| riskFactors | array | List of risk concerns identified |
| tokenInfo | object | Basic token information |
| tokenInfo.name | string | Token name |
| tokenInfo.symbol | string | Token symbol |
| tokenInfo.decimals | number | Token decimal places |
| tokenInfo.liquidity | number | Total DEX liquidity in USD |
| tokenInfo.holders | number | Total holder count |

---

## Notes

1. Grade thresholds: A (0-20), B (21-40), C (41-60), D (61-80), F (81-100)
2. Analysis combines multiple data sources: bytecode patterns, on-chain state, and DEX data
3. Safe indicators and risk factors provide a balanced view rather than just a score
4. A low risk score does not guarantee safety; always perform additional research
5. Newly deployed tokens with limited history may receive higher scores due to insufficient data
