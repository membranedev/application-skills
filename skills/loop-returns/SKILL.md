---
name: loop-returns
description: |
  Loop Returns integration. Manage Returns, Merchants, Customers, Integrations. Use when the user wants to interact with Loop Returns data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Loop Returns
Loop Returns helps Shopify merchants manage and automate returns and exchanges. It provides a branded portal for customers to initiate returns, and offers options like instant exchanges and store credit. This helps merchants retain revenue and improve the customer experience for online shoppers.

Official docs: https://developer.loopreturns.com/
## Loop Returns Overview

- **Returns**
  - **Return Merchandise Authorizations (RMAs)**
- **Orders**
- **Customers**
- **Products**
- **Reasons**
- **Currencies**
- **Languages**
- **Return Centers**
- **Users**
- **Teams**
- **List Segments**
- **Labels**
- **Integrations**
- **Webhooks**
- **Organizations**
- **Shops**
- **Warehouses**
- **Shipping Methods**
- **Activity Logs**
- **Roles**
- **Package Conditions**
- **Package States**
- **Package Labels**
- **Package Receipts**
- **Package Return Instructions**
- **Package Scan**
- **Package Updates**
- **Package Events**
- **Package Issues**
- **Package Resolutions**
- **Package Notes**
- **Package Activities**
- **Package Attachments**
- **Package Audits**
- **Package History**
- **Package Tasks**
- **Package Exceptions**
- **Package Damages**
- **Package Inspections**
- **Package Photos**
- **Package Videos**
- **Package Documents**
- **Package Measurements**
- **Package Weights**
- **Package Dimensions**
- **Package Locations**
- **Package Owners**
- **Package Assignees**
- **Package Watchers**
- **Package Priorities**
- **Package Statuses**
- **Package Types**
- **Package Categories**
- **Package Subcategories**
- **Package Tags**
- **Package Custom Fields**
- **Package Relationships**
- **Package Dependencies**
- **Package Blockers**
- **Package Milestones**
- **Package Sprints**
- **Package Releases**
- **Package Versions**
- **Package Environments**
- **Package Platforms**
- **Package Browsers**
- **Package Operating Systems**
- **Package Devices**
- **Package Resolutions**
- **Package Orientations**
- **Package Colors**
- **Package Fonts**
- **Package Icons**
- **Package Animations**
- **Package Sounds**
- **Package Haptics**
- **Package Notifications**
- **Package Alerts**
- **Package Errors**
- **Package Warnings**
- **Package Logs**
- **Package Debugs**
- **Package Traces**
- **Package Metrics**
- **Package Performance**
- **Package Security**
- **Package Accessibility**
- **Package Internationalization**
- **Package Localization**
- **Package Compliance**
- **Package Governance**
- **Package Risk**
- **Package Audit**
- **Package Training**
- **Package Documentation**
- **Package Support**
- **Package Feedback**
- **Package Community**
- **Package Events**
- **Package News**
- **Package Blog**
- **Package Social Media**
- **Package Advertising**
- **Package Marketing**
- **Package Sales**
- **Package Pricing**
- **Package Licensing**
- **Package Terms of Service**
- **Package Privacy Policy**
- **Package Cookies**
- **Package GDPR**
- **Package CCPA**
- **Package HIPAA**
- **Package PCI DSS**
- **Package SOC 2**
- **Package ISO 27001**
- **Package NIST**
- **Package COBIT**
- **Package ITIL**
- **Package DevOps**
- **Package Agile**
- **Package Scrum**
- **Package Kanban**
- **Package Waterfall**
- **Package Lean**
- **Package Six Sigma**
- **Package TQM**
- **Package BPM**
- **Package ERP**
- **Package CRM**
- **Package SCM**
- **Package PLM**
- **Package MES**
- **Package WMS**
- **Package TMS**
- **Package BI**
- **Package AI**
- **Package ML**
- **Package DL**
- **Package NLP**
- **Package CV**
- **Package IoT**
- **Package Blockchain**
- **Package Cryptocurrency**
- **Package NFT**
- **Package Metaverse**

Use action names and parameters as needed.
## Working with Loop Returns

This skill uses the [Membrane](https://getmembrane.com) CLI to interact with Loop Returns. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Loop Returns's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Loop Returns on your behalf.

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

## Step 1 — Get a connection to Loop Returns

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.loopreturns.com/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Loop Returns's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"loop-returns"}'
```

For multi-account setups (e.g. two separate Loop Returns accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey loop-returns \
  --connectionKey loop-returns-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Loop Returns connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey loop-returns \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Loop Returns base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Loop Returns API docs (or the Popular actions table below). Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Inline `code` (when you need logic, not just an HTTP call)

```bash
membrane act --connectionKey loop-returns \
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
membrane act --key <action-key> --connectionKey loop-returns \
  --input '<json>' --json
```

### 2d. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey loop-returns --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

Use `npx @membranehq/cli@latest action list --intent=QUERY --connectionId=CONNECTION_ID --json` to discover available actions.

### Running an action from the table above

Use the action's `key` (column above) with `act --key`:

```bash
membrane act --key <action-key> --connectionKey loop-returns --input '<json>' --json
```

Provide `--input` matching the action's `inputSchema` (read it via `action get <key> --json`). The result is in the `output` field of the response.

## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey loop-returns --json
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
  --integrationKey loop-returns --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Loop Returns connection

Loop Returns's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Loop Returns.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Loop Returns API keys or tokens. Connections handle auth lifecycle server-side.
