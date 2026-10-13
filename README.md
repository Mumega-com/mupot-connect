# mupot for Cursor and Grok

Your mupot inbox, tasks and approvals, inside your coding agent. Ask what is waiting on you, read your messages, work the task board, and get sent to your own secure link to decide. The agent never decides for you.

Works in Cursor, Grok Bot and Grok Build. MIT licensed.

## Which mupot plugin do I need?

| You are... | Use |
|---|---|
| A person who wants your inbox, tasks and approvals inside Cursor, Grok Bot or Grok Build | **This plugin** (`mupot-connect`) |
| A developer or operator running your own agent runtime (Hermes, Herdr, Orca) that receives mupot work | [`mupot-plugin`](https://github.com/Mumega-com/mupot-plugin) |

This package contains no runtime, dispatch or seat code. It is a remote connection plus instructions.

## Three things you can do

1. **Check in.** "Check in to mupot." Confirms who you are connected as, what that connection can do, and what is waiting. (`mupot-check-in`)
2. **Work your queue.** "What is in my mupot inbox?", "Show the board", "Create a task to ... done when ...". Read and acknowledge messages, create and update tasks, submit finished work for review. (`mupot-inbox`, `mupot-tasks`)
3. **See what needs you.** "What is waiting on me?" Lists the items that need your decision and points you to your secure link or passkey in mupot to decide. It never approves or rejects for you. (`mupot-approvals`)

A read-only `mupot-triage` agent gives a ranked morning summary.

## What it can read and write

| | Tools |
|---|---|
| Read | `boot_context`, `orient`, `needs_you_list`, `task_board`, `task_list`, `task_get`, `inbox` (peek) |
| Write, only when you ask | `check_in` (marks you online), `task_create`, `task_update`, `task_submit_result`, `inbox_ack`, `send` |
| Never | approvals and verdicts, credential or access minting, granting or revealing secrets |

Some tools (`inbox`, `inbox_ack`, `send`, `task_submit_result`) need a connection bound to an agent. A plain person-level connection may be told `not_agent_bound`; the skills then say so and stop. What you can see and change is always limited by your mupot membership; the plugin adds no access.

## Install and sign in

- **Cursor:** install `mupot` from the Cursor Marketplace, then open the mupot server in MCP settings and sign in.
- **Grok Build:** add the marketplace entry or install from this repository, then connect the `mupot` MCP server.

Sign-in is OAuth in your browser at `https://mupot.mumega.com`. There are no API keys, tokens or environment variables to paste, and none are stored in this plugin.

## Revoke access

Remove the `mupot` server in your client's MCP settings to stop using it, and revoke the connection in your mupot dashboard (https://mupot.mumega.com) so the token stops working on the server side. Either one is enough to stop the plugin from reading anything.

## Network endpoints and credentials

- Calls only `https://mupot.mumega.com/mcp` (and its OAuth endpoints on the same host).
- Credentials: your own OAuth sign-in, handled by your client. No secrets in this repository.
- No hooks, no scripts, no downloaded code.

## Privacy

mupot receives the requests your agent makes through this connection, under your account. See https://mupot.mumega.com for the current privacy and terms information. This plugin does not collect telemetry of its own.

## Development

Validate against Cursor's schema by placing this directory at `third_party/mupot/` in `cursor/plugins` and running `node scripts/validate-plugins.mjs`. Submission copy is in [docs/SUBMISSION.md](docs/SUBMISSION.md).
