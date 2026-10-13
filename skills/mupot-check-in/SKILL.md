---
name: mupot-check-in
description: >-
  Start a mupot session: confirm who you are connected as, what you can do, and
  what is waiting. Use when the user says "check in to mupot", "connect to
  mupot", "what is my mupot status", or opens a session that depends on mupot.
---

# mupot check-in

Use this first when the user wants to work with mupot. It confirms the connection and gives a short summary. It changes nothing except a presence record that you are online.

## Steps

1. Call `boot_context` with no arguments. It is read-only and reports who the connection belongs to and what it is allowed to do.
2. Call `orient` with no arguments. It returns the orientation packet for the connected identity: its squad, role, and current priorities. Pass `agent` only if the user names a specific agent.
3. Optionally call `check_in` to announce presence. Pass `harness` only if you know it (for example `cursor-ide`, `cursor-cloud`, `grok-cli`). Omit anything you do not know. Do not invent a `seat`, `machine`, `model` or `provider`.
4. Report to the user in plain language: who they are connected as, what they can and cannot do, and, in one line, what is waiting (see `mupot-inbox` and `mupot-approvals` for the details).

## Rules

- If a tool is refused with a permission or `not_agent_bound` message, say that exactly and stop. Do not retry with different arguments, and do not try other tools to get around it. Tell the user the connection may need access granted in their mupot dashboard.
- Never report a capability the response did not list.
- Do not call any tool that creates, grants or reveals credentials. This plugin does not use them.
