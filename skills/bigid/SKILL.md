---
name: bigid
description: |
  BigID integration. Manage data, records, and automate workflows. Use when the user wants to interact with BigID data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# BigID
BigID is a data intelligence platform that helps companies discover, manage, protect, and get more value from their sensitive and personal data. It's used by privacy, security, and data governance professionals to comply with data privacy regulations and improve data security posture.

Official docs: https://docs.bigid.com/
## BigID Overview

- **Entity**
  - **Entity Data**
- **Task**
- **Consent**
- **Data Source**
- **Data Policy**
- **Data Retention Policy**
- **Data Subject Right**
- **Report**
- **Integration**
- **Workflow**
- **Catalog**
- **Classification**
- **Configuration**
- **Dashboard**
- **License**
- **User**
- **Group**
- **Role**
- **System**
- **Event**
- **Alert**
- **Notification**
- **Setting**
- **Template**
- **Connector**
- **Scan**
- **Remediation**
- **Privacy Impact Assessment**
- **Vendor**
- **Activity**
- **Application**
- **Dataset**
- **Data Field**
- **Data Attribute**
- **Data Element**
- **Data Inventory**
- **Data Map**
- **Data Flow**
- **Data Processing Agreement**
- **Third Party**
- **Business Purpose**
- **Legal Basis**
- **Location**
- **Process**
- **Project**
- **Request**
- **Risk**
- **Schedule**
- **Search**
- **Sensitive Data**
- **Subscription**
- **Tag**
- **Taxonomy**
- **Terms of Service**
- **Vulnerability**
- **Watchlist**
- **Assessment**
- **Certification**
- **Challenge**
- **Control**
- **Evidence**
- **Finding**
- **Framework**
- **Guideline**
- **Objective**
- **Policy**
- **Procedure**
- **Regulation**
- **Requirement**
- **Standard**
- **Test**
- **Training**
- **Audit**
- **Breach**
- **Complaint**
- **Investigation**
- **Review**
- **Survey**
- **Violation**
- **Activity Log**
- **Change Request**
- **Data Breach Notification**
- **Incident**
- **Privacy Assessment**
- **Security Assessment**
- **Security Incident**
- **Vulnerability Assessment**
- **Access Request**
- **Consent Request**
- **Data Subject Access Request**
- **Deletion Request**
- **Opt Out Request**
- **Portability Request**
- **Rectification Request**
- **Restriction Request**
- **Review Request**
- **Right to be Forgotten Request**
- **Stop Processing Request**
- **Withdrawal of Consent Request**
- **Data Source Connection**
- **Data Source Scan**
- **Data Source Profile**
- **Data Source Sample**
- **Data Source Schema**
- **Data Source Table**
- **Data Source Column**
- **Data Source File**
- **Data Source Folder**
- **Data Source Object**
- **Data Source Relationship**
- **Data Source Transformation**
- **Data Source Validation**
- **Data Source Anomaly**
- **Data Source Metric**
- **Data Source Report**
- **Data Source Alert**
- **Data Source Notification**
- **Data Source Setting**
- **Data Source Template**
- **Data Source Connector**
- **Data Source Task**
- **Data Source Workflow**
- **Data Source Catalog**
- **Data Source Classification**
- **Data Source Configuration**
- **Data Source Dashboard**
- **Data Source License**
- **Data Source User**
- **Data Source Group**
- **Data Source Role**
- **Data Source System**
- **Data Source Event**
- **Data Source Activity**
- **Data Source Application**
- **Data Source Dataset**
- **Data Source Data Field**
- **Data Source Data Attribute**
- **Data Source Data Element**
- **Data Source Data Inventory**
- **Data Source Data Map**
- **Data Source Data Flow**
- **Data Source Data Processing Agreement**
- **Data Source Third Party**
- **Data Source Business Purpose**
- **Data Source Legal Basis**
- **Data Source Location**
- **Data Source Process**
- **Data Source Project**
- **Data Source Request**
- **Data Source Risk**
- **Data Source Schedule**
- **Data Source Search**
- **Data Source Sensitive Data**
- **Data Source Subscription**
- **Data Source Tag**
- **Data Source Taxonomy**
- **Data Source Terms of Service**
- **Data Source Vulnerability**
- **Data Source Watchlist**
- **Data Source Assessment**
- **Data Source Certification**
- **Data Source Challenge**
- **Data Source Control**
- **Data Source Evidence**
- **Data Source Finding**
- **Data Source Framework**
- **Data Source Guideline**
- **Data Source Objective**
- **Data Source Policy**
- **Data Source Procedure**
- **Data Source Regulation**
- **Data Source Requirement**
- **Data Source Standard**
- **Data Source Test**
- **Data Source Training**
- **Data Source Audit**
- **Data Source Breach**
- **Data Source Complaint**
- **Data Source Investigation**
- **Data Source Review**
- **Data Source Survey**
- **Data Source Violation**
- **Data Source Activity Log**
- **Data Source Change Request**
- **Data Source Data Breach Notification**
- **Data Source Incident**
- **Data Source Privacy Assessment**
- **Data Source Security Assessment**
- **Data Source Security Incident**
- **Data Source Vulnerability Assessment**
- **Data Source Access Request**
- **Data Source Consent Request**
- **Data Source Data Subject Access Request**
- **Data Source Deletion Request**
- **Data Source Opt Out Request**
- **Data Source Portability Request**
- **Data Source Rectification Request**
- **Data Source Restriction Request**
- **Data Source Review Request**
- **Data Source Right to be Forgotten Request**
- **Data Source Stop Processing Request**
- **Data Source Withdrawal of Consent Request**

Use action names and parameters as needed.
## Working with BigID

This skill uses the [Membrane](https://getmembrane.com) CLI to interact with BigID. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit BigID's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call BigID on your behalf.

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

## Step 1 — Get a connection to BigID

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://bigid.com" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive BigID's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"bigid"}'
```

For multi-account setups (e.g. two separate BigID accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey bigid \
  --connectionKey bigid-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new BigID connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey bigid \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The BigID base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the BigID API docs (or the Popular actions table below). Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Inline `code` (when you need logic, not just an HTTP call)

```bash
membrane act --connectionKey bigid \
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
membrane act --key <action-key> --connectionKey bigid \
  --input '<json>' --json
```

### 2d. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey bigid --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

Use `npx @membranehq/cli@latest action list --intent=QUERY --connectionId=CONNECTION_ID --json` to discover available actions.

### Running an action from the table above

Use the action's `key` (column above) with `act --key`:

```bash
membrane act --key <action-key> --connectionKey bigid --input '<json>' --json
```

Provide `--input` matching the action's `inputSchema` (read it via `action get <key> --json`). The result is in the `output` field of the response.

## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey bigid --json
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
  --integrationKey bigid --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected BigID connection

BigID's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with BigID.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for BigID API keys or tokens. Connections handle auth lifecycle server-side.
