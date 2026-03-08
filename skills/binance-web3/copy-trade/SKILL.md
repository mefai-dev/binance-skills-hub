---
name: copy-trade
description: |
  Monitor known alpha wallets on BSC for real-time trading signals. Tracks recent transactions
  across curated high-performing wallets and surfaces actionable signals including token swaps,
  large transfers, and new position entries.
metadata:
  author: mefai
  version: "1.0"
---

# Copy Trade Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Copy Trade | Alpha wallet monitoring | Track real-time trading activity from known profitable BSC wallets |

## Use Cases

1. **Signal Discovery**: Get real-time trading signals from proven alpha wallets
2. **Whale Tracking**: Monitor large wallet movements for early market signals
3. **Strategy Analysis**: Study the trading patterns of successful BSC traders
4. **Market Sentiment**: Gauge market direction by aggregating smart money activity

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Copy Trade

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/copy-trade
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| (none) | - | - | No parameters required; returns latest signals from monitored wallets |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/copy-trade'
```

**Response Example**:
```json
{
  "activeWallets": 12,
  "totalSignals": 5,
  "signals": [
    {
      "wallet": "0xAlphaWallet1",
      "walletLabel": "DeFi Whale #3",
      "action": "BUY",
      "token": "0xTokenAddress",
      "tokenSymbol": "TOKEN",
      "amount": "15000000000000000000",
      "timeAgo": "2m",
      "txHash": "0xTransactionHash"
    },
    {
      "wallet": "0xAlphaWallet2",
      "walletLabel": "Smart Money #7",
      "action": "SELL",
      "token": "0xAnotherToken",
      "tokenSymbol": "ATOKEN",
      "amount": "8000000000000000000000",
      "timeAgo": "5m",
      "txHash": "0xAnotherTxHash"
    }
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| activeWallets | number | Number of monitored wallets currently active |
| totalSignals | number | Total number of signals in the response |
| signals | array | List of recent trading signals |
| signals[].wallet | string | Alpha wallet address |
| signals[].walletLabel | string | Human-readable wallet label |
| signals[].action | string | Trade action: BUY or SELL |
| signals[].token | string | Token contract address being traded |
| signals[].tokenSymbol | string | Token symbol |
| signals[].amount | string | Raw token amount involved |
| signals[].timeAgo | string | Human-readable time since the transaction |
| signals[].txHash | string | Transaction hash on BSC |

---

## Notes

1. Monitors a curated set of known profitable BSC wallets
2. Signals are generated from recent blocks and update in near real-time
3. Wallet labels are assigned based on historical performance and behavior patterns
4. Copy trading carries significant risk; signals are informational and not financial advice
5. Multiple wallets buying the same token in a short window may indicate strong alpha
6. The monitored wallet list is periodically updated based on performance metrics
