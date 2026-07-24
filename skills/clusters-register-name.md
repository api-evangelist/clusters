---
name: Check availability and register a Clusters name
description: Check whether a cluster name is free, then build the on-chain registration transaction data for the user to sign.
api: openapi/clusters-v1-openapi.yml
operations: [checkNameAvailability, getRegistrationDataEvm, getRegistrationDataSolana]
---

# Check availability and register a Clusters name

Registration is on-chain: the API returns transaction data that the user's own
wallet must sign and broadcast. Clusters never holds the user's keys.

## Steps

1. Call `checkNameAvailability` — `POST /v1/names/register/check` with a JSON
   array of candidate names. Each result is `{ name, isAvailable }`.
2. For an available name, build the transaction data:
   - EVM: `getRegistrationDataEvm` — `POST /v1/names/register/evm` with
     `network` (chain id such as `"1"`, `"10"`, `"8453"`), `sender`, and
     `names` (`[{ name, amountWei? }]`; bid defaults to 0.01 ETH).
   - Solana: `getRegistrationDataSolana` — `POST /v1/names/register/solana`.
3. Hand the returned transaction data to the user's wallet to sign and submit.

## Rules

- Only build registration data for names confirmed `isAvailable: true`.
- `amountWei` bids are in ETH-denominated wei; omit to accept the 0.01 ETH default.
- Managing an existing cluster (create/wallet changes) needs a wallet-signature
  bearer token — see `skills/` auth flow and `authentication/clusters-authentication.yml`.
