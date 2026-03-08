---
name: wallet-scanner
description: |
  Scan a BSC wallet to retrieve balances for BNB and top BEP-20 tokens with current USD
  valuations. Provides a quick portfolio snapshot showing token holdings, individual values,
  and total portfolio worth.
metadata:
  author: mefai
  version: "1.0"
---

# Wallet Scanner Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Wallet Scanner | Wallet balance scanner | Get BNB and top token balances with USD values for any BSC wallet |

## Use Cases

1. **Portfolio Check**: Quick snapshot of a wallet's holdings and total value
2. **Balance Verification**: Confirm token balances before initiating transactions
3. **Wallet Investigation**: Inspect any public wallet's holdings on BSC
4. **Asset Discovery**: See which major tokens a wallet holds

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Wallet Scanner

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/wallet-scanner
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BSC wallet address to scan |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/wallet-scanner?address=0xWalletAddress'
```

**Response Example**:
```json
{
  "address": "0xWalletAddress",
  "totalValueUsd": 15420.50,
  "balances": [
    {
      "token": "BNB",
      "contract": "native",
      "balance": "12.500000000000000000",
      "valueUsd": 7375.00,
      "price": 590.00
    },
    {
      "token": "USDT",
      "contract": "0x55d398326f99059ff775485246999027b3197955",
      "balance": "5000.000000000000000000",
      "valueUsd": 5000.00,
      "price": 1.00
    },
    {
      "token": "CAKE",
      "contract": "0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82",
      "balance": "1245.500000000000000000",
      "valueUsd": 3045.50,
      "price": 2.45
    }
  ],
  "tokensChecked": 10
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Scanned wallet address |
| totalValueUsd | number | Total portfolio value in USD |
| balances | array | List of token balances |
| balances[].token | string | Token symbol |
| balances[].contract | string | Token contract address ("native" for BNB) |
| balances[].balance | string | Token balance (formatted with decimals) |
| balances[].valueUsd | number | Balance value in USD |
| balances[].price | number | Current token price in USD |
| tokensChecked | number | Number of tokens checked |

---

## Notes

1. Scans BNB balance plus the top BSC tokens: USDT, BUSD, USDC, WBNB, CAKE, ETH, BTCB, DAI, XRP, DOGE
2. Tokens with zero balance are omitted from the response
3. Prices are fetched in real-time from on-chain oracle and DEX data
4. Only checks a curated list of top tokens; smaller tokens are not included
5. BNB is listed with contract "native" as it is the chain's native currency
6. For comprehensive token discovery, use in combination with other wallet analysis skills
