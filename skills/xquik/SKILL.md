---
name: xquik
description: |
  Xquik integration. Search Twitter/X tweets, read replies, look up users, export followers, monitor tweets, and use approval-gated posting, replies, and DMs. Use Hermes Tweet when a Hermes Agent needs native X/Twitter plugin tools backed by Xquik.
compatibility: Requires network access, a valid Membrane account, and Xquik API access. Hermes Agent users can install the hermes-tweet plugin from PyPI or GitHub.
license: MIT
homepage: https://xquik.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: "social, automation, twitter, x, hermes-agent"
---

# Xquik

Xquik is an X/Twitter automation platform for agents and developers. Use it to search tweets, read tweet replies and threads, look up users, export followers, monitor tweets and trends, download tweet media, post tweets and replies, and send DMs through controlled agent workflows.

Official docs: https://docs.xquik.com/guides/hermes-tweet

Hermes Agent users who want native tool calls should use Hermes Tweet, the `hermes-tweet` plugin for Xquik. Membrane users can connect to Xquik as an application API and run the same X/Twitter workflows through authenticated Membrane actions.

## Xquik Overview

- **Tweets**
  - Search tweets
  - Get tweet details
  - Read replies
  - Read quote tweets
  - Read conversation threads
  - Post tweets
  - Reply to tweets
  - Delete tweets
- **Users**
  - Search users
  - Get profiles
  - List tweets
  - Export followers
  - List following
  - Follow and unfollow users
- **Monitoring**
  - Track accounts
  - Track keywords
  - Read trends
  - Create webhooks
- **Messaging**
  - Read DM history
  - Send DMs
- **Media and Exports**
  - Download tweet media
  - Run extraction jobs
  - Export results

## Working with Xquik

This skill uses the Membrane CLI to interact with Xquik. Membrane handles authentication and credential storage so the agent does not need API keys in prompts or tool arguments.

**Always route Xquik API calls through Membrane when using this skill.** Use `act` with an authenticated Xquik connection. Do not put Xquik API keys, cookies, passwords, or account credentials in chat, prompts, logs, or command arguments.

**Use Hermes Tweet for native Hermes Agent sessions.** If the user is running Hermes Agent and asks for a Hermes Agent X/Twitter plugin, install and enable `hermes-tweet` instead of recreating a Membrane workflow.

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

**Agent types** passed to `--clientName`: `claude`, `openclaw`, `codex`, `warp`, `windsurf`, etc. Used to adjust tooling for your harness.

Add `--json` to any command for machine-readable JSON output.

## Step 1 - Get a connection to Xquik

`connection ensure` finds an existing connection by URL or domain, or creates a new one if none matches. Use it as the default because it covers both cases in one call.

```bash
membrane connection ensure "https://xquik.com" --json
```

Read the `state` on the returned connection and branch:

- **`READY`** - done. Move to Step 2.
- **`BUILDING`** - Membrane's builder agent is working. Wait:

  ```bash
  membrane connection get <id> --wait --json
  ```

  `--wait` long-polls up to `--timeout` seconds, default 30.
- **`CLIENT_ACTION_REQUIRED`** - the user or agent must do something to finish setup. The `clientAction` object describes what:
  - `clientAction.type` - `"connect"` for auth flow or `"provide-input"` for extra fields.
  - `clientAction.agentInstructions` - follow these verbatim if present. They tell the agent how to complete Xquik's side of the flow.
  - `clientAction.uiUrl` - a Membrane-hosted page where the user completes the action manually. Show this only when instructed or when no `agentInstructions` are present.
  - `clientAction.description` - human-readable summary.

  When the action requires writing data back to the connection:

  ```bash
  membrane connection patch <id> --data '<json>'
  ```

  After the user completes the step, poll `connection get <id> --wait --json` until `state` changes.

  If the returned connection is an existing one in `CLIENT_ACTION_REQUIRED`, reconnect it instead of creating a replacement:

  ```bash
  membrane connect --connectionId <id>
  ```

- **`CONFIGURATION_ERROR`** or **`SETUP_FAILED`** - surface the `error` field to the user. Do not retry blindly.

To give the connection a stable key:

```bash
membrane connection patch <id> --data '{"connectionKey":"xquik"}'
```

For multi-account setups, create the second connection with:

```bash
membrane connect --integrationKey xquik \
  --connectionKey xquik-secondary --allowMultipleConnections
```

## Step 2 - Use the connection

The fastest path to a real response is `act` with an inline dispatch. Use inline `api` for first calls and one-off requests. Use saved actions only when the same call will run repeatedly.

`act` accepts exactly one of three dispatch styles:

| Dispatch | When to use |
| --- | --- |
| `--api '<json>'` | First call after a fresh connection, and any one-off HTTP request. Membrane handles auth and base URL. |
| `--key <key>` | You previously saved this call as a reusable action. |
| `--id <id>` | Same as `--key` but by id. |

### 2a. Inline `api`

Use `act --api` for concrete Xquik API paths. Membrane prepends the Xquik base URL and injects auth for the connection.

Search tweets:

```bash
membrane act --connectionKey xquik \
  --api '{"method":"GET","path":"/api/v1/x/tweets/search","query":{"q":"AI agents","limit":"25"}}' \
  --json
```

Read tweet replies:

