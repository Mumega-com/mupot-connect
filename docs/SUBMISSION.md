# Submission pack

Nothing here has been submitted. Repository `Mumega-com/mupot-connect` does not exist yet; creating it and opening the two PRs below is a separate approval.

Pre-flight: the repo must be public under the Mumega-com org and the pinned commit must be reachable before the xAI PR. Both marketplaces warn about branded plugins sourced from personal accounts.

## 1. cursor/plugins (third-party PR)

Copy this repository's contents (without `.git` and `docs/`) to `third_party/mupot/`.

`.cursor-plugin/marketplace.json`, append to `plugins`:

```json
{
  "name": "mupot",
  "source": "third_party/mupot",
  "description": "See your mupot inbox, tasks, and approvals waiting on you; decide in your own secure link.",
  "minClientVersions": {
    "cursor": "3.13.0",
    "grokbot": "0.49.0"
  }
}
```

`README.md`, add a table row (match the neighbouring rows):

```
| `mupot` | [mupot](third_party/mupot/) | Mumega | Productivity | See your mupot inbox, tasks, and approvals waiting on you; decide in your own secure link. |
```

Check: `npm install --no-save ajv ajv-formats && node scripts/validate-plugins.mjs` prints "All plugins validated successfully." (run 2026-10-13 against this package).

Note: `grokbot: 0.49.0` is copied from the other third-party entries that list a Grok Bot minimum. It is an assumption; confirm the right value with the maintainers, or drop the key if Cursor-only listing is wanted.

## 2. xai-org/plugin-marketplace (remote source, SHA pinned)

Add to `.grok-plugin/marketplace.json` `plugins`:

```json
{
  "name": "mupot",
  "description": "Your mupot inbox, tasks and approvals inside Grok Build. Read messages, work the task board, and get sent to your own secure link to decide. Never approves for you.",
  "category": "productivity",
  "source": {
    "source": "url",
    "url": "https://github.com/Mumega-com/mupot-connect.git",
    "sha": "<40-char sha from: git ls-remote https://github.com/Mumega-com/mupot-connect.git HEAD>"
  },
  "homepage": "https://mupot.mumega.com",
  "keywords": ["mupot", "mumega", "mupot inbox", "mupot tasks", "mupot approvals"],
  "domains": ["mupot.mumega.com"]
}
```

Then run `python3 scripts/generate-plugin-index.py`, `python3 scripts/validate-catalog.py`, and `python3 scripts/generate-plugin-index.py --check`, commit the regenerated `.grok-plugin/plugin-index.json`, and fill the PR template:

- Plugin name: mupot. Type: remote source.
- Network endpoints: `https://mupot.mumega.com/mcp` (and its OAuth endpoints on the same host).
- Credentials: the user's own OAuth sign-in in the client. No tokens, keys or env vars in the plugin.
- Hooks: none. Scripts: none. Components: 4 skills, 1 agent, 1 rule, 1 MCP server.
- Security checklist: no remote code execution, no secret reading, least-privilege (approvals are not callable from the plugin).

The repository root is the plugin: it carries `.claude-plugin/plugin.json` and `.mcp.json`, which Grok Build accepts for Claude-ecosystem plugins.

## 3. Listing copy

- Name: mupot
- Tagline: Your inbox, tasks and approvals, inside your coding agent.
- Short description (marketplace): See your mupot inbox, tasks, and approvals waiting on you; decide in your own secure link.
- Long description: mupot connects your coding agent to your team's work queue. Ask what is waiting on you, read and acknowledge messages, create tasks with a clear done condition, and submit finished work for review. When something needs your decision, the agent shows you what it is and sends you to your own secure link to decide. It cannot approve or reject for you. Sign in once with OAuth; no keys to paste.
- Privacy statement: mupot receives the requests your agent makes through this connection, under your account. The plugin has no telemetry. Revoke any time by removing the server in your client and revoking the connection in your mupot dashboard.
- Category: productivity (Cursor), productivity (Grok Build).

## 4. Demo video script (60 seconds)

1. 0:00 Title card "mupot in Cursor". Narration: "Your inbox, tasks and approvals, without leaving your editor."
2. 0:05 Install from the marketplace, click Connect, browser OAuth, return. Narration: "Sign in once. No keys."
3. 0:15 Type "check in to mupot". Show who you are connected as and a one-line queue summary.
4. 0:25 Type "what is waiting on me?" Show two items with plain-language reasons.
5. 0:35 Type "approve the first one". The agent answers that approvals happen in your secure link and shows the link. Open it; show the passkey prompt. Narration: "The agent can show you. It cannot decide for you."
6. 0:48 Type "create a task: update the pricing page, done when the PR is merged". Show the preview, confirm, show the new task id.
7. 0:55 End card: "mupot. Install from the Cursor Marketplace or Grok Build."

Use a demo account with fictional data. Do not show tokens, emails of real customers, or real approval content.

## 5. Screenshots to capture

1. The "what is waiting on me" answer, with two items and the secure-link pointer.
2. The task board summary (blocked and review highlighted), then a created task with its `done_when`.
3. The refusal moment: the user types "approve all", the agent declines and points to the secure link.

## 6. Review-trap checklist

- [ ] Repository is public under Mumega-com and the pinned SHA is reachable and a full 40-char lowercase commit.
- [ ] `plugin.json` name equals the marketplace entry name (`mupot`).
- [ ] No hardcoded tokens, no `variables`, no headers in `mcp.json` or `.mcp.json`.
- [ ] No hooks, scripts, or remote code. No prompt-injection style text in skills (reviewers read `SKILL.md` and descriptions).
- [ ] Keywords and domains are brand-scoped (no `inbox`, `tasks`, `approvals` alone).
- [ ] Logo is 192x192 PNG at `assets/logo.png`.
- [ ] README states endpoints, credentials, what is read and written, how to revoke.
- [ ] A working privacy and terms URL is available. As of 2026-10-13 `https://mupot.mumega.com/privacy` redirects to the login page. Publish a public privacy page first.
- [ ] OAuth works with the client: see the "mupot-side gaps" report (CIMD, unbound directory sessions have zero capabilities).
- [ ] The demo account works end to end on the pinned SHA before submitting.
- [ ] Cursor `minClientVersions` is at least 3.13.0 like other third-party entries; confirm the `grokbot` value.
- [ ] Not a duplicate of `Mumega-com/mupot-plugin` (runtime plugin) or the operator pack; this one is end-user only.
