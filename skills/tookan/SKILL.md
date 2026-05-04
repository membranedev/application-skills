---
name: tookan
description: |
  Tookan integration. Manage data, records, and automate workflows. Use when the user wants to interact with Tookan data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Tookan
Tookan is a delivery management and field service automation platform. It helps businesses manage and optimize their dispatch operations, track agents in real-time, and automate tasks. It's used by businesses with delivery fleets or field service teams, such as restaurants, retailers, and logistics companies.

Official docs: https://tookan.freshdesk.com/support/home
## Tookan Overview

- **Task**
  - **Task Template**
- **Team**
- **Agent**
- **Customer**
- **Geofence**
- **User**
- **Add On**
- **Tag**
- **Template**
- **Form**
- **Report**
- **Pricing Add On**
- **Task Attributes**
- **Region**
- **Offer**
- **Wallet Transaction**
- **Reward**
- **Inventory**
- **Product**
- **Store**
- **Order**
- **Driver App**
- **Marketplace Subscription**
- **Subscription Package**
- **Payment Log**
- **Email Template**
- **SMS Template**
- **Custom Field**
- **File**
- **Notification**
- **Role**
- **Workforce**
- **Expense**
- **Leave**
- **Device**
- **Chat**
- **Label**
- **Announcement**
- **Auto Allocation**
- **Task Auto Allocation**
- **Template Auto Allocation**
- **Segment**
- **Booking**
- **Task Category**
- **Quick Task**
- **Dynamic Block**
- **Task Pickup Delivery Settings**
- **Task Reassignment**
- **Task Reassignment Reason**
- **Task Priority**
- **Task Type**
- **Task Checklist**
- **Task Custom Field**
- **Task Marketplace**
- **Task Default**
- **Task Time Slot**
- **Task Working Hours**
- **Task Sla**
- **Task Recurring**
- **Task Location**
- **Task Question**
- **Task Question Field**
- **Task Question Option**
- **Task Question Rule**
- **Task Question Dependency**
- **Task Question Visibility**
- **Task Question Validation**
- **Task Question Section**
- **Task Question Page**
- **Task Question Group**
- **Task Question Conditional**
- **Task Question Trigger**
- **Task Question Action**
- **Task Question Event**
- **Task Question Schedule**
- **Task Question Reminder**
- **Task Question Escalation**
- **Task Question Approval**
- **Task Question Rejection**
- **Task Question Comment**
- **Task Question Attachment**
- **Task Question Signature**
- **Task Question Location**
- **Task Question Geofence**
- **Task Question Barcode**
- **Task Question Qrcode**
- **Task Question Image**
- **Task Question Video**
- **Task Question Audio**
- **Task Question Date**
- **Task Question Time**
- **Task Question Datetime**
- **Task Question Number**
- **Task Question Text**
- **Task Question Textarea**
- **Task Question Select**
- **Task Question Multiselect**
- **Task Question Radio**
- **Task Question Checkbox**
- **Task Question File**
- **Task Question Table**
- **Task Question Map**
- **Task Question Rating**
- **Task Question Slider**
- **Task Question Signature Pad**
- **Task Question Drawing**
- **Task Question Html**
- **Task Question Css**
- **Task Question Javascript**
- **Task Question Json**
- **Task Question Xml**
- **Task Question Yaml**
- **Task Question Markdown**
- **Task Question Code**
- **Task Question Formula**
- **Task Question Calculation**
- **Task Question Summary**
- **Task Question Report**
- **Task Question Dashboard**
- **Task Question Integration**
- **Task Question Automation**
- **Task Question Workflow**
- **Task Question Api**
- **Task Question Webhook**
- **Task Question Email**
- **Task Question Sms**
- **Task Question Push**
- **Task Question Notification**
- **Task Question Log**
- **Task Question Error**
- **Task Question Debug**
- **Task Question Test**
- **Task Question Mock**
- **Task Question Example**
- **Task Question Tutorial**
- **Task Question Help**
- **Task Question Documentation**
- **Task Question Support**
- **Task Question Feedback**
- **Task Question Review**
- **Task Question Rating**
- **Task Question Comment**
- **Task Question Share**
- **Task Question Print**
- **Task Question Export**
- **Task Question Import**
- **Task Question Backup**
- **Task Question Restore**
- **Task Question Version**
- **Task Question History**
- **Task Question Audit**
- **Task Question Security**
- **Task Question Privacy**
- **Task Question Compliance**
- **Task Question Legal**
- **Task Question Terms**
- **Task Question Policy**
- **Task Question Disclaimer**
- **Task Question Copyright**
- **Task Question Trademark**
- **Task Question Patent**
- **Task Question License**
- **Task Question Attribution**
- **Task Question Citation**
- **Task Question Reference**
- **Task Question Source**
- **Task Question Author**
- **Task Question Contributor**
- **Task Question Editor**
- **Task Question Publisher**
- **Task Question Date**
- **Task Question Location**
- **Task Question Language**
- **Task Question Format**
- **Task Question Size**
- **Task Question Duration**
- **Task Question Frequency**
- **Task Question Priority**
- **Task Question Status**
- **Task Question Category**
- **Task Question Type**
- **Task Question Tag**
- **Task Question Keyword**
- **Task Question Description**
- **Task Question Summary**
- **Task Question Abstract**
- **Task Question Introduction**
- **Task Question Body**
- **Task Question Conclusion**
- **Task Question Appendix**
- **Task Question Glossary**
- **Task Question Index**
- **Task Question Table Of Contents**
- **Task Question List Of Figures**
- **Task Question List Of Tables**
- **Task Question List Of Equations**
- **Task Question List Of Symbols**
- **Task Question List Of Abbreviations**
- **Task Question List Of Acronyms**
- **Task Question List Of Definitions**
- **Task Question List Of Examples**
- **Task Question List Of Exercises**
- **Task Question List Of Solutions**
- **Task Question List Of References**
- **Task Question List Of Appendices**
- **Task Question List Of Glossaries**
- **Task Question List Of Indexes**
- **Task Question List Of Tables Of Contents**

Use action names and parameters as needed.
## Working with Tookan

This skill uses the Membrane CLI to interact with Tookan. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Tookan's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Tookan on your behalf.

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

## Step 1 — Get a connection to Tookan

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://jungleworks.com/tookan/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Tookan's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"tookan"}'
```

For multi-account setups (e.g. two separate Tookan accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey tookan \
  --connectionKey tookan-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Tookan connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey tookan \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Tookan base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Tookan API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey tookan \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey tookan --intent "describe what you want" --limit 10 --json
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
membrane action create "describe what the action should do" --connectionKey tookan --json
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
  --integrationKey tookan --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Tookan connection

Tookan's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Tookan.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Tookan API keys or tokens. Connections handle auth lifecycle server-side.
