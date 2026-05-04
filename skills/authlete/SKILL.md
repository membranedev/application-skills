---
name: authlete
description: |
  Authlete integration. Manage data, records, and automate workflows. Use when the user wants to interact with Authlete data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Authlete
Authlete is a specialized platform for implementing OAuth 2.0 and OpenID Connect flows. It's used by developers and organizations looking to outsource the complexities of secure authentication and authorization in their applications and APIs.

Official docs: https://www.authlete.com/developers/
## Authlete Overview

- **Service**
  - **Credential**
- **Client**
- **User**
- **Auth Session**
- **Authentication Session**
- **Device Authorization**
- **Grant**
- **Trusted Issuer**
- **Extension Attribute Definition**
- **UMA Resource**
  - **Policy**
- **Scope**
- **SPICE**
- **DCR**
- **JWK Set**
- **Client Registration**
- **Revocation**
- **Pushed Authorization Request**
- **Authorization**
- **Token**
- **Introspection**
- **Federation Registration**
- **Federation Configuration**
- **CIBA Authentication Request**
- **PAR Response**
- **OAuth 2.0**
- **OpenID Connect**
- **Web API**
- **End User**
- **Resource Server**
- **Authorization Server**
- **Software Statement**
- **Developer**
- **API Key**
- **Log**
- **Server**
- **Configuration**
- **Statistic**
- **Health Check**
- **Service Owner**
- **License**
- **Terms Of Service**
- **Support**
- **Contact**
- **FAQ**
- **Release Note**
- **GDPR**
- **Privacy Policy**
- **Security**
- **Incident**
- **Vulnerability**
- **Bug**
- **Performance**
- **Availability**
- **Scalability**
- **Disaster Recovery**
- **Backup**
- **Restore**
- **Monitoring**
- **Alerting**
- **Logging**
- **Auditing**
- **Compliance**
- **Regulation**
- **Standard**
- **Certification**
- **Accreditation**
- **Insurance**
- **Legal**
- **Contract**
- **Agreement**
- **Policy**
- **Procedure**
- **Guideline**
- **Template**
- **Example**
- **Demo**
- **Tutorial**
- **Training**
- **Documentation**
- **SDK**
- **Library**
- **Tool**
- **Plugin**
- **Extension**
- **Integration**
- **API**
- **Webhook**
- **Event**
- **Notification**
- **Message**
- **Email**
- **SMS**
- **Push Notification**
- **Report**
- **Dashboard**
- **Chart**
- **Graph**
- **Table**
- **List**
- **Form**
- **Page**
- **Modal**
- **Dialog**
- **Wizard**
- **Editor**
- **Viewer**
- **Search**
- **Filter**
- **Sort**
- **Pagination**
- **Import**
- **Export**
- **Download**
- **Upload**
- **Print**
- **Share**
- **Comment**
- **Like**
- **Follow**
- **Subscribe**
- **Unsubscribe**
- **Block**
- **Unblock**
- **Report Abuse**
- **Flag**
- **Bookmark**
- **Save**
- **Archive**
- **Delete**
- **Restore**
- **Purge**
- **Empty**
- **Reset**
- **Restart**
- **Shutdown**
- **Update**
- **Upgrade**
- **Downgrade**
- **Install**
- **Uninstall**
- **Configure**
- **Customize**
- **Personalize**
- **Theme**
- **Layout**
- **Accessibility**
- **Internationalization**
- **Localization**
- **Translation**
- **Currency**
- **Timezone**
- **Date Format**
- **Number Format**
- **Address Format**
- **Phone Number Format**
- **Name Format**
- **Gender**
- **Language**
- **Country**
- **Region**
- **City**
- **Zip Code**
- **Address**
- **Phone Number**
- **Email Address**
- **Name**
- **Password**
- **Username**
- **ID**
- **Code**
- **Key**
- **Secret**
- **Token**
- **URL**
- **IP Address**
- **MAC Address**
- **User Agent**
- **Referer**
- **Cookie**
- **Session**
- **Header**
- **Query Parameter**
- **Path Parameter**
- **Body Parameter**
- **File**
- **Image**
- **Video**
- **Audio**
- **Document**
- **Text**
- **HTML**
- **JSON**
- **XML**
- **CSV**
- **PDF**
- **ZIP**
- **RAR**
- **7Z**
- **EXE**
- **DLL**
- **SO**
- **JAR**
- **WAR**
- **EAR**
- **CLASS**
- **JAVA**
- **C**
- **CPP**
- **H**
- **PY**
- **JS**
- **CSS**
- **SQL**
- **SH**
- **BAT**
- **PS1**
- **RB**
- **PHP**
- **GO**
- **SWIFT**
- **KOTLIN**
- **TS**
- **JSX**
- **TSX**
- **MD**
- **YAML**
- **TOML**
- **INI**
- **CONF**
- **LOG**
- **TXT**
- **RTF**
- **DOC**
- **DOCX**
- **XLS**
- **XLSX**
- **PPT**
- **PPTX**
- **ODT**
- **ODS**
- **ODP**
- **SVG**
- **PNG**
- **JPG**
- **JPEG**
- **GIF**
- **BMP**
- **TIFF**
- **WEBP**
- **MP4**
- **MOV**
- **AVI**
- **MKV**
- **WMV**
- **FLV**
- **MP3**
- **WAV**
- **OGG**
- **FLAC**
- **AAC**
- **M4A**
- **WMA**
- **AIFF**
- **AU**
- **RA**
- **RM**
- **MID**
- **MIDI**
- **KAR**
- **SND**
- **VOC**
- **IFF**
- **AIFC**
- **AIF**
- **S3M**
- **MOD**
- **XM**
- **IT**
- **MTM**
- **UMX**
- **STM**
- **669**
- **FAR**
- **MED**
- **OKT**
- **ULT**
- **DMF**
- **AMF**
- **DSM**
- **PTM**
- **PSM**
- **ScreamTracker**
- **Impulse Tracker**
- **FastTracker**
- **MultiTracker**
- **Unreal Music Format**
- **Digital Music File**
- **ProTracker Module**
- **PolySample Module**
- **Scream Tracker Module**
- **Impulse Tracker Module**
- **Fast Tracker Module**
- **Multi Tracker Module**

Use action names and parameters as needed.
## Working with Authlete

This skill uses the Membrane CLI to interact with Authlete. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Authlete's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Authlete on your behalf.

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

## Step 1 — Get a connection to Authlete

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.authlete.com/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Authlete's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"authlete"}'
```

For multi-account setups (e.g. two separate Authlete accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey authlete \
  --connectionKey authlete-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Authlete connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey authlete \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Authlete base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Authlete API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey authlete \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey authlete --intent "describe what you want" --limit 10 --json
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
membrane action create "describe what the action should do" --connectionKey authlete --json
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
  --integrationKey authlete --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Authlete connection

Authlete's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Authlete.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Authlete API keys or tokens. Connections handle auth lifecycle server-side.
