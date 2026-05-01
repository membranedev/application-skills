---
name: instana
description: |
  Instana integration. Manage data, records, and automate workflows. Use when the user wants to interact with Instana data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Instana
Instana is an observability platform that helps monitor the performance of applications and infrastructure. It provides automated discovery, tracing, and analysis of complex systems. DevOps teams and SREs use it to identify and resolve performance issues quickly.

Official docs: https://www.ibm.com/docs/en/instana-observability
## Instana Overview

- **Dashboard**
  - **Dashboard Group**
- **Application**
- **Website**
- **Alerting Channel**
- **User**
- **Infrastructure Monitoring**
- **Kubernetes Monitoring**
- **Technology Monitoring**
- **Trace Monitoring**
- **Event**
- **Call**
- **Service**
- **Endpoint**
- **Database**
- **Query**
- **Setting**
- **Log Source**
- **Log Mapping Template**
- **Synthetic Monitor**
- **Test Catalog**
- **API Token**
- **Release Marker**
- **Maintenance Window**
- **Agent Setting**
- **License**
- **Bill**
- **Usage Profile**
- **Integration**
  - **AWS Integration**
  - **Azure Integration**
  - **Google Cloud Integration**
  - **PCF Integration**
  - **Custom Integration**
- **Technology**
- **Span**
- **Website Group**
- **Mobile App**
- **Mobile App Group**
- **Session**
- **User Session**
- **Process Group**
- **Container Group**
- **Snapshot**
- **Anomaly**
- **Incident**
- **Problem**
- **Story**
- **Perspective**
- **Notebook**
- **Report**
- **Automation Rule**
- **Correlation Configuration**
- **Custom Event**
- **Global Notification Group**
- **External Website**
- **Application Monitoring**
- **Configuration Profile**
- **Log Rule**
- **Log Forwarding Rule**
- **Log Management**
- **Monitoring Template**
- **Process**
- **Container**
- **Host**
- **Virtual Machine**
- **Datastore**
- **Load Balancer**
- **Network Interface**
- **Volume**
- **Queue Manager**
- **Topic**
- **Channel**
- **Listener**
- **Transaction**
- **Program**
- **Job**
- **Step**
- **Message**
- **Cache**
- **Lock**
- **Sensor**
- **Device**
- **Firmware**
- **Certificate**
- **Key**
- **Secret**
- **Policy**
- **Rule**
- **Action**
- **Task**
- **Workflow**
- **Schedule**
- **Template**
- **Script**
- **Variable**
- **Constant**
- **Enumeration**
- **Annotation**
- **Comment**
- **Attachment**
- **Link**
- **Bookmark**
- **Tag**
- **Category**
- **Group**
- **Role**
- **Permission**
- **Audit Log**
- **System Log**
- **Error Log**
- **Access Log**
- **Change Log**
- **Event Log**
- **Security Log**
- **Performance Log**
- **Debug Log**
- **Trace Log**
- **Alert**
- **Notification**
- **Incident Preference**
- **Team**
- **Calendar**
- **Service Level Agreement (SLA)**
- **Outage**
- **Change Request**
- **Knowledge Base Article**
- **FAQ**
- **Tutorial**
- **Documentation**
- **Release Note**
- **Roadmap**
- **Case**
- **Opportunity**
- **Lead**
- **Contact**
- **Account**
- **Contract**
- **Quote**
- **Order**
- **Invoice**
- **Payment**
- **Shipment**
- **Return**
- **Refund**
- **Coupon**
- **Discount**
- **Tax**
- **Currency**
- **Language**
- **Region**
- **Country**
- **City**
- **Location**
- **IP Address**
- **Domain**
- **Subnet**
- **Network**
- **Firewall**
- **VPN**
- **Proxy**
- **DNS**
- **DHCP**
- **Routing Table**
- **ARP Table**
- **MAC Address**
- **Port**
- **Protocol**
- **Interface**
- **Bandwidth**
- **Latency**
- **Packet Loss**
- **Jitter**
- **Throughput**
- **Availability**
- **Reliability**
- **Scalability**
- **Performance**
- **Security**
- **Compliance**
- **Cost**
- **Risk**
- **Impact**
- **Urgency**
- **Priority**
- **Severity**
- **Status**
- **Resolution**
- **Owner**
- **Assignee**
- **Reviewer**
- **Approver**
- **Watcher**
- **Subscriber**
- **Follower**
- **Mention**
- **Commenter**
- **Reporter**
- **Creator**
- **Modifier**
- **Deleter**
- **Archiver**
- **Restorer**
- **Exporter**
- **Importer**
- **Validator**
- **Analyzer**
- **Optimizer**
- **Simulator**
- **Emulator**
- **Debugger**
- **Profiler**
- **Monitor**
- **Controller**
- **Adapter**
- **Connector**
- **Driver**
- **Plugin**
- **Extension**
- **Module**
- **Library**
- **Framework**
- **Platform**
- **Environment**
- **Configuration**
- **Deployment**
- **Release**
- **Version**
- **Build**
- **Test**
- **Quality**
- **Defect**
- **Issue**
- **Problem Report**
- **Feature Request**
- **Enhancement**
- **Improvement**
- **Refactoring**
- **Technical Debt**
- **Code Review**
- **Unit Test**
- **Integration Test**
- **System Test**
- **Acceptance Test**
- **Regression Test**
- **Performance Test**
- **Security Test**
- **Usability Test**
- **Accessibility Test**
- **Localization Test**
- **Internationalization Test**
- **Globalization Test**
- **Data Migration**
- **Data Integration**
- **Data Synchronization**
- **Data Replication**
- **Data Backup**
- **Data Recovery**
- **Data Archiving**
- **Data Purging**
- **Data Masking**
- **Data Encryption**
- **Data Compression**
- **Data De-duplication**
- **Data Validation**
- **Data Cleansing**
- **Data Transformation**
- **Data Enrichment**
- **Data Analysis**
- **Data Visualization**
- **Data Reporting**
- **Data Mining**
- **Data Warehousing**
- **Data Lake**
- **Big Data**
- **Machine Learning**
- **Artificial Intelligence**
- **Natural Language Processing**
- **Computer Vision**
- **Robotics**
- **Internet of Things (IoT)**
- **Blockchain**
- **Cloud Computing**
- **Edge Computing**
- **Quantum Computing**
- **Augmented Reality (AR)**
- **Virtual Reality (VR)**
- **Mixed Reality (MR)**
- **Extended Reality (XR)**
- **Metaverse**

