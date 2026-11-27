---
name: therapy
description: "At a correction, when the correction check's answer names a suspect or a memory that never arrived, or a memory whose arrival hurt (one that stopped a call in this conversation, also when the correction is about something else), or the user says a memory misled Claude, diagnose that memory beside the correction, with the evidence for it: say whether its wording misled it (words that say other than its why, an intent that no longer holds, or words that do not land) and reword or reframe it on the user's answer; for a memory that did not arrive when it should, propose the moment of its arrival and write it; for one whose arrival hurts, offer a form that hurts less in place of the moment that hurt (a narrower cue, taking back read first, a failure mark, a reply mark, its index line) with what it gives up, and write it. When the user types /neocortex:therapy, or asks to go through their memories, sit down over every memory this place sees: one list of the memories that misfire, grouped when long, then one treatment at a time. It changes nothing without the user's answer for that memory."
allowed-tools: mcp__plugin_neocortex_neocortex__get_memory, mcp__plugin_neocortex_neocortex__search_memories, mcp__plugin_neocortex_neocortex__memories_in_play, mcp__plugin_neocortex_neocortex__count_cues
---

# Treating a memory that made Claude act wrongly

You treat memories that made Claude act wrongly, with the user. Five dysfunctions, and each has one treatment:

| Dysfunction | What shows it | Treatment |
| --- | --- | --- |
| 1. does not arrive when it should | a memory of this place whose subject is what the user corrected did not reach you before the mistake: it did not arrive, reached you only as its name in a warning, or its moment did not come | give it a moment of its own: a call, failure or reply mark, or a condition that takes this place in |
| 2. arrives in a way that hurts | it stopped a call in this conversation, or the user says it arrives so often it disturbs their work | give it a form that hurts less, in place of the moment that hurt: a narrower cue, taking back read first, a failure mark, a reply mark, its index line |
| 5. misleads: worded wrongly | its summary or body says other than its why and the user's own words | reword it back to the intent |
| 6. wrong at heart | the intent no longer holds: the code, doc or skill it names says otherwise, what it names is gone, or the user's later words contradict it | reframe it: keep the why, change the conclusion or narrow where it applies |
| 7. right but does not land | it is vague, a list of keywords, or too long to show | reword it for clarity, same meaning |

Anything else about **when** a memory arrives — it arrived too late, or arrived and was ignored — is not this
skill's yet: say so in one line and do not treat it here. A memory that did not arrive when it should is this skill's,
as dysfunction 1, and its treatment is a moment, never a wording. A memory whose arrival hurts is this skill's too, as
dysfunction 2, treated with a form and never a wording.

The diagnosis is a hypothesis you show with its evidence, never a verdict. The user may dispute it and is right to.

## 0. Which sitting is this

Two starts, and they are different sittings. Read this first, every time.

**You started it, at a correction**, because the correction check's answer named a suspect, a memory that did not
arrive, a memory whose arrival hurt, or the user said a memory misled Claude. Treat what that answer named alone,
beside the correction — its suspects, the memories it says did not arrive, and those whose arrival hurt — and go
straight to section 1, working through sections 1 to 6. Show no list of the store, offer no other memory, and ask
nothing about looking the store over (S65). The correction's own words stay the evidence, as section 2 says. Every
memory the answer names for a stop is treated here, whether or not the correction is about that stop: you start this
skill for it and diagnose it even when the mistake the user corrected was something else (S144).

