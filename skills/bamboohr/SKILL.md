---
name: bamboohr
description: |
  BambooHR integration. Manage hris data, records, and workflows. Use when the user wants to interact with BambooHR data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# BambooHR
BambooHR is an HRIS platform that helps small and medium-sized businesses manage employee data, payroll, benefits, and other HR functions. It's used by HR professionals and managers to streamline HR processes and improve employee experience.

Official docs: https://documentation.bamboohr.com/docs
## BambooHR Overview

- **Employee**
  - **Employee Directory**
- **Time Off**
- **Report**
- **Compensation**
- **Goal**
- **Performance**
- **Training Course**
- **Applicant**
- **Offer**
- **Task**
- **Checklist**
- **Custom Report**
- **Table**
- **List**
- **Dashboard**
- **Integration**
- **Approval**
- **File**
- **Email**
- **Note**
- **Audit Trail**
- **User**
- **Settings**
- **Alert**
- **Form**
- **Workflow**
- **Event**
- **Policy**
- **Document**
- **Update**
- **Change Log**
- **Comment**
- **History**
- **Log**
- **Subscription**
- **Role**
- **Group**
- **Access Level**
- **Permission**
- **Category**
- **Field**
- **Tab**
- **Section**
- **Item**
- **Request**
- **Assignment**
- **Activity**
- **Reminder**
- **Notification**
- **Survey**
- **Question**
- **Answer**
- **Signature**
- **Device**
- **Location**
- **Department**
- **Division**
- **Subsidiary**

Use action names and parameters as needed.
## Working with BambooHR

This skill uses the Membrane CLI to interact with BambooHR. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit BambooHR's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call BambooHR on your behalf.

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

## Step 1 — Get a connection to BambooHR

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://bamboohr.com" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive BambooHR's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"bamboohr"}'
```

For multi-account setups (e.g. two separate BambooHR accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey bamboohr \
  --connectionKey bamboohr-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new BambooHR connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey bamboohr \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The BambooHR base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the BambooHR API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey bamboohr \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey bamboohr --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
| --- | --- | --- |
| Get Time Off Policies | get-time-off-policies | Retrieves time off policies configured in the company |
| Get Employee Trainings | get-employee-trainings | Retrieves training records for an employee |
| Get Training Types | get-training-types | Retrieves the list of training types configured in BambooHR |
| Get Employee Dependents | get-employee-dependents | Retrieves employee dependents, optionally filtered by employee ID |
| Get Employee Table Rows | get-employee-table-rows | Retrieves tabular data rows for an employee (e.g., job history, compensation, emergency contacts) |
| Run Custom Report | run-custom-report | Runs a custom report with specified fields and filters |
| Get Job Applications | get-job-applications | Retrieves job applications from the applicant tracking system |
| Get Job Openings | get-job-openings | Retrieves job summaries/openings from the applicant tracking system |
| Get Fields | get-fields | Retrieves the list of available fields in BambooHR |
| Get Users | get-users | Retrieves the list of users (admin accounts) in BambooHR |
| Get Company Information | get-company-information | Retrieves company information from BambooHR |
| Get Time Off Types | get-time-off-types | Retrieves the list of time off types configured in the company |
| Get Who's Out | get-whos-out | Retrieves a list of employees who are out during a specified date range |
| Create Time Off Request | create-time-off-request | Creates a new time off request for an employee |
| Get Time Off Requests | get-time-off-requests | Retrieves time off requests with optional filtering by employee, date range, status, and type |
| Get Employee Directory | get-employee-directory | Retrieves a company directory of employees |
| Update Employee | update-employee | Updates an existing employee's information in BambooHR |
| Create Employee | create-employee | Creates a new employee in BambooHR |
| Get Employee | get-employee | Retrieves a single employee by their ID with specified fields |
| List Employees | list-employees | Retrieves a list of employees with optional filtering, sorting, and pagination |


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey bamboohr --json
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
  --integrationKey bamboohr --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected BambooHR connection

BambooHR's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with BambooHR.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for BambooHR API keys or tokens. Connections handle auth lifecycle server-side.
