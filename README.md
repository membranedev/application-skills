<div align="center">
  <a href="https://getmembrane.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="../.github/images/logo-light.png">
      <source media="(prefers-color-scheme: light)" srcset="../.github/images/logo-dark.png">
      <img alt="Membrane" src="../.github/images/logo-dark.png" width="300">
    </picture>
  </a>

  <h1>3,000+ Integration Skills by <a hred="https://getmembrane.com">Membrane</a></h1>

  <p><strong>Give your AI agent  skills to connect with Gmail, Slack, HubSpot, Salesforce, GitHub, and 3,000+ other apps.</strong></p>

  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License: MIT"></a>
  <a href="https://agentskills.io/"><img src="https://img.shields.io/badge/Agent_Skills-compatible-green.svg" alt="Agent Skills"></a>
</div>

<br>

Each skill teaches your agent how to work with a specific app: its structure, actions, and when to use what. Built on [Membrane](https://getmembrane.com) and the open [Agent Skills](https://agentskills.io/) specification.

---

## What’s in this repo?

- **3,000+ app-specific skills** — One skill per app (e.g. `gmail`, `slack`, `hubspot`, `salesforce`). Each skill describes the app’s concepts, available actions, and how to use the Membrane CLI.
- **Works with your agent** — Drop-in support for [OpenClaw](https://docs.openclaw.ai/skills), Cursor, Claude Code, GitHub Copilot, Gemini CLI, and any agent that supports the Agent Skills spec. Use with OpenClaw’s ClawHub or install via `npx skills add`.
- **Auth handled for you** — OAuth, API keys, token refresh: Membrane manages credentials so your agent doesn’t have to.

---

## What your agent can do

Once a skill for an App is installed, your agent can connect to that app and run actions on your behalf — no API keys in the prompt, no copy-pasting docs.

| You say | Agent does |
|--------|------------|
| *"Send a message to #general on Slack: deploy is done"* | Uses the Slack skill, finds the right action, runs it with the right params. |
| *"Create a Jira ticket for the login bug, assign to me, high priority"* | Uses the Jira skill, creates the issue, sets assignee and priority. |
| *"Add a new row to our Google Sheet with today’s metrics"* | Uses the Google Sheets skill, finds the connection, runs the action. |
| *"List my last 10 emails from Gmail"* | Uses the Gmail skill, lists messages. |

Each skill includes the app’s **structure** (entities and resources), **available actions**, and **when to use which action** when it’s not obvious.

---

## Skill layout

Skills are organized as:

```
skills/
  gmail/       → SKILL.md
  slack/      → SKILL.md
  hubspot/    → SKILL.md
  salesforce/ → SKILL.md
  ...         (3,000+ more)
```

Install by **skill name** = folder name (e.g. `gmail`, `slack`, `hubspot`).

---

## Popular skills (examples)

| App | Skill name |
|-----|------------|
| Gmail | `gmail` |
| Slack | `slack` |
| HubSpot | `hubspot` |
| Salesforce | `salesforce` |
| GitHub | `github` |
| Jira | `jira` |
| Google Drive | `google-drive` |
| Microsoft Excel | `microsoft-excel` |
| Dropbox | `dropbox` |
| Pipedrive | `pipedrive` |

Browse the `skills/` folder for the full list.

---

## Resources

- [Membrane Console](https://console.getmembrane.com) — Manage workspaces, connections, and API tokens
- [Membrane Docs](https://getmembrane.com) — Full documentation
- [Agent Skills Spec](https://agentskills.io/) — Open specification these skills are built on
- [OpenClaw Skills](https://docs.openclaw.ai/skills) — Use these skills with OpenClaw; install via [ClawHub](https://learnopenclaw.com/core-concepts/skills) or the Agent Skills CLI

---

## License

MIT