**This branch names the label.** The diagnosis here opens with the table's own label, verbatim and in bold —
**does not arrive when it should**, **misleads: worded wrongly**, **wrong at heart**, **right but does not land** or
**arrives in a way that hurts** — and never with a description in its place. It is the one sentence that may not be
skipped at a correction, where 7 of 15 live runs described the dysfunction ("the wording is wrong", "the intent no
longer holds") and never named it (S11 to S13). Section 2 says the same, in full, for both starts.

**The user started it**, by typing `/neocortex:therapy` or by asking you in words to go through their memories. That
is the sitting over this place, from section 7 on. The words they typed after the command are here, and nothing else
is:

$ARGUMENTS

Read those words as one of four cases:

- **Nothing was typed** (the command was typed bare, or with nothing after it): draw up the list and show it (S1).
- **What they typed names memories or a subject** (an id, a title, "the tabs one", a file): diagnose and treat those
  first, then ask whether to look over the rest of this place (S62, S63). That ask is your reply's closing line, in
  the same turn as the diagnosis and its offer — after them, never before, and never held back for a later turn: for
  example "That's this one. Shall I look over the rest of the memories here too?" It asks, and it never starts the
  walk.

  Until they answer the memory's offer, do no search of the store (S63): one `get_memory` per memory they named is the
  whole read this path makes, and a `search_memories` before their yes to looking further is the walk they did not ask
  for.
- **What they typed asks for the whole store in words** ("go through my memories", "all of them"): treat it as empty
  and draw up the list.
- **What they typed names nothing you can find**: say so, and offer the list instead (S62).

Never decide this from those words alone: a correction's answer can still be in front of you a turn later, and Claude
may pass a suspect's id as a skill argument. Who started the skill decides, and only then what the words mean.

## 1. One memory at a time

Work on the memory the user pointed you at. Before proposing anything, read it with `get_memory` for its `revision`,
its `why`, its `userWords`, its `summary`, `body`, `title`, `created` and `updated`, and show the user when it was saved
and when it was last changed (`created` and `updated`) beside the evidence below, so they treat it knowing how old its
words are.

If `get_memory` fails, say it failed and quote its error. Never say the memory shows no dysfunction, and change
nothing.

## 2. What the evidence must hold

Show the diagnosis and its evidence **before** you offer anything: the user sees what is wrong with the memory before
being asked what to do about it, and nothing is treated on the way. What stands beside the memory depends on the
sitting: **at a correction**, the correction the memory misfired at and the user's correcting words as the correction
check gave them (S5); **in the sitting over the store**, a memory that no correction in this conversation named has no
correction on record, so its own words and why are the evidence, and you say so (S71). A memory that a correction earlier
in this conversation named (S77) has that correction as its evidence: quote the user's own sentence, as they typed it,
from this conversation, beside the memory's own words.

**Name the dysfunction in its own words from the table above**, as that label, in bold, on the diagnosis's own first
line: "does not arrive when it should", "misleads: worded wrongly", "wrong at heart", "right but does not land" or
"arrives in a way that hurts". So
the user hears which one it is and not only a description of it. Describe the dysfunction after that label, never in
place of it: "the wording is wrong", "the intent no longer holds" and "it's a list of keywords" are descriptions of
the same ones, and a reader who gets only those cannot tell which one the skill diagnosed. **This holds at a
correction too, and it is the one thing that may not be skipped there** (S65: nothing the user sees at a correction
changes). Starting the skill is not optional either: `Skill`, with `neocortex:therapy`, runs before the diagnosis, as
section 0 says.

For each dysfunction, show what you actually have — never what you would expect:

- **1, does not arrive when it should**: the three sides, all at once — the user's correcting words; what reached you
  before the mistake, from the correction check's answer (nothing, or only the memory's title, and when); and the
  mistake itself, the call or the reply as it happened. Beside them, where it arrives today, as the check's `arrives`
  field words it. When the check said some notes of this conversation could not be read, say so: it may have arrived
  in one of them, and the dysfunction is not certain. A memory with `notHere` does not hold at this place: say the
  condition the check gives as the reason.
  - When its moment came before the mistake — its mark meets the very call that went wrong — and it still did not
    arrive there, say its mark beside that call and that the record shows no arrival there. The record does not keep
    why: no room at that moment is one cause it does not record. Never say that the memory has no moment; it has one,
    and it did not arrive at it. Offer no second moment for the same call: say "none that fits", as such (S135).
  - A memory you looked up yourself before the mistake, with `get_memory` or `search_memories`, is never named one that
    did not arrive: say that you read it yourself and when, as this conversation shows, and that the record covers only
    what NeoCortex delivered. Its text was in front of you, so offer it no moment (S106).
