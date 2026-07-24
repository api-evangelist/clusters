---
name: Resolve a Clusters name to addresses
description: Given one or more cluster names, resolve the wallet addresses they point to and read the full cluster profile.
api: openapi/clusters-v1-openapi.yml
operations: [getAddressesByName, getClusterByName]
---

# Resolve a Clusters name to addresses

Use this to auto-populate a destination wallet in a bridge or wallet UI from a
human-typed cluster name, reducing manual entry and address-paste scams.

## Steps

1. Call `getAddressesByName` — `POST /v1/names` with a JSON array of
   `{ "name": "..." }` entries. You may pass a bare cluster name (`clusters`) or
   a specific wallet name (`clusters/main`). Names not found are omitted from the
   response, so match results back to your inputs by `name`.
2. Each result carries `address`, `type` (`evm`/`solana`), `clusterName`,
   `walletName`, and `isVerified`.
3. To read the whole profile (all enrolled wallets), call `getClusterByName` —
   `GET /v1/clusters/name/{name}`.

## Rules

- Batch lookups through `getAddressesByName` rather than looping single calls.
- A missing entry means the name is unregistered — do not send funds to a guess.
- See `data-model/clusters-data-model.yml` for the Cluster/Wallet/Name graph.
