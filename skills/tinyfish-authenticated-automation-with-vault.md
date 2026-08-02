---
name: Automate logged-in sites with vault credentials
description: Connect a password manager and run TinyFish Web Agent automations that authenticate into sites using vault credentials.
api: openapi/tinyfish-main-openapi.json
operations: [connectVault, listVaultConnections, syncVaultItems, listVaultItems, run, disconnectVault]
---

# Automate logged-in sites with vault credentials

## Auth
Send `X-API-Key: <key>`. Base URL `https://agent.tinyfish.ai`. Vault is a gated feature (503 `Vault feature is not enabled` if off).

## Steps
1. **Connect provider:** `POST /v1/vault/connections` (`connectVault`) for 1Password or Bitwarden. Syncs display-safe credential metadata.
2. **Verify:** `GET /v1/vault/connections` (`listVaultConnections`).
3. **Sync items:** `POST /v1/vault/items/sync` (`syncVaultItems`), then `GET /v1/vault/items` (`listVaultItems`) to see available credentials.
4. **Run with login:** `POST /v1/automation/run` (`run`) with `use_vault: true` so the agent authenticates into the target site during the run.
5. **Disconnect:** `DELETE /v1/vault/connections/{connectionId}` (`disconnectVault`) to remove stored items.

## Rules
- Never place raw secrets in the `goal`; rely on vault credentials.
- Only display-safe metadata is exposed via the API — secret values stay in the provider.
- Combine with a browser context profile to persist logged-in state across runs.
