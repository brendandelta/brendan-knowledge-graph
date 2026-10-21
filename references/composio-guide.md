# Composio guide (from Grokbot)

# Composio MCP guide (Arrow / Grok Bot)

**Audience:** teammates and agents who need Gmail, Todoist, Notion, Calendar, Drive, and other apps inside Cursor / Grok Bot.

Composio is the **hub** that connects 500–1000+ apps with managed OAuth. Install **one** Composio connector, then authorize each app inside Composio — do **not** install a separate Gmail/Todoist plugin for day-to-day agent work.

## Install (one-time)

1. Ask the bot: “Connect Composio” / “Add the Composio connector.”
2. Search catalog → **Composio** (plugin id `32661537`) → confirm → install.
3. Success: `user-composio` status is **connected**.

## Connect an app

1. Name the app + login (“Connect Todoist as you@domain.com”).
2. Bot runs `COMPOSIO_MANAGE_CONNECTIONS` (add) → show OAuth link.
3. User completes OAuth in browser.
4. Bot runs `COMPOSIO_WAIT_FOR_CONNECTIONS` until **ACTIVE**.
5. Optional: rename connection alias (`work`, `personal`).

### Account tips

- Todoist = execution layer; Notion = info layer (don’t swap).
- Prefer Composio Google toolkits over a standalone Gmail plugin stuck at `needsAuth`.
- WhatsApp chat triage may use a **separate** custom WhatsApp MCP — don’t confuse with Composio WhatsApp toolkit.
- For shared team access, use Composio **Team connections** in the dashboard (private OAuth stays private).

## Agent workflow (authors)

1. `COMPOSIO_SEARCH_TOOLS` — new session `{generate_id: true}`; reuse `session_id` after.
2. If inactive → `COMPOSIO_MANAGE_CONNECTIONS` → auth link → `COMPOSIO_WAIT_FOR_CONNECTIONS`.
3. Incomplete schema → `COMPOSIO_GET_TOOL_SCHEMAS`.
4. `COMPOSIO_MULTI_EXECUTE_TOOL` with real slugs; large payloads → workbench/bash.

## Hard rules

- Never invent tool slugs; never claim “no access” before SEARCH_TOOLS.
- Outbound Gmail send / Dropbox Sign requires **explicit user chat approval**.
- Keep ~5–7 hottest Todoist items due Today for the Mac widget.

## Smoke test

Todoist list/create → Gmail unread (read) → Notion search → Calendar tomorrow.

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| No access to app | SEARCH then MANAGE_CONNECTIONS |
| Auth incomplete | Finish OAuth; wait/poll; reconnect |
| Wrong workspace | Remove + reconnect correct login |
| Truncated dumps | sync_response_to_workbench + remote tools |

Full long-form copy lives at `/workspace/composio-guide/COMPOSIO-MCP-GUIDE.md` when drafted on the box.
