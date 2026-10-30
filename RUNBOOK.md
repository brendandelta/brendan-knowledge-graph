# Runbook

## The mini rebooted and an MCP server won't start
1. `launchctl list | grep brendan` — see what's loaded
2. `launchctl load ~/Library/LaunchAgents/com.brendan.<service>.plist`
3. Logs: `~/Library/Logs/brendan-knowledge-graph/<service>.log`

## iMessage stopped sending after a macOS update
Apple broke the automation path. Don't auto-update. Check the MCP server's repo for a fix before touching anything.

## Can't reach the mini from phone
Tailscale on both ends. `tailscale status` on the mini.

## The audit job flagged something
Read `memory/audit/latest.md`. Fix the skill or the index, commit, changelog.

## Adding a skill
1. Write `skills/<name>/SKILL.md`
2. Add a row to `INDEX.md`
3. Line in `CHANGELOG.md`
4. Commit
