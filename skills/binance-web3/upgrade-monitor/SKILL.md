---
name: upgrade-monitor
description: |
  Monitor upgradeable proxy contracts on BSC. Reads EIP-1967 storage slots to identify proxy patterns,
  implementation addresses, and admin addresses. Assesses upgrade risk level based on admin type,
  code size changes, and proxy architecture.
metadata:
  author: mefai
  version: "1.0"
---

# Upgrade Monitor Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Upgrade Monitor | Proxy contract monitoring | Detect upgradeable proxy contracts and assess upgrade risk |

## Use Cases

1. **Proxy Detection**: Determine if a contract is an upgradeable proxy
2. **Admin Identification**: Find the admin address that can trigger upgrades
3. **Risk Assessment**: Evaluate the risk level of potential contract upgrades
4. **Implementation Tracking**: Get the current implementation address behind a proxy

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Upgrade Monitor

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/upgrade-monitor
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BSC contract address to monitor |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/upgrade-monitor?address=0xContractAddress'
```

**Response Example**:
```json
{
  "address": "0xContractAddress",
  "isProxy": true,
  "implementationAddress": "0xImplementationAddress",
  "adminAddress": "0xAdminAddress",
  "adminLabel": "ProxyAdmin Contract",
  "codeSize": 1245,
  "implCodeSize": 15890,
  "upgradeRisk": "MEDIUM"
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Analyzed contract address |
| isProxy | boolean | Whether the contract is an upgradeable proxy |
| implementationAddress | string/null | Current implementation contract address |
| adminAddress | string/null | Admin address that can trigger upgrades |
| adminLabel | string | Human-readable label for the admin (e.g., "EOA", "ProxyAdmin Contract", "Timelock") |
| codeSize | number | Proxy contract bytecode size in bytes |
| implCodeSize | number | Implementation contract bytecode size in bytes |
| upgradeRisk | string | Risk assessment: LOW, MEDIUM, HIGH, or CRITICAL |

---

## Notes

1. Reads EIP-1967 standard storage slots: implementation slot (`0x360894...`) and admin slot (`0xb53127...`)
2. Risk levels: LOW (timelock admin), MEDIUM (multi-sig admin), HIGH (EOA admin), CRITICAL (unverified proxy)
3. Non-proxy contracts return `isProxy: false` with null implementation and admin fields
4. A proxy with an EOA admin means a single private key can change the entire contract logic
5. Timelock admins provide the lowest risk as upgrades are delayed and publicly visible
6. Always verify the implementation contract source code is verified on BscScan
