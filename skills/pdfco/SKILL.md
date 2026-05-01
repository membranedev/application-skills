---
name: pdfco
description: |
  PDF.co integration. Manage Jobs, Templates. Use when the user wants to interact with PDF.co data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# PDF.co
PDF.co is a SaaS platform that provides a suite of tools for working with PDF documents. It's used by developers and businesses to automate PDF-related tasks like conversion, merging, splitting, and data extraction.

Official docs: https://pdf.co/developers/api
## PDF.co Overview

- **PDF**
  - **Text**
  - **Images**
  - **Information**
  - **Bookmarks**
  - **Annotations**
- **Barcodes**
- **Tables**
- **Forms**
- **Search**
- **Conversion**
  - **HTML to PDF**
  - **Image to PDF**
  - **PDF to Text**
  - **PDF to JSON**
  - **PDF to CSV**
  - **PDF to XML**
  - **PDF to HTML**
  - **PDF to Image**
  - **Spreadsheet to PDF**
  - **PDF to PDF/A**
  - **PDF to Searchable PDF**
- **Merge PDF**
- **Split PDF**
- **Delete Pages From PDF**
- **Add PDF Annotation**
- **Protect PDF**
- **Repair PDF**
- **Watermark PDF**
- **Edit PDF**
- **Optimize PDF**
- **Sign PDF**
- **Extract Data From PDF**
- **Convert Web Page to PDF**
- **Make Searchable PDF**
- **Check If PDF Is Searchable**
- **Get PDF Information**
- **Get PDF Bookmarks**
- **Get PDF Annotations**
- **Read PDF Form**
- **Fill PDF Form**
- **Execute PDF Query**
- **Create PDF From Barcode**
- **Create PDF From Images**
- **Validate PDF/A Compliance**
- **Preflight PDF**
- **Encrypt PDF**
- **Decrypt PDF**
- **Stamp PDF**
- **Unstamp PDF**
- **Rasterize PDF**
- **Flatten PDF**
- **Remove PDF Objects**
- **Compare PDF**
- **Count PDF Objects**
- **Detect Anomalies In PDF**
- **Repair PDF By Rebuilding**
- **Get PDF Text Coordinates**
- **Get PDF Version**
- **Change PDF Version**
- **Embed Fonts To PDF**
- **Remove Embedded Fonts From PDF**
- **Extract Attachments From PDF**
- **Embed Files To PDF**
- **Get PDF Attachments**
- **Split PDF By Barcodes**
- **Linearize PDF**
- **Merge PDF By Bookmarks**
- **Remove Duplicates From PDF**
- **Get PDF Security**
- **Set PDF Security**
- **Remove PDF Security**
- **Convert Any To PDF**
- **Convert Office To PDF**
- **Convert Email To PDF**
- **Convert Markdown To PDF**
- **Convert Presentation To PDF**
- **Convert Diagram To PDF**
- **Convert Archive To PDF**
- **Convert CAD To PDF**
- **Convert Epub To PDF**
- **Convert PS To PDF**
- **Convert XPS To PDF**
- **Convert SVG To PDF**
- **Convert TEX To PDF**
- **Convert RTF To PDF**
- **Convert Web Archive To PDF**
- **Convert Emf To PDF**
- **Convert Wmf To PDF**
- **Convert Tiff To PDF**
- **Convert Avif To PDF**
- **Convert HEIC To PDF**
- **Convert HEIF To PDF**
- **Convert ICO To PDF**
- **Convert BMP To PDF**
- **Convert GIF To PDF**
- **Convert Jpeg To PDF**
- **Convert Png To PDF**
- **Convert Psd To PDF**
- **Convert Raw To PDF**
- **Convert WebP To PDF**
- **Convert DjVu To PDF**
- **Convert Dicom To PDF**
- **Convert OpenOffice To PDF**
- **Convert Mobi To PDF**
- **Convert MS Project To PDF**
- **Convert Visio To PDF**
- **Convert iWork To PDF**
- **Convert 3D To PDF**
- **Convert PostScript To PDF**
- **Convert Gerber To PDF**
- **Convert DXF To PDF**

Use action names and parameters as needed.
## Working with PDF.co

This skill uses the [Membrane](https://getmembrane.com) CLI to interact with PDF.co. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit PDF.co's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call PDF.co on your behalf.

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

## Step 1 — Get a connection to PDF.co

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://pdf.co" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive PDF.co's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"pdfco"}'
```

For multi-account setups (e.g. two separate PDF.co accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey pdfco \
  --connectionKey pdfco-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new PDF.co connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey pdfco \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The PDF.co base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the PDF.co API docs (or the Popular actions table below). Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Inline `code` (when you need logic, not just an HTTP call)

```bash
membrane act --connectionKey pdfco \
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
membrane act --key <action-key> --connectionKey pdfco \
  --input '<json>' --json
```

### 2d. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey pdfco --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

Use `npx @membranehq/cli@latest action list --intent=QUERY --connectionId=CONNECTION_ID --json` to discover available actions.

### Running an action from the table above

Use the action's `key` (column above) with `act --key`:

```bash
membrane act --key <action-key> --connectionKey pdfco --input '<json>' --json
```

Provide `--input` matching the action's `inputSchema` (read it via `action get <key> --json`). The result is in the `output` field of the response.

## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey pdfco --json
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
  --integrationKey pdfco --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected PDF.co connection

PDF.co's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with PDF.co.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for PDF.co API keys or tokens. Connections handle auth lifecycle server-side.
