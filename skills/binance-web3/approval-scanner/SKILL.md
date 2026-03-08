---
name: approval-scanner
description: |
  Scan a BSC wallet for active token approvals across major DEX routers. Checks 8 top BSC tokens
  against 9 major DEX routers to identify unlimited or excessive allowances that could be exploited.
  Returns detailed approval data with spender labels and risk indicators.
metadata:
  author: mefai
  version: "1.0"
---

# Approval Scanner Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Approval Scanner | Scan token approvals | Find active token approvals granted to DEX routers and other spender contracts |

## Use Cases

1. **Security Audit**: Review all active token approvals on a wallet to find potential risks
2. **Unlimited Approval Detection**: Identify unlimited allowances that grant full token access to spenders
3. **Revoke Preparation**: Get a list of approvals to review before revoking dangerous ones
4. **Wallet Hygiene**: Regular approval scanning as part of wallet security maintenance

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Approval Scanner

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/approval-scanner
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BSC wallet address to scan |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/approval-scanner?address=0xYourWalletAddress'
```

**Response Example**:
```json
{
  "address": "0xYourWalletAddress",
  "tokensChecked": 8,
  "spendersChecked": 9,
  "totalApprovals": 3,
  "approvals": [
    {
      "token": "0x55d398326f99059ff775485246999027b3197955",
      "symbol": "USDT",
      "spender": "0x10ED43C718714eb63d5aA57B78B54704E256024E",
      "spenderLabel": "PancakeSwap Router V2",
      "allowance": "115792089237316195423570985008687907853269984665640564039457584007913129639935",
      "isUnlimited": true
    },
    {
      "token": "0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56",
      "symbol": "BUSD",
      "spender": "0x13f4EA83D0bd40E75C8222255bc855a974568Dd4",
      "spenderLabel": "PancakeSwap Router V3",
      "allowance": "50000000000000000000000",
      "isUnlimited": false
    }
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Scanned wallet address |
| tokensChecked | number | Number of tokens checked (8 top BSC tokens) |
| spendersChecked | number | Number of spender contracts checked (9 major DEX routers) |
| totalApprovals | number | Total active approvals found |
| approvals | array | List of active approvals |
| approvals[].token | string | Token contract address |
| approvals[].symbol | string | Token symbol |
| approvals[].spender | string | Approved spender contract address |
| approvals[].spenderLabel | string | Human-readable spender name |
| approvals[].allowance | string | Raw allowance amount |
| approvals[].isUnlimited | boolean | Whether allowance is set to max uint256 (unlimited) |

---

## Notes

1. Scans the following BSC tokens: USDT, BUSD, USDC, WBNB, CAKE, ETH, BTCB, DAI
2. Checks against major DEX routers: PancakeSwap V2/V3, BiSwap, ApeSwap, BabySwap, and others
3. Unlimited approvals (max uint256) are flagged as they grant full token access to the spender
4. Approval data is read directly from on-chain allowance mappings
5. Consider revoking approvals for contracts you no longer interact with
6. This tool reads approvals only and does not perform any revocation transactions
