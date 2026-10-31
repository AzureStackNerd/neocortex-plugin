# NeoCortex

Central memory for Claude Code: one place for everything Claude should know or follow across sessions, projects and
machines. The plugin adds an MCP server and hooks that give Claude each memory at its own moment: at session start,
with every prompt, before every edit, and when a subagent starts. Claude saves memories on its own initiative. They
are files in one folder you choose, outside the plugin, so they stay when you update or remove the plugin.

NeoCortex is proprietary; see [LICENSE](LICENSE). `server/THIRD-PARTY-NOTICES` holds the licences of the open-source
code bundled into the server.

## Requirements

- Claude Code. The one-command install below needs version 2.1.292 or later.
- Node 24.19 or later as `node` on your `PATH`, a real executable. The hooks and the server start `node` without a
  shell, and "On Windows, exec form requires command to resolve to a real executable such as a .exe. The .cmd and .bat
  shims ... can't be spawned without a shell" (<https://code.claude.com/docs/en/hooks>). On Windows, `where.exe node`
  must list a `node.exe`.
- git on your `PATH`. In a repository NeoCortex reads the `origin` remote to know which project's memories belong
  there, and its server does not start in a repository where git cannot run.
- Access to the public GitHub repository `AzureStackNerd/neocortex-plugin`, which needs no GitHub account or
  credentials; see [GitHub over SSH or HTTPS](#github-over-ssh-or-https).

## Install

1. Create the memory folder, for example `C:\Users\<you>\NeoCortex` on Windows or `~/NeoCortex` on macOS and Linux.
   Any folder works. A folder that a sync tool keeps in step between your machines shares your memories between them;
   a local folder that does not sync keeps them on this machine only, and another machine then has memories of its
   own. When two machines change the same memory before the sync tool has carried the first change across, both
   versions survive only if the sync tool keeps a conflict copy, which NeoCortex then reports; otherwise one change is
   lost, and NeoCortex cannot see that.

2. In a terminal, add the marketplace and install the plugin, with the full path of that folder as its option
   `memory_folder`. In PowerShell:

   ```powershell
   claude plugin marketplace add AzureStackNerd/neocortex-plugin
   claude plugin install neocortex@azurestacknerd --config "memory_folder=C:\Users\<you>\NeoCortex"
   ```

   In bash or zsh on macOS or Linux:

   ```bash
   claude plugin marketplace add AzureStackNerd/neocortex-plugin
   claude plugin install neocortex@azurestacknerd --config "memory_folder=$HOME/NeoCortex"
   ```

   With Claude Code 2.1.292 or later, `claude plugin install neocortex --marketplace AzureStackNerd/neocortex-plugin`
   does both in one command; add the same `--config`.

   `memory_folder` must be an absolute path to a folder that exists. The install checks neither (measured with Claude
   Code 2.1.306): with a relative path the server does not start, and with a folder that does not exist it starts but
   refuses every read and save, naming the folder.

   `claude plugin install` in a shell "never prompts for userConfig values"
   (<https://code.claude.com/docs/en/plugins/troubleshooting>). Without `--config` it installs the plugin and prints
   that the option is not set, and NeoCortex does not run until it is. Set it in a session with
   `/plugin configure neocortex@azurestacknerd`, or install in a session with `/plugin install neocortex@azurestacknerd`,
   which asks for it. The same `/plugin configure` changes the folder later; that does not move the memories in the old
   folder.

3. Turn off Claude Code's built-in auto memory, so NeoCortex is the only memory. The plugin cannot do this for you: of
   the settings a plugin ships, "Only two keys take effect from a plugin, agent and subagentStatusLine"
   (<https://code.claude.com/docs/en/plugins/components>). In a session, the auto memory toggle in `/memory` "saves
   `autoMemoryEnabled` to your user settings at `~/.claude/settings.json`" (<https://code.claude.com/docs/en/memory>);
   or add the key there yourself:

   ```json
   {
     "autoMemoryEnabled": false
   }
   ```

   Read this first if Claude Code has saved auto memories for you. "When false, Claude doesn't read from or write to
   the auto memory directory" (<https://code.claude.com/docs/en/settings-reference>). Their files are not deleted:
   "`MEMORY.md` and topic files stay until you or Claude edits or deletes them"
   (<https://code.claude.com/docs/en/memory>), in `~/.claude/projects/<project>/memory/`. To bring them into
   NeoCortex, run `/plugin:neocortex:neocortex:import_auto_memory` in a session in that repository, before or after you
   turn auto memory off; it imports one repository at a time. The prompt has Claude show you what would come in and
   ask you before it imports: one memory per topic file, and one per `MEMORY.md` list item that links no topic file,
   into that project's scope. The original files are never changed. A folder set with `autoMemoryDirectory` in your
   `~/.claude/settings.json` is read too: its user and feedback memories come into user scope, where they load in
   every project, in a preview of their own before the repository's; its other memories come into user scope
   unsorted, labelled `auto-memory-unsorted` and loaded only on request, and after the import Claude offers to move the
   ones that belong to the repository you are in into its project. As long as one of them is unsorted, every session
   start, but no subagent start, reminds Claude to offer the sort: into the repository, everywhere, or left to load
   only on request without the label. A file there that is no auto memory, neither
   `MEMORY.md`, nor linked from it, nor of an auto memory type, is named and not imported. NeoCortex reads only that
   key from that file, from the repository's `.claude/settings.json` and
   `.claude/settings.local.json`, and from the managed settings files; the import's answer names each folder those
   other files set, which it does not read, rather than saying there is nothing to import. A memory you delete after an
   import does not come back at the next one: the memory labelled `auto-memory-import` lists what was imported, and
   deleting it lets the next import bring back what you deleted. Each import rewrites that list and keeps only the lines with a
   hash; any other field you set on that memory stays.

4. Start Claude Code again. A plugin installed from the shell loads "the next time you start Claude Code, or when you
   run `/reload-plugins` in a session that's already open" (<https://code.claude.com/docs/en/plugins/install>).

## Check that it runs

In a session, `/mcp` lists the server as `plugin:neocortex:neocortex`, and it must show as connected. From a shell,
`claude mcp list` prints the same name followed by `✔ Connected` (measured with Claude Code 2.1.306). Connected does
not prove that the memory folder exists; see `memory_folder` above. When the server is not connected, check `node`
under [Requirements](#requirements) and `memory_folder` under [Install](#install).

`/plugin:neocortex:neocortex:status` shows that the server of this session is connected, which memory folder it
reads, its version and format, the memories it loaded per scope and those that are not active, and each record it
could not load, with the reason. A memory folder that cannot be read gives the reason on its first line. Claude Code
gives the prompt's text to Claude, not to you, so the prompt asks Claude to repeat the status to you as it stands, in
a code block. When the prompt is missing from the `/` menu, the server is not connected: check `/mcp` or
`claude mcp list`, as above.

## Use

You do not have to do anything. Claude saves a memory on its own when you correct it, confirm an approach, or tell it
about yourself, a decision or where something lives, and it updates a memory that turns out wrong. You can also ask it
to remember something, or to forget it. A memory has a scope: `user` loads in every project, `project` only in the
repository it was saved in, which NeoCortex knows by that repository's `origin` remote. Outside a repository, or in one
without `origin`, `project` means that folder only, not its subfolders.

At every session start Claude gets an index: one line for each active memory delivered by index, with its scope,
title, summary and id, as many as fit; the rest are counted, and Claude finds them with `search_memories`. Memories
delivered at startup come in full. Claude reads a memory in full when the detail matters. `/plugin:neocortex:neocortex:refresh` reloads your memories in a running session, for
example after another machine changed them.

A rule is a memory that loads on its own, at session start, with every prompt or short question, before every edit or
when a subagent starts, or that is enforced. Claude changes a rule only with `save_rule`, `update_rule` or
`delete_rule`, and Claude Code asks you before every call of them; see [Permissions](#permissions).

Claude is told to delete a memory only when you ask; to have Claude Code ask you as well, keep `delete_memory` out of
the allow rule (see [Permissions](#permissions)). A deleted memory moves to the folder `deleted` in your memory folder,
under the same path, from where you can put it back by hand. When that folder already holds a deleted copy of the same
memory, the later one is `<id>.2.md`, then `<id>.3.md`; rename it to `<id>.md` when you put it back.

## GitHub over SSH or HTTPS

The plugin's repository is public, so the install and every update clone it without a GitHub account or credentials.
For `owner/repo` shorthand, as above, "Claude Code checks whether your SSH key authenticates to `github.com`, then
clones over SSH if it does and over HTTPS if it doesn't" (<https://code.claude.com/docs/en/plugins/install>). For SSH,
"the host must already be in your `known_hosts` file and the key must work without a passphrase prompt" (same page).

To skip the SSH check and always use HTTPS, set
`CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` "to clone GitHub owner/repo shorthand sources over HTTPS instead of SSH. Applies to
plugin install and update, and to /plugin marketplace add and update" (<https://code.claude.com/docs/en/env-vars>).
Set it for your user account, so it also holds for updates in later sessions: on Windows
`setx CLAUDE_CODE_PLUGIN_PREFER_HTTPS 1`, which applies to terminals opened after it, and on macOS and Linux a line
`export CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` in your shell's profile.

## Permissions

The server's tools are `save_memory`, `update_memory`, `get_memory`, `search_memories`, `list_labels`,
`set_context_labels`, `delete_memory`, `save_rule`, `update_rule`, `delete_rule`, `import_auto_memory`,
`dream_report`, which shows how optimized NeoCortex is for this place and changes nothing, `dream_proposals`, which
lists the Dreamer's proposals that wait in this session and changes nothing, `dream_decide`, which records the user's
answer to one of them and changes no memory, and `hook_context`, which is for the hooks, not for Claude. `import_auto_memory` previews the memories Claude Code's auto
memory kept for this project, or those of the folder `autoMemoryDirectory` names, and imports
the plan of a preview. In a permission rule
a tool of the plugin is `mcp__plugin_neocortex_neocortex__<tool>`, and `mcp__plugin_neocortex_neocortex__*` matches
all of them (<https://code.claude.com/docs/en/plugins/components>). To let Claude use them without a permission prompt,
add the rule to `permissions.allow` in `~/.claude/settings.json`, next to the rules already there:

```json
{
  "permissions": {
    "allow": ["mcp__plugin_neocortex_neocortex__*"]
  }
}
```

That includes `delete_memory`. To keep the prompt for a delete, list the other tools one by one instead, such as
`mcp__plugin_neocortex_neocortex__search_memories`.

It also includes `import_auto_memory`, so Claude Code does not prompt before an import writes; only the conversation
asks you, as the prompt tells Claude to. To be prompted before every call of it as well, add it to `permissions.ask`
next to that allow rule:

```json
{
  "permissions": {
    "allow": ["mcp__plugin_neocortex_neocortex__*"],
    "ask": ["mcp__plugin_neocortex_neocortex__import_auto_memory"]
  }
}
```

"Rules are evaluated in order: deny, then ask, then allow. The first match in that order determines the outcome", and
"a matching ask rule prompts even when a more specific allow rule also matches the same call"
(<https://code.claude.com/docs/en/permissions>). The rule names the tool, not its mode, so it prompts for the preview
too (not measured).

The allow rule does not cover `save_rule`, `update_rule` and `delete_rule`: they write rules, and Claude Code asks
before every call of them. Such a call carries the text that will load, and each line of its `affects` names how many
characters a rule puts in front of Claude; how Claude Code shows a long argument in the prompt is not checked. "Allow
rules that match the tool don't skip the prompt either"
(<https://code.claude.com/docs/en/mcp>, Require approval for a specific tool). In `dontAsk` mode, and with
`--permission-prompt-tool`, Claude Code denies them. An application built on the Agent SDK is the exception: "The
Agent SDK's `canUseTool` callback does receive these calls and can approve them" (same page), so there the application
decides. A rule such an application approves is still named for 7 days in the index notices of every session start
where it loads that runs NeoCortex's hooks. NeoCortex's own tools refuse a call that would make a rule start or stop
loading unless a person confirms it in the client's prompt; what stays outside, such as two sessions or machines writing at once, clients that ignore the
flag and writes that bypass the tools, is in section 7 of `docs/design-rule-guard.md` in the NeoCortex repository.

## Known issues

These are open and will be fixed in a later version:

- A memory you deleted after importing it into one project can come back, into user scope, when you run the auto
  memory import in another project and the folder `autoMemoryDirectory` names holds the same text. Delete it again.
- A repository cloned through an SSH host alias on one machine, such as a `Host github-work` entry in
  `~/.ssh/config`, and directly on another gets a different project key on each, so each machine sees only its own
  project memories for it.
- Versions up to 0.4.0 left an empty folder in `.neocortex/locks` in your home folder for every memory folder they
  wrote to. They hold no memories; you can delete the empty ones.
- While a memory moves between scopes, a session that loads at that moment can report "same id as a memory in another
  folder" for it, although nothing is wrong.
- When two machines change the same memory before your sync tool has carried the first change across, one change can
  be lost; see step 1 under [Install](#install).

## Update

Auto-update is off by default for this marketplace, as for every marketplace that is not one of Anthropic's own or
added from claude.ai (<https://code.claude.com/docs/en/plugins/install>). To update, in a shell:

```shell
claude plugin update neocortex@azurestacknerd
```

Or in a session, run `/plugin` and choose either of these (<https://code.claude.com/docs/en/plugins/install>):

- On the **Installed** tab, select `neocortex` and then **Update now**.
- On the **Marketplaces** tab, select `azurestacknerd` and then **Update marketplace**, which "refreshes the
  marketplace's listing, updates the plugins you installed from it".

The new version "loads in your next session, or after you run `/reload-plugins` in a running one"
(<https://code.claude.com/docs/en/plugins/cli-reference>). Your memories stay: they are in your memory folder, not in
the plugin. What each version changed is in [CHANGELOG.md](CHANGELOG.md).

## Remove

```shell
claude plugin uninstall neocortex@azurestacknerd
```

The uninstall "also deletes the plugin's stored options and secrets and its data directory"
(<https://code.claude.com/docs/en/plugins/cli-reference>), so `memory_folder` is forgotten and a later install needs
`--config` again. The memory folder and its memories stay (both measured with Claude Code 2.1.306).
`claude plugin marketplace remove azurestacknerd` then removes the marketplace too. To have Claude Code's auto memory
back, use the toggle in `/memory` or remove `"autoMemoryEnabled": false` from `~/.claude/settings.json`.
