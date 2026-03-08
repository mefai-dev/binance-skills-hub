---
name: wallet-cluster
description: |
  Identify clusters of related BSC wallets by analyzing shared token holdings, transaction patterns,
  and fund flow connections. Starting from a seed wallet, discovers associated addresses that may
  belong to the same entity or operator.
metadata:
  author: mefai
  version: "1.0"
---

# Wallet Cluster Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Wallet Cluster | Wallet relationship mapping | Discover clusters of related wallets from a seed address |

## Use Cases

1. **Entity Identification**: Determine if multiple wallets belong to the same operator
2. **Sybil Detection**: Identify sybil wallet clusters used for airdrop farming or vote manipulation
3. **Fund Tracing**: Follow fund flows to discover connected wallet networks
4. **Holder Analysis**: Understand if a token's "many holders" are actually controlled by few entities

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Wallet Cluster

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/wallet-cluster
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | Seed wallet address to analyze |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/wallet-cluster?address=0xSeedWalletAddress'
```

**Response Example**:
```json
{
  "seedWallet": {
    "address": "0xSeedWalletAddress",
    "bnbBalance": 12.5,
    "tokenCount": 8,
    "txCount": 342
  },
  "clusterWallets": [
    {
      "address": "0xRelatedWallet1",
      "bnbBalance": 3.2,
      "sharedTokens": 5,
      "txCount": 156
    },
    {
      "address": "0xRelatedWallet2",
      "bnbBalance": 0.8,
      "sharedTokens": 4,
      "txCount": 89
    }
  ],
  "totalClusterValue": 16500,
  "clusterSize": 3
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| seedWallet | object | Information about the seed wallet |
| seedWallet.address | string | Seed wallet address |
| seedWallet.bnbBalance | number | BNB balance of seed wallet |
| seedWallet.tokenCount | number | Number of token types held |
| seedWallet.txCount | number | Total transaction count |
| clusterWallets | array | List of discovered related wallets |
| clusterWallets[].address | string | Related wallet address |
| clusterWallets[].bnbBalance | number | BNB balance |
| clusterWallets[].sharedTokens | number | Number of tokens shared with seed wallet |
| clusterWallets[].txCount | number | Total transaction count |
| totalClusterValue | number | Combined USD value across all cluster wallets |
| clusterSize | number | Total number of wallets in the cluster |

---

## Notes

1. Cluster detection analyzes shared token holdings, direct fund transfers, and timing patterns
2. A high number of shared tokens between wallets strongly suggests common ownership
3. Wallets funded from the same source within a short time window are likely related
4. Cluster analysis depth is limited to direct connections from the seed wallet
5. Large cluster sizes (>10 wallets) may indicate airdrop farming or wash trading operations
6. Results are based on on-chain heuristics and may include false positives
