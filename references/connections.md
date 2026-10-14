# Connections: giving the employee its tools

An AI employee is only as useful as what it can reach. This file is how to connect the tools from TOOLS.md, in the order that keeps the CEO safe. Platform-specific screens and commands are in `platforms.md`.

## The five ways a tool gets connected

| Way | What it is, in one line | Best for | Who sets it up |
|---|---|---|---|
| **Built-in connector** | The platform already knows the app; you click Connect and sign in | Gmail, Calendar, Drive, Slack, Notion, HubSpot on hosted platforms | the CEO, in the platform's screen |
| **MCP server** | A standard plug (Model Context Protocol) that lets an AI use a tool through a defined set of actions | anything with an official or well-kept MCP: GitHub, Slack, Google Workspace, Stripe, Notion, Linear, Vercel | the CEO pastes one config line; the skill writes it |
| **Composio / Zapier** | A hub that holds the logins for hundreds of apps and exposes actions to the agent | long-tail SaaS (GoHighLevel, Kit, Circle, QuickBooks), and triggers from those apps | the CEO signs in once per app in the hub |
| **CLI** | A typed command the agent can run on the machine (`gh`, `vercel`, `stripe`, `gcloud`) | developer tools; Hermes and OpenClaw | the CEO installs it and logs in once |
| **API key** | A direct secret for one service | when nothing above exists | the CEO creates it in the service and stores it in the platform's secret store |

Prefer the top of the table. A connector or an MCP the vendor maintains will keep working; a hand-written API call is yours to fix.

## Rules, in order of how much trouble they prevent

1. **Never paste a secret into the chat.** Not an API key, not a password, not a token. The skill tells the CEO *where* to put it (the platform's secret store, the hub's connect screen) and waits. If the CEO pastes one anyway, say it must be rotated and show where.
2. **Narrowest access that works.** Read-only, one folder, one mailbox, one pipeline. You can widen later; you cannot un-leak. Every row in TOOLS.md says read or act, and the first job is all read except the one delivery.
3. **The employee's own identity.** Its own bot user, service account, or app, not the CEO's login. If its credential is missing, the action fails; it never falls back to acting as the CEO. One shared login means you cannot tell who did what, cannot revoke one without the others, and cannot pause one runaway without an outage.
4. **Two tiers of keys.** The read key sees the source and nothing else. The act key can change state or speak as the employee. The read key must never be able to act.
5. **Prohibition by absence.** If AGENTS.md says it never spends money, it holds no payment credential. If it never emails customers, there is no send-to-external tool. Do not restrict in prose what you can simply not grant.
6. **Record who owns each secret and when it rotates** in docs/runbook.md. Never record the value.
7. **Before an alert says "missing credential", it names where it looked.** Most of those escalations are lookups, not blockers.

## The connection walk (Phase 4 → go-live)

For each tool in TOOLS.md, in this order: the first job's source, then its delivery destination, then everything else.

1. **Check first.** If you can see an MCP loaded, a CLI that answers, a hub connection listed, or a connector active, say so and skip to step 4.
2. **Pick the way** from the table, prefer the top. Say which and why in one line.
3. **Give the CEO numbered, literal steps**: the site, the menu, the button, the exact value to paste. Assume they are not technical and may be reading on a phone. One system at a time; wait for "done" before the next.
4. **Prove it with a tiny read.** "List my five most recent deals." "Show me yesterday's unread messages' subjects." The tool is connected when the employee can read one real thing and the CEO recognises it.
5. **Mark it** in TOOLS.md and the tracker (`tools[]` → `connected`, then `verified` after the tiny read).
6. **Scope check.** Confirm it is read-only unless the row says act. If the platform only offers full access, write that down in TOOLS.md and in the runbook as a known risk, and keep the never-do list tighter to compensate.

## When the source is not a connector

On a hosted platform the first job's source may simply not be on the connector list (a call recorder, a niche CRM, a form tool). Do not stall and do not widen the job. The source becomes **a place the CEO or their assistant already fills**: a Drive or Dropbox folder the recorder exports to, an email forward or label, a shared sheet. The employee reads that place, read-only. Write it in TOOLS.md as "via folder (filled by <who>)", say in one line what the CEO has to do each time (usually nothing, once the export is switched on), and mark the direct connector as a future Wire-mode visit.

## Triggers through a hub

When the event source is not something the platform can watch itself (Stripe, a form tool, the CRM), the usual path is: the source → Composio or Zapier trigger → the employee (a webhook on Hermes/OpenClaw, or a hub-native action on the simple platforms). Set the trigger to **human-first**: the action is "draft and notify", never "send", until promoted. Give the trigger a per-event claim (the event id) so a replay does not act twice.

For tool *access* through a hub on the advanced tier, both hubs publish MCP endpoints: Composio `https://connect.composio.dev/mcp` (header `x-consumer-api-key`; or a per-server URL under `backend.composio.dev/v3/mcp/…` with `x-api-key`), Zapier `https://mcp.zapier.com/api/v1/connect` (`Authorization: Bearer <connection token>`). The token goes in the platform's `.env`, referenced by name; see `platforms.md` for the exact `mcp_servers` / `openclaw mcp add` form.

## When the CEO gets stuck here

This step defeats more first builds than any other. If the permission screen does not match the steps given, ask the CEO to paste what they see (never a secret), re-derive the steps from the real screen, and do not grant broader access to move on. Being stuck here is not being behind.
