# Changelog

The sections up to 0.3.1 were written afterwards from the history.

## 0.5.10 - 2026-11-09

- Changed: the README's sections on when and where a memory loads are rewritten as short paragraphs and lists, with
  rules for one kind of file, refused changes and using one store from several machines each in a section of their
  own. They now also say that a rule for a kind of file arrives once at the first read or search of such a file, as
  it does since 0.5.9. NeoCortex itself does not change.

## 0.5.9 - 2026-11-09

- New: a rule about one kind of file, such as how you write Bicep files, arrives when Claude writes, edits, reads or
  searches such a file, wherever it lies, also in a subagent, and at no other edit, read or search. At a write or an
  edit it arrives every time; at a read or a search once per conversation and once in each subagent, and again after a
  compaction or `/clear`. Ask Claude to remember it: Claude proposes the pattern, such as `**/*.bicep`, and you confirm
  it like any rule. The confirmation names the rule's files, and a new rule that would leave another one without room
  at the edit of some file names that one.
- A search filtered only by a language type brings the rule when the type is the file's extension, such as `ts` or
  `py`; for a type such as `ps`, `csharp` or `rust` the rule arrives at the first read of such a file instead.
- For now, a pattern that names a folder, a file pattern on a project memory or on an exception, and a change of a
  saved rule's pattern are refused, with the reason.
- Changed: Read, Grep and Glob now call NeoCortex's hook as well; each call takes about as long as the edit hook
  already does.
- If you run NeoCortex from a clone, the plugin needs nothing, but your `settings.json` does. Run
  `node "<clone>/scripts/update-clone-hooks.ts"` and then again with `--apply`: it gives the edit entry its new inputs,
  the file, the session and the agent. It never adds a hook, so it lists the three new `PreToolUse` groups for `Read`,
  `Grep` and `Glob` with the JSON to paste, which README.md shows as well. Until the edit entry is changed, every rule
  for a kind of file arrives at every edit with a warning that names them; until the read groups are added, such a
  rule arrives at writes and edits only.
- If several machines share your memory folder, update each one: a machine still on 0.5.8 refuses to save a rule for a
  kind of file, and delivers one saved elsewhere at every edit, with a warning, and at no read.

## 0.5.8 - 2026-11-08

- New: you can have Claude record that inside repos a memory replaces a general rule, as an exception that applies
  only inside a repo. Saving or changing one asks you first, as any change to which rules load does; once confirmed,
  the rule no longer arrives in any repo, at any moment, and still arrives in folders outside one. A label or a file
  pattern on such an exception is still refused, with the reason.
- New: reading a memory that does not apply where you are says why, for example "not here: needs a repo", and the
  answer to saving or changing an exception names the memories it replaces.
- Changed: when a memory cannot be read, such as a broken file or one written by a newer NeoCortex, a change to an
  exception of it is refused, with the reason, whenever it would change where that memory is hidden: here, in a repo
  or folder elsewhere, or in another project. Before, some of those changes went through without a word.
- If several machines share your memory folder, update each one: a machine still on 0.5.7 cannot save or change such
  an exception and lets through the changes above that 0.5.8 refuses, and one on 0.4.x reads no memory of the current
  format, so it still delivers the rule in repos.

## 0.5.7 - 2026-11-08

- Changed: a change to a memory that is not yet on the current format now first keeps a copy of the file exactly as it
  was, under `previous/` in your memory folder, as the upgrade of 0.5.6 does. That covers a memory the upgrade has not
  reached yet, one it left behind, and one that came back on the old format later, such as a memory restored by hand
  or saved on a machine that was not updated. It holds for a change by Claude, a move to another scope, an accepted
  Dreamer proposal, the auto memory import and `move-folder-memories`, and one copy per memory is kept: a later change
  keeps no second one. When that copy cannot be written, the change is refused with the reason and the memory stays
  as it was.
- New: while memories still wait to be brought up to date, the session start says how many, in Claude's context and
  as one line on screen; a subagent's start and the refresh prompt say it too, until none wait. A memory the upgrade
  could not bring up to date, such as a file you need to repair by hand, counts there in any project until you repair
  or remove it.

## 0.5.6 - 2026-11-08