Use action names and parameters as needed.
## Working with Instana

This skill uses the [Membrane](https://getmembrane.com) CLI to interact with Instana. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Instana's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Instana on your behalf.

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

## Step 1 — Get a connection to Instana

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.instana.com/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Instana's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"instana"}'
```

For multi-account setups (e.g. two separate Instana accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey instana \
  --connectionKey instana-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Instana connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey instana \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Instana base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Instana API docs (or the Popular actions table below). Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Inline `code` (when you need logic, not just an HTTP call)

```bash
membrane act --connectionKey instana \
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
membrane act --key <action-key> --connectionKey instana \
  --input '<json>' --json
```

### 2d. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey instana --intent "describe what you want" --limit 10 --json
```

Each result carries `id`, `key`, `name`, `description`, `inputSchema`, `outputSchema`. Read `inputSchema` before running — it's authoritative.

If nothing matches, fall back to Step 2a (`act --api`) or save a new action (Step 3).

## Popular actions

Use `npx @membranehq/cli@latest action list --intent=QUERY --connectionId=CONNECTION_ID --json` to discover available actions.

### Running an action from the table above

Use the action's `key` (column above) with `act --key`:

```bash
membrane act --key <action-key> --connectionKey instana --input '<json>' --json
```

Provide `--input` matching the action's `inputSchema` (read it via `action get <key> --json`). The result is in the `output` field of the response.

## Step 3 — Save reusable actions (optional)

When you find yourself about to make the same `act --api` call a second time, save it. Future calls become `act --key <key>` instead of the full inline spec.

Two ways:

**By intent** — describe what you want; Membrane builds the config and validates it:

```bash
membrane action create "describe what the action should do" --connectionKey instana --json
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
  --integrationKey instana --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Instana connection

Instana's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Instana.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Instana API keys or tokens. Connections handle auth lifecycle server-side.
