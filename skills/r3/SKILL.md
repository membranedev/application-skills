---
name: r3
description: |
  R3 integration. Manage data, records, and automate workflows. Use when the user wants to interact with R3 data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# R3
R3 is a platform for building and deploying multi-party workflows. It's used by businesses, governments, and other organizations that need to collaborate on complex processes. Think of it as a blockchain-inspired system for automating agreements and data sharing.

Official docs: https://docs.corda.net/
## R3 Overview

- **Cases**
  - **Case Notes**
- **Contacts**
- **Tasks**
- **Expenses**
- **Time Entries**
- **Calendar Events**
- **Case Types**
- **Users**
- **Companies**
- **Tags**
- **Vendors**
- **Bank Accounts**
- **Invoice**
- **Payment**
- **Trust Request**
- **Ledger Activities**
- **Document**
- **Email**
- **Phone Log**
- **Message**
- **Referral**
- **Product**
- **Purchase Order**
- **Retainer Request**
- **Settlement**
- **Check**
- **Credit Card**
- **Reconciliation**
- **Rule**
- **Subscription**
- **Task List**
- **Tax Rate**
- **Time Off Request**
- **Time Off Policy**
- **Workflow**
- **Integration**
- **Matter Template**
- **Note Template**
- **Product Category**
- **Quickbooks Online**
- **Xero**
- **Lawpay**
- **Google Calendar**
- **Office 365**
- **Box**
- **Dropbox**
- **Google Drive**
- **OneDrive**
- **Sharefile**
- **Netdocuments**
- **iCloud Calendar**
- **Contact Group**
- **Document Category**
- **Expense Category**
- **Firm Setting**
- **Goal**
- **Invoice Theme**
- **Journal Entry**
- **Lexicata**
- **Notification**
- **Office 365 Contact**
- **Office 365 Email**
- **Office 365 Calendar Event**
- **Payment Method**
- **Permission**
- **Pipeline**
- **Report**
- **Role**
- **Salesforce**
- **Smart Advocate**
- **Task Template**
- **Trust Account**
- **Trust Transaction**
- **User Role**
- **Zapier**
- **Clock**
- **Credit Note**
- **Deposit**
- **General Retainer**
- **Operating Account**
- **Recurring Invoice**
- **Task Dependency**
- **Time Activity**
- **User Permission**
- **Client Portal**
- **Contact Type**
- **Credit Card Transaction**
- **Custom Field**
- **Data Import**
- **Email Template**
- **Firm User**
- **Form**
- **Google Contact**
- **Google Email**
- **Invoice Payment**
- **Matter User**
- **Plaid Connection**
- **Product Unit**
- **QuickBooks Online Vendor**
- **Recurring Expense**
- **Recurring Task**
- **Tax Payment**
- **Time Rounding Rule**
- **User Time Entry**
- **Xero Contact**
- **Xero Invoice**
- **Xero Vendor**
- **Billing Contact**
- **Case Custom Field**
- **Case Fee**
- **Case Task**
- **Check Template**
- **Client Request**
- **Contact Custom Field**
- **Contract Template**
- **Credit Card Charge**
- **Custom Report**
- **Deposit Transaction**
- **Document Assembly**
- **Expense Custom Field**
- **Firm Credit Card**
- **Google Group**
- **Invoice Custom Field**
- **Matter Custom Field**
- **Payment Custom Field**
- **Product Custom Field**
- **Task Custom Field**
- **Time Entry Custom Field**
- **Trust Request Custom Field**
- **User Custom Field**
- **Xero Bill**
- **Xero Credit Note**
- **Xero Payment**
- **Xero Purchase Order**
- **Xero Tax Rate**
- **Case Product**
- **Case Referral**
- **Case Task List**
- **Contact Referral**
- **Credit Card Refund**
- **Expense Payment**
- **Firm Bank Account**
- **Firm Expense Category**
- **Firm Task Template**
- **Invoice Credit Note**
- **Matter Subscription**
- **Payment Refund**
- **Product Purchase Order**
- **Recurring Credit Note**
- **Recurring Invoice Payment**
- **Recurring Task List**
- **Retainer Payment**
- **Task List Task**
- **Time Off Request Policy**
- **Trust Request Payment**
- **User Task Template**
- **Xero Bank Account**
- **Xero Journal Entry**
- **Xero Tracking Category**
- **Case Expense**
- **Case Invoice**
- **Case Payment**
- **Case Time Entry**
- **Contact Case**
- **Contact Invoice**
- **Contact Payment**
- **Contact Time Entry**
- **Expense Custom Field Value**
- **Invoice Custom Field Value**
- **Payment Custom Field Value**
- **Product Custom Field Value**
- **Task Custom Field Value**
- **Time Entry Custom Field Value**
- **Trust Request Custom Field Value**
- **User Custom Field Value**
- **Case Task Template**
- **Contact Custom Field Value**
- **Case Custom Field Value**
- **Case Task List Template**

Use action names and parameters as needed.
## Working with R3

This skill uses the Membrane CLI to interact with R3. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit R3's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call R3 on your behalf.

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

## Step 1 — Get a connection to R3

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.r3.com/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive R3's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"r3"}'
```

For multi-account setups (e.g. two separate R3 accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey r3 \
  --connectionKey r3-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new R3 connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey r3 \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The R3 base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the R3 API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey r3 \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey r3 --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

Use `npx @membranehq/cli@latest action list --intent=QUERY --connectionId=CONNECTION_ID --json` to discover available actions.


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey r3 --json
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
  --integrationKey r3 --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected R3 connection

R3's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with R3.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for R3 API keys or tokens. Connections handle auth lifecycle server-side.
