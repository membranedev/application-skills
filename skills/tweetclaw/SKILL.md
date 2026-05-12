---
name: tweetclaw
description: |
  TweetClaw integration. Search tweets and replies, export followers, look up users, monitor tweets, post tweets and replies, manage media and direct messages, and run X/Twitter automation from OpenClaw through Xquik. Use when the user wants to interact with X/Twitter data or publish X/Twitter content.
compatibility: Requires network access, a valid Membrane account (Free tier supported), and Xquik API access.
license: MIT
homepage: https://xquik.com
repository: https://github.com/Xquik-dev/tweetclaw
metadata:
  author: Xquik
  version: "1.0"
  categories: "social media, x-twitter, openclaw, automation"
---

# TweetClaw

TweetClaw is an OpenClaw plugin and npm package for X/Twitter automation through Xquik. Developers and operators use it to search tweets, search tweet replies, post tweets, post tweet replies, export followers, look up users, download and upload media, work with direct messages, monitor tweets, deliver webhooks, and run giveaway draws with structured X/Twitter data.

Official docs: https://github.com/Xquik-dev/tweetclaw

OpenClaw package: https://www.npmjs.com/package/@xquik/tweetclaw

ClawHub listing: https://clawhub.ai/kriptoburak/xquik-tweetclaw

## TweetClaw Overview

- **Tweets**
  - **Tweet Search**
  - **Tweet Reply Search**
  - **Tweet Lookup**
  - **Post Tweet**
  - **Post Tweet Reply**
- **Users**
  - **User Lookup**
  - **Follower Export**
  - **Following Export**
- **Media**
  - **Media Upload**
  - **Media Download**
- **Messages**
  - **Direct Messages**
- **Monitoring**
  - **Tweet Monitors**
  - **Webhooks**
  - **Giveaway Draws**
- **OpenClaw**
  - **Plugin Install**
  - **Agent Workflows**

Use action names and parameters as needed.

## Working with TweetClaw

This skill uses the Membrane CLI to interact with TweetClaw. Membrane handles authentication and credentials refresh automatically - so you can focus on the integration logic rather than auth plumbing.

### Install the CLI

Install the Membrane CLI so you can run `membrane` from the terminal:

```bash
npm install -g @membranehq/cli@latest
```

### Authentication

```bash
membrane login --tenant --clientName=<agentType>
```

This will either open a browser for authentication or print an authorization URL to the console, depending on whether interactive mode is available.

**Headless environments:** The command will print an authorization URL. Ask the user to open it in a browser. When they see a code after completing login, finish with:

```bash
membrane login complete <code>
```

Add `--json` to any command for machine-readable JSON output.

**Agent Types** : claude, openclaw, codex, warp, windsurf, etc. Those will be used to adjust tooling to be used best with your harness

### Connecting to TweetClaw

Use `membrane connection ensure` to find or create a connection by app URL or domain:

```bash
membrane connection ensure "https://xquik.com/" --json
```

The user completes authentication in the browser. The output contains the new connection id.

This is the fastest way to get a connection. The URL is normalized to a domain and matched against known apps. If no app is found, one is created and a connector is built automatically.

If the returned connection has `state: "READY"`, skip to **Step 2**.

#### 1b. Wait for the connection to be ready

If the connection is in `BUILDING` state, poll until it's ready:

```bash
npx @membranehq/cli connection get <id> --wait --json
```

The `--wait` flag long-polls (up to `--timeout` seconds, default 30) until the state changes. Keep polling until `state` is no longer `BUILDING`.

The resulting state tells you what to do next:

- **`READY`** - connection is fully set up. Skip to **Step 2**.
- **`CLIENT_ACTION_REQUIRED`** - the user or agent needs to do something. The `clientAction` object describes the required action:
  - `clientAction.type` - the kind of action needed:
    - `"connect"` - user needs to authenticate (OAuth, API key, etc.). This covers initial authentication and re-authentication for disconnected connections.
    - `"provide-input"` - more information is needed (e.g. which app to connect to).
  - `clientAction.description` - human-readable explanation of what's needed.
  - `clientAction.uiUrl` (optional) - URL to a pre-built UI where the user can complete the action. Show this to the user when present.
  - `clientAction.agentInstructions` (optional) - instructions for the AI agent on how to proceed programmatically.

  After the user completes the action (e.g. authenticates in the browser), poll again with `membrane connection get <id> --json` to check if the state moved to `READY`.