- Changed: NeoCortex now brings every memory saved by an earlier version up to the current format by itself, once, in
  the background of the first session after you update: your user memories and those of every project, not only the
  one you are in. Before it rewrites a memory it keeps a copy of the file exactly as it was, under `previous/` in your
  memory folder, which you can copy back by hand; NeoCortex never changes or removes those copies. A memory reads,
  shows and loads exactly as before: its text, dates, labels, links, end date and condition are kept, and no rule asks
  for a confirmation or shows as changed. A `format.json` in your memory folder records that the upgrade finished.
- A memory NeoCortex cannot bring up to date, such as a file with a typo in its frontmatter, stays exactly as it is.
  Session starts name it with the reason until you repair or remove it by hand; the next session then brings it up to
  date.
- Update every machine that shares your memory folder, one at a time: update one, let its first session finish the
  upgrade and the folder sync, then update the others. After the upgrade, a machine on NeoCortex 0.4.32 or older can
  read none of your memories: it says they were written by a newer NeoCortex. Update it.
- Once, at this upgrade, the Dreamer asks again about clean-ups you already answered for the memories it brought up to
  date: a proposal still waiting is withdrawn and proposed again at its next run, one you rejected or kept both on is
  asked once more, and one you accepted that was not yet applied is closed with its reason and writes nothing. Accept
  it again if you still want it.

## 0.5.5 - 2026-11-08

- Fixed: a Dreamer proposal you accepted no longer stays unfinished for good when one of its memories changed, moved
  or was deleted before the accept could be applied. The list of proposals now shows it as closed, with its reason:
  what changed, and what of the accept was written and what was not. Accepting it again gives the same reason and
  writes nothing, and a later proposal over the same memories takes its place. An accept refused for something you
  can clear, such as the rule confirmation or a record to repair, still finishes with the same accept, and one whose
  changes have all landed is marked applied.
- Using one memory folder from several machines? Update NeoCortex on each: a machine still on 0.5.4 keeps listing
  such a proposal as unfinished.

## 0.5.4 - 2026-11-07

- Fixed: accepting the Dreamer's proposal to share one of your project rules with your other places no longer loops on
  a refusal. One prompt shows the rule's text and where it starts loading; confirmed, the rule loads there from the
  next prompt. Such a proposal you accepted on an earlier version, and that never applied, finishes with the same
  accept as long as the rule has not changed since.
- Using one memory folder from several machines? Update NeoCortex on each: a machine still on 0.5.3 keeps refusing
  such an accept.

## 0.5.3 - 2026-11-07

- Changed: the condition of an existing rule can be narrowed, widened or removed in one change you confirm, such as
  moving one customer's rule to another customer's label, or letting it apply everywhere again; the rule keeps its
  id, so the exceptions that name it keep holding. Before, that took three confirmed steps. A rule that is itself an
  exception to another memory still cannot have its condition changed.
- Changed: every question about a rule names, by their labels, the places where it starts or stops loading, such as
  `in other places (labels contoso)`. When none of your memories has a condition, the questions read as before.
- Using one memory folder from several machines? Update NeoCortex on each: a machine still on 0.5.2 refuses a change
  of a rule's condition and asks its questions without labels.

## 0.5.2 - 2026-11-07

- Added: a rule can apply only in places with a context label, such as one customer's rules in the places labelled for
  that customer. Ask Claude to give a rule a label and confirm it as any rule change; the rule then arrives at the
  session start, at every prompt, at a short question, at every edit and in every subagent only in places with that
  label. A place nobody has labelled yet hides nothing, so such a rule arrives there until the place is labelled.
  Claude proposes a label for a rule that belongs to one kind of place, and you confirm it. An existing rule gets a
  label in three steps, each confirmed: make it a note, give the note the label, and make it a rule again; it keeps
  its id.
- Changed: a change of a place's labels that would make a rule start or stop loading there now asks you first and
  names the rules (the first ones and the number of the others when there are many); without your yes, the labels stay
  as they were. A place's first labels still take effect at once. While a memory in your user scope or this project
  cannot be read, a change of a place's labels is refused, naming that memory.
- Using one memory folder from several machines? Update NeoCortex on each: a machine still on 0.5.1 changes a place's
  labels without asking, also when that starts or stops a labelled rule.

## 0.5.1 - 2026-11-07

