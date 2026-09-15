# Claude Code Memory Backup — Laser Auto Parts

This repo is a portable backup of the Claude Code auto-memory files used for the Laser Auto Parts (Zid store) project. Each `.md` file is one memory: a project-status note or a standing behavioral rule, written by Claude across sessions.

## Why this exists

Claude Code's memory lives in a local, per-machine/per-account folder (`~/.claude/projects/<hash>/memory/` — on Windows, `C:\Users\<user>\.claude\projects\<hash>\memory\`). It does **not** sync automatically across different accounts, machines, or Claude Code entry points (CLI vs. desktop app vs. web). This repo is a manual sync point: push from one account, pull into another.

## Resuming from a different account/machine

1. Clone or pull this repo.
2. Copy the `.md` files from here into that environment's local memory folder (create the folder if it doesn't exist yet — Claude Code will pick it up automatically once files are present).
3. Start a Claude Code session as normal; it will read these memories at session start.

## Keeping it in sync

This is a manual backup, not a live sync. After a session that updates memory in a meaningful way, re-copy the current local memory folder here and push again.
