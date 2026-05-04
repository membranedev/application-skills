---
name: intellexer-api
description: |
  Intellexer API integration. Manage data, records, and automate workflows. Use when the user wants to interact with Intellexer API data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Intellexer API
Intellexer API provides text analytics and natural language processing tools. It's used by developers and businesses to extract meaning from text, analyze sentiment, and summarize documents. This API helps automate tasks like content analysis and information retrieval.

Official docs: https://intellexer.com/text-analytics-api/
## Intellexer API Overview

- **Analyze Text**
  - **Linguistic Analysis**
    - **Sentences**
    - **Tokens**
    - **Named Entities**
  - **Semantic Analysis**
    - **Concepts**
    - **Relations**
    - **Sentiment**
- **Summarize Text**
- **Extract Text**
- **Compare Texts**
- **Search in Knowledge Base**
- **Get Similar Concepts**
- **Get Concept Relations**
- **Classify Text**

Use action names and parameters as needed.
## Working with Intellexer API

This skill uses the Membrane CLI to interact with Intellexer API. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Intellexer API's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Intellexer API on your behalf.

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

## Step 1 — Get a connection to Intellexer API

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.intellexer.com/intellexer_api.html" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Intellexer API's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"intellexer-api"}'
```

For multi-account setups (e.g. two separate Intellexer API accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey intellexer-api \
  --connectionKey intellexer-api-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Intellexer API connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey intellexer-api \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Intellexer API base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Intellexer API API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey intellexer-api \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey intellexer-api --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
| --- | --- | --- |
| Summarize Multiple URLs | summarize-multiple-urls | Generate a combined summary from multiple documents at different URLs |
| Get Topics from Text | get-topics-from-text | Extract topics from provided text |
| Get Topics from URL | get-topics-from-url | Extract topics from a document at the specified URL |
| Parse Document from URL | parse-document-url | Parse and extract content from a document at the specified URL |
| Get Supported Document Topics | get-supported-document-topics | Get list of supported document topics |
| Get Supported Document Structures | get-supported-document-structures | Get list of supported document structures for parsing |
| Convert Query to Boolean | convert-query-to-bool | Convert a natural language query to boolean search expression |
| Analyze Text Linguistically | analyze-text | Perform linguistic analysis on text (tokenization, relations, etc.) |
| Check Text Spelling | check-text-spelling | Check spelling errors in the provided text |
| Compare URLs | compare-urls | Compare two documents by URL and get their similarity score |
| Compare Texts | compare-texts | Compare two texts and get their similarity score |
| Clusterize Text | clusterize-text | Group concepts hierarchically from provided text |
| Recognize Language | recognize-language | Detect the language and encoding of the provided text |
| Recognize Named Entities from Text | recognize-named-entities-text | Extract named entities (people, organizations, locations, etc.) from provided text |
| Recognize Named Entities from URL | recognize-named-entities-url | Extract named entities (people, organizations, locations, etc.) from a document at a URL |
| Get Sentiment Analyzer Ontologies | get-sentiment-ontologies | Get list of available ontologies for sentiment analysis |
| Analyze Sentiments | analyze-sentiments | Analyze sentiments and opinions in texts |
| Summarize Text | summarize-text | Generate a summary from provided text |
| Summarize URL | summarize-url | Generate a summary from a document at a given URL |


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey intellexer-api --json
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
  --integrationKey intellexer-api --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Intellexer API connection

Intellexer API's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Intellexer API.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Intellexer API API keys or tokens. Connections handle auth lifecycle server-side.