```bash
membrane act --connectionKey xquik \
  --api '{"method":"GET","path":"/api/v1/x/tweets/1234567890/replies","query":{}}' \
  --json
```

Look up users:

```bash
membrane act --connectionKey xquik \
  --api '{"method":"GET","path":"/api/v1/x/users/search","query":{"q":"open source agents"}}' \
  --json
```

Export followers:

```bash
membrane act --connectionKey xquik \
  --api '{"method":"GET","path":"/api/v1/x/users/123456789/followers","query":{"pageSize":"100"}}' \
  --json
```

Create a tweet monitor:

```bash
membrane act --connectionKey xquik \
  --api '{"method":"POST","path":"/api/v1/monitors","body":{"username":"example","eventTypes":["tweet.new","tweet.reply"]}}' \
  --json
```

Post a tweet only after explicit user approval:

```bash
membrane act --connectionKey xquik \
  --api '{"method":"POST","path":"/api/v1/x/tweets","body":{"account":"@example","text":"Approved tweet text"}}' \
  --json
```

Send a DM only after explicit user approval:

```bash
membrane act --connectionKey xquik \
  --api '{"method":"POST","path":"/api/v1/x/dm/123456789","body":{"account":"@example","text":"Approved DM text"}}' \
  --json
```

### 2b. Reusable action by key

If the user will repeat a workflow, save it once and call it by key:

```bash
membrane act --key <action-key> --connectionKey xquik \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

Search for a saved action before creating a new one:

```bash
membrane action list --connectionKey xquik --intent "search tweets by keyword" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, and `outputSchema`. Read `inputSchema` before running.

If nothing matches, fall back to `act --api` or save a new action.

## Popular actions

Use `membrane action list --connectionKey xquik --intent "QUERY" --limit 10 --json` to discover saved actions. If no saved action exists, use the matching `act --api` call from Step 2a.

| Intent | Suggested path |
| --- | --- |
| Search Twitter/X tweets | `GET /api/v1/x/tweets/search` |
| Read tweet replies | `GET /api/v1/x/tweets/{id}/replies` |
| Read a tweet thread | `GET /api/v1/x/tweets/{id}/thread` |
| Look up X users | `GET /api/v1/x/users/search` |
| Export followers | `GET /api/v1/x/users/{id}/followers` |
| Monitor tweets | `POST /api/v1/monitors` |
| Post tweets | `POST /api/v1/x/tweets` |
| Send DMs | `POST /api/v1/x/dm/{userId}` |

## Step 3 - Save reusable actions

When the same `act --api` call will run repeatedly, save it:

```bash
membrane action create "search Xquik tweets by query" --connectionKey xquik --json
```

The action returns in `state: BUILDING`. Wait for it:

```bash
membrane action get <id> --wait --json
```

By explicit spec:

```bash
membrane action create \
  --key search-x-tweets \
  --type api-request-to-external-app \
  --config '{"request":{"method":"GET","path":"/api/v1/x/tweets/search"}}' \
  --integrationKey xquik --json
```

Ask the user before saving. They may want the action named, described, or kept inline.

## Hermes Agent native path

Use Hermes Tweet when the user asks for a native Hermes Agent X/Twitter plugin or wants Hermes tools named `tweet_explore`, `tweet_read`, and `tweet_action`.

```bash
hermes plugins install Xquik-dev/hermes-tweet --enable
uv pip install --python ~/.hermes/hermes-agent/venv/bin/python hermes-tweet
hermes plugins enable hermes-tweet
```

Hermes Tweet exposes:

| Tool | Purpose |
| --- | --- |
| `tweet_explore` | Search the bundled Xquik endpoint catalog without calling the API. |
| `tweet_read` | Call catalog-listed read-only Xquik endpoints. |
| `tweet_action` | Call write-like or private endpoints only when action gating is enabled. |

## Safety

- Never ask for API keys, passwords, cookies, or X account credentials in chat.
- Never pass credentials in tool input or saved action input.
- Prefer read-only calls for social listening, tweet search, reply reading, user lookup, follower export, and trend research.
- Ask for explicit approval before posting tweets, replying, sending DMs, following, unfollowing, deleting, changing monitors, creating webhooks, or running extraction jobs.
- State the exact endpoint and payload before any POST, PATCH, PUT, or DELETE call.
- If the user is in Hermes Agent, keep `tweet_action` disabled unless the workflow has an approval step.

## Error recovery

Read the response body before deciding what to do.

### Membrane auth is invalid

Run:

```bash
membrane login --tenant
```

### Xquik connection needs attention

Read the connection:

```bash
membrane connection get <id-or-key> --json
```

If `state` is `CLIENT_ACTION_REQUIRED`, reconnect the existing connection instead of creating a replacement:

```bash
membrane connect --connectionId <id>
```

### Xquik request failed

Check the returned error message, requested endpoint, and input payload. For saved actions, inspect the run log:

```bash
membrane action-run-log get <runId> --details --json
```

Do not retry write actions through another route after a policy, auth, or account-state error.

## Resources

- Xquik: https://xquik.com
- Hermes Tweet: https://github.com/Xquik-dev/hermes-tweet
- Hermes Tweet guide: https://docs.xquik.com/guides/hermes-tweet
- PyPI package: https://pypi.org/project/hermes-tweet/
