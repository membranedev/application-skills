---
name: moosend
description: |
  Moosend integration. Manage Campaigns, Templates, Automations. Use when the user wants to interact with Moosend data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Moosend
Moosend is an email marketing automation platform. It's used by businesses of all sizes to create, send, and track email campaigns, manage subscriber lists, and automate marketing workflows.

Official docs: https://developers.moosend.com/
## Moosend Overview

- **Mailing List**
  - **Custom Field**
- **Campaign**
- **Template**
- **Subscriber**
- **Automation**

Use action names and parameters as needed.
## Working with Moosend

This skill uses the Membrane CLI to interact with Moosend. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Moosend's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Moosend on your behalf.

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

## Step 1 — Get a connection to Moosend

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://moosend.com" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Moosend's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"moosend"}'
```

For multi-account setups (e.g. two separate Moosend accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey moosend \
  --connectionKey moosend-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Moosend connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey moosend \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Moosend base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Moosend API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey moosend \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey moosend --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
|---|---|---|
| List Mailing Lists | list-mailing-lists | Gets a list of your active mailing lists in your account. |
| List Subscribers | list-subscribers | Gets a list of all subscribers in a given mailing list. |
| List Campaigns | list-campaigns | Returns a list of all campaigns in your account with detailed information, with optional paging. |
| List Segments | list-segments | Get a list of all segments with their criteria for the given mailing list. |
| Get Mailing List Details | get-mailing-list-details | Gets details for a given mailing list including subscriber statistics. |
| Get Campaign Details | get-campaign-details | Returns a complete set of properties that describe the requested campaign in detail. |
| Get Subscriber by Email | get-subscriber-by-email | Searches for a subscriber with the specified email address in the specified mailing list. |
| Get Subscriber by ID | get-subscriber-by-id | Searches for a subscriber with the specified unique ID in the specified mailing list. |
| Create Mailing List | create-mailing-list | Creates a new empty mailing list in your account. |
| Create Draft Campaign | create-draft-campaign | Creates a new campaign in your account as a draft, ready for sending or testing. |
| Add Subscriber | add-subscriber | Adds a new subscriber to the specified mailing list. |
| Add Multiple Subscribers | add-multiple-subscribers | Adds multiple subscribers to a mailing list with a single call. |
| Update Mailing List | update-mailing-list | Updates the properties of an existing mailing list. |
| Update Subscriber | update-subscriber | Updates a subscriber in the specified mailing list. |
| Delete Mailing List | delete-mailing-list | Deletes a mailing list from your account. |
| Delete Campaign | delete-campaign | Deletes a campaign from your account, draft or even sent. |
| Send Campaign | send-campaign | Sends an existing draft campaign to all recipients specified in its mailing list immediately. |
| Unsubscribe Subscriber | unsubscribe-subscriber | Unsubscribes a subscriber from the specified mailing list. |
| Remove Subscriber | remove-subscriber | Removes a subscriber from the specified mailing list permanently. |
| Get Campaign Summary | get-campaign-summary | Provides a basic summary of the results for any sent campaign. |


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey moosend --json
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
  --integrationKey moosend --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Moosend connection

Moosend's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Moosend.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Moosend API keys or tokens. Connections handle auth lifecycle server-side.
