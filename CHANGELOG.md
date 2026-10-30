# Changelog

The sections up to 0.3.1 were written afterwards from the history.

## 0.4.9 - 2026-10-30

- Added: startup memories now arrive in two parts of 10,000 characters each instead of one, so a long startup rule no
  longer pushes your other startup memories out of the session start. When part 2 holds memories, part 1 names the
  first five of them and counts the rest, so they are never missing without a word. This also lets you deliver a long
  rule once per session instead of with every prompt, with a short reminder at every prompt: the whole startup
  memory, its title and why included, must fit one part.

## 0.4.8 - 2026-10-29

- Added: the tool `dream_report` shows how lean NeoCortex is in the place you work in: how many characters it loads
  at a session start, at every prompt and at every edit, the index lines and startup memories that do not fit,
  near-duplicate pairs among the memories this session sees, this place's memories that repeat in other projects
  (with only the number of other places), expired memories, links to no record, older handoffs and records that did
  not load. It adds counts for the whole store (memories, near-duplicate pairs, repeats, dead weight, records that did
  not load) that name nothing of another project. It only reports and changes nothing.

## 0.4.7 - 2026-10-29

- Added: when you save a memory that reads much the same as one you already have, the answer to `save_memory` lists
  it as a near duplicate, with how close the two are, and asks Claude to show you both and offer to keep one; the
  save itself changes nothing else. The check compares plain word sets, so it finds copies and light rewordings, not
  the same idea written in other words.

## 0.4.6 - 2026-10-28

- Fixed: on Windows, a memory that another session was updating at that moment could be missing from a load, for
  example from the memories given at session start or from `get_memory`, without any notice, or be reported as a
  memory that could not be read. Windows briefly leaves the memory's file name absent, or its file busy, while the new
  version is renamed into place; a load now looks again for up to a quarter of a second before it counts a memory as
  gone or unreadable.

## 0.4.5 - 2026-10-28

- Fixed: `/plugin:neocortex:neocortex:status` now shows you the status. Claude Code gives a prompt's text to Claude,
  not to you, so until now you saw only Claude's own summary of it; the prompt now asks Claude to repeat the status to
  you as it stands, in a code block.

## 0.4.4 - 2026-10-28

- Added: the prompt `/plugin:neocortex:neocortex:status` shows that the server of this session is connected, which
  memory folder it reads, its version and format, the memories it loaded per scope and those that are not active, the
  imported memories waiting to be sorted, and each record it could not load, with the reason. A memory folder that
  cannot be read gives the reason on its first line. When the prompt is missing from the `/` menu, the server is not
  connected: check `/mcp` or `claude mcp list`.

## 0.4.3 - 2026-10-28

- Fixed: a memory can no longer get a second copy in another scope. An import or a move that would put it in a scope
  while another scope holds it is refused, naming the scope that holds it. Before, a re-import or a move into user
  scope could leave two copies and silently stop a confirmed rule from loading in another project.
- Fixed: a memory NeoCortex cannot read (a failed read, a newer format, a broken file) counts as unknown, not as
  absent. An update or delete of it, and a change that would hide or unhide it through `supersedes` or an override, is
  refused, so a rule never starts or stops loading through a memory NeoCortex could not read. The answer says what to
  do: try again, update NeoCortex, or repair the memory.
- Fixed: deleting a user memory that has a second copy in a project, left by an earlier version, no longer lets that
  copy start loading as a rule without asking. A copy in this project goes through the rule check, so a delete that
  changes a rule is sent to `delete_rule`; a copy in another project lets the delete through only when it is no rule
  and has no link, otherwise the delete is refused and names that project.
- Changed: a move to another scope that is refused because another scope holds the memory shows every copy, which
  one is newer, and the ways out. A copy in a project this session cannot see shows its scope, revision, date and
  body length, but no title or summary.
- Fixed: a delete no longer replaces a copy of the same memory that was deleted earlier; the later copy is kept beside
  it in the `deleted` folder as `<id>.2.md`, then `<id>.3.md`.
- Fixed: an error from the operating system is shown by its code, such as `EPERM`, in a load problem and in every tool
  answer, never with the path of your memory folder.

## 0.4.2 - 2026-10-26

