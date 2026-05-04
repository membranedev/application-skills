---
name: nowsecure
description: |
  NowSecure integration. Manage data, records, and automate workflows. Use when the user wants to interact with NowSecure data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
homepage: https://getmembrane.com
repository: https://github.com/membranedev/application-skills
metadata:
  author: membrane
  version: "2.0"
  categories: ""
---

# NowSecure
NowSecure is a mobile app security testing platform. It helps developers and security teams automate security testing for iOS and Android apps. It's used by organizations looking to identify and remediate vulnerabilities in their mobile applications.

Official docs: https://support.nowsecure.com/hc/en-us
## NowSecure Overview

- **Assessment**
  - **Binary**
- **Finding**
- **User**
- **Workspace**
- **Group**
- **Role**
- **Permission**
- **License**
- **Subscription**
- **Task**
- **Annotation**
- **Integration**
- **Report**
- **Audit Log**
- **Notification**
- **Billing**
- **Support Ticket**
- **Mobile Security Provider**
- **Data Retention Policy**
- **Single Sign-On**
- **Static Analysis Configuration**
- **Dynamic Analysis Configuration**
- **Mobile Environment Configuration**
- **Vulnerability Management**
- **Issue Tracking**
- **Communication Channel**
- **Alert**
- **Comment**
- **Attachment**
- **Evidence**
- **Remediation**
- **Workflow**
- **Dashboard**
- **Mobile App Store**
- **Software Development Lifecycle**
- **Compliance Standard**
- **Security Policy**
- **Threat Model**
- **Attack Surface**
- **Risk Assessment**
- **Penetration Test**
- **Security Training**
- **Incident Response Plan**
- **Data Breach Notification**
- **Privacy Policy**
- **Terms of Service**
- **Cookie Policy**
- **Acceptable Use Policy**
- **Vulnerability Disclosure Policy**
- **Bug Bounty Program**
- **Security Champion**
- **Security Awareness Training**
- **Secure Coding Practices**
- **Code Review**
- **Static Application Security Testing (SAST)**
- **Dynamic Application Security Testing (DAST)**
- **Interactive Application Security Testing (IAST)**
- **Mobile Application Security Testing (MAST)**
- **Software Composition Analysis (SCA)**
- **Application Programming Interface (API) Security Testing**
- **Web Application Firewall (WAF)**
- **Runtime Application Self-Protection (RASP)**
- **Security Information and Event Management (SIEM)**
- **Security Orchestration, Automation and Response (SOAR)**
- **Extended Detection and Response (XDR)**
- **Cloud Security Posture Management (CSPM)**
- **Cloud Workload Protection Platform (CWPP)**
- **Data Loss Prevention (DLP)**
- **Endpoint Detection and Response (EDR)**
- **User and Entity Behavior Analytics (UEBA)**
- **Identity and Access Management (IAM)**
- **Privileged Access Management (PAM)**
- **Multi-Factor Authentication (MFA)**
- **Key Management**
- **Certificate Management**
- **Hardware Security Module (HSM)**
- **Database Security**
- **Network Security**
- **Operating System Security**
- **Firmware Security**
- **Supply Chain Security**
- **Internet of Things (IoT) Security**
- **Industrial Control System (ICS) Security**
- **Medical Device Security**
- **Automotive Security**
- **Financial Technology (FinTech) Security**
- **Cryptocurrency Security**
- **Blockchain Security**
- **Artificial Intelligence (AI) Security**
- **Machine Learning (ML) Security**
- **Robotics Security**
- **Quantum Computing Security**
- **5G Security**
- **Edge Computing Security**
- **Serverless Security**
- **Container Security**
- **Kubernetes Security**
- **Microservices Security**
- **DevSecOps**
- **Cloud Native Security**
- **Zero Trust Security**
- **Data Security**
- **Application Security**
- **Infrastructure Security**
- **Endpoint Security**
- **Network Segmentation**
- **Virtual Private Network (VPN)**
- **Firewall**
- **Intrusion Detection System (IDS)**
- **Intrusion Prevention System (IPS)**
- **Web Security Gateway**
- **Email Security**
- **Phishing Protection**
- **Malware Protection**
- **Ransomware Protection**
- **Distributed Denial-of-Service (DDoS) Protection**
- **Bot Management**
- **Content Delivery Network (CDN) Security**
- **Domain Name System (DNS) Security**
- **Secure Socket Layer (SSL)/Transport Layer Security (TLS)**
- **Wireless Security**
- **Mobile Security**
- **Bring Your Own Device (BYOD) Security**
- **Remote Access Security**
- **Data Encryption**
- **Data Masking**
- **Data Redaction**
- **Data Anonymization**
- **Data Tokenization**
- **Data Loss Prevention (DLP)**
- **Data Governance**
- **Data Compliance**
- **Data Privacy**
- **General Data Protection Regulation (GDPR)**
- **California Consumer Privacy Act (CCPA)**
- **Health Insurance Portability and Accountability Act (HIPAA)**
- **Payment Card Industry Data Security Standard (PCI DSS)**
- **Sarbanes-Oxley Act (SOX)**
- **National Institute of Standards and Technology (NIST)**
- **International Organization for Standardization (ISO)**
- **Center for Internet Security (CIS)**
- **Open Web Application Security Project (OWASP)**

