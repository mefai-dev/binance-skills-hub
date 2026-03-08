---
name: contract-audit
description: |
  Automated security audit for BSC smart contracts. Analyzes ownership model, proxy status,
  ERC standards compliance, access control patterns, and common vulnerability indicators.
  Produces a structured audit report with findings categorized by severity.
metadata:
  author: mefai
  version: "1.0"
---

# Contract Audit Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Contract Audit | Automated security audit | Analyze BSC contracts for ownership, proxy status, standards compliance, and vulnerabilities |

## Use Cases

1. **Security Review**: Automated first-pass audit of any BSC contract
2. **Ownership Analysis**: Understand who controls the contract and what powers they have
3. **Standards Compliance**: Verify ERC-20/BEP-20 interface compliance
4. **Vulnerability Detection**: Identify common smart contract vulnerability patterns

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Contract Audit

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/contract-audit
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BSC contract address to audit |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/contract-audit?address=0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82'
```

**Response Example**:
```json
{
  "address": "0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82",
  "auditScore": 85,
  "ownership": {
    "owner": "0x73feaa1eE314F8c655E354234017bE2193C9E24E",
    "isRenounced": false,
    "ownerType": "contract",
    "powers": ["mint", "transferOwnership"]
  },
  "proxyStatus": {
    "isProxy": false,
    "implementation": null,
    "admin": null
  },
  "standards": {
    "isERC20": true,
    "hasName": true,
    "hasSymbol": true,
    "hasDecimals": true,
    "hasTotalSupply": true,
    "hasTransfer": true,
    "hasApprove": true,
    "hasTransferFrom": true
  },
  "findings": [
    {
      "severity": "info",
      "category": "ownership",
      "message": "Owner is a contract address (likely multi-sig or governance)"
    },
    {
      "severity": "low",
      "category": "supply",
      "message": "Mint function detected - new tokens can be created"
    }
  ]
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Audited contract address |
| auditScore | number | Overall audit score (0-100, higher is better) |
| ownership | object | Ownership analysis results |
| ownership.owner | string | Current owner address |
| ownership.isRenounced | boolean | Whether ownership has been renounced |
| ownership.ownerType | string | Owner type: "eoa", "contract", "renounced" |
| ownership.powers | array | List of owner-restricted functions |
| proxyStatus | object | Proxy pattern analysis |
| proxyStatus.isProxy | boolean | Whether the contract uses a proxy pattern |
| proxyStatus.implementation | string/null | Implementation address if proxy |
| proxyStatus.admin | string/null | Proxy admin address |
| standards | object | ERC-20/BEP-20 compliance check |
| standards.isERC20 | boolean | Whether all required ERC-20 functions are present |
| findings | array | List of audit findings |
| findings[].severity | string | Finding severity: info, low, medium, high, critical |
| findings[].category | string | Finding category (ownership, supply, access, proxy) |
| findings[].message | string | Human-readable finding description |

---

## Notes

1. This is an automated bytecode-level audit and does not replace a professional security audit
2. Audit score reflects structural safety indicators; 100 does not guarantee zero vulnerabilities
3. Ownership analysis checks Ownable, AccessControl, and custom admin patterns
4. Renounced ownership (owner = 0x0) scores highest for ownership safety
5. Standards compliance checks the presence of required ERC-20 function selectors in bytecode
6. Findings are categorized by severity: info (informational), low, medium, high, critical
