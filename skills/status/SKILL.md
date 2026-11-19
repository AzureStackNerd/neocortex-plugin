---
name: status
description: Say which part of NeoCortex is broken and what to do next, when NeoCortex seems broken - no memory arrives, a "UserPromptSubmit hook error" shows under a prompt, the session start names a NeoCortex failure or that NeoCortex is unavailable, or NeoCortex's own tools are missing. Runs without this session's NeoCortex server, and needs Node 24.19 or later on the PATH as a real executable, not a .cmd shim. When the command itself fails, say so and quote its own error line.
allowed-tools: Bash(node *), PowerShell(node *)
---

# NeoCortex status

Claude Code has already run the NeoCortex status for you: what stands where its command was is the fenced NeoCortex
status after that line, and it is the answer. Answer as the three points below say, quoting that block as point 2 asks.
You may start this yourself when the user asks in words why NeoCortex is not working, or when you find the
`mcp__plugin_neocortex_neocortex__*` tools are missing; the user does not have to type anything.

```!
node "${CLAUDE_PLUGIN_ROOT}/server/cli.mjs" status "${CLAUDE_PLUGIN_ROOT}" "${CLAUDE_PROJECT_DIR}"
```

## What to say

1. **Whether this session's NeoCortex server is connected.** Answer from your own tool list, which the status cannot
   see: if a tool named `mcp__plugin_neocortex_neocortex__*` is available to you, the server is connected; if none is,
   it is not. You cannot tell a failed server from one disabled for this project or one still connecting, so do not
   guess: say that it is not connected, and that `/mcp` shows which of the three it is and fixes the first two —
   **Reconnect** there, or enable it there if it shows as disabled for this project; if the session just started, look
   again in a moment. Never say it is connected when you have no such tool.
2. **The fenced NeoCortex status after that line**, as it stands. Show it exactly — do not reword, shorten or
   summarise it, and keep its first line first. It names the memory folder, what NeoCortex read there, and, when
   something is wrong, the reason and the one thing to do about it.
3. **The order of the next step.** If that status names a step, that step comes first: the memories cannot arrive
   while the memory folder is wrong. Reconnecting comes after it, and only when point 1 said the server is not
   connected: reconnect it with **Reconnect** in `/mcp`, or `/mcp reconnect all`. When the server is connected but the
   status still names a folder step, look at what it says about that folder. If the folder was changed during
   the session, this session keeps reading the folder it started with, and the next session has no NeoCortex until that
   folder is fixed. If the folder was moved or deleted, this session's server cannot read its memories either: say that
   it is connected but cannot read its memories, and that the folder does not exist — create it, or set the folder that
   holds the memories — never that all is well.

When the status reads "the status command failed: …", say plainly that the status command failed and quote that line.