- Added: a rule can apply only inside a git repository. Ask Claude to keep a rule, such as a definition of done, to
  sessions inside a repo, and it arrives at the session start, at every prompt, at a short question, at every edit and
  in every subagent only there, never in a folder outside one. Claude proposes this condition itself for a rule that
  only makes sense in code work, and you confirm it as any rule change. A rule still takes no labels and no file
  condition. An existing rule gets the condition in three steps, each confirmed: make it a note, give the note the
  condition, and make it a rule again; it keeps its id.
- Using one memory folder from several machines? Update NeoCortex on each: a machine still on 0.5.0 delivers a rule
  that applies only inside a repo in folders too, at its prompts, edits and subagents.

## 0.5.0 - 2026-11-07

- Added: a memory can say where it applies. Ask Claude to keep a memory to places with a given context label, such as
  "only for places labelled contoso", or to sessions inside a git repository, and it stores that as the memory's
  condition. The memory then loads at a session start, a subagent's start and a refresh only where its condition holds,
  with the condition on its index line. `search_memories` still finds it everywhere and says what it would need, such as
  "not here: needs label contoso". A place whose labels nobody has set yet hides nothing, so such a memory shows there
  until the place is labelled. Every later change keeps the condition unless you ask Claude to remove it, and a label
  change made in another session counts at the next answer here.
- A condition is refused, with the reason and with nothing written, on a rule, on a memory that overrides another, and
  as a file condition, none of which is possible yet, and on a project memory, which already applies only in its own
  project. `save_memory`, `save_rule`, `update_memory` and `update_rule` now also refuse a field they do not know,
  instead of dropping it while the answer read as a success.
- Changed: any memory may now override another, not only a project memory a user one, and the memory it overrides stops
  loading only where both apply. An override is still refused for an enforced memory, and now also for one that is more
  specific than the override: there, change that memory itself.
- Changed: memories are now written in format 2.0. NeoCortex 0.4.32 and older cannot read a memory this version saved
  or changed: they say it was written by a newer NeoCortex and leave it alone, and their session start leaves it out.
  Update every machine that shares your memory folder.

## 0.4.32 - 2026-11-05

- Fixed: a shared memory proposed for all your projects could show a second copy from the project you were in, as though
  both would be shared, while only one of them was. It also counted that copy as another project. It now shows your one
  copy and one from each other project, and counts only those.

## 0.4.31 - 2026-11-05

- Each place now records its remote's organisation and repository, learned on your next visit, so a run in one project
  can tell whether a memory belongs to one customer or to all of them. Nothing is added to a place whose labels you have
  not set yet, so the first-visit reminder still appears.
- The Dreamer now proposes a memory that repeats in three or more of your projects to be shared with all of them, once
  per project, moving one copy to user scope and leaving your wording alone. Two things it will not do: it never shares
  a repeat whose projects share a context label or belong to one customer (that waits for a later release), and it
  never shares text that names one of those organisations, repositories or labels. A copy that only repeats the same
  rule in other words is left alone rather than rewritten.
- Fixed: the proposal format is no longer pinned when a record is read. Before this, raising the format would have made
  every proposal and answer that was already stored read as invalid instead of as an older version.

## 0.4.30 - 2026-11-04

- Claude now searches your memories with your own words. A search finds only memories that hold every word in it, so
  a word Claude added on its own could hide the very memory you meant. Only when your words find too little does Claude
  swap one of them for a synonym or a broader word, and it tells you which.

## 0.4.29 - 2026-11-03

- Fixed: with the memory folder on a drive that has no hard links (FAT or exFAT, and some network shares), the Dreamer
  could not store a single proposal and no proposal could be answered: every run ended as failed and started again in
  the next session. Proposals, answers and applies are now written another way there, which still keeps each record
  whole and still refuses one that is already there. On such a drive, two machines that answer the same proposal at the
  same moment can overwrite each other's answer; on a drive with hard links nothing changes.
- Fixed: a file held open for a moment by a virus scanner or a sync client no longer turns a stored proposal or answer
  into an error, after which asking again said it was answered already, and no longer starts a Dreamer run that was
  not due.

## 0.4.28 - 2026-11-03

- Fixed: a memory you deleted in one project could come back through another project's import of Claude Code's
  shared auto memory. When the folder `autoMemoryDirectory` names, or the pinned folder, held the same text, the import
  offered it into user scope, where it then loaded in every project. The import of the shared folders now reads every
  project's import record and keeps out what any of them imported before, whether or not that memory is still there.
  Those memories are listed apart as `suppressed`, each with the project whose record lists it and where that record
  is, and the answer says how to have one anyway: save its text yourself, or remove its line from that import record.
  This place's own auto memory folder still imports into this project as before.
