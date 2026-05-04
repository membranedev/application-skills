---
name: creatio
description: |
  Creatio integration. Manage Leads, Organizations, Users. Use when the user wants to interact with Creatio data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Creatio
Creatio is a low-code platform for CRM and process management. It's used by sales, marketing, and service teams to automate workflows and improve customer experiences.

Official docs: https://academy.creatio.com/docs/
## Creatio Overview

- **Contact**
- **Account**
- **Opportunity**
- **Case**
- **Lead**
- **Document**
- **Invoice**
- **Order**
- **Contract**
- **Activity**
- **Email**
- **Call**
- **Meeting**
- **Task**
- **Product**
- **Knowledge Base Article**
- **Campaign**
- **Event**
- **Bulk Email**
- **Landing Page**
- **Social Post**
- **Survey**
- **Chat**
- **Dashboard**
- **Section**
- **Process**
- **Business Rule**
- **System Setting**
- **User**
- **Role**
- **Queue**
- **Tag**
- **File**
- **Comment**
- **Change Log**
- **Approval**
- **Translation**
- **Integration Log**
- **Mobile App**
- **Portal**
- **Web Form**
- **REST API**
- **SOAP API**
- **Data Import**
- **Data Export**
- **Duplicate Search Rule**
- **Email Template**
- **Print Form**
- **Report**
- **Access Right**
- **Audit**
- **License**
- **Marketplace App**
- **Update**
- **Backup**
- **Restore**
- **Monitoring**
- **Security**
- **Performance**
- **Configuration**
- **Lookup**
- **Relationship**
- **Filter**
- **View**
- **Business Process**
- **Workflow**
- **Script**
- **Data Validation**
- **UI Element**
- **Page**
- **Field**
- **Button**
- **List**
- **Form**
- **Chart**
- **Calendar**
- **Timeline**
- **Feed**
- **Notification**
- **Search**
- **Global Search**
- **Advanced Search**
- **Filter**
- **Sort**
- **Group**
- **Aggregate**
- **Import**
- **Export**
- **Print**
- **Send Email**
- **Create Record**
- **Read Record**
- **Update Record**
- **Delete Record**
- **Search Records**
- **Get Records**
- **Run Business Process**
- **Add Attachment**
- **Remove Attachment**
- **Add Comment**
- **Remove Comment**
- **Approve**
- **Reject**
- **Delegate**
- **Escalate**
- **Resolve**
- **Close**
- **Reopen**
- **Pause**
- **Resume**
- **Start**
- **Stop**
- **Complete**
- **Cancel**
- **Send**
- **Receive**
- **Reply**
- **Forward**
- **Link**
- **Unlink**
- **Subscribe**
- **Unsubscribe**
- **Share**
- **Assign**
- **Verify**
- **Validate**
- **Calculate**
- **Predict**
- **Analyze**
- **Monitor**
- **Sync**
- **Test**
- **Deploy**
- **Configure**
- **Customize**
- **Extend**
- **Integrate**
- **Automate**
- **Manage**
- **Administer**
- **Design**
- **Develop**
- **Debug**
- **Document**
- **Train**
- **Support**

Use action names and parameters as needed.
## Working with Creatio

This skill uses the Membrane CLI to interact with Creatio. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Creatio's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Creatio on your behalf.

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

## Step 1 — Get a connection to Creatio

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://creatio.com" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Creatio's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"creatio"}'
```

For multi-account setups (e.g. two separate Creatio accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey creatio \
  --connectionKey creatio-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Creatio connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey creatio \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Creatio base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Creatio API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey creatio \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey creatio --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
|---|---|---|
| List Accounts | list-accounts | Retrieve a paginated list of accounts from Creatio |
| List Contacts | list-contacts | Retrieve a paginated list of contacts from Creatio |
| List Leads | list-leads | Retrieve a paginated list of leads from Creatio |
| List Opportunities | list-opportunities | Retrieve a paginated list of opportunities from Creatio |
| List Activities | list-activities | Retrieve a paginated list of activities (tasks, calls, emails) from Creatio |
| List Cases | list-cases | List customer service cases with optional filtering and pagination. |
| List Products | list-products | List products in the product catalog. |
| List Orders | list-orders | List sales orders with optional filtering and pagination. |
| Get Account | get-account | Retrieve a single account by ID from Creatio |
| Get Contact | get-contact | Retrieve a single contact by ID from Creatio |
| Get Lead | get-lead | Retrieve a single lead by ID from Creatio |
| Get Opportunity | get-opportunity | Retrieve a single opportunity by ID from Creatio |
| Get Activity | get-activity | Retrieve a single activity by ID from Creatio |
| Get Case | get-case | Get a single customer service case by ID. |
| Get Product | get-product | Get a single product by ID. |
| Get Order | get-order | Get a single order by ID. |
| Create Account | create-account | Create a new account in Creatio |
| Create Contact | create-contact | Create a new contact in Creatio |
| Create Lead | create-lead | Create a new lead in Creatio |
| Update Account | update-account | Update an existing account in Creatio |


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey creatio --json
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
  --integrationKey creatio --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Creatio connection

Creatio's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Creatio.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Creatio API keys or tokens. Connections handle auth lifecycle server-side.
