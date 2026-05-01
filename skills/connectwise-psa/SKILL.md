---
name: connectwise-psa
description: |
  Connectwise PSA integration. Manage data, records, and automate workflows. Use when the user wants to interact with Connectwise PSA data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Connectwise PSA
Connectwise PSA is a professional services automation platform. It's used by IT solution providers and MSPs to manage their business operations. This includes things like service desk, project management, and billing.

Official docs: https://developer.connectwise.com/
## Connectwise PSA Overview

- **Agreement**
  - **Addition**
- **Company**
  - **Contact**
- **Configuration**
- **Opportunity**
- **Project**
  - **Ticket**
- **RMA**
- **Sales Order**
- **Service Ticket**
- **System Ticket**
- **Time Entry**
## Working with Connectwise PSA

This skill uses the [Membrane](https://getmembrane.com) CLI to interact with Connectwise PSA. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Connectwise PSA's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Connectwise PSA on your behalf.

### Install the CLI

```bash
npm install -g @membranehq/cli@latest
```

### Authenticate

```bash
membrane login --tenant --clientName=<agentType>
```

`--tenant` gets a tenant-scoped token (workspace + customer) so you don't need to pass `--workspaceKey` and `--tenantKey` on every subsequent command.

This will either open a browser for authentication or print an authorization URL to the console, depending on whether interactive mode is available.

**Headless environments:** The command will print an authorization URL. Ask the user to open it in a browser. When they see a code after completing login, finish with:

```bash
membrane login complete <code>
```

**Agent types** (passed to `--clientName`): `claude`, `openclaw`, `codex`, `warp`, `windsurf`, etc. Used to adjust tooling for your harness.

Add `--json` to any command for machine-readable JSON output.

## Step 1 — Get a connection to Connectwise PSA

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.connectwise.com/platform/business-management/psa" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Connectwise PSA's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"connectwise-psa"}'
```

For multi-account setups (e.g. two separate Connectwise PSA accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey connectwise-psa \
  --connectionKey connectwise-psa-secondary --allowMultipleConnections
```

## Step 2 — Use the connection

The fastest path to a real response is `act` with an inline dispatch. **No "create action → wait → run" ceremony required.**

`act` accepts exactly one of four dispatch styles:

| Dispatch | When to use |
|---|---|
| `--api '<json>'` | **First call after a fresh connection, and any one-off HTTP request.** Membrane handles auth + base URL. |
| `--code '<js>'` | You need a small piece of logic (loop, transform, multi-step). |
| `--key <key>` | You've previously saved this call as a reusable action (see Step 3). |
| `--id <id>` | Same as `--key` but by id. |

### 2a. Inline `api` (recommended for the first call after a fresh connection, and for one-off calls)

**Use this as the default for the very first call against a new Connectwise PSA connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey connectwise-psa \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Connectwise PSA base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Connectwise PSA API docs (or the Popular actions table below). Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Inline `code` (when you need logic, not just an HTTP call)

```bash
membrane act --connectionKey connectwise-psa \
  --code 'module.exports = async ({ input, membrane }) => {
    // Use membrane.api({ method, path, ... }) for authenticated calls
    const res = await membrane.api({ method: "GET", path: "/path/to/endpoint" })
    return res
  }' \
  --input '{}' --json
```

The function receives `{ input, membrane, connection, integration }`. Use `membrane.api(...)` inside it for authenticated calls. Whatever you return becomes the response `output`.

### 2c. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey connectwise-psa \
  --input '<json>' --json
```

### 2d. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey connectwise-psa --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
|---|---|---|
| List Tickets | list-tickets | Retrieve a list of service tickets with optional filtering, sorting, and pagination |
| List Companies | list-companies | Retrieve a list of companies with optional filtering, sorting, and pagination |
| List Contacts | list-contacts | Retrieve a list of contacts with optional filtering, sorting, and pagination |
| List Projects | list-projects | Retrieve a list of projects with optional filtering, sorting, and pagination |
| List Opportunities | list-opportunities | Retrieve a list of sales opportunities with optional filtering, sorting, and pagination |
| List Time Entries | list-time-entries | Retrieve a list of time entries with optional filtering, sorting, and pagination |
| List Products | list-products | List products with optional filtering |
| List Expense Entries | list-expense-entries | List expense entries with optional filtering |
| List Configurations | list-configurations | List configuration items (assets) with optional filtering and pagination |
| Get Ticket | get-ticket | Retrieve a single service ticket by ID |
| Get Company | get-company | Retrieve a single company by ID |
| Get Contact | get-contact | Retrieve a single contact by ID |
| Get Project | get-project | Retrieve a single project by ID |
| Get Opportunity | get-opportunity | Retrieve a single opportunity by ID |
| Get Time Entry | get-time-entry | Get a single time entry by ID |
| Get Product | get-product | Get a product by ID |
| Get Expense Entry | get-expense-entry | Get an expense entry by ID |
| Get Configuration | get-configuration | Get a single configuration item (asset) by ID |
| Create Ticket | create-ticket | Create a new service ticket |
| Update Ticket | update-ticket | Update an existing service ticket using JSON Patch operations |

### Running an action from the table above

Use the action's `key` (column above) with `act --key`:

```bash
membrane act --key <action-key> --connectionKey connectwise-psa --input '<json>' --json
```

Provide `--input` matching the action's `inputSchema` (read it via `action get <key> --json`). The result is in the `output` field of the response.

## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey connectwise-psa --json
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
  --integrationKey connectwise-psa --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Connectwise PSA connection

Connectwise PSA's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Connectwise PSA.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Connectwise PSA API keys or tokens. Connections handle auth lifecycle server-side.
