---
name: codereadr
description: |
  CodeREADr integration. Manage data, records, and automate workflows. Use when the user wants to interact with CodeREADr data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# CodeREADr
CodeREADr is a barcode and QR code scanning software development kit (SDK). It's used by developers and businesses to add scanning functionality to mobile apps for inventory management, asset tracking, and other data capture applications.

Official docs: https://www.codereadr.com/developers/
## CodeREADr Overview

- **Barcode**
  - **Scan Session**
- **Scan Data**
- **Template**
- **User**
- **License**
- **Reader**
- **Group**
- **Project**
- **Scan Destination**
- **Scan Source**
- **Scan Setting**
- **Workflow**
- **Integration**
- **Label**
- **Location**
- **Mobile App**
- **Access Control List**
- **Data Verification Rule**
- **Data Transformation Rule**
- **Notification Rule**
- **Report**
- **Subscription**
- **Payment Method**
- **Invoice**
- **Audit Log**
- **API Key**
- **Custom Field**
- **Picklist**
- **Role**
- **Permission**
- **Event**
- **Batch**
- **Shipment**
- **Order**
- **Product**
- **Asset**
- **Contact**
- **Account**
- **Case**
- **Opportunity**
- **Lead**
- **Campaign**
- **Task**
- **Event**
- **Note**
- **Attachment**
- **Contract**
- **Quote**
- **Price Book**
- **Territory**
- **Goal**
- **Dashboard**
- **Chatter Post**
- **Chatter Group**
- **Content**
- **Article**
- **Forum**
- **Topic**
- **Reply**
- **Vote**
- **Comment**
- **Review**
- **Rating**
- **Bookmark**
- **Tag**
- **Category**
- **Channel**
- **Playlist**
- **Video**
- **Audio**
- **Image**
- **Document**
- **File**
- **Folder**
- **Link**
- **Email**
- **SMS**
- **Push Notification**
- **Calendar Event**
- **Task**
- **Survey**
- **Form**
- **Signature**
- **Drawing**
- **Location**
- **Geofence**
- **Beacon**
- **Sensor**
- **Alert**
- **Incident**
- **Maintenance**
- **Inspection**
- **Checklist**
- **Timer**
- **Counter**
- **Gauge**
- **Map**
- **Route**
- **Area**
- **Volume**
- **Weight**
- **Temperature**
- **Pressure**
- **Humidity**
- **Speed**
- **Distance**
- **Duration**
- **Frequency**
- **Amount**
- **Quantity**
- **Status**
- **Priority**
- **Severity**
- **Risk**
- **Score**
- **Probability**
- **Impact**
- **Effort**
- **Progress**
- **Trend**
- **Forecast**
- **Variance**
- **Anomaly**
- **Outlier**
- **Correlation**
- **Regression**
- **Classification**
- **Clustering**
- **Prediction**
- **Recommendation**
- **Sentiment**
- **Emotion**
- **Intent**
- **Context**
- **Insight**
- **Pattern**
- **Trend**
- **Anomaly**
- **Outlier**
- **Correlation**
- **Regression**
- **Classification**
- **Clustering**
- **Prediction**
- **Recommendation**
- **Sentiment**
- **Emotion**
- **Intent**
- **Context**
- **Insight**
- **Pattern**

Use action names and parameters as needed.
## Working with CodeREADr

This skill uses the [Membrane](https://getmembrane.com) CLI to interact with CodeREADr. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit CodeREADr's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call CodeREADr on your behalf.

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

## Step 1 — Get a connection to CodeREADr

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.codereadr.com/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive CodeREADr's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"codereadr"}'
```

For multi-account setups (e.g. two separate CodeREADr accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey codereadr \
  --connectionKey codereadr-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new CodeREADr connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey codereadr \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The CodeREADr base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the CodeREADr API docs (or the Popular actions table below). Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Inline `code` (when you need logic, not just an HTTP call)

```bash
membrane act --connectionKey codereadr \
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
membrane act --key <action-key> --connectionKey codereadr \
  --input '<json>' --json
```

### 2d. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey codereadr --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
|---|---|---|
| List Scans | list-scans | Retrieve and search scans from CodeREADr with various filters |
| List Database Values | list-database-values | Retrieve or search for barcode values in a database |
| List Databases | list-databases | Retrieve a list of databases from CodeREADr |
| List Devices | list-devices | Retrieve a list of devices from CodeREADr |
| List Users | list-users | Retrieve a list of users from CodeREADr |
| List Services | list-services | Retrieve a list of services from CodeREADr |
| Create Database | create-database | Create a new database in CodeREADr |
| Create User | create-user | Create a new user in CodeREADr |
| Create Service | create-service | Create a new service in CodeREADr |
| Edit Database Value | edit-database-value | Edit a barcode value's response text and/or validity in a database |
| Edit Device | edit-device | Edit/rename a device in CodeREADr |
| Edit User | edit-user | Edit an existing user in CodeREADr |
| Edit Service | edit-service | Edit an existing service in CodeREADr |
| Delete Scans | delete-scans | Delete scans from CodeREADr |
| Delete Database Value | delete-database-value | Delete a barcode value from a database in CodeREADr |
| Delete Database | delete-database | Delete a database from CodeREADr |
| Delete User | delete-user | Delete a user from CodeREADr |
| Delete Service | delete-service | Delete a service from CodeREADr |
| Add Database Value | add-database-value | Add a barcode value to a database in CodeREADr |
| Rename Database | rename-database | Rename an existing database in CodeREADr |

### Running an action from the table above

Use the action's `key` (column above) with `act --key`:

```bash
membrane act --key <action-key> --connectionKey codereadr --input '<json>' --json
```

Provide `--input` matching the action's `inputSchema` (read it via `action get <key> --json`). The result is in the `output` field of the response.

## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey codereadr --json
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
  --integrationKey codereadr --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected CodeREADr connection

CodeREADr's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with CodeREADr.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for CodeREADr API keys or tokens. Connections handle auth lifecycle server-side.
