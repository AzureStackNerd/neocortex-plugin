---
name: dreamer
description: Go through what the Dreamer found in this place with the user, one offer at a time, and change a memory only on the user's answer for it. Start it when the user types /neocortex:dreamer, asks in words to tidy their memories or what the Dreamer found, when the session start says index lines lost their summaries to fit it (a crowded start) or that proposals of the Dreamer wait here, and when the user asks for dream_report or dream_proposals by name. Use it instead of calling those tools and acting on their answers on your own reading.
allowed-tools: mcp__plugin_neocortex_neocortex__dream_report, mcp__plugin_neocortex_neocortex__dream_proposals, mcp__plugin_neocortex_neocortex__get_memory
---

# The Dreamer's findings, one offer at a time

You go through what NeoCortex's Dreamer found in this place with the user: first what it found, then each proposal,
then each change of the recommended plan, one at a time. Every offer says what it costs and where the memory still
arrives afterwards, because a memory that no longer arrives when its subject comes up is a memory the user has lost.
Never apply a plan as it stands, and never change a memory before the user answers for it: a write goes out only
after the user's yes for that one offer.

## 0. Is NeoCortex's server here?

Look at your own tool list. If no tool named `mcp__plugin_neocortex_neocortex__*` is there, this session's NeoCortex
server is not connected: say that the Dreamer's findings cannot be read because the server is not connected, and that
`/neocortex:status` says why. Say never that there is nothing to tidy, and change nothing.

## 1. What the Dreamer found

Call `dream_report`. If it fails, say that it failed and quote its error; never say that nothing was found, and change
nothing. Otherwise show the user, from its `place` (this place is its `project`):

- **The start.** Its size (`start`, with `characters`, `indexNotShown`, `indexShortened`, `startupNotFit`) against its
  budget: whether it fits its budget, or how far over it is. With no index line here (an empty store, or every memory
  on demand), say no memory has to move. When it fits, say every index memory shows with its summary and no memory has
  to move, and offer no move or rewrite.
- **The cost at each moment:** `prompt` (`characters`, `questionCharacters`) and `edit` (`characters`).
- **Near duplicates** (`nearDuplicates`: `pairs` counted, `shown` as pairs `a` and `b` with their `score`), by title.
- **Repeats** in other places (`repeats`): each copy here with how many other places hold one (`otherPlaces`), counted
  and never named; whether it may be promoted (`promotable`), and when not, what stopped it (`blockedBy`: a `rule`
  with the `token` that would leak, or what it must `wait` for: more `places`, the shared `label` its copies carry (or
  one still to be confirmed), copies all in places of one organisation (its `name`), or the `project` whose
  organisation is unknown).
- **Dead weight** (`deadWeight`: `expired`, `danglingLinks`, `olderHandoffs`) and records that did not load
  (`notLoaded`, per kind: `newerFormat`, `secondRecords`, `noMemory`, `unreadable`), with the index's pointer to
  repair them. A finding with no proposal is shown and offered no change.
- **The whole store** (`store`: `projects`, `memories`, `nearDuplicatePairs`, `repeatClusters`, `deadWeight`,
  `notLoaded`). When it holds only an `error`, say that part could not be read and why; the rest goes on.
- **The last run** (`dream`). With no `dream` field, no run is recorded yet: say that one runs on its own at most once
  a week, at the first prompt of a session, and that until then there may be no proposals. Otherwise say when it
  `started` and `finished`, its `outcome`, and how many proposals it `created`; a run still going may make more. One
  that stopped at its time budget says where (`stoppedAt`), and the proposals it made are kept. When `dream` holds
  `unreadable`, `newerFormat` or an `error`, the last run cannot be told; for `newerFormat`, update NeoCortex on this
  machine.

Each shown memory comes as an entry with its short `id`, its `scope`, its `title`, and where it applies: `appliesWhen`
(`inRepo`, `labels`, `files`) and its marks (`failure`, `callInput`, `reply`), the call, failure or reply it arrives
at: a `tool` and a `pattern`, or the `phrases` of a reply.