- When another project's import record cannot be read, or its folder cannot be listed, only the import of the shared
  folders waits until that is repaired; this place's own folder still imports, and the answer says so. A record written
  by a newer NeoCortex asks you to update NeoCortex on this machine.

## 0.4.27 - 2026-11-03

- Fixed: `dream_decide_rule`, which applies a Dreamer proposal that changes a rule, now takes `accept` only. A `reject` or
  `keep-both` through it asked you to confirm a rule change that then could not happen; those two answers change no
  memory and belong to `dream_decide`, which asks you nothing.
- The README's description of the Dreamer is now what a user needs: when a run starts, what it reads, the four kinds it
  proposes with what accepting each one does, and what it does not do yet. Nothing about the plugin points at the
  source repository any more.

## 0.4.26 - 2026-11-03

- The Dreamer now proposes on its own, at most once a week: on the first prompt of a session, when its last run is more
  than 7 days old -- or sooner, after a run that failed or was interrupted, since those are never waited out -- the
  server starts one run in the background after the answer to that prompt is composed, so the hook never waits on it.
  The run reads this session's own memories, makes the deterministic proposals for near-identical duplicates, expired
  memories, dead links and old handoffs, and stores them; it writes no memory, only proposals and one run record in
  `dream/run.yaml` beside `proposals/`. It has 5,000 ms of wall clock and keeps what it made when that runs out, and
  `dream_report` now shows the last run and where a budget stop stopped. Two sessions on one machine cannot both claim
  the run, and a run that dies leaves the next session free to try again. Still nothing across projects: a memory
  repeated in three or more of your projects is not proposed on yet.

## 0.4.25 - 2026-11-02

- The README now says what you can control about a memory, instead of leaving it to Claude:
  - A new "When a memory loads" table lists all seven `delivery` values — `index`, `startup`, `prompt`, `question`,
    `subagent`, `edit` and `on-demand` — with when Claude gets the memory and what it gets, and which of them cost you
    every prompt rather than once a session.
  - "What a memory holds" describes every field `save_memory` and `update_memory` take, including the ones the README
    never mentioned: `expires`, `supersedes`, `overrides`, `overrideReason`, `importance` and `enforced`.
  - "Finding a memory again" covers `search_memories` (its query, labels, `includeInactive`, and the report of memories
    that exist but could not be read) and `list_labels`.
- Under the hood, and nothing you see yet: the Dreamer's proposals, decisions and apply records are now kept in the
  store, in a `proposals` folder beside `places`, so they outlive the session. Nothing created a proposal in a live
  session yet at this version, which is why this part changed nothing you could see; 0.4.26 is where the Dreamer
  started making them.

## 0.4.24 - 2026-11-02

- Fixed: a rule write now tells you, in the lines you confirm, when a rule's text will not arrive. Until now you only
  found out at the next session start, in a warning, and the rule was not in context until then.
  - A line that ends with `, N characters` adds ` (more than one hook part holds, so it does not load)` when the rule's
    block is larger than one hook part, or ` (here, the other rules at that moment leave no room, so it does not load
    here)` when rules that sort before it, such as an enforced rule, take the room it needs.
  - A rule the write pushes out of its moment gets a line of its own: `<id> "<title>" here: this write leaves it no
    room at the prompt and question moments, so its text does not load there`. A newer rule sorts before an older one
    of the same rank, so this is the usual case: save a second `prompt` rule of 6,000 characters beside a first one,
    and the first one is named. At most five are named; the rest are counted in one line.
  - Both are judged in the project you are in; another project may have more or less room. Nothing is refused: the
    write goes through when you confirm it.

## 0.4.23 - 2026-11-02

- Fixed: `dream_report`'s `indexBudget` and its `plans` no longer recommend different sets of memories to move. The
  older text used to pick the longest index lines whatever scope they belonged to, so it named user memories whose move
  stops them loading at the start of every project, while the plan beside it reached the same budget without moving any.
  It now takes this project's own memories first, like the plan, and when a plan fits without moving a user memory the
  text names none and points at that plan instead. `toFit` names the fewest moves in that order, so on a store whose
  long lines are the user ones it can count a few more moves than the old ranking did.

