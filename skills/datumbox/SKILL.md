---
name: datumbox
description: |
  Datumbox integration. Manage Organizations, Users, Goals, Filters. Use when the user wants to interact with Datumbox data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Datumbox
Datumbox is a machine learning platform that provides a suite of pre-trained models and APIs for various NLP and data science tasks. It's used by developers and businesses to quickly integrate machine learning capabilities into their applications without needing to build models from scratch.

Official docs: https://www.datumbox.com/apidocs/
## Datumbox Overview

- **Datumbox Machine Learning Models**
  - **Text Classification**
    - Train Text Classification Model
    - Predict Text Classification
  - **Topic Modeling**
    - Train Topic Modeling Model
    - Predict Topic Modeling
  - **Sentiment Analysis**
    - Train Sentiment Analysis Model
    - Predict Sentiment Analysis
  - **Spam Detection**
    - Train Spam Detection Model
    - Predict Spam Detection
  - **Keyword Extraction**
    - Train Keyword Extraction Model
    - Predict Keyword Extraction
  - **Image Classification**
    - Train Image Classification Model
    - Predict Image Classification
  - **Document Classification**
    - Train Document Classification Model
    - Predict Document Classification
  - **Language Detection**
    - Train Language Detection Model
    - Predict Language Detection
  - **Speech to Text**
    - Train Speech to Text Model
    - Predict Speech to Text
  - **Translation**
    - Train Translation Model
    - Predict Translation
  - **Question Answering**
    - Train Question Answering Model
    - Predict Question Answering
  - **Text Summarization**
    - Train Text Summarization Model
    - Predict Text Summarization
  - **Chatbots**
    - Train Chatbots Model
    - Predict Chatbots
  - **Named Entity Recognition**
    - Train Named Entity Recognition Model
    - Predict Named Entity Recognition
  - **Part of Speech Tagging**
    - Train Part of Speech Tagging Model
    - Predict Part of Speech Tagging
  - **Optical Character Recognition**
    - Train Optical Character Recognition Model
    - Predict Optical Character Recognition
  - **Recommender Systems**
    - Train Recommender Systems Model
    - Predict Recommender Systems

Use action names and parameters as needed.
## Working with Datumbox

This skill uses the Membrane CLI to interact with Datumbox. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Datumbox's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Datumbox on your behalf.

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

## Step 1 — Get a connection to Datumbox

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "http://www.datumbox.com/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Datumbox's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"datumbox"}'
```

For multi-account setups (e.g. two separate Datumbox accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey datumbox \
  --connectionKey datumbox-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Datumbox connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey datumbox \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Datumbox base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Datumbox API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey datumbox \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey datumbox --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

| Name | Key | Description |
| --- | --- | --- |
| Text Extraction | text-extraction | Extracts the important information from a given webpage. |
| Document Similarity | document-similarity | Estimates the degree of similarity between two documents. |
| Keyword Extraction | keyword-extraction | Extracts from an arbitrary document all the keywords and word-combinations along with their occurrences in the text. |
| Readability Assessment | readability-assessment | Determines the degree of readability of a document based on its terms and idioms. |
| Gender Detection | gender-detection | Identifies if a particular document is written-by or targets-to a man or a woman based on the context, the words and ... |
| Educational Detection | educational-detection | Classifies documents as educational or non-educational based on their context. |
| Commercial Detection | commercial-detection | Labels documents as commercial or non-commercial based on their keywords and expressions. |
| Adult Content Detection | adult-content-detection | Classifies documents as adult or noadult based on their context. |
| Spam Detection | spam-detection | Labels documents as spam or nospam by taking into account their context. |
| Language Detection | language-detection | Identifies the natural language of the given document based on its words and context. |
| Topic Classification | topic-classification | Assigns documents to one of 12 thematic categories based on their keywords, idioms and jargon. |
| Subjectivity Analysis | subjectivity-analysis | Categorizes documents as subjective or objective based on their writing style. |
| Twitter Sentiment Analysis | twitter-sentiment-analysis | Performs sentiment analysis specifically on Twitter messages. |
| Sentiment Analysis | sentiment-analysis | Classifies documents as positive, negative or neutral depending on whether they express a positive, negative or neutr... |


## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey datumbox --json
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
  --integrationKey datumbox --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Datumbox connection

Datumbox's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Datumbox.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Datumbox API keys or tokens. Connections handle auth lifecycle server-side.