Use action names and parameters as needed.
## Working with NowSecure

This skill uses the Membrane CLI to interact with NowSecure. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

**Always route through Membrane.** Don't hit NowSecure's API directly. Use the `act` command (or the `/act` HTTP endpoint), which proxies every request through an authenticated connection.

**Never handle external-app credentials yourself.** OAuth tokens, API keys, refresh tokens — Membrane stores and manages them server-side. Pass a `connectionKey` (or `connectionId`) and the tools call NowSecure on your behalf.

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

## Step 1 — Get a connection to NowSecure

`connection ensure` finds an existing connection by URL/domain, or creates a new one if none matches. Use it as the default — it covers both cases in one call.

```bash
membrane connection ensure "https://www.nowsecure.com" --json
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
  - `clientAction.agentInstructions` (optional) — **follow these verbatim if present**. They tell the agent how to drive NowSecure's side of the flow programmatically. Don't shortcut to "paste this URL" — the instructions exist because the agent is expected to handle it.
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
membrane connection patch <id> --data '{"connectionKey":"nowsecure"}'
```

For multi-account setups (e.g. two separate NowSecure accounts), create the second one with `connect`:

```bash
membrane connect --integrationKey nowsecure \
  --connectionKey nowsecure-secondary --allowMultipleConnections
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

**Use this as the default for the very first call against a new NowSecure connection.** It's the fastest way to confirm the connection works and to give the user a real response — no build step, no `BUILDING` state, no waiting.

```bash
membrane act --connectionKey nowsecure \
  --api '{"method":"GET","path":"/path/to/endpoint","query":{}}' \
  --json
```

Spec shape: `{ method, path, body?, headers?, query? }`. The NowSecure base URL is prepended automatically. Auth is injected automatically.

Look up the right `path` and `method` from the NowSecure API docs. Only escalate to a saved action (Step 3) if the user is going to run the same call repeatedly.

### 2b. Reusable action by key (for repeat use)

If the user is going to run the same call repeatedly, save it once (see Step 3) and call it by `key`:

```bash
membrane act --key <action-key> --connectionKey nowsecure \
  --input '<json>' --json
```

### 2c. Discover existing reusable actions

If you don't already know whether a saved action exists for what you need:

```bash
membrane action list --connectionKey nowsecure --intent "describe what you want" --limit 10 --json
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
membrane action create "describe what the action should do" --connectionKey nowsecure --json
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
  --integrationKey nowsecure --json
```

**Ask the user before saving** — they may want the action named, described, or kept inline.

## Error recovery

Read the response body — never branch on HTTP status alone. Three error paths:

### 401 — Membrane auth is bad

Your CLI session is invalid or expired. Run `membrane login --tenant` again.

### Disconnected NowSecure connection

NowSecure's auth no longer works (token revoked, OAuth expired, credentials rotated). Read the connection state:

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

You get the mapped input, output, errors, plus the raw HTTP exchange with NowSecure.

## Best practices

- **Prefer `act --api` for the first call** — it skips the build/wait dance and gives the user a real response in one round-trip.
- **Reuse, don't recreate.** When a connection is disconnected, reconnect it — don't make a fresh one. Saved actions and `connectionKey` references stay valid.
- **Discover before you build.** Run `action list --intent "..."` before saving a custom action — Membrane may already have one that fits.
- **Let Membrane handle credentials.** Never ask the user for NowSecure API keys or tokens. Connections handle auth lifecycle server-side.
