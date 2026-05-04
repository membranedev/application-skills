---
name: credit-repair-cloud
description: |
  Credit Repair Cloud integration. Manage Users, Clients, Affiliates, Providers, Disputes, Products and more. Use when the user wants to interact with Credit Repair Cloud data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Credit Repair Cloud
Credit Repair Cloud is software for starting, running, and growing a credit repair business. It's used by entrepreneurs and credit repair business owners to help their clients improve their credit scores.

Official docs: https://support.creditrepaircloud.com/support/home
## Credit Repair Cloud Overview

- **Client**
  - **Client Task**
- **Affiliate**
- **Referral Partner**
- **User**
- **Credit Report**
- **Dispute Letter**
- **Creditor**
- **Bureau**
- **Product**
- **Affiliate Payout**
- **Affiliate Invoice**
- **Client Payment**
- **Email**
- **SMS Message**
- **Funnel**
- **Tag**
- **Integration**
- **Notification**
- **Report**
- **Subscription**
- **Template**
- **Automation**
- **Lead**
- **Document**
- **Goal**
- **Note**
- **Offer**
- **Order**
- **Refund**
- **Task**
- **Video**
- **Website**
- **Challenge**
- **Credit Monitoring Account**
- **Credit Score**
- **Credit Analyzer Result**
- **Import**
- **Client Portal Access**
- **Client Dispute Summary**
- **Client Credit Report Summary**
- **Client Login History**
- **Client Task Category**
- **Client Custom Field**
- **Client Credit Report Category**
- **Client Milestone**
- **Client Tag**
- **Client File**
- **Client Note**
- **Client Goal**
- **Client Automation**
- **Client Communication**
- **Client Product**
- **Client Order**
- **Client Refund**
- **Client Subscription**
- **Client Session**
- **Client Credit Report Item**
- **Client Credit Report Account**
- **Client Credit Report Inquiry**
- **Client Credit Report Collection**
- **Client Credit Report Public Record**
- **Client Credit Report Employment**
- **Client Credit Report Address**
- **Client Credit Report Statement**
- **Client Credit Report Disputed Item**
- **Client Credit Report Disputed Account**
- **Client Credit Report Disputed Inquiry**
- **Client Credit Report Disputed Collection**
- **Client Credit Report Disputed Public Record**
- **Client Credit Report Disputed Employment**
- **Client Credit Report Disputed Address**
- **Client Credit Report Disputed Statement**
- **Client Credit Report Analysis**
- **Client Credit Report Item Category**
- **Client Credit Report Item Status**
- **Client Credit Report Item Reason**
- **Client Credit Report Item Error**
- **Client Credit Report Item Recommendation**
- **Client Credit Report Item Analysis Result**
- **Client Credit Report Item Analysis**
- **Client Credit Report Item Analysis Category**
- **Client Credit Report Item Analysis Status**
- **Client Credit Report Item Analysis Reason**
- **Client Credit Report Item Analysis Error**
- **Client Credit Report Item Analysis Recommendation**
- **Client Credit Report Item Analysis Result Category**
- **Client Credit Report Item Analysis Result Status**
- **Client Credit Report Item Analysis Result Reason**
- **Client Credit Report Item Analysis Result Error**
- **Client Credit Report Item Analysis Result Recommendation**
- **Client Credit Report Item Analysis Result Analysis**
- **Client Credit Report Item Analysis Result Analysis Category**
- **Client Credit Report Item Analysis Result Analysis Status**
- **Client Credit Report Item Analysis Result Analysis Reason**
- **Client Credit Report Item Analysis Result Analysis Error**
- **Client Credit Report Item Analysis Result Analysis Recommendation**
- **Client Credit Report Item Analysis Result Analysis Result**
- **Client Credit Report Item Analysis Result Analysis Result Category**
- **Client Credit Report Item Analysis Result Analysis Result Status**
- **Client Credit Report Item Analysis Result Analysis Result Reason**
- **Client Credit Report Item Analysis Result Analysis Result Error**
- **Client Credit Report Item Analysis Result Analysis Result Recommendation**
- **Client Credit Report Item Analysis Result Analysis Result Analysis**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Category**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Status**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Reason**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Error**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Recommendation**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Category**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Status**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Reason**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Error**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Recommendation**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Category**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Status**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Reason**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Error**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Recommendation**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Category**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Status**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Reason**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Error**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Recommendation**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Category**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Status**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Reason**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Error**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Recommendation**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Category**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Status**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Reason**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Error**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Recommendation**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Analysis**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Analysis Category**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Analysis Status**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Analysis Reason**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Analysis Error**
- **Client Credit Report Item Analysis Result Analysis Result Analysis Result Analysis Result Analysis Result Analysis Recommendation**

Use action names and parameters as needed.
## Working with Credit Repair Cloud

This skill uses the Membrane CLI to interact with Credit Repair Cloud. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Credit Repair Cloud's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Credit Repair Cloud on your behalf.

### Install the CLI

```bash
npm install -g @membranehq/cli@latest
```

### Authenticate

```bash
membrane login --tenant --clientName=<agentType>
```

This will either open a browser for authentication or print an authorization URL to the console, depending on whether interactive mode is available.