## 2. The proposals, one at a time

Call `dream_proposals`. If it fails, say the proposals could not be read and to try again; never that none waits. The
report's findings still stand. With nothing under `waiting`, say no proposal of the Dreamer waits here.

First say what is left over from earlier answers:

- `unfinished`: an accept here whose writes are not all made. Name it, say why it was left when `next` says, and offer
  to finish it: `dream_decide` with `accept` and the same id.
- `closedAccepts`: an accept that was not applied, or not fully, each with its `id`, `kind` and `reason`. Say so and
  why; ask no answer for it.
- `closed` (`unreadable`, `newerFormat`, `appliedUnreadable`, `appliedNewer`) and `notRead` (`unreadable`,
  `newerFormat`): how many proposals are closed or were not read because a record cannot be read or is of a newer
  NeoCortex. Say how many and what to do: repair the record the index names, or update NeoCortex on this machine.

Then offer each entry under `waiting`, one at a time. Each has its `id`, its `kind` (`duplicate`, `expired`,
`dead-link`, `repeat`, `old-handoff`), the memories it `shown`s, and `act`, what accepting it does (`supersede`,
`retire`, `drop-link`, `promote`). When it rests on memories of places this session does not show, say how many
(`others`) and never name them. Name each memory by title and short id, and say before the user answers:

- **A `supersede`** (a `duplicate` in one scope): which memory `survives` and which one it `retires`. The retired one
  stops arriving; the survivor arrives where it does today. When the retired one carries a mark the survivor does not
  take (`dropsFailureMark`, `dropsCallInputMark`, `dropsReplyMark`), say the moment no memory arrives at any more, in
  the words of `failureMarkGoes`, `callInputMarkGoes` or `replyMarkGoes`.
- **A `retire` of a `duplicate` across scopes**: the project copy it `retires` and the user memory that `stays`.
  Accepting deletes the copy; the user memory arrives where it does today, here and in every other place; the copy can
  be put back by hand from the deleted copies in the memory folder.
- **A `retire` of a `repeat`**: this project's copy it `retires` and the user memory that `stays` and covers it, which
  goes on arriving in every place, or where its condition holds; and how many other places hold a copy, counted.
- **A `retire` of a copy with no `stays`**: no user memory repeats it any more. Say so, warn that deleting it could
  lose its text, and recommend keeping it; delete it only on the user's explicit yes after that warning.
- **A `retire` of an `expired` memory**: the memory and its end date (`expires`). Accepting deletes it; it already
  arrives nowhere, being past its date; it can be put back by hand from the deleted copies.
- **A `retire` of an `old-handoff`**: the older handoff it `retires` and the `newer` one of the same name, which stays
  and is what the next session starts from. With no `newer`, the newer handoff of that name is not there: deleting this
  one could leave no handoff of that name to start from. Recommend keeping it; delete it only on the user's explicit yes
  after that warning.
- **A `drop-link`** (`dead-link`): the memory and the links to memories that no longer exist (`targets`). Accepting
  removes only those links; the memory arrives as before.
- **A `promote`** (a `repeat`): the memory and how many other places hold a copy, counted and never named. Accepting
  makes it a user memory: with `loadsEverywhere` it arrives in every place; with `carriesCondition` only where its
  condition holds; its marks go with it (`carriesFailureMark`, `carriesCallInputMark`, `carriesReplyMark`).
- **No `act`**: an old proposal that carries no writes. Accepting it changes nothing; offer `reject` or `keep-both`.

One answer per proposal: it is given for every memory the offer names. Record it with `dream_decide` and the
proposal's whole `id`: `accept` applies it and you say what changed; `reject` and `keep-both` are recorded, change
nothing, and the proposal is not offered again.

## 3. The recommended plan, one change at a time