## 0.4.22 - 2026-11-02

- Added: the Dreamer works out its own clean-up proposals now, without a model: near-identical duplicates, expired
  memories, links to a memory that no longer exists, and an older handoff. Each waits for your answer in
  `dream_proposals` and an `accept` carries it out as before. Nothing proposed yet in a session at this version: the
  store the server kept was in memory and stayed empty until something ran the finder, which 0.4.26 is the first
  version to do.
- Changed: a handoff counts as old only against another handoff of its own name, so a project with two live
  workstreams keeps a handoff for each instead of counting the second as dead weight. `dream_report`'s count changes
  with it.
- Fixed: a proposal closed because the record of an earlier apply of it could not be read used to disappear from
  `dream_proposals` without a count. It is counted under `closed` now, and the answer says what that means.
- Fixed: an answer recorded by an earlier version still reads after a format change. The answer record no longer names
  one version, so a later one cannot silently make every earlier answer unreadable.

## 0.4.21 - 2026-11-02

- Added: `dream_decide` takes `accept`, which writes the clean-up the proposal describes instead of only recording the
  answer. Each write goes through the same rule guard as `update_memory` and `delete_memory`, in the order the proposal
  states, and a write that fails stops the rest: the answer stays recorded, `dream_proposals` lists that proposal's id
  under `unfinished`, and the same accept again finishes what is left and changes nothing already done. A plan that
  cannot start at all -- a memory changed, moved or deleted since the proposal was made, one that cannot be read, an id
  with two records, a link `update_memory` would refuse, or a memory this session may not write -- is answered before
  anything is written, so the proposal stays open for you to read and refuse.
- Added: the tool `dream_decide_rule`, the confirmed twin of `dream_decide` for an accept whose writes change which
  rules load. Claude Code asks you in its own prompt on every call of it, and an accept that would change a rule through
  `dream_decide` is refused and names this tool instead.
- Not changed, deliberately: nothing created proposals yet at this version. The store the server kept was in memory
  and stayed empty until something added the finder's proposals, so you saw no proposal to accept; 0.4.26 is where
  they started arriving.

## 0.4.20 - 2026-11-01

- Changed: a session's load reads 8 records at a time instead of one. On a memory folder a sync tool keeps current,
  each file costs a round trip, so the old load spent most of its time waiting: on the owner's store (267 records) a
  session's load went from 116 to 44 ms and the whole-store read from 193 to 81 ms. This speeds up every session start
  and every prompt, not only `dream_report`. The memories, their revisions and the order of the problems reported are
  exactly as before. The bound is `LOAD_CONCURRENCY` in the store contract, so another backend picks its own.
- Not changed, deliberately: a memory folder on a mapped network drive or a spinning disk was not measured, and on such
  a share parallel reads could be slower than one at a time.

## 0.4.19 - 2026-11-01

- Changed: `dream_report` answers faster on a large store. The search for ways under the start budget now prices only
  whether a way fits while it walks, and works out how much room its rewrites buy for the three ways it names. On the
  owner's store that is 736 fills of the start down to 413, the search 129 ms down to 69, and the whole report about
  90 ms faster. Every way the previous version named is still named, with the same memories; nothing in the answer
  changes.

## 0.4.18 - 2026-11-01

- Changed: the way `dream_report` recommends now moves this project's own memories before the memories every project
  loads at its start. A user-scope memory moved down stops loading at the start of every other project, so the report
  prefers a plan that changes a few more memories here over one that empties somewhere else. The answer still says how
  many of the memories it moves are user memories. On a store where the shortest plan also hurts other projects, that
  plan is still named, so the choice stays yours.
- Changed: both levers are now searched in both orders — largest line first and this project first — and every count is
  priced rather than stopping at the first that fits, which the new ranking needs.

## 0.4.17 - 2026-11-01

- Added: `dream_report` now shows every way under the start budget, not demotion alone (`indexBudget.plans`). It
  prices moving memories off the start, rewriting their summaries shorter, and the two together, each way priced by
  re-running the start's own fill rather than estimated. It recommends one, fewest memories changed first, and says in
  the same sentence what that recommendation costs outside this place: how many of the memories it moves down stop
  loading at the start of every other project, and how many it only shortens keep a shortened line there. Nothing is
  written; every change a way names is your own `update_memory` call, one memory at a time.

## 0.4.16 - 2026-11-01

