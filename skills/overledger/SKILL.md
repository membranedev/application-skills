---
name: overledger
description: |
  Overledger integration. Manage data, records, and automate workflows. Use when the user wants to interact with Overledger data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# Overledger
Overledger is a blockchain operating system that connects different blockchains, allowing them to interact with each other. It's used by enterprises and developers who want to build decentralized applications that can run on multiple blockchains simultaneously. This simplifies the development process and reduces reliance on any single blockchain.

Official docs: https://docs.quant.network/
## Overledger Overview

- **Transaction**
  - **Source Account**
  - **Destination Account**
- **Account**
- **Block**
- **Read Request**
- **Resource**
- **Token**
- **Location**
- **Network**
- **Smart Contract**
- **API**
- **Application**
- **DLT**
- **Wallet**
- **Fee**
- **Technology**
- **User**
- **Profile**
- **Notification**
- **Provider**
- **Plan**
- **Payment**
- **Invoice**
- **Node**
- **Channel**
- **Event**
- **Subscription**
- **Address**
- **Message**
- **Data**
- **Identity**
- **Key**
- **Lock**
- **Allowance**
- **Configuration**
- **Setting**
- **Template**
- **License**
- **Log**
- **Metadata**
- **Request**
- **Response**
- **Status**
- **Version**
- **Transaction Status**
- **Balance**
- **Entitlement**
- **Endpoint**
- **Parameter**
- **Schema**
- **Script**
- **Secret**
- **Certificate**
- **Policy**
- **Role**
- **Group**
- **Member**
- **Asset**
- **Order**
- **Trade**
- **Position**
- **Portfolio**
- **Watchlist**
- **Alert**
- **Report**
- **Dashboard**
- **Integration**
- **Workflow**
- **Task**
- **Comment**
- **Attachment**
- **Category**
- **Tag**
- **Statistic**
- **Counter**
- **Gauge**
- **Timer**
- **Histogram**
- **Distribution**
- **Alarm**
- **Incident**
- **Change**
- **Problem**
- **Release**
- **Deployment**
- **Test**
- **Artifact**
- **Repository**
- **Environment**
- **Credential**
- **Registry**
- **Process**
- **Thread**
- **Connection**
- **Session**
- **Query**
- **Result**
- **Record**
- **Field**
- **Value**
- **Document**
- **Image**
- **Video**
- **Audio**
- **File**
- **Folder**
- **Link**
- **Note**
- **Bookmark**
- **Contact**
- **Calendar**
- **Event**
- **Reminder**
- **Location**
- **Route**
- **Area**
- **Building**
- **Room**
- **Device**
- **Sensor**
- **Actuator**
- **Rule**
- **Schedule**
- **Trigger**
- **Action**
- **Condition**
- **Variable**
- **Constant**
- **Function**
- **Module**
- **Package**
- **Library**
- **Service**
- **Component**
- **Element**
- **Attribute**
- **Property**
- **Method**
- **Class**
- **Interface**
- **Enum**
- **Annotation**
- **Exception**
- **Error**
- **Warning**
- **Debug**
- **Trace**
- **Log Entry**
- **Stack Trace**
- **Thread Dump**
- **Heap Dump**
- **Garbage Collection**
- **Memory Usage**
- **CPU Usage**
- **Disk Usage**
- **Network Usage**
- **Performance Metric**
- **System Metric**
- **Application Metric**
- **Security Metric**
- **Compliance Metric**
- **Risk Metric**
- **Audit Log**
- **Access Control**
- **Authentication**
- **Authorization**
- **Encryption**
- **Decryption**
- **Signature**
- **Verification**
- **Key Management**
- **Data Masking**
- **Data Redaction**
- **Vulnerability**
- **Threat**
- **Attack**
- **Incident Response**
- **Disaster Recovery**
- **Business Continuity**
- **Compliance Report**
- **Security Assessment**
- **Penetration Test**
- **Code Review**
- **Configuration Management**
- **Change Management**
- **Release Management**
- **Deployment Automation**
- **Infrastructure as Code**
- **Continuous Integration**
- **Continuous Delivery**
- **Continuous Deployment**
- **DevOps Pipeline**
- **Agile Methodology**
- **Scrum Framework**
- **Kanban Board**
- **Sprint Planning**
- **Daily Standup**
- **Sprint Review**
- **Sprint Retrospective**
- **Product Backlog**
- **User Story**
- **Acceptance Criteria**
- **Definition of Done**
- **Velocity Chart**
- **Burn Down Chart**
- **Gantt Chart**
- **Project Plan**
- **Risk Assessment**
- **Issue Tracking**
- **Bug Report**
- **Feature Request**
- **Support Ticket**
- **Knowledge Base**
- **FAQ**
- **Tutorial**
- **Documentation**
- **Release Notes**
- **Roadmap**
- **Vision Statement**
- **Mission Statement**
- **Value Proposition**
- **Business Model**
- **Market Analysis**
- **Competitive Analysis**
- **SWOT Analysis**
- **Financial Projections**
- **Investor Presentation**
- **Pitch Deck**
- **Term Sheet**
- **Due Diligence**
- **Valuation**
- **Exit Strategy**
- **Mergers and Acquisitions**
- **Initial Public Offering**
- **Venture Capital**
- **Private Equity**
- **Angel Investor**
- **Crowdfunding**
- **Grant**
- **Loan**
- **Debt Financing**
- **Equity Financing**
- **Revenue Model**
- **Cost Structure**
- **Profit Margin**
- **Cash Flow**
- **Balance Sheet**
- **Income Statement**
- **Statement of Cash Flows**
- **Financial Ratio**
- **Key Performance Indicator**
- **Business Intelligence**
- **Data Analytics**
- **Machine Learning**
- **Artificial Intelligence**
- **Big Data**
- **Cloud Computing**
- **Internet of Things**
- **Blockchain Technology**
- **Virtual Reality**
- **Augmented Reality**
- **Mixed Reality**
- **Cybersecurity**
- **Data Privacy**
- **Ethical AI**
- **Sustainable Technology**
- **Social Impact**
- **Global Development**
- **Human Rights**
- **Environmental Protection**
- **Climate Change**
- **Public Health**
- **Education Reform**
- **Economic Development**
- **Political Stability**
- **Social Justice**
- **Cultural Preservation**
- **Technological Innovation**
- **Scientific Discovery**
- **Artistic Expression**
- **Philosophical Inquiry**
- **Spiritual Growth**
- **Personal Development**
- **Community Engagement**
- **Civic Participation**
- **Global Citizenship**
- **Human Potential**

Use action names and parameters as needed.
## Working with Overledger

This skill uses the Membrane CLI to interact with Overledger. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit Overledger's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call Overledger on your behalf.

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

## Step 1 — Get a connection to Overledger

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://quant.network/overledger-platform/" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive Overledger's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"overledger"}'
```

For multi-account setups (e.g. two separate Overledger accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey overledger \
  --connectionKey overledger-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new Overledger connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey overledger \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The Overledger base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the Overledger API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey overledger \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey overledger --intent "describe what you want" --limit 10 --json
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
membrane action create "describe what the action should do" --connectionKey overledger --json
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
  --integrationKey overledger --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected Overledger connection

Overledger's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with Overledger.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for Overledger API keys or tokens. Connections handle auth lifecycle server-side.
