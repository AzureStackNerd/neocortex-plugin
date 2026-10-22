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
- Read access to the private GitHub repository `AzureStackNerd/neocortex-plugin`; see
  [GitHub over SSH or HTTPS](#github-over-ssh-or-https).

## Install

1. Create the memory folder, for example `C:\Users\<you>\NeoCortex` on Windows or `~/NeoCortex` on macOS and Linux.
   Any folder works. To share your memories between machines, choose a folder that syncs between them.

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
   into that project's scope. The original files are never changed. A folder set with `autoMemoryDirectory` is not
   imported yet: NeoCortex reads only that key from your `~/.claude/settings.json`, from the repository's
   `.claude/settings.json` and `.claude/settings.local.json`, and from the managed settings files, and the import's
   answer names each such folder rather than saying there is nothing to import. A memory you delete after an import
   does not come back at the next one: the memory labelled `auto-memory-import` lists what was imported, and deleting
   it lets the next import bring back what you deleted. Each import rewrites that list and keeps only the lines with a
   hash; any other field you set on that memory stays.

4. Start Claude Code again. A plugin installed from the shell loads "the next time you start Claude Code, or when you
   run `/reload-plugins` in a session that's already open" (<https://code.claude.com/docs/en/plugins/install>).

## Check that it runs

In a session, `/mcp` lists the server as `plugin:neocortex:neocortex`, and it must show as connected. From a shell,
`claude mcp list` prints the same name followed by `✔ Connected` (measured with Claude Code 2.1.306). Connected does
not prove that the memory folder exists; see `memory_folder` above. When the server is not connected, check `node`
under [Requirements](#requirements) and `memory_folder` under [Install](#install).

## GitHub over SSH or HTTPS

The plugin's repository is private, so the install and every update clone it with the git credentials on your machine,
without prompting. For `owner/repo` shorthand, as above, "Claude Code checks whether your SSH key authenticates to
`github.com`, then clones over SSH if it does and over HTTPS if it doesn't"
(<https://code.claude.com/docs/en/plugins/install>):

- SSH: "the host must already be in your `known_hosts` file and the key must work without a passphrase prompt" (same
  page).
- HTTPS: a stored credential. "On GitHub, `gh auth login` followed by `gh auth setup-git` stores one"
  (<https://code.claude.com/docs/en/plugins/host-marketplace>).

To skip the SSH check and always use HTTPS, as on a machine without an SSH key for GitHub, set
`CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` "to clone GitHub owner/repo shorthand sources over HTTPS instead of SSH. Applies to
plugin install and update, and to /plugin marketplace add and update" (<https://code.claude.com/docs/en/env-vars>).
Set it for your user account, so it also holds for updates in later sessions: on Windows
`setx CLAUDE_CODE_PLUGIN_PREFER_HTTPS 1`, which applies to terminals opened after it, and on macOS and Linux a line
`export CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` in your shell's profile.

## Permissions

The server's tools are `save_memory`, `update_memory`, `get_memory`, `search_memories`, `list_labels`,
`set_context_labels`, `delete_memory`, `save_rule`, `update_rule`, `delete_rule`, `import_auto_memory`, and
`hook_context`, which is for the hooks, not for Claude. `import_auto_memory` previews the memories Claude Code's auto
memory kept for this project, and imports the plan of a preview. In a permission rule
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
before every call of them. "Allow rules that match the tool don't skip the prompt either"
(<https://code.claude.com/docs/en/mcp>, Require approval for a specific tool). In `dontAsk` mode, and with
`--permission-prompt-tool`, Claude Code denies them. An application built on the Agent SDK is the exception: "The
Agent SDK's `canUseTool` callback does receive these calls and can approve them" (same page), so there the application
decides. NeoCortex's own tools refuse a call that would make a rule start or stop loading unless a person confirms it in
the client's prompt; what stays outside, such as two sessions or machines writing at once, clients that ignore the
flag and writes that bypass the tools, is in section 7 of `docs/design-rule-guard.md` in the NeoCortex repository.

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
the plugin.

## Remove

```shell
claude plugin uninstall neocortex@azurestacknerd
```

The uninstall "also deletes the plugin's stored options and secrets and its data directory"
(<https://code.claude.com/docs/en/plugins/cli-reference>), so `memory_folder` is forgotten and a later install needs
`--config` again. The memory folder and its memories stay (both measured with Claude Code 2.1.306).
`claude plugin marketplace remove azurestacknerd` then removes the marketplace too. To have Claude Code's auto memory
back, use the toggle in `/memory` or remove `"autoMemoryEnabled": false` from `~/.claude/settings.json`.