- **2, arrives in a way that hurts**: the three sides, all at once — the user's words, the calls it stopped (each with
  its tool and when, from the check's `stops`; the record keeps no call text, so say you cannot quote the call and
  never make one up), and where it arrives today, as `arrives` words it. Then how often and at which moments it came,
  from `arrived` (its count) and `moments`, with `noiseNote` and `stopNote` saying what the record could not read: say
  the count with what it leaves out ("at least 12 here; notes before the /clear cannot be checked", or "cannot be
  counted here"), never "it arrived only N times" from a partial record, and never that it does not disturb. A memory
  with `disturbing` and no arrival rests on the user's words alone: say that, and never that it did not arrive. A
  memory that stopped a call is diagnosed and offered its forms at every correction, whether or not the correction is
  about that stop (S144). A stop is never left out because it did not cause, lead to or explain the mistake the user
  corrected, since the hurt is the stop itself. A stop that worked as set, a guard doing its job, is still diagnosed
  and offered its forms. For a memory that stopped a call, say that stopping such a call once is the form it was set
  to have, so keeping it is an answer the user gives, never one you give for them by leaving its forms unoffered. A
  memory that also shows a wording dysfunction (5 to 7) is named once, with each dysfunction that has
  evidence of its own and none the evidence does not show; each gets its own offer, answered on its own.
- **5, worded wrongly**: the `summary` and `body` beside the `why` and the `userWords`, all at once. Name the kind
  of difference, and ask the method's question of the memory's own words:
  - a **deletion** — what the wording left out: *"… about what, specifically?"*
  - a **generalization** — what it made absolute: *"never? always? who, specifically?"*
  - a **distortion** — a cause or reading the why does not hold: *"how, specifically, does that lead to this?"*
- **6, wrong at heart**: the memory as it reads beside what contradicts it — the line of the code, doc or skill it
  names, the fact that what it names is gone, or the user's later words quoted.
- **7, does not land**: its summary, and what keeps it from landing (keywords, no action, how much of it shows at the
  start).

Say when a memory keeps no `userWords`: the evidence then rests on its `why` alone, and you add no words the user did
not say.

A fault in the **wording** is 5, not 6. A memory whose intent still holds but whose words are unclear is 7, not 5.

## 3. What you offer

One offer per treatment, each answered on its own. Show before and after, so the user sees both.

- **A reword** (5 and 7): the summary or body as it is, and the wording you propose with the same meaning. It should
  say what to do, not only what not to do. Give both lengths when the length is the problem.
- **A reframe** (6): the why stays; either the conclusion changes, or where the memory applies narrows (its condition
  or its scope).
- **A moment** (1): a moment of its own that comes before such a mistake as this one, said as the cue it fires on —
  the tool and pattern of a call or failure mark, the file of a file rule, or the phrase of a reply mark — and what
  that moment does: at a shell command or another tool call that changes something, it stops that call once and the
  call made again runs when the memory is set to be **read first**, and arrives beside the result when it is not; at a
  write, an edit, a read or a skill, and at a reply, it arrives beside the result, after the call or the reply has run.
  Show it before and after, and say where the memory then arrives besides the moment it
  keeps: a call, failure or reply mark and a wider condition are added beside the one it has, never in place of it.
  - **Every cue comes with how often it met one.** Call `count_cues` once per offer, with the memory's id, the
    conversation from the "Track record: conversation" line of your index, and every cue of that offer in the one call, so
    they are counted over the same sessions. Say each cue's count beside it before they answer, in calls (or replies) met,
    never as times it arrives: "met 12 calls in 4 of the last 40 sessions of this place" for a call or failure mark, and
    "met 9 replies in 3 of the last 40 sessions of this place" for a reply mark. Name the sessions actually counted
    (`sessionsCounted`), never the reach's 40: with 3 sessions on this machine say "in 2 of the 3 sessions here". When
    `unreadable` is more than 0, say how many sessions could not be read, so they know what the count leaves out. Where
    the count could not be made, say **could not be counted** and the reason it gives, and never that the cue fires. A cue
    that met no call in those sessions is said as such, and it is never recommended: recommend one that did fire, or say
    you found none. For a user memory, say that the count covers only this place's sessions, since its cue fires in every
    place. A cue the answer says can never fire — a phrase NeoCortex never matches, a mark NeoCortex would not store, or a
    memory whose files part stops it — is said as that, never as 0 times. Count and keep nothing: no call, reply or error
    text of theirs is ever shown back.
  - When the memory's condition does not hold at this place (`notHere`), the moment to propose is a condition that
    takes this place in, shown before and after with every other place it then reaches, and say only the moments that
    memory itself has, never one it has not: if it carries a call, failure or reply mark, it arrives at its next
    matching call, failure, read or reply once the condition holds there, this turn included if it has not arrived here
    yet (a reply mark waits for their next message when NeoCortex has already kept this turn going once), and otherwise
    after the next session start, `/clear` or compaction, in a subagent that has not had it, or a day in which NeoCortex
    brought nothing new here; if it is delivered at index (its line at each start), that index line shows there from the
    next session start, `/clear` or compaction; a rule loads there from its next such moment; with none of these,
    nothing arrives on its own there, and it is still reached only when looked up.
  - When the check could not read this place's labels or repository (`placeUnknown`), propose **no** condition change
    on that reading: say what could not be read, and offer a mark instead, or wait until the place can be read.
  - What this memory can take: a call, failure or reply mark, and a condition, on an index or on-demand memory of
    either scope. A project memory takes no condition and no file rule (its project already fixes where it applies);
    the file patterns of an `edit` memory belong to a rule. A mark cannot be set on a rule.
  - A moment this memory cannot take is refused, and you say why and offer the one it can take, never writing a
    different one without the user's answer.
- **A form** (2): one form that takes the place of the moment that hurt, each with what it gives up in plain words, and
  each answerable on its own. The offer says which moment goes and which stay, and where the memory then arrives:
  - **A narrower cue**: the mark's tool and pattern, or the replies' phrases, as they are and as proposed, with which
    calls or replies it still meets and which it no longer does. At a call that changes something that it still meets,
    it still stops that call once when the memory is set to be read first, and arrives beside the result when it is
    not; at a call that only reads, it arrives beside the result. A cue that cannot be made
    narrower without missing the call it is about is "none that fits", said as such.
  - **Take back read first**: the memory then stops no call: its cue stays, and at a call that changes something it
    then arrives beside the result, after the call ran, as it already does at a call that only reads; it gives up
    being read before the call, and it arrives as often as before, so say that it ends the stop and not the arrivals.
    Offer it only where a stop exists, read from the one field that says so in both starts. At a correction: when the
    check's `arrives` for that memory says such a call "is stopped once", which it says only for a memory set to be
    read first; when the memory has stops and `arrives` does not say that, say it stops no call any more. In the sitting
    the user started: when `get_memory`'s `arrives` says the same words. Never judge it from the tool's name: `arrives`
    is where NeoCortex has already decided, and it alone knows which calls stop. When the user asks for it on any other
    memory, or on a rule, say before any write that it ends no stop and changes nothing in when the memory arrives.
  - **A failure mark**, in place of a call mark: the memory then arrives only when such a call fails, beside its error,
    after the call has run, and never at a call that succeeds. A failure mark names one tool, so a call mark on more
    than one (`Write|Edit`) keeps only the one named, and the offer says which are dropped.
  - **A reply mark**: it then arrives at the end of a reply that says its phrase, after the call and the reply have
    run, never before the call. Show the phrase before they answer.
  - **Its index line**: it then arrives only as its one line at each session start, cut to its title when the start is
    crowded, and its text only when looked up. An on-demand memory then arrives at every start, which the offer says.
  - A form the memory already has (a reply mark offered in place of its call mark when it has one) is offered as the
    call mark dropped and the one it has kept, never as a second mark of that kind. A memory with two or three marks
    keeps every mark that did not hurt, and the offer names each. A form that would add arrivals — a mark on a memory
    with none — is never offered for a hurt. A memory with no mark has no cue to narrow: for a user memory offer a
    narrower condition (where its index line arrives), shown before and after with the places it then no longer
    reaches, and that its index line follows the new condition from the next session start, `/clear` or compaction; for
    a project memory, "none that fits", said as such. Never offer on demand or a delete unasked (a fix for
    a noisy delivery changes how a memory arrives, never whether): if they ask for one, say what it gives up, and that
    a delete is their own ask outside therapy. For a memory that stopped a call, say the user may keep it, or, where a
    stop exists as above, take back read first, which stops no call from then on.
  - A **project memory** changes in this project only, and takes no condition. A **user memory** changes in every
    place, not only this one; say so before they answer, and where it then stops arriving.
  - **A rule** is offered only another moment at which it still loads in full and arrives less often, or a narrower
    condition or file patterns, said as when and where it then loads; never a wording. A mark or its index line only
    when the user asks for one, and the offer then says beforehand that it stops being a rule and no longer loads in
    full. Say that Claude Code asks the user to confirm the change with what it affects; on their decline nothing
    changes and you say so.
  - The new form fires at its next moment, not at once, so name both cases. **A stop ends sooner than a new mark
    fires**: taking back read first ends it at the very next matching call that changes something, this turn included,
    in every agent. In an agent where the memory already stopped a call it has arrived there, so it arrives there
    again only as a memory that has already arrived here does, below; in one that has not had it, such as a subagent,
    the call runs without a stop and the memory arrives beside its result. A memory that has not arrived at a call,
    failure, read or reply in this conversation yet — one that reached you only as its title or by a lookup, or whose
    moment hurt in an earlier session or in a subagent and that has not arrived here either — can arrive at the very
    next matching call, failure or reply, this turn included; a reply mark waits for their next message when NeoCortex
    has already kept this turn going once.
    A memory that has already arrived here arrives again only after the next session start, `/clear` or compaction, in a
    subagent that has not had it, or after a day in which NeoCortex brought nothing new here, and a resume is no such
    start. A new call mark on a memory set to be read first that has not stopped a call here yet stops the next matching
    call that can change something, in this conversation. Never say only that it waits for a new session, and never that
    it arrives at once for a memory that already arrived here. A rule's new load moment holds from its next such moment.
- **A rule gets no proposed wording at all.** A rule is any memory that loads in full at the start, at a prompt or at an
  edit, or is `enforced`. For a rule you show the diagnosis and its evidence, and you propose **no wording** — not a
  rewording, not one "with the same meaning", not a reframe, and not a suggestion of the words to use. Say that therapy
  proposes no wording for a rule, and that **the user can change the rule itself on their own ask** — rewording it,
  narrowing it, widening it or deleting it — which Claude Code asks them to confirm with what it affects.
  - Do not write out the words they could use, and do not show a before and after: a rule's own wording is theirs to
    set. "Same meaning" is no exception, because every loosened rule reads as the same meaning to the one who loosens it.
  - A rule whose intent no longer holds, or whose wording drifted from its `why`, is changed or deleted by the user on
    their own ask. Neither is a therapy treatment: treat the diagnosis as the whole of what you give.
  - **A rule may be offered another moment** (dysfunction 1): only at moments at which it still loads, and never a
    mark, which a rule cannot carry. Say beforehand that Claude Code asks the user to confirm the change with what it
    affects; on their decline nothing changes and you say so. A moment change never makes a rule a memory that is no
    rule, and a rule is never offered a wording afterwards.
- **A user memory** reaches every place, not only this one; say so before they answer.
- **Making it a rule**: a moment that would make an ordinary memory load in full at the start, at a prompt, at a
  question, at a subagent's start or at an edit (a file rule among them) makes it a rule. Say beforehand that Claude
  Code asks the user to confirm it with what it affects, and name the call, failure or reply mark it loses, since a
  rule keeps none.
- When the proposed wording says otherwise than **another memory** that arrives at the same moment or in the same
  place, name that memory and quote what it says before they answer; they may treat both, one or neither.

If the memory is a handoff, offer nothing: the next handoff replaces it. If it does not load at all, say it cannot be
treated here, give the reason and what to do (update NeoCortex, or repair the record the index names), and offer
nothing.

## 4. Their answers

- **Yes** → write it. **Their own wording** → show it back and write it as given, and it joins the memory's
  `userWords`.
- **No** → nothing changes, and you do not offer that treatment again in this conversation. At a later correction the
  memory that hurt is named again with its evidence and that earlier no, and no new form is offered for it.
- **A question or an unclear word** ("maybe", "what does that do?") → answer it and ask again, until they give a clear
  answer for that offer; record nothing until then.
- **A cue of their own** ("at a push, not at every commit") → show it back as the cue it fires on, and which calls,
  files or replies it meets, and write it as given on their word. A cue NeoCortex cannot use — a call mark whose tool
  names nothing, a failure mark holding a call mark's `|` — is refused: say why and ask for another. Show it back with its
  count too, through `count_cues`, as an offered cue is: at 0 say so, and write it only on their word.
- **"All the rest"** → show every remaining offer in one reply, each with what it changes and where its memory then
  arrives; that answer stands for exactly those offers, and each write still goes on its own.
- **They dispute the diagnosis** → nothing changes on it; ask what they see instead, and treat a diagnosis they give
  with its evidence like any other.
- **They ask for a treatment of another kind** → say what it changes and what it leaves as it is (a rewording keeps
  the conclusion a reframe would change, and leaves when the memory arrives as it is; a moment leaves what it says),
  and write it on their word. A rule still gets no wording.
- **They ask to change the why** → no: a treatment keeps the why, so the trail of what was said stays. A change to the
  why is their own ask outside therapy, as a delete is.
- **They go on with their work or stop** → nothing changes; go on with their work, and say which offers were not
  answered. Do not offer that treatment again in this conversation, unless that memory misfires again.

## 5. Writing it

Write with `update_memory`, or with `update_rule` when the memory is a rule; both need the `revision` you read. For a
rule you write only the user's own words, never one you proposed, since you propose none.

- On a yes to a wording you proposed, write that wording, and pass the user's correcting words in `userWords` when
  this sitting has a correction behind it (as the correction check gave them).
- **A form that replaces a moment that hurt is one write** (2): the mark that hurt passed as `null` and the new form
  in the same `update_memory` call — a narrower cue in `callInput` or `failure`, a reply mark in `reply`, its index
  line as `delivery: index` — so the moment that hurt is gone and the new one is there together. **Taking back read
  first is no mark**: pass `readFirst: false` and no mark at all, and every mark it has stays as it is. A rule's change
  goes through `update_rule` and Claude Code's confirmation. A refused write leaves the memory as it was, the moment
  that hurt still in place: never leave it with that moment removed and the new form not written.
- **A moment** is written on the memory itself, never as a second memory saved beside it unless the user asks for
  one: `callInput` or `failure` for a mark, `reply` for a reply mark, `appliesWhen` for a condition. Pass only what
  changes: a field the call leaves out stays as it is, and a mark or condition the memory already has stays beside
  the new one. Pass the user's correcting words in `userWords` too, when this sitting has a correction behind it, as a
  wording write does (S110, S37): the words are what the trail keeps beside the change, and a moment write keeps them
  the same way. That is the one field a moment write carries beyond the moment. A write that changes a moment and
  nothing else writes no summary, and its answer says the summary stays on purpose: say that, and never tell the user
  their words came without a new summary. When the answer instead says the write changed what the memory says as well,
  quote that sentence: this call changed wording too, and you do not claim it stayed.
- **`userWords` gets only words the user said in this conversation.** In the sitting over the store there is no
  correction, so a yes to your wording adds no `userWords` at all and the stored ones stay as they are; when the user
  gives a wording of their own, write theirs and pass theirs (S79). Say this when it matters, so they know a yes to
  your words writes your words only.
- **Never change the `why`.** Never save a second memory beside the misfiring one unless they ask for it; if they do,
  say the old one still arrives as before until it is treated too.
- An update keeps the earlier wording, readable with `get_memory`, and says so: only deleting the memory removes it.
- Say what changed **and where that memory now arrives**: for a moment change, quote the sentence the write's own
  answer gives, which names the moments it had and the ones it has now; for a wording change, the moments it already
  had are unchanged.

A write can be refused. Say why in plain words and offer the way out it names:

- the memory changed since you read it → read it again and offer the moment on its new record, or say the dysfunction
  is gone;
- it is gone → say so and pass it;
- the moment this memory cannot take → say why in plain words and offer the moment it can take, never writing a
  different one without the user's answer;
- a form in place of a moment that hurt, refused → say why in plain words and offer the way out: read the memory
  again and offer on its new record, or say the hurt is gone; and if the cue they gave cannot be used (a call mark
  whose tool names nothing, a phrase with no letter or digit), say so and ask for another, writing nothing;
- the user's words hold what looks like a secret → say so, and let them give the words without it;
- the wording is longer than a summary may be (300 characters), or their words longer than a memory keeps (2,000) →
  say the limit and offer a shorter one; never cut their words without their word;
- a move to another scope refused because a copy is already there → say so and offer to narrow it by a condition
  instead;
- a rule change → Claude Code asks the user to confirm it with what it affects; on decline nothing changes, and you say
  so.

Show the memory's text as the tool gives it. Where it shows through a stand-in (U+FFFD), do not retype it.

## 6. The end

When every offer is answered, or they go on with their work, say per memory what changed and what did not, in the
words they answered with, and for each change how they will see it worked: what the memory should no longer make
Claude do. A correction that treated both a memory that did not arrive and one that misled says each for its own
treatment. For a **moment** change that wrote no wording, say that it changed when the memory arrives and not what it
says, and how the user will see it worked: the memory arrives at its new moment, said as the offer said what that
moment does (section 3). A mark comes before the mistake only where it stops the call once: a call mark on a memory
set to be read first, at a shell command or another call that changes something, never at a write, an edit, a read or
a skill. Otherwise a call, failure or reply mark comes after the mistake ran — beside the call's result, beside a
failure's error, at the end of the reply — so Claude can correct the mistake but not prevent it; say that, and never
that such a mark brings the memory before the mistake. A rule's new load moment loads it before Claude acts at that
moment, with file patterns before each write or edit of a matching file, and a condition brings the memory where
section 3 says, by what it carries: its index line from the next session start, `/clear` or compaction when it is
delivered at index, its marks as above, a rule from its next such moment, and nothing on its own when it has none of
these.
A write that changed the wording too says both, as the write's own answer does (S136). For a
**wording** change, say that this changed what the memory says, not when it arrives. For a **form** written in place
of a moment that hurt, say which moment went and which came, what the new form gives up, and how they will see it
worked: the next such call runs without the stop, or the memory arrives at fewer prompts, calls or replies — and when
the new mark first brings it, in both cases: for a memory that has not arrived at a call, failure, read or reply in
this conversation yet — one that reached you only as its title or by a lookup, or whose moment hurt in an earlier
session or in a subagent and that has not arrived here either — at the very next matching call, failure or reply, this
turn included, with a reply mark waiting for their next message when this turn is already kept going once; and for a
memory that has already arrived here, only after the next session start, `/clear` or compaction, in a subagent that has
not had it, or after a day in which NeoCortex brought nothing new here, a resume being no such start. A new call mark on
a memory set to be read first that has not stopped a call here yet stops the next matching call that can change
something; for a rule, from its next such moment. In the sitting over the store, add
which listed memories were not looked at or not answered, and that the next `/neocortex:therapy` reaches them (S87).

Do not check later in this conversation whether it worked; that is not part of this sitting.

## 7. The sitting: every memory this place sees

The rest of this skill is the sitting the user started. Sections 1 to 6 still hold for each memory you treat here;
what follows is how the list is drawn up, in what order, and where the walk stops.

### 7.1 Look at the store

First say what you are about to look at, then call `search_memories` with no query and no labels: that matches every
memory this place sees. Walk it in pages:

1. Call `search_memories` with `offset` (0 on the first call). Its answer carries `total` (every match), `results`
   (this page, at most 50), and `nextOffset` while more remain.
2. Call it again with `offset` = the `nextOffset` you were given. **Stop after two pages** (two calls), even when
   `nextOffset` says more remain.
3. Collect each entry's `id`. If an id comes back twice, the store changed under the walk: say so and that you may have
   missed some (never a number), and go on with what you have.
4. If the `total` of a later answer differs from the first, say the store changed while you read and that you may have
   missed some. Never compare your count of ids with `total`: the two-page stop makes those differ on any store larger
   than 100, and that is not a change.

5. Read each candidate with `get_memory` before it is shown: the search entry carries no `created` or `updated`, so the
   list's "when it was saved and last changed" and the memory's own words in its evidence come from that read. A rule
   is read in full, `body` included, during the walk, and a candidate that read clears is dropped from the list (see
   "Every rule, in full" below).

**Which lines showed only their title, and which of those arrive nowhere else.** Call `memories_in_play` once, with the
conversation from the "Track record: conversation" line of your index, and hold its answer for the walk's join. A memory
it lists as arriving "as its index line cut to its title" reached you at this start with nothing but its name, and the
same line ends with what decides the rest: "it has a call, failure or reply of its own", "it has no call, failure or
reply of its own", or that this cannot be told now. Both halves of a start-only memory's evidence are on that one line,
so you never read a cut memory again to ask.

- **Which start counts.** The answer splits at a compaction when the conversation had one: what it lists under
  "Arrived before the compaction at … and no longer in context in full" is an earlier start's, not this one's. Take
  only the lines before that heading.
- **When the conversation line is not in view** — you were resumed or compacted and the line is gone — call neither
  `memories_in_play` nor `count_cues`, and say that you cannot tell which memories reached you only as their title,
  and why. Never say that none did. Draw up the rest of the list as these sections say.
- **When `memories_in_play` cannot tell what this start cut** — its notes cannot be read, were written by a newer
  NeoCortex, or hold no start of this conversation — say that it cannot tell which lines showed only their title and
  why, never that none did, and name no start-only memory. The rest of the list is drawn up as usual.
- **A line that cannot say whether it has a moment.** "whether it has a moment of its own cannot be told now" is said
  of one memory whose record today's store cannot give (deleted since, or unreadable). The answer says which case it
  is: when the store itself could not be read, it also carries "Whether these memories changed since they arrived
  cannot be checked now: the store could not be read.", and that sentence — never the clauses alone — is what makes the
  whole list unknown (S205). Without it, every line that says it is that memory's own, the other lines are decided by
  their own clauses, and a start that cut only that one line is one memory unknown, not a store failure. A line that
  says "it is expired now, so it no longer arrives at the start" (or replaced, or not here) has no moment and is no
  start-only patient either (S217): that is this line's own answer, and no read of `get_memory` is needed for any of
  these. 7.2 says what the list does with each.

Finish the whole walk **before the first write**. Never call `search_memories` again after a write in this sitting;
a treatment moves a memory in the order, and a page read afterwards both skips one and repeats one.

**Unreadable records.** The answer may carry `unreadable`, with its own `total`, `matching` and `next`. That part is
never paged: say how many records could not be read and what to do (update NeoCortex, or repair the record the index
names), and never count them as memories that show nothing (S83).

**What you judged, and what you did not.** A memory is judged for a dysfunction from its `title`, `summary`, `why`,
`delivery` and its marks — not from its `body`. Say how many memories you judged that way, naming those fields, and that
each memory you show was read in full; never say that every memory was read in full. When this place holds a rule, say
that the rules were read in full with it (the paragraph below says what that read is, and how a place holding none must
not claim it). A memory whose summary is fine but whose body has drifted stays unflagged until the user names or opens
it, and that is what the list's own sentence about what it looked at must say.

**Every rule, in full.** A rule is the one exception, and it is read whole during the walk, `body` included, before it
is put on the list or left off it: a rule governs what Claude does, so its body decides as much as its summary does. A
rule whose summary is fine and whose `body` has drifted from its `why` is flagged on that read (S71), and a rule the
full read clears is dropped from the list. When this place holds a rule, say that the rules were read in full when you
say what you looked at (S1); a place that holds none has no rule read to report, and saying it read rules it does not
hold is a false account of what it looked at.

If a page's `nextOffset` is absent when you reach it, you have reached the end of the store: say how many you read. If
two pages are full and `nextOffset` says more remain, say how many memories lie after the first hundred, "in the order
NeoCortex lists them", and that they can have the next hundred on their word (S52). Serving them is a **new walk**:
start again at that `offset`, after this sitting's writes, and only when they ask.

### 7.2 Draw up the list

Judge every memory you read, and put on the list only those whose own words show a dysfunction with evidence:

- **misleads: worded wrongly (5)** — its summary or body says other than its `why` (S71).
- **wrong at heart (6)** — the user names it as outdated or wrong, and their words are the evidence; you have not
  checked the files or docs it names in the sitting (S95). Never put a memory under 6 on another memory's words: two
  memories that disagree with each other are in conflict, and that is not this sitting's diagnosis. Show the two side
  by side instead, say that only the user can tell which is right, and keep that memory off the list until their words
  say which (S95).
- **right but does not land (7)** — its summary is a list of keywords, vague, or says nothing to do (S75).

Leave off, and say so where the row says to: a handoff (S82); a memory whose condition does not hold here, counted
(S84). A memory whose record could not be read goes in the "cannot be read" group (S83).

**A memory that reaches you only as its title.** A memory whose line the start cut to its title, and whose line says it
has no call, failure or reply of its own, arrives nowhere but this start's one line: it is a patient for a moment of its
own, and it is listed as that. Its evidence is these two things together: that its line showed only its title at this
start, and that it has no call, failure or reply of its own, so it arrives nowhere else. Name the group for what it is —
"Reaches you only as its title, and arrives nowhere else" — and say how many memories it holds. A memory whose line was
cut and which also shows a dysfunction 5 or 7 is listed once, under that dysfunction, with "reaches you only as its
title" named on its line, and each treatment is offered on its own.

- **A mark that can never fire is its own evidence.** When `get_memory` shows a mark while its `ownMoment` is false,
  that mark is one that can never fire — that is what the field means, and it needs no list of cases here. Say which
  mark it is and that it can never fire, so the user is never told it has no mark while their record shows one. Its
  moment replaces that mark with one that can fire, in one write, or you say that none fits. This is the one read
  `get_memory` is still needed for in this group: the line at the start gives the verdict, and the record itself is
  what shows a mark to name.
- **A memory that is no longer active is not on this list.** A line that says it is "expired now", "not here now" or
  "replaced or overridden now" is that memory's own answer: say that its line no longer reaches you at this start and
  treat nothing for it here (S217), with no `get_memory` read. A handoff whose line showed only its title is not on this
  list either: the next handoff replaces each (S82).
- **A line that cannot say whether it has a moment is not on this list, and not counted as start-only.** When the
  answer does not carry the store's own "the store could not be read" sentence, one such line is that memory's alone:
  say that you cannot tell whether that memory has a moment of its own and why, never that none are start-only; every
  other line is still decided by its own clause, and the rest of the list is drawn up as usual. When that sentence is
  there, say that you cannot tell which of the lines it cut are start-only and why, and name none of them (S205). The
  count 7.5 asks for still holds every such line, since "so far reached you only as their title" is true of it; what
  the sitting cannot say is which of them arrive nowhere else.
- **On the pick.** When the user picks one, diagnose it as sections 1 and 2 say, then offer it a moment as section 3
  offers one: the cue in plain words, where it then arrives, the count of what the cue met, and that the cue can bring
  the memory at the very next matching call, failure or reply in this conversation, this turn included (a reply mark
  waits for their next message when this turn is already kept going once), since it has never arrived at a moment here —
  and, for a call cue on a memory set to be read first, that it stops the next matching call that can change something,
  once. When the moment is written, say that its title line still shows at every start until `/neocortex:dreamer` moves
  it off.
- **The others with no moment of their own arrive at every start.** Say that a memory with no moment of its own whose
  line the start did not cut arrives at every start with its summary, so this list's number can be lower than the
  Dreamer's, which counts every memory with no moment of its own whether its line was cut or not.
- **What the count of these lines does not mean.** The count 7.5 asks for is every line this start noted cut to its
  title, the memories with a moment of their own among them; the list names the others, the cut memories with no moment
  of their own. Say both, so the number and the names never read as a contradiction.
- **A memory that does arrive at its own cue gets no shorter summary offered here.** A cut memory with a moment of its
  own is not on this list, and it is offered no shorter summary for its length alone: what it needs is nothing, since
  its cue already brings it, and shortening its line is `/neocortex:dreamer`'s to offer, never this sitting's.

A memory the user declined or left unanswered earlier in this conversation, and one a correction earlier in it named,
is listed: quote **the correction's own sentence, as the user typed it in this conversation** — not the memory's stored
`userWords`, which is a different thing and is already on the memory — as its evidence, mark the declined one "you said
no to this earlier in this conversation", and offer it again only on their ask (S77).

**If no memory shows a dysfunction with evidence**, say you found none (never that you found a memory that shows no
dysfunction when one reached you only as its title at this start), how many memories you read (none, in an empty
store), and what you looked at (S3). A start-only memory is this sitting's, so its group is the list even when nothing
else is on it. Change nothing, offer nothing.

### 7.3 Show the list

Show it **before any treatment**, in one list:

- each memory by **title and short id**, its **dysfunction in the table's own words**, said as what the evidence
  suggests and never as a verdict, one line of evidence, and when it was saved and last changed;
- say what you looked at and how many memories you read (see 7.1);
- then ask which to start with, recommending the first in the list's order.

Nothing is written before the user's first answer (S1, S70).

**Order.** The user's own concern first, if they named one (S63). Then the groups by how many memories each holds,
the largest first. Within a group, the memories that reach Claude at every start before those that arrive only when
asked for. The user may pick a different first.

**More than about 10 memories show a dysfunction.** Name no memory yet: show each dysfunction as a group with how many
memories it holds, one line of the group's evidence, and **no id of any group's memories** until the user picks one
(S42). Their pick, then: name ten of that group's memories in the list's order and say how many remain; name the next
ten on their ask, never that the rest are fine (S66). Ten of every group in the first reply is not this: it spends the
reply on ids they did not ask for and it is not the pick S42 asks for.

**One memory showing two dysfunctions** is listed once, under the one its evidence shows most directly, with the
second named on its line; never in two groups (S68).

### 7.4 Their answers to the list

- **They pick a group or a memory** → diagnose it in full (sections 1 and 2, with the sitting's evidence), then offer
  one treatment at a time. The rest of the list waits (S70). When the memory they pick is a rule, section 3's rule
  treatment holds here as it does at a correction: the rule gets its diagnosis and its evidence, and no proposed wording;
  say that the user can change the rule on their own ask, and that Claude Code asks them to confirm what the change
  affects. Sections 1 to 6 hold for every memory treated in this sitting, and this sentence stands here because it is
  where the pick is read, beside the offer it governs (AC4, S78).
- **They say a memory is not a problem, or answer "maybe"** → "not a problem": it leaves this sitting's list, unchanged,
  and is not offered again in it. "Maybe": show its full evidence and ask again, recording nothing until a clear
  answer (S69).
- **"All the rest"** → show every remaining offer in one reply with what each changes and where its memory then
  arrives; that answer stands for exactly those offers, and each write still goes on its own (S86).
- **They ask for the next ten** → name the next ten of the group they are in, from the list you hold (S66). Do not
  call `search_memories` again.

### 7.5 The count of "…" lines

The count comes from the same `memories_in_play` answer 7.1 already read, and never from `dream_report`: count the
lines it lists as "as its index line cut to its title" that stand before the compaction heading, whatever the memory's
marks. That is how many memories have **so far** reached you at this start only as their title; a memory whose mark
fired later in the conversation has since arrived in full, so the number can be lower than it was at the first prompt,
and "so far" is what keeps that from reading as a contradiction. Say the number, how many of those it names below as
reaching you only as their title (S7), and that `/neocortex:dreamer` goes through the start (S96). Never try to name them
from a search: a search matches words, not which lines a start cut.

### 7.6 A memory that arrives at the wrong time

If the user says a memory arrives too late or is ignored, say in one line that this command does not treat that yet,
and treat nothing for it in this sitting. A wording dysfunction the same memory shows is still listed, with its
evidence (S76).

A memory whose arrival hurts is treated at a correction, not in this sitting: say that in one line, and treat nothing
for it here. A memory that did not arrive when it should, before a mistake this conversation shows, is treated at that
correction, at the mistake it should have prevented: say that in one line, and treat nothing for it here. A memory whose
line this start cut to its title and that has no moment of its own is not this case — it is this sitting's, as 7.2
says, whether or not a correction named it.

## 8. When NeoCortex is not connected

If no `mcp__plugin_neocortex_neocortex__*` tool is in your tool list, the server is not connected: say that the
memories cannot be read, that `/neocortex:status` says why, and change nothing. Never say no memory misfires (S2).
