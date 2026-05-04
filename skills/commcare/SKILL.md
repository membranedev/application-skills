---
name: commcare
description: |
  CommCare integration. Manage data, records, and automate workflows. Use when the user wants to interact with CommCare data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# CommCare
CommCare is a mobile data collection and service delivery platform. It's used by community health workers and field staff, primarily in low-resource settings, to track and support clients.

Official docs: https://commcare-hq.readthedocs.io/
## CommCare Overview

- **Case**
  - **Case Property**
- **Form**
- **Application**
- **User**
- **Group**
- **Location**
- **Mobile Worker**
- **Web User**
- **Application Build**
- **CommCare Project**
- **Subscription**
- **API Usage**
- **Bulk Export**
- **Data Export**
- **Multimedia**
- **Report**
- **Schedule**
- **SMS Message**
- **Project Setting**
- **Download**
- **Project Transfer**
- **Domain**
- **App Release**
- **Linked Project**
- **Data File**
- **User Role**
- **Location Type**
- **Custom Data Field**
- **Case Search**
- **Fixture**
- **Call Center**
- **OTP Authenticator**
- **Audit Event**
- **Report Configuration**
- **User History**
- **Form History**
- **Case History**
- **Application History**
- **SMS History**
- **Bulk Migration**
- **Short Code**
- **Keyword**
- **Data Dictionary**
- **Remote App User**
- **CommCare Supply Point**
- **Stock Transaction**
- **Ledger**
- **Supply Point Group**
- **Program**
- **Product**
- **Stock**
- **Workflow**
- **Integration**
- **Service**
- **Task**
- **Lookup Table**
- **Lookup Table Item**
- **User Invitation**
- **Role**
- **Application Version**
- **Menu**
- **Module**
- **Form Image**
- **Case Export**
- **Form Export**
- **SMS Opt-Out**
- **App User**
- **User Group**
- **Form Question**
- **Case Index**
- **Data Source**
- **Data Source Column**
- **Dashboard**
- **Dashboard Item**
- **Report Email**
- **User Action History**
- **Case Attachment**
- **Form Attachment**
- **Data Export Group**
- **Data Export Column**
- **Case Rule**
- **Form Rule**
- **User Role Assignment**
- **Location Assignment**
- **Mobile Worker Assignment**
- **Web User Assignment**
- **Group Assignment**
- **Data Export Schedule**
- **Case Search Property**
- **Form Search Property**
- **User Search Property**
- **Location Search Property**
- **Mobile Worker Search Property**
- **Web User Search Property**
- **Group Search Property**
- **Data Export Search Property**
- **Case Rule Schedule**
- **Form Rule Schedule**
- **User Role Assignment Schedule**
- **Location Assignment Schedule**
- **Mobile Worker Assignment Schedule**
- **Web User Assignment Schedule**
- **Group Assignment Schedule**
- **Data Export Schedule Schedule**
- **Case Search Schedule**
- **Form Search Schedule**
- **User Search Schedule**
- **Location Search Schedule**
- **Mobile Worker Search Schedule**
- **Web User Search Schedule**
- **Group Search Schedule**
- **Data Export Search Schedule**
- **Case Rule Search**
- **Form Rule Search**
- **User Role Assignment Search**
- **Location Assignment Search**
- **Mobile Worker Assignment Search**
- **Web User Assignment Search**
- **Group Assignment Search**
- **Data Export Schedule Search**
- **Case Search Search**
- **Form Search Search**
- **User Search Search**
- **Location Search Search**
- **Mobile Worker Search Search**
- **Web User Search Search**
- **Group Search Search**
- **Data Export Search Search**
- **Case Rule Export**
- **Form Rule Export**
- **User Role Assignment Export**
- **Location Assignment Export**
- **Mobile Worker Assignment Export**
- **Web User Assignment Export**
- **Group Assignment Export**
- **Data Export Schedule Export**
- **Case Search Export**
- **Form Search Export**
- **User Search Export**
- **Location Search Export**
- **Mobile Worker Search Export**
- **Web User Search Export**
- **Group Search Export**
- **Data Export Search Export**
- **Case Rule Import**
- **Form Rule Import**
- **User Role Assignment Import**
- **Location Assignment Import**
- **Mobile Worker Assignment Import**
- **Web User Assignment Import**
- **Group Assignment Import**
- **Data Export Schedule Import**
- **Case Search Import**
- **Form Search Import**
- **User Search Import**
- **Location Search Import**
- **Mobile Worker Search Import**
- **Web User Search Import**
- **Group Search Import**
- **Data Export Search Import**

Use action names and parameters as needed.
## Working with CommCare

This skill uses the Membrane CLI to interact with CommCare. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit CommCare's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call CommCare on your behalf.

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

## Step 1 — Get a connection to CommCare

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.dimagi.com/commcare/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive CommCare's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"commcare"}'
```

For multi-account setups (e.g. two separate CommCare accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey commcare \
  --connectionKey commcare-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new CommCare connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey commcare \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The CommCare base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the CommCare API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey commcare \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey commcare --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
| --- | --- | --- |
| List Location Types | list-location-types | List location types defined in a CommCare domain |
| List Fixtures | list-fixtures | List lookup tables (fixtures) in a CommCare domain |
| Get Group | get-group | Retrieve a specific group by its ID |
| List Groups | list-groups | List user groups in a CommCare domain |
| Get Location | get-location | Retrieve a specific location by its ID |
| List Locations | list-locations | List locations in a CommCare domain |
| Get Application | get-application | Retrieve a specific application by its ID |
| List Applications | list-applications | List applications in a CommCare domain |
| List Web Users | list-web-users | List web users (admin/project users) in a CommCare domain |
| List Mobile Workers | list-mobile-workers | List mobile workers (field users) in a CommCare domain |
| Get Form | get-form | Retrieve a specific form submission by its ID |
| List Forms | list-forms | List form submissions in a CommCare domain with optional filtering |
| Update Case | update-case | Update an existing case in CommCare using the Case Data API v2 |
| Create Case | create-case | Create a new case in CommCare using the Case Data API v2 |
| Get Case | get-case | Retrieve a specific case by its ID |
| List Cases | list-cases | List cases in a CommCare domain with optional filtering by type, owner, status, and date ranges |


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey commcare --json
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
  --integrationKey commcare --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected CommCare connection

CommCare's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with CommCare.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for CommCare API keys or tokens. Connections handle auth lifecycle server-side.
