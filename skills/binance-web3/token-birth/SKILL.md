---
name: token-birth
description: |
  Investigate the origin story of any BSC token. Returns token age, creator wallet profile
  (balance, transaction count, holding percentage), supply breakdown, pair count, and
  current market data for comprehensive launch analysis.
metadata:
  author: mefai
  version: "1.0"
---

# Token Birth Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Token Birth | Token origin analysis | Investigate the creation history and creator profile of any BSC token |

## Use Cases

1. **Token Age Verification**: Determine exactly when a token was created
2. **Creator Investigation**: Profile the creator wallet's activity and current holdings
3. **Supply Analysis**: Understand the supply breakdown and distribution
4. **Launch Assessment**: Evaluate a token's origin for signs of legitimacy or fraud

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Token Birth

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/token-birth
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BEP-20 token contract address |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/token-birth?address=0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82'
```

**Response Example**:
```json
{
  "address": "0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82",
  "name": "PancakeSwap Token",
  "symbol": "CAKE",
  "decimals": 18,
  "ageDays": 1825,
  "creatorProfile": {
    "address": "0xCreatorAddress",
    "bnbBalance": 45.2,
    "txCount": 15230,
    "tokenHoldPct": 0.5
  },
  "supplyBreakdown": {
    "totalSupply": "389000000000000000000000000",
    "burned": "305000000000000000000000000",
    "burnedPct": 78.4,
    "circulating": "84000000000000000000000000"
  },
  "pairCount": 85,
  "marketData": {
    "price": 2.45,
    "volume24h": 45000000,
    "liquidity": 12500000,
    "marketCap": 205800000
  }
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Token contract address |
| name | string | Token name |
| symbol | string | Token symbol |
| decimals | number | Token decimal places |
| ageDays | number | Token age in days since deployment |
| creatorProfile | object | Creator wallet analysis |
| creatorProfile.address | string | Creator wallet address |
| creatorProfile.bnbBalance | number | Creator's current BNB balance |
| creatorProfile.txCount | number | Creator's total transaction count |
| creatorProfile.tokenHoldPct | number | Percentage of token supply held by creator |
| supplyBreakdown | object | Token supply analysis |
| supplyBreakdown.totalSupply | string | Total supply (raw) |
| supplyBreakdown.burned | string | Burned supply (raw) |
| supplyBreakdown.burnedPct | number | Percentage of supply burned |
| supplyBreakdown.circulating | string | Circulating supply (raw) |
| pairCount | number | Number of active DEX trading pairs |
| marketData | object | Current market data |
| marketData.price | number | Current price in USD |
| marketData.volume24h | number | 24-hour volume in USD |
| marketData.liquidity | number | Total liquidity in USD |
| marketData.marketCap | number | Market capitalization in USD |

---

## Notes

1. Token age is calculated from the contract deployment transaction timestamp
2. Creator address is identified from the deployment transaction
3. High creator holding percentage (>20%) may indicate centralization risk
4. Tokens deployed by contracts (vs EOAs) may have different ownership structures
5. Supply breakdown checks standard dead addresses (0x0, 0xdead) for burn calculations
6. Very new tokens (< 7 days) may have limited market data available
