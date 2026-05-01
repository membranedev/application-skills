---
name: paragon
description: |
  Paragon integration. Manage data, records, and automate workflows. Use when the user wants to interact with Paragon data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Paragon
Paragon is a customer data platform (CDP) that helps businesses centralize, understand, and activate their customer data. It's used by marketing, sales, and customer success teams to personalize experiences and improve customer relationships. Think of it as a central hub for all customer information.

Official docs: https://help.useparagon.com/
## Paragon Overview

- **Candidate**
  - **Activity**
- **Job**
- **User**
- **Application**
- **Requisition**
- **Task**
- **Comment**
- **Email**
- **Attachment**
- **Stage**
- **Question**
- **Question Option**
- **Availability**
- **Company**
- **Referral**
- **Report**
- **Integration**
- **Job Post**
- **Offer**
- **Document Template**
- **Approval**
- **Reason**
- **Close Reason**
- **EEO Category**
- **Team**
- **Site**
- **Department**
- **Source**
- **User Group**
- **Workflow**
- **Dashboard**
- **Configuration**
- **Note**
- **Time Off Request**
- **Time Off Policy**
- **Holiday**
- **Pay Period**
- **Pay Group**
- **Pay Code**
- **Expense Report**
- **Expense Category**
- **Invoice**
- **Vendor**
- **Interview Kit**
- **Scorecard**
- **Event**
- **Room**
- **Equipment**
- **Checklist**
- **Alert**
- **Audit Log**
- **Field**
- **Form**
- **Rule**
- **Template**
- **Snippet**
- **Signature**
- **Text Message**
- **Call**
- **Video Conference**
- **Assessment**
- **Background Check**
- **Drug Test**
- **Reference Check**
- **Skills Test**
- **Personality Test**
- **Cognitive Ability Test**
- **Language Test**
- **Typing Test**
- **Coding Test**
- **Sales Test**
- **Customer Service Test**
- **Project Management Test**
- **Leadership Test**
- **Compliance Training**
- **Diversity Training**
- **Harassment Prevention Training**
- **Safety Training**
- **Security Training**
- **Ethics Training**
- **Accessibility Training**
- **Data Privacy Training**
- **Financial Training**
- **Technical Training**
- **Product Training**
- **Sales Training**
- **Customer Service Training**
- **Management Training**
- **Leadership Training**
- **Communication Training**
- **Teamwork Training**
- **Problem Solving Training**
- **Decision Making Training**
- **Time Management Training**
- **Stress Management Training**
- **Conflict Resolution Training**
- **Negotiation Training**
- **Presentation Skills Training**
- **Writing Skills Training**
- **Public Speaking Training**
- **Interpersonal Skills Training**
- **Critical Thinking Training**
- **Creative Thinking Training**
- **Innovation Training**
- **Change Management Training**
- **Project Management Training**
- **Risk Management Training**
- **Quality Management Training**
- **Process Improvement Training**
- **Lean Training**
- **Six Sigma Training**
- **Agile Training**
- **Scrum Training**
- **Kanban Training**
- **DevOps Training**
- **Cloud Computing Training**
- **Cybersecurity Training**
- **Data Science Training**
- **Artificial Intelligence Training**
- **Machine Learning Training**
- **Deep Learning Training**
- **Blockchain Training**
- **Internet of Things Training**
- **Virtual Reality Training**
- **Augmented Reality Training**
- **3D Printing Training**
- **Robotics Training**
- **Nanotechnology Training**
- **Biotechnology Training**
- **Renewable Energy Training**
- **Sustainability Training**
- **Environmental Training**
- **Social Responsibility Training**
- **Governance Training**
- **Ethics Training**
- **Compliance Training**
- **Risk Management Training**
- **Financial Training**
- **Accounting Training**
- **Auditing Training**
- **Tax Training**
- **Investment Training**
- **Insurance Training**
- **Real Estate Training**
- **Mortgage Training**
- **Banking Training**
- **Credit Training**
- **Debt Management Training**
- **Retirement Planning Training**
- **Estate Planning Training**
- **Legal Training**
- **Human Resources Training**
- **Marketing Training**
- **Sales Training**
- **Customer Service Training**
- **Management Training**
- **Leadership Training**
- **Communication Training**
- **Teamwork Training**
- **Problem Solving Training**
- **Decision Making Training**
- **Time Management Training**
- **Stress Management Training**
- **Conflict Resolution Training**
- **Negotiation Training**
- **Presentation Skills Training**
- **Writing Skills Training**
- **Public Speaking Training**
- **Interpersonal Skills Training**
- **Critical Thinking Training**
- **Creative Thinking Training**
- **Innovation Training**
- **Change Management Training**
- **Project Management Training**
- **Risk Management Training**
- **Quality Management Training**
- **Process Improvement Training**
- **Lean Training**
- **Six Sigma Training**
- **Agile Training**
- **Scrum Training**
- **Kanban Training**
- **DevOps Training**
- **Cloud Computing Training**
- **Cybersecurity Training**
- **Data Science Training**
- **Artificial Intelligence Training**
- **Machine Learning Training**
- **Deep Learning Training**
- **Blockchain Training**
- **Internet of Things Training**
- **Virtual Reality Training**
- **Augmented Reality Training**
- **3D Printing Training**
- **Robotics Training**
- **Nanotechnology Training**
- **Biotechnology Training**
- **Renewable Energy Training**
- **Sustainability Training**
- **Environmental Training**
- **Social Responsibility Training**
- **Governance Training**

Use action names and parameters as needed.
## Working with Paragon

This skill uses the [Membrane](https://getmembrane.com) CLI to interact with Paragon. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Paragon's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Paragon on your behalf.

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

## Step 1 — Get a connection to Paragon

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.useparagon.com/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Paragon's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"paragon"}'
```

For multi-account setups (e.g. two separate Paragon accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey paragon \
  --connectionKey paragon-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Paragon connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey paragon \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Paragon base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Paragon API docs (or the Popular actions table below). Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Inline `code` (when you need logic, not just an HTTP call)

```bash
membrane act --connectionKey paragon \
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
membrane act --key <action-key> --connectionKey paragon \
  --input '<json>' --json
```

### 2d. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey paragon --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

Use `npx @membranehq/cli@latest action list --intent=QUERY --connectionId=CONNECTION_ID --json` to discover available actions.

### Running an action from the table above

Use the action's `key` (column above) with `act --key`:

```bash
membrane act --key <action-key> --connectionKey paragon --input '<json>' --json
```

Provide `--input` matching the action's `inputSchema` (read it via `action get <key> --json`). The result is in the `output` field of the response.

## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey paragon --json
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
  --integrationKey paragon --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Paragon connection

Paragon's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Paragon.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Paragon API keys or tokens. Connections handle auth lifecycle server-side.
