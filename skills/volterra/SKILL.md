---
name: volterra
description: |
  Volterra integration. Manage data, records, and automate workflows. Use when the user wants to interact with Volterra data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Volterra
Volterra provides a distributed

Official docs: https://www.volterra.io/docs/
## Volterra Overview

- **Customer Edge (CE) Site**
  - **Device**
- **RE Site**
- **Name Server**
- **Origin Pool**
- **Origin Server**
- **DNS Domain**
- **App Firewall**
- **TLS Configuration**
- **Service Policy Set**
- **Advertise Policy**
- **Security Policy**
- **Network Policy**
- **HTTP Load Balancer**
- **TCP Load Balancer**
- **UDP Load Balancer**
- **Virtual Host**
- **Route**
- **WAF**
- **API Protection**
- **Data Guard Profile**
- **Service Domain**
- **VoltAD Domain**
- **Global Network**
- **VPC**
- **Azure VNet**
- **AWS VPC**
- **Transit VPC**
- **Inside VIP**
- **Outside VIP**
- **Interface**
- **Static Route**
- **BGP Configuration**
- **VPC Network**
- **Hub**
- **Spoke**
- **Transit Hub**
- **Transit Spoke**
- **Cloud Link**
- **Billing Account**
- **Namespace**
- **User**
- **API Credential**
- **Tenant**
- **Project**
- **Role**
- **Authentication Policy**
- **Authorization Policy**
- **Alert Policy**
- **Log Receiver**
- **Monitoring Metric Policy**
- **Upgrade Policy**
- **Image**
- **Hardware**
- **Location**
- **Resource Group**
- **Object Group**
- **Prefix List**
- **Asn List**
- **Threat List**
- **Virtual Network**
- **Firewall**
- **Subnet**
- **Network Interface**
- **Public IP Address**
- **Route Table**
- **Network Security Group**
- **Disk**
- **Load Balancer**
- **Backend Pool**
- **Health Probe**
- **Load Balancing Rule**
- **Virtual Machine**
- **Virtual Machine Scale Set**
- **Availability Set**
- **Container Registry**
- **Kubernetes Cluster**
- **Node Pool**
- **Application Gateway**
- **SQL Database**
- **SQL Server**
- **Cosmos DB Account**
- **Cosmos DB Database**
- **Cosmos DB Container**
- **Storage Account**
- **Blob Container**
- **Queue**
- **Table**
- **File Share**
- **Virtual WAN**
- **VPN Gateway**
- **VPN Site**
- **VPN Connection**
- **Express Route Circuit**
- **Express Route Connection**
- **Network Watcher**
- **Packet Capture**
- **Flow Log**
- **Traffic Analytics**
- **Security Center**
- **Security Alert**
- **Security Recommendation**
- **Automation Account**
- **Runbook**
- **Logic App**
- **Event Grid Topic**
- **Event Grid Subscription**
- **Key Vault**
- **Secret**
- **Certificate**
- **Policy Definition**
- **Policy Assignment**
- **Resource Policy**
- **Management Group**
- **Subscription**
- **Resource Lock**
- **Cost Management**
- **Budget**
- **Reservation**
- **Advisor Recommendation**
- **Monitor**
- **Activity Log**
- **Diagnostic Setting**
- **Metric Alert**
- **Action Group**
- **Autoscale Setting**
- **Service Health Alert**
- **Log Analytics Workspace**
- **Data Collection Rule**
- **Virtual Machine Extension**
- **Virtual Desktop**
- **Application Group**
- **Workspace**
- **Host Pool**
- **Image Definition**
- **Image Version**
- **Shared Image Gallery**
- **Network Function**
- **Network Function Definition**
- **Network Function Instance**
- **Service Function Chain**
- **Service Function Forwarder**
- **Service Function**
- **Traffic Steering Policy**
- **Traffic Classifier**
- **Traffic Action**
- **Traffic Filter**
- **Traffic Mirroring Session**
- **Traffic Mirroring Target**
- **Traffic Mirroring Filter**
- **Network Tap**
- **Network Tap Rule**
- **Network Packet Broker**
- **Network Packet Broker Rule**
- **Network Packet Broker Filter**
- **Network Packet Broker Target**
- **Network Packet Broker Session**
- **Network Packet Broker Mirroring Session**
- **Network Packet Broker Mirroring Target**
- **Network Packet Broker Mirroring Filter**
- **Network Packet Broker Tap**
- **Network Packet Broker Tap Rule**
- **Network Packet Broker Packet Capture**
- **Network Packet Broker Packet Capture Rule**
- **Network Packet Broker Packet Capture Filter**
- **Network Packet Broker Packet Capture Target**
- **Network Packet Broker Packet Capture Session**
- **Network Packet Broker Packet Capture Mirroring Session**
- **Network Packet Broker Packet Capture Mirroring Target**
- **Network Packet Broker Packet Capture Mirroring Filter**
- **Network Packet Broker Packet Capture Tap**
- **Network Packet Broker Packet Capture Tap Rule**

Use action names and parameters as needed.
## Working with Volterra

This skill uses the Membrane CLI to interact with Volterra. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Volterra's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Volterra on your behalf.

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

## Step 1 — Get a connection to Volterra

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.volterra.io/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Volterra's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"volterra"}'
```

For multi-account setups (e.g. two separate Volterra accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey volterra \
  --connectionKey volterra-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Volterra connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey volterra \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Volterra base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Volterra API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey volterra \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey volterra --intent "describe what you want" --limit 10 --json
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
membrane action create "describe what the action should do" --connectionKey volterra --json
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
  --integrationKey volterra --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Volterra connection

Volterra's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Volterra.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Volterra API keys or tokens. Connections handle auth lifecycle server-side.
