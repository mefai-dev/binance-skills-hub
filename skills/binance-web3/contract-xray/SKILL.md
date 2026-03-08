---
name: contract-xray
description: |
  Deep-inspect any BSC smart contract to reveal its internal structure including code size, proxy status,
  implementation address, detected function selectors, capability patterns (mintable, pausable, blacklistable,
  burnable), owner address, and ERC-20 metadata.
metadata:
  author: mefai
  version: "1.0"
---

# Contract X-Ray Skill

## Overview

| API | Function | Use Case |
|-----|----------|----------|
| Contract X-Ray | Deep contract inspection | Reveal internal structure, proxy patterns, function selectors, and capability flags of any BSC contract |

## Use Cases

1. **Contract Verification**: Inspect unknown contracts before interaction to understand capabilities
2. **Proxy Detection**: Identify upgradeable proxy contracts and their implementation addresses
3. **Function Discovery**: List all detected function selectors in contract bytecode
4. **Capability Audit**: Check if a contract is mintable, pausable, blacklistable, or burnable

## Supported Chains

| Chain Name | chainId |
|------------|---------|
| BSC | 56 |

---

## API: Contract X-Ray

### Method: GET

**URL**:
```
https://mefai.io/superbsc/api/bnbchain/mefai/contract-xray
```

**Request Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| address | string | Yes | BSC contract address to inspect |

**Example Request**:
```bash
curl 'https://mefai.io/superbsc/api/bnbchain/mefai/contract-xray?address=0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82'
```

**Response Example**:
```json
{
  "address": "0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82",
  "codeSize": 8432,
  "isProxy": false,
  "implementationAddress": null,
  "owner": "0x73feaa1eE314F8c655E354234017bE2193C9E24E",
  "detectedFunctions": [
    { "name": "transfer", "selector": "0xa9059cbb" },
    { "name": "approve", "selector": "0x095ea7b3" },
    { "name": "transferFrom", "selector": "0x23b872dd" },
    { "name": "mint", "selector": "0x40c10f19" },
    { "name": "burn", "selector": "0x42966c68" }
  ],
  "hasPatterns": {
    "mintable": true,
    "pausable": false,
    "blacklistable": false,
    "burnable": true
  },
  "erc20Info": {
    "name": "PancakeSwap Token",
    "symbol": "CAKE"
  }
}
```

**Response Fields**:

| Field | Type | Description |
|-------|------|-------------|
| address | string | Inspected contract address |
| codeSize | number | Contract bytecode size in bytes |
| isProxy | boolean | Whether the contract is a proxy pattern |
| implementationAddress | string/null | Implementation address if proxy, null otherwise |
| owner | string | Contract owner address |
| detectedFunctions | array | List of detected function selectors |
| detectedFunctions[].name | string | Human-readable function name |
| detectedFunctions[].selector | string | 4-byte function selector |
| hasPatterns | object | Detected capability patterns |
| hasPatterns.mintable | boolean | Contract can mint new tokens |
| hasPatterns.pausable | boolean | Contract can pause transfers |
| hasPatterns.blacklistable | boolean | Contract can blacklist addresses |
| hasPatterns.burnable | boolean | Contract can burn tokens |
| erc20Info | object | ERC-20 token metadata (if applicable) |
| erc20Info.name | string | Token name |
| erc20Info.symbol | string | Token symbol |

---

## Notes

1. Code size of 0 indicates an externally owned account (EOA), not a contract
2. Proxy detection reads EIP-1967 storage slots for implementation and admin addresses
3. Function selectors are matched against a database of known signatures
4. Capability patterns are detected via bytecode analysis, not source code
5. Owner detection attempts common patterns: Ownable, AccessControl, and custom admin slots