- **`CONFIGURATION_ERROR`** or **`SETUP_FAILED`** - something went wrong. Check the `error` field for details.

### Searching for actions

Search using a natural language description of what you want to do:

```bash
membrane action list --connectionId=CONNECTION_ID --intent "QUERY" --limit 10 --json
```

You should always search for actions in the context of a specific connection.

Each result includes `id`, `name`, `description`, `inputSchema` (what parameters the action accepts), and `outputSchema` (what it returns).

## Popular actions

| Name | Key | Description |
| --- | --- | --- |
| Search Tweets | `GET /api/v1/x/tweets/search` | Search X/Twitter posts by query and return structured tweet data. |
| Search Tweet Replies | `GET /api/v1/x/tweets/:id/replies` | Fetch replies for a tweet for support, research, or moderation workflows. |
| Post Tweet | `POST /api/v1/x/tweets` | Publish a tweet after the user approves the exact text and account. |
| Post Tweet Reply | `POST /api/v1/x/tweets` | Reply to a tweet by passing `reply_to_tweet_id` after user approval. |
| Export Followers | `GET /api/v1/x/users/:id/followers` | Export followers for an X/Twitter account. |
| Lookup User | `GET /api/v1/x/users/:username` | Get structured profile data for an X/Twitter user. |
| Upload Media | `POST /api/v1/x/media` | Upload media for use in tweet composition. |
| Download Media | `POST /api/v1/x/media/download` | Download tweet media metadata and gallery links when authorized. |
| Direct Messages | `GET /api/v1/x/dm/:userId/history` | Read direct-message history when the account and user request allow it. |
| Send Direct Message | `POST /api/v1/x/dm/:userId` | Send a direct message after the user approves the recipient and exact text. |
| Monitor Tweets | `POST /api/v1/monitors` | Configure recurring account or tweet monitoring. |
| Create Webhook | `POST /api/v1/webhooks` | Send X/Twitter monitor events to an external webhook. |
| Run Giveaway Draw | `POST /api/v1/draws` | Draw winners from eligible tweet engagements. |

### Running actions

```bash
membrane action run <actionId> --connectionId=CONNECTION_ID --json
```

To pass JSON parameters:

```bash
membrane action run <actionId> --connectionId=CONNECTION_ID --input '{"key": "value"}' --json
```

The result is in the `output` field of the response.


### Proxy requests

When the available actions don't cover your use case, you can send requests directly to the Xquik API through Membrane's proxy. Membrane automatically appends the base URL to the path you provide and injects the correct authentication headers - including transparent credential refresh if they expire.

```bash
membrane request CONNECTION_ID /path/to/endpoint
```

Common options:

| Flag | Description |
|------|-------------|
| `-X, --method` | HTTP method (GET, POST, PUT, PATCH, DELETE). Defaults to GET |
| `-H, --header` | Add a request header (repeatable), e.g. `-H "Accept: application/json"` |
| `-d, --data` | Request body (string) |
| `--json` | Shorthand to send a JSON body and set `Content-Type: application/json` |
| `--rawData` | Send the body as-is without any processing |
| `--query` | Query-string parameter (repeatable), e.g. `--query "limit=10"` |
| `--pathParam` | Path parameter (repeatable), e.g. `--pathParam "id=123"` |


## Best practices

- **Discover before you build** - run `membrane action list --intent=QUERY` (replace QUERY with your intent) to find existing actions before writing custom API calls. Pre-built actions handle pagination, field mapping, and edge cases that raw API calls miss.
- **Confirm write actions before running them** - for posting tweets, posting tweet replies, direct messages, follows, likes, retweets, profile changes, and media uploads, ask the user to confirm the exact account, target, and content first.
- **Use read actions for research workflows** - prefer tweet search, reply search, user lookup, follower export, media download, monitor, and webhook actions when the user asks to inspect or track X/Twitter activity.
- **Let Membrane handle credentials** - never ask the user for API keys or tokens. Create a connection instead; Membrane manages the full Auth lifecycle server-side with no local secrets.