- Changed: the server instructions no longer say `delivery: index` is right for almost everything. Claude is told to
  choose `on-demand` for reference memories and for any memory whose summary alone is not actionable, that this
  choice at a save needs no confirmation, and that moving an existing memory to `on-demand` is a rebalance, which is
  your call. Handoffs stay at `index`, because the next session starts from them. `search_memories` and `get_memory`
  still reach an on-demand memory, so it is out of sight at a start, never lost.
- Changed: `save_memory`'s `delivery` field carries the same clause where Claude sets the value. `index` remains the
  field's default; no format change, no new field.

## 0.4.15 - 2026-10-31

- Added: a session start whose index has to shorten lines or leave them out now says so once, in part 1, with both
  counts and the way out. Claude reads the moves `dream_report` prices, offers them to you, and calls `update_memory`
  with `delivery on-demand` only when you agree, one memory per call; the same call with `delivery index` puts a memory
  back on the start. The line is absent from a start that fits, from a subagent's index and from the legacy `startup`
  command, and `/refresh` packs its parts without it. Part 1 keeps room for the line in its longest form, so where
  index part 2 begins does not depend on the counts, and a place under its budget delivers exactly what it delivered
  before. On a store that needs it, part 1 shows one or two more shortened lines than before: the line pays for itself
  out of that part's room.
- Changed: the server instructions tell Claude to offer the shrinking at most once per session, to call
  `update_memory` only on your yes, and to name the way back.

## 0.4.14 - 2026-10-31

- Added: `dream_report` now prices the start index memory by memory (`indexBudget`). It lists the index memories that
  could move off the session start — the ones that are neither enforced nor of high importance, largest line first, at
  most 20 with the rest counted — and says how many of those moves bring the start under its budget (`toFit`) and how
  many characters they free (`freed`). It changes nothing: the move is `update_memory` with `delivery on-demand`, one
  call per memory, which you confirm, and the same call with `delivery index` puts a memory back on the start.
  `toFit` is `null` when even moving every candidate cannot fit the place, because enforced and high lines alone
  overflow it.
- Changed: `dream_report` counts the index lines the start leaves out (`indexNotShown`) from the start's own fill
  instead of reading the number back out of the delivered text, so a memory whose summary quotes that line can no
  longer change the count.

## 0.4.13 - 2026-10-31

- Fixed: a subagent's index and the legacy `startup` command receive part 1 alone, but they were packed as if parts 2
  and 3 would follow. On a store over the budget they showed summaries dropped (`— …`) to make room in parts nobody
  reads, and their last line pointed at an index part 2 that never arrives. They now show whole lines and count what
  does not fit: on a 600-memory store of 16-character titles and 200-character summaries, 40 whole lines and a count of
  560 replace 237 lines of which every one had lost its summary and 354 pointed at part 2, with the remaining 9
  counted only at the end of a part 3 they never receive. The session start is unchanged.

## 0.4.12 - 2026-10-31

- Changed: when the index of a place does not fit the session start, the lowest-ranked memories now show their title
  and id without their summary (`— …`) instead of being left out; only if it still does not fit are the last ones left
  out and counted. Enforced and high-importance memories always keep their summary, and an index that fits is
  unchanged. Claude reads a shortened memory with `get_memory` before acting on it. `dream_report` counts the
  shortened lines (`indexShortened`).

## 0.4.11 - 2026-10-31

- Added: the tool `dream_decide` records your answer to a proposal that `dream_proposals` lists: `reject` or
  `keep-both`, and neither changes a memory. An answered proposal is not asked again; once a memory it shows changes,
  a proposal on the changed memory asks again. At a session start and in the `refresh` prompt, the index says how many
  proposals wait here for your answer, without naming them. Nothing creates proposals yet, so you see no such line
  until a later version adds the Dreamer's first proposals.
- Changed: at a session start, part 1 of the index keeps room for that line, so it may hold one index line fewer than
  before; that line moves to part 2.

## 0.4.10 - 2026-10-30

- Added: the tool `dream_proposals` lists the Dreamer's proposals that wait in the session you work in for your
  answer, and changes nothing. A proposal is listed only where every memory it shows belongs: your user memories or
  this project's. Of memories elsewhere it gives only how many there are, never their titles, ids or projects. Nothing
  creates proposals yet, so it lists none until a later version adds the Dreamer's first proposals.

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