- Fixed: an update, a move to another scope or a delete never writes over a memory or a place record whose format is
  lower than this version's own and that no NeoCortex ever wrote; it is shown as unreadable, to be fixed by hand. No
  record can be in that state yet, because every NeoCortex so far writes format 1.0.
- Changed: the `delete_memory` description that Claude reads no longer names a deleted folder; it says NeoCortex keeps
  a deleted memory, so it can be restored by hand. A deleted memory still goes to the `deleted` folder of your memory
  folder.
- Changed: when reading the context labels fails with an error, the notice names it "the place area" instead of the
  folder `places`.

## 0.4.1 - 2026-10-25

- Fixed: NeoCortex no longer leaves an empty folder in `.neocortex/locks` in your home folder for every memory folder
  it writes to; the last write to finish removes its lock folder. Folders that older versions left stay; the empty
  ones are safe to delete.

## 0.4.0 - 2026-10-25

- Changed: `update_memory`, `delete_memory`, `update_rule` and `delete_rule` require the `revision` from the last
  answer that showed the memory. `get_memory`, each search result, `saved`, `updated` and a conflict's `current` carry
  it, so Claude reads a memory again only after a refusal, or first when it knows the memory only from the index.
- Fixed: when another session changed a memory after Claude read it, Claude's update or delete is refused with the
  memory as it is now, instead of silently overwriting the other change.
- Note: a machine still on an older version does not check the revision, so update every machine that shares your
  memory folder.
- Changed: the README says the plugin repository is public and needs no GitHub account or credentials, and has new
  sections on using NeoCortex and its known issues.

## 0.3.11 - 2026-10-25

- Changed: with `CLAUDE_CONFIG_DIR` and `CLAUDE_CODE_PROJECT_DIR_NAME` both set, the auto memory import now treats
  the folder that pins like the folder `autoMemoryDirectory` names: its user and feedback memories come into user
  scope, the others unsorted. Before, they came into the project you happened to import from.
- Note: memories imported from such a folder by 0.2.0 stay in that project. The import in another project offers
  them again into user scope, so after importing there you may have two copies; delete one by hand.

## 0.3.10 - 2026-10-25

- Added: while imported Claude Code memories wait unsorted, every session start tells Claude how many there are, and
  Claude offers to sort them: into this repository, everywhere, or left on demand, each only on your yes. Subagents
  do not get the notice.

## 0.3.9 - 2026-10-25

- Added: the auto memory import also brings in the other memories of the folder `autoMemoryDirectory` names, such as
  project and reference memories, into user scope unsorted: no index shows them, and the label
  `auto-memory-unsorted` finds them. After the import Claude offers to sort the ones for this repository, only if you
  say yes. Files in that folder that are no auto memory are named and never imported.

## 0.3.8 - 2026-10-24

- Added: the auto memory import also reads the folder `autoMemoryDirectory` names in your `~/.claude/settings.json`.
  Its user and feedback memories come into user scope, where they load in every project, in a preview of their own
  before this project's folder; its other memories are counted and not imported yet. If you say no to that preview,
  the import can still preview this project's folder on its own.

## 0.3.7 - 2026-10-24

- Fixed: a memory that another session deletes while yours loads your memories no longer shows, for that moment, as a
  memory that cannot be read.

## 0.3.6 - 2026-10-24

- Changed: a prompt counts as a short question, which loads your memories with `delivery: question`, only when it has
  at most 30 words and a question mark: `?`, `？` or `؟`. A leading question word without a question mark, such as
  "what changed in the repo", no longer counts, in any language.
- Changed: the README says what a folder that does not sync means: your memories stay on that machine. When two
  machines change the same memory before the sync tool has carried the first change across, both versions survive
  only if the sync tool keeps a conflict copy.

## 0.3.5 - 2026-10-23

- Fixed: when the auto memory import finds nothing new, Claude now also tells you how many memories of this place were
  imported before, counting any you deleted from NeoCortex since.

## 0.3.4 - 2026-10-23

- Added: for 7 days after a rule that loads in a place is saved or changed, every session start there names it, for
  Claude in the index notices and for you in one line on screen.

## 0.3.3 - 2026-10-23

- Fixed: a memory with `overrides` but no override reason is refused at once, by `save_memory` and `update_memory` as
  well as by the rule tools. Before, a rule change like that first asked you to confirm it in Claude Code, and only then
  failed.

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
