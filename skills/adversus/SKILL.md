---
name: adversus
description: |
  Adversus integration. Manage data, records, and automate workflows. Use when the user wants to interact with Adversus data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Adversus
Adversus is a competitive intelligence platform. It helps businesses monitor and analyze their competitors' strategies, marketing efforts, and overall market presence. This allows product managers and marketing teams to make data-driven decisions.

Official docs: https://www.adversus.io/api-documentation
## Adversus Overview

- **Case**
  - **Case Note**
- **Contact**
- **Task**
- **User**
- **Template**
- **Document**
- **Billing Rate**
- **Expense**
- **Invoice**
- **Payment**
- **Time Entry**
- **Product and Service**
- **Trust Request**
- **Email**
- **Phone Number**
- **Address**
- **Firm Setting**
- **Integration**
- **Role**
- **Permission**
- **Note**
- **Journal Entry**
- **Account**
- **Tax Rate**
- **Vendor**
- **Client Request**
- **Lead**
- **Referral**
- **Activity**
- **Marketing Campaign**
- **Form**
- **Form Submission**
- **Automation**
- **Tag**
- **Checklist**
- **Checklist Template**
- **Court**
- **Judge**
- **Opposing Party**
- **Settlement**
- **Medical Record**
- **Insurance Policy**
- **Property**
- **Vehicle**
- **Will**
- **Trust**
- **Power of Attorney**
- **Healthcare Directive**
- **Contract**
- **Intellectual Property**
- **Financial Account**
- **Safe Deposit Box**
- **Digital Asset**
- **Pet**
- **Personal Property**
- **Life Insurance Policy**
- **Retirement Account**
- **Document Template**
- **Email Template**
- **SMS Template**
- **Report**
- **Dashboard**
- **Workflow**
- **Workflow Template**
- **Stage**
- **Stage Template**
- **Custom Field**
- **Custom Field Template**
- **Relationship**
- **Relationship Type**
- **Matter Type**
- **Task Template**
- **Event**
- **Event Template**
- **User Group**
- **Goal**
- **Key Result**
- **Scorecard**
- **Survey**
- **Survey Template**
- **Question**
- **Question Template**
- **Answer**
- **Answer Template**
- **Clause**
- **Clause Library**
- **Fee Schedule**
- **Fee**
- **Tax**
- **Discount**
- **Credit**
- **Escrow Account**
- **Escrow Transaction**
- **User Subscription**
- **Plan**
- **Add-on**
- **Coupon**
- **Integration Setting**
- **Notification**
- **Audit Log**
- **Data Import**
- **Data Export**
- **Firm User**
- **Firm**
- **Office**
- **Department**
- **Practice Area**
- **Source**
- **Language**
- **Country**
- **State**
- **City**
- **Zip Code**
- **Area Code**
- **Phone Type**
- **Email Type**
- **Address Type**
- **Note Type**
- **Task Status**
- **Task Priority**
- **Event Type**
- **Relationship Status**
- **Payment Type**
- **Invoice Status**
- **Case Status**
- **Lead Status**
- **Referral Status**
- **Trust Request Status**
- **Client Request Status**
- **Marketing Campaign Status**
- **Form Status**
- **Automation Status**
- **Checklist Status**
- **Workflow Status**
- **Stage Status**
- **Goal Status**
- **Key Result Status**
- **Survey Status**
- **Question Type**
- **Answer Type**
- **Custom Field Type**
- **Document Category**
- **Email Category**
- **SMS Category**
- **Report Category**
- **Dashboard Category**
- **Workflow Category**
- **Stage Category**
- **Task Category**
- **Event Category**
- **Goal Category**
- **Key Result Category**
- **Survey Category**
- **Question Category**
- **Answer Category**
- **Clause Category**
- **Fee Category**
- **Tax Category**
- **Discount Category**
- **Credit Category**
- **Escrow Account Category**
- **User Subscription Category**
- **Plan Category**
- **Add-on Category**
- **Coupon Category**
- **Integration Setting Category**
- **Notification Category**
- **Activity Type**
- **Activity Category**
- **Product and Service Category**
- **Template Category**
- **Document Template Category**
- **Email Template Category**
- **SMS Template Category**
- **Report Template Category**
- **Dashboard Template Category**
- **Workflow Template Category**
- **Stage Template Category**
- **Task Template Category**
- **Event Template Category**
- **Goal Template Category**
- **Key Result Template Category**
- **Survey Template Category**
- **Question Template Category**
- **Answer Template Category**
- **Clause Template Category**
- **Fee Template Category**
- **Tax Template Category**
- **Discount Template Category**
- **Credit Template Category**
- **Escrow Account Template Category**
- **User Subscription Template Category**
- **Plan Template Category**
- **Add-on Template Category**
- **Coupon Template Category**
- **Integration Setting Template Category**
- **Notification Template Category**

Use action names and parameters as needed.
## Working with Adversus

This skill uses the Membrane CLI to interact with Adversus. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Adversus's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Adversus on your behalf.

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

## Step 1 — Get a connection to Adversus

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://adversus.io" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Adversus's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"adversus"}'
```

For multi-account setups (e.g. two separate Adversus accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey adversus \
  --connectionKey adversus-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Adversus connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey adversus \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Adversus base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Adversus API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey adversus \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey adversus --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
| --- | --- | --- |
| Add Activity to Lead | add-activity-to-lead | Add an activity to a lead in Adversus |
| Add Note to Lead | add-note-to-lead | Add a note to a lead in Adversus |
| Assign Lead to Campaign | assign-lead-to-campaign | Assign a lead to a campaign in Adversus |
| Change Lead Status | change-lead-status | Change the status of a lead in Adversus |
| Get Lead | get-lead | Retrieve a lead by ID from Adversus |
| Update Lead | update-lead | Update an existing lead in Adversus |
| Create Lead | create-lead | Create a new lead in Adversus |
| List Campaigns | list-campaigns | Retrieve a list of campaigns for the authenticated user |
| Get Organization | get-organization | Retrieve organization details for the authenticated user |


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey adversus --json
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
  --integrationKey adversus --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Adversus connection

Adversus's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Adversus.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Adversus API keys or tokens. Connections handle auth lifecycle server-side.