After the proposals, call `dream_report` again, so the plan is priced on the store as it now is. Then, from
`indexBudget`:

- When the start fits its budget, say so; no change is offered.
- `startOnly`: how many index memories reach Claude only at the start. None of them is offered to move; each needs a
  moment of its own (a call, failure or reply mark, or a file rule) before it can leave the start. Where it is above
  0, say also that the user can give those memories a moment of their own with `/neocortex:therapy`, which offers one
  to the memories that show only their title at a session start. Where `startOnly` is 0, say no sentence about
  start-only memories and point at no command for them.
- When no plan fits (`fitting` under `plans` is empty), say no combination of shorter summaries and moves takes the
  start under its budget, with the report's reason and advice (the `next` of `plans`, and the budget's own `next`);
  offer no change of your own and delete nothing.
- Otherwise take the recommended plan, the first under `fitting`: name its cost (`changes`, how many are user
  memories in `userScope`, what it costs other places in `elsewhere`, and the start's size after it in `characters`)
  and that other plans fit (`today` is the start without any change: its `characters`, and how many lines are
  `shortened` or `unshown`). The move list (`candidates`, `toFit`, `freed`,
  `others`) is for that picture; the offers are the plan's.

Offer each change of that plan one at a time. Before each, call `get_memory` for that memory. If its `revision`
differs from the plan entry's `revision`, the memory changed since the report: call `dream_report` again and offer it
as it now is, or not at all when it would then arrive only by search or its id, and say why. A memory that is gone is
said and passed.

- **A move** (an entry under `demote`): it stops showing at this place's start, and it still arrives where
  `arrivesAt` says, in the report's own words, and by search or its id; it can be put back on the start. For a user
  memory it leaves the start of every project, not only this one. On yes: `update_memory` with
  `update_memory.delivery=on-demand` and the entry's `revision` as `update_memory.revision`.
- **A rewrite** (an entry under `shorten`): show the summary today and a shorter one you propose, of at least 120
  characters and at most the entry's `summaryLength` `after` (its length `today` is the one now), with both lengths.
  It stays at the start with that summary; the summary it has now stays readable with the memory afterwards, so it can
  be put back by hand. For a user
  memory every other place shows the shorter summary too. On yes: `update_memory` with `update_memory.summary` and the
  entry's `revision` as `update_memory.revision`. When the user gives a summary of their own, show it back and write
  it as given; when it is longer than `after`, say the start may no longer fit.

When the user says no, nothing changes for that memory: say the plan's other changes no longer take the start under
its budget on their own, and go on with the next. A change declined in this walk is not offered again in it.

## 4. Answers, refusals and the end

- **One at a time.** A question or an unclear word ("maybe", "what does that do?") records nothing: answer it, or ask
  again, until a clear answer for that offer. A skipped or unanswered offer changes nothing and comes again with the
  next `/neocortex:dreamer`.
- **"All the rest."** When the user asks to apply all the remaining offers, or the whole plan, show every remaining
  offer with its cost and where its memory still arrives in one reply. That one answer then stands for exactly the
  offers shown; the writes still go one at a time, each with its own checks, and a refusal stops that offer only.
- **A refused write.** When NeoCortex refuses a write, or answers with a `next` step, say why in plain words and offer
  the way out it names: the record to repair or keep, to reject the proposal or keep both, to retire this copy instead,
  or to accept again to finish a write made in part. A memory another session is writing: try again. A memory changed
  since its offer: offer it again on its new text. A write that would change which rules load is refused and names
  its twin: `dream_decide_rule` for an accept, `update_rule` for a move or rewrite, which Claude Code asks the user to
  confirm with what it affects; on decline nothing changes and a proposal still waits.
- **The end.** When every offer is answered, or the user stops, call `dream_report` again and say, per memory, what
  changed and what did not, and whether the start now fits its budget or by how much it is over. What was not answered
  is unchanged and comes again with the next `/neocortex:dreamer`.
