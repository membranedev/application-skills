---
name: accelo
description: |
  Accelo integration. Manage Organizations, Leads, Pipelines, Users, Goals, Filters. Use when the user wants to interact with Accelo data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: "CRM, Project Management"
---

# Accelo
Accelo is a business automation platform designed for service-based businesses. It helps manage clients, projects, sales, and billing in one integrated system. Professional services teams like IT, marketing, and consulting firms use it to streamline operations and improve profitability.

Official docs: https://developers.accelo.com/
## Accelo Overview

- **Company**
- **Contact**
- **Task**
- **Project**
- **Sale**
- **Invoice**
- **Ticket**
- **Timesheet**
- **Object**
  - **Attachment**
- **Activity**
- **Staff**
- **Product**
- **Purchase**
- **Subscription**
- **Leave Request**
- **Bill**
- **Credit**
- **Queue**
- **Custom Field**
- **Email Template**
- **Recurring Invoice**
- **Material**
- **Retainer**
- **Order**
- **Contract**
- **Budget**
- **Delivery**
- **Asset**
- **Build**
- **Production Run**
- **BOM**
- **Transfer**
- **Pick**
- **Pack**
- **Ship**
- **Receive**
- **Count**
- **Adjustment**
- **Work Order**
- **RMA**
- **Opportunity**
- **Pay Run**
- **Payment**
- **Expense**
- **Pay Item**
- **Training**
- **Group**
- **Campaign**
- **List**
- **Landing Page**
- **Form**
- **Automation**
- **Knowledge Base**
- **Article**
- **Forum**
- **Topic**
- **Reply**
- **Survey**
- **Question**
- **Response**
- **Location**
- **Equipment**
- **Booking**
- **Checklist**
- **Template**
- **License**
- **Integration**
- **User**
- **Role**
- **Permission**
- **Profile**
- **Setting**
- **Notification**
- **Report**
- **Dashboard**
- **Widget**
- **Filter**
- **View**
- **Layout**
- **Theme**
- **Language**
- **Currency**
- **Tax**
- **Term**
- **Unit**
- **Category**
- **Tag**
- **Status**
- **Priority**
- **Type**
- **Reason**
- **Source**
- **Stage**
- **Resolution**
- **SLA**
- **Workflow**
- **Trigger**
- **Action**
- **Condition**
- **Event**
- **Schedule**
- **Log**
- **Error**
- **Backup**
- **Restore**
- **Import**
- **Export**
- **Merge**
- **Clean**
- **Archive**
- **Delete**
- **Test**
- **Deploy**
- **Upgrade**
- **Monitor**
- **Alert**
- **Incident**
- **Problem**
- **Change**
- **Release**
- **Request**
- **Service**
- **Configuration**
- **Inventory**
- **Capacity**
- **Demand**
- **Forecast**
- **Plan**
- **Risk**
- **Issue**
- **Decision**
- **Goal**
- **Strategy**
- **Policy**
- **Procedure**
- **Guideline**
- **Standard**
- **Framework**
- **Model**
- **Simulation**
- **Analysis**
- **Report**
- **Dashboard**
- **Widget**
- **Filter**
- **View**
- **Layout**
- **Theme**
- **Language**
- **Currency**
- **Tax**
- **Term**
- **Unit**
- **Category**
- **Tag**
- **Status**
- **Priority**
- **Type**
- **Reason**
- **Source**
- **Stage**
- **Resolution**
- **SLA**
- **Workflow**
- **Trigger**
- **Action**
- **Condition**
- **Event**
- **Schedule**
- **Log**
- **Error**
- **Backup**
- **Restore**
- **Import**
- **Export**
- **Merge**
- **Clean**
- **Archive**
- **Delete**
- **Test**
- **Deploy**
- **Upgrade**
- **Monitor**
- **Alert**
- **Incident**
- **Problem**
- **Change**
- **Release**
- **Request**
- **Service**
- **Configuration**
- **Inventory**
- **Capacity**
- **Demand**
- **Forecast**
- **Plan**
- **Risk**
- **Issue**
- **Decision**
- **Goal**
- **Strategy**
- **Policy**
- **Procedure**
- **Guideline**
- **Standard**
- **Framework**
- **Model**
- **Simulation**
- **Analysis**
## Working with Accelo

This skill uses the Membrane CLI to interact with Accelo. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Accelo's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Accelo on your behalf.

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

## Step 1 — Get a connection to Accelo

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.accelo.com/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Accelo's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"accelo"}'
```

For multi-account setups (e.g. two separate Accelo accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey accelo \
  --connectionKey accelo-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Accelo connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey accelo \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Accelo base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Accelo API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey accelo \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey accelo --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
|---|---|---|
| List Jobs | list-jobs | List all jobs/projects with optional filtering and pagination |
| List Issues | list-issues | List all issues/tickets with optional filtering and pagination |
| List Tasks | list-tasks | List all tasks with optional filtering and pagination |
| List Activities | list-activities | List all activities with optional filtering and pagination |
| List Contacts | list-contacts | List all contacts with optional filtering and pagination |
| List Companies | list-companies | List all companies with optional filtering and pagination |
| List Prospects | list-prospects | List all prospects/sales opportunities with optional filtering and pagination |
| Get Job | get-job | Retrieve a single job/project by its ID |
| Get Issue | get-issue | Retrieve a single issue/ticket by its ID |
| Get Task | get-task | Retrieve a single task by its ID |
| Get Activity | get-activity | Retrieve a single activity by its ID |
| Get Contact | get-contact | Retrieve a single contact by its ID |
| Get Company | get-company | Retrieve a single company by its ID |
| Get Prospect | get-prospect | Retrieve a single prospect/sales opportunity by its ID |
| Create Job | create-job | Create a new job/project in Accelo |
| Create Issue | create-issue | Create a new issue/ticket in Accelo |
| Create Task | create-task | Create a new task in Accelo |
| Create Activity | create-activity | Create a new activity in Accelo (e.g., notes, emails, meetings) |
| Create Contact | create-contact | Create a new contact in Accelo. |
| Create Company | create-company | Create a new company in Accelo |


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey accelo --json
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
  --integrationKey accelo --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Accelo connection

Accelo's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Accelo.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Accelo API keys or tokens. Connections handle auth lifecycle server-side.
