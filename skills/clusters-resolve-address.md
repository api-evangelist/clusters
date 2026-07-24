---
name: Resolve an address to a Clusters name
description: Given an EVM or Solana wallet address, resolve its human-readable cluster/wallet name and list all names owned by that wallet.
api: openapi/clusters-v1-openapi.yml
operations: [getNameByAddress, getNamesByOwner]
---

# Resolve an address to a Clusters name

Use the Clusters v1 API to turn a raw wallet address into a human-readable name.
Read endpoints are public — no API key or auth token is required (an optional
`X-API-KEY` header only raises rate limits).

## Steps

1. Call `getNameByAddress` — `GET /v1/names/address/{address}` with the EVM or
   Solana address. A 200 returns `clusterName`, `walletName`, `type`, and
   `isVerified`. If unregistered, the body echoes the address with
   `clusterName`/`walletName` set to `null` (HTTP 404) — treat that as "no name".
2. To list everything a wallet owns, call `getNamesByOwner` —
   `GET /v1/names/owner/address/{address}` — which returns an array of owned
   names with `clusterId`, `owner`, and timestamps.
3. For testnet (Sepolia) state, append `?testnet=true`.

## Rules

- Never assume a name exists; a null `clusterName` means unregistered.
- Prefer `isVerified: true` names when displaying identity to users.
- See `conventions/clusters-conventions.yml` for the testnet flag and error shape.