**Headless environments:** The command will print an authorization URL. Ask the user to open it in a browser. When they see a code after completing login, finish with:

```bash
membrane login complete <code>
```

**Agent types** (passed to `--clientName`): `claude`, `openclaw`, `codex`, `warp`, `windsurf`, etc. Used to adjust tooling for your harness.

Add `--json` to any command for machine-readable JSON output.

## Step 1 — Get a connection to Credit Repair Cloud

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.creditrepaircloud.com/" --json
```

Read the `state` on the returned connection and branch:

- **`READY`** — done. Move to Step 2.
- **`BUILDING`** — Membrane's builder agent is working. Wait:

  ```bash
  membrane connection get <id> --wait --json
  ```

  `--wait` long-polls (up to `--timeout` seconds, default 30).
- **`CLIENT_ACTION_REQUIRED`** — the user or agent must do something to finish setup. The `clientAction` object describes what:
  - `clientAction.type` — `"connect"` (auth flow) or `"provide-input"` (extra fields needed).
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Credit Repair Cloud's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
  - `clientAction.uiUrl` (optional) — a Membrane-hosted page where the user completes the action manually. Show this only when `agentInstructions` tells you to, or when no `agentInstructions` are present.
  - `clientAction.description` — human-readable summary.

  When the action requires writing data back to the connection (e.g. captured OAuth credentials, custom params):

  ```bash
  membrane connection patch <id> --data '<json>'
  ```

  After the user completes their step, poll `connection get <id> --wait --json` until `state` changes.

  **Important:** if the connection you got back is an existing one in `CLIENT_ACTION_REQUIRED` (i.e. previously disconnected), reconnect it instead of creating a new one:

  ```bash
  membrane connect --connectionId <id>
  ```

  Creating a fresh connection in this case leaves orphaned records and breaks anything that referenced the old `connectionKey`.

- **`CONFIGURATION_ERROR`** / **`SETUP_FAILED`** — surface the `error` field to the user. These are terminal — don't retry blindly.

To give the connection a stable, human-readable key for later lookup:

```bash
membrane connection patch <id> --data '{"connectionKey":"credit-repair-cloud"}'
```

For multi-account setups (e.g. two separate Credit Repair Cloud accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey credit-repair-cloud \
  --connectionKey credit-repair-cloud-secondary --allowMultipleConnections
```

## Step 2 — Use the connection

The fastest path to a real response is `act` with an inline dispatch. **No "create action → wait → run" ceremony required.**

`act` accepts exactly one of three dispatch styles:

| Dispatch | When to use |
|---|---|
| `--api '<json>'` | **First call after a fresh connection, and any one-off HTTP request.** Membrane handles auth + base URL. |
| `--key <key>` | You've previously saved this call as a reusable action (see Step 3). |
| `--id <id>` | Same as `--key` but by id. |

### 2a. Inline `api` (recommended for the first call after a fresh connection, and for one-off calls)

**Use this as the default for the very first call against a new Credit Repair Cloud connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey credit-repair-cloud \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Credit Repair Cloud base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Credit Repair Cloud API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey credit-repair-cloud \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey credit-repair-cloud --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
| --- | --- | --- |
| Delete Affiliate | delete-affiliate | Delete an affiliate record from Credit Repair Cloud |
| Update Affiliate | update-affiliate | Update an existing affiliate record in Credit Repair Cloud |
| Get Affiliate | get-affiliate | Retrieve an affiliate record by ID from Credit Repair Cloud |
| Create Affiliate | create-affiliate | Create a new affiliate record in Credit Repair Cloud |
| Delete Lead/Client | delete-lead-client | Delete a lead or client record from Credit Repair Cloud |
| Update Lead/Client | update-lead-client | Update an existing lead or client record in Credit Repair Cloud |
| Get Lead/Client | get-lead-client | Retrieve a lead or client record by ID from Credit Repair Cloud |
| Create Lead/Client | create-lead-client | Create a new lead or client record in Credit Repair Cloud |


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey credit-repair-cloud --json
```

The action returns in `state: BUILDING`. Wait for it:

```bash
membrane action get <id> --wait --json
```

**By explicit spec** — supply `type` + `config` directly. Common when lifting a tested inline `api` call into a saved action:

```bash
membrane action create \
  --key my-action-key \
  --type api-request-to-external-app \
  --config '{"request":{"method":"POST","path":"/path/to/endpoint"}}' \
  --integrationKey credit-repair-cloud --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Credit Repair Cloud connection

Credit Repair Cloud's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

```bash
membrane connection get <id-or-key> --json
```

If `state` is `CLIENT_ACTION_REQUIRED`, **reconnect the existing connection** (don't create a new one):

```bash
membrane connect --connectionId <id>
```

After re-auth, retry the original `act` call.

### Action failed

Every `act` response carries `actionRunId`, on success AND on error. Pull the full log:

```bash
membrane action-run-log get <actionRunId> --details --json
```

You get the mapped input, output, errors, plus the raw HTTP exchange with Credit Repair Cloud.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Credit Repair Cloud API keys or tokens. Connections handle auth lifecycle server-side.
