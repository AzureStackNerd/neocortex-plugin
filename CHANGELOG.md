# Changelog

The sections up to 0.3.1 were written afterwards from the history.

## 0.3.2 - 2026-10-23

- Fixed: `save_memory` and `update_memory` refuse a title, summary, why, body or override reason that holds a lone
  surrogate (half of an emoji or another character), and name it as U+XXXX. A stored one shows as U+FFFD at session
  start, in every hook and in the refresh prompt, so it can no longer break a Claude Code session.
- Added: this changelog, with a section for every release.

## 0.3.1 - 2026-10-23

- Changed: when Claude Code asks you to confirm `save_rule`, `update_rule` or `delete_rule`, the call carries the text
  that will load, and each line of its `affects` names how many characters the rule puts in front of Claude.
- Changed: the heading of a memory that loads in full at session start shows at most 80 characters of its title.
- Changed: the related memories a save lists are those that share a topic label with the new one, rarest label first,
  each with only its id, scope and title; sharing only a kind label, such as `feedback`, no longer counts.
- Fixed: a short id that also matches a memory that could not be read is refused as ambiguous, instead of acting on
  the memory that did load.

## 0.3.0 - 2026-10-22

- Added: the tools `save_rule`, `update_rule` and `delete_rule`, the only way to change a rule: a memory that loads on
  its own, such as at session start, or that is enforced. Claude Code asks you before every call of them, even with an
  allow rule for the plugin's tools.
- Changed: `save_memory`, `update_memory` and `delete_memory` refuse a call that would make a rule start, stop or change
  loading, and name the rule tool to use instead.

## 0.2.2 - 2026-10-22

- Changed: `save_memory` and `update_memory` refuse a title or summary that holds a hidden character, and name it as
  U+XXXX.
- Fixed: a hidden character in a stored title or summary shows as U+FFFD wherever NeoCortex shows that text.

## 0.2.1 - 2026-10-22

- Fixed: with `autoMemoryDirectory` set, the auto memory import no longer answers that there is nothing to import; it
  names the folder it does not import yet.

## 0.2.0 - 2026-10-22

- Added: `/plugin:neocortex:neocortex:import_auto_memory` and the tool `import_auto_memory`, which bring the memories
  Claude Code's auto memory kept for a repository into NeoCortex. Claude shows you what would come in and asks before it
  imports, and the original files are never changed.
- Added: a memory you delete after an import does not come back at the next one.

## 0.1.0 - 2026-10-21

- Added: the NeoCortex plugin, an MCP server and hooks that give Claude each memory at its own moment: at session start,
  with every prompt, before every edit and when a subagent starts.
- Added: the prompt `/plugin:neocortex:neocortex:refresh`, which reloads your memories in a running session.
- Added: the tools `save_memory`, `update_memory`, `get_memory`, `search_memories`, `list_labels`,
  `set_context_labels` and `delete_memory`. The memories are files in the folder you set as `memory_folder`, so they
  stay when you update or remove the plugin.
