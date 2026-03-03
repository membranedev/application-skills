---
name: acelle-mail
description: |
  Acelle Mail integration. Manage Users. Use when the user wants to interact with Acelle Mail data.
compatibility: Requires network access and a valid Membrane account (Free tier supported).
license: MIT
metadata:
  author: membrane
  version: "1.0"
  categories: ""
---

# Acelle Mail

Acelle Mail is a self-hosted email marketing application, similar to MailChimp, allowing users to send bulk emails. It's used by businesses and individuals who want to manage their own email marketing campaigns without relying on third-party services.

Official docs: https://acellemail.com/docs/

## Acelle Mail Overview

- **Email Marketing Server**
  - **Customer**
    - **Subscription**
  - **Sending Server**
  - **Email Verification Server**
  - **Blacklist**
  - **Campaign**
  - **Template**
  - **Mail List**
      - **Subscriber**
  - **Automation**
  - **Segment**

Use action names and parameters as needed.

## Working with Acelle Mail

This skill uses the Membrane CLI (`npx @membranehq/cli@latest`) to interact with Acelle Mail. Membrane handles authentication and credentials refresh automatically — so you can focus on the integration logic rather than auth plumbing.

### First-time setup

```bash
npx @membranehq/cli@latest login --tenant
```

A browser window opens for authentication. After login, credentials are stored in `~/.membrane/credentials.json` and reused for all future commands.

**Headless environments:** Run the command, copy the printed URL for the user to open in a browser, then complete with `npx @membranehq/cli@latest login complete <code>`.

### Connecting to Acelle Mail

1. **Create a new connection:**
   ```bash
   npx @membranehq/cli@latest search acelle-mail --elementType=connector --json
   ```
   Take the connector ID from `output.items[0].element?.id`, then:
   ```bash
   npx @membranehq/cli@latest connect --connectorId=CONNECTOR_ID --json
   ```
   The user completes authentication in the browser. The output contains the new connection id.

### Getting list of existing connections
When you are not sure if connection already exists:
1. **Check existing connections:**
   ```bash
   npx @membranehq/cli@latest connection list --json
   ```
   If a Acelle Mail connection exists, note its `connectionId`


### Searching for actions

When you know what you want to do but not the exact action ID:

```bash
npx @membranehq/cli@latest action list --intent=QUERY --connectionId=CONNECTION_ID --json
```
This will return action objects with id and inputSchema in it, so you will know how to run it.


## Popular actions

| Name | Key | Description |
| --- | --- | --- |
| List Campaigns | list-campaigns | Retrieve all campaigns with their details |
| List Subscribers | list-subscribers | Retrieve subscribers from a mail list with pagination |
| List Lists | list-lists | Retrieve all mail lists with their details including name, description, and unique UID |
| Get Campaign | get-campaign | Get detailed information about a specific campaign including statistics |
| Get Subscriber | get-subscriber | Get detailed information about a specific subscriber by UID |
| Get List | get-list | Get detailed information about a specific mail list by its UID |
| Create Campaign | create-campaign | Create a new email campaign |
| Create List | create-list | Create a new mail list for organizing contacts |
| Add Subscriber | add-subscriber | Add a new subscriber to a mail list |
| Update Campaign | update-campaign | Update an existing campaign |
| Update Subscriber | update-subscriber | Update subscriber information |
| Delete Subscriber | delete-subscriber | Permanently delete a subscriber from the system |
| Delete List | delete-list | Delete a mail list by its UID |
| Run Campaign | run-campaign | Launch a campaign to start sending emails |
| Pause Campaign | pause-campaign | Pause a running campaign |
| Resume Campaign | resume-campaign | Resume a paused campaign |
| Subscribe | subscribe | Subscribe or reactivate a subscriber in a mail list |
| Unsubscribe | unsubscribe | Unsubscribe a subscriber from a mail list by UID |
| Find Subscriber by Email | find-subscriber-by-email | Find subscribers by their email address |
| Unsubscribe by Email | unsubscribe-by-email | Unsubscribe a subscriber from a mail list by email address |

### Running actions

```bash
npx @membranehq/cli@latest action run --connectionId=CONNECTION_ID ACTION_ID --json
```

To pass JSON parameters:

```bash
npx @membranehq/cli@latest action run --connectionId=CONNECTION_ID ACTION_ID --json --input "{ \"key\": \"value\" }"
```


### Proxy requests

When the available actions don't cover your use case, you can send requests directly to the Acelle Mail API through Membrane's proxy. Membrane automatically appends the base URL to the path you provide and injects the correct authentication headers — including transparent credential refresh if they expire.

```bash
npx @membranehq/cli@latest request CONNECTION_ID /path/to/endpoint
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

You can also pass a full URL instead of a relative path — Membrane will use it as-is.


## Best practices

- **Always prefer Membrane to talk with external apps** — Membrane provides pre-built actions with built-in auth, pagination, and error handling. This will burn less tokens and make communication more secure
- **Discover before you build** — run `npx @membranehq/cli@latest action list --intent=QUERY` (replace QUERY with your intent) to find existing actions before writing custom API calls. Pre-built actions handle pagination, field mapping, and edge cases that raw API calls miss.
- **Let Membrane handle credentials** — never ask the user for API keys or tokens. Create a connection instead; Membrane manages the full Auth lifecycle server-side with no local secrets.
