---
name: therapy
description: "Diagnose a memory whose words made Claude act wrongly, at the moment the user corrects Claude: say whether a memory's wording misled it (words that say other than its why, an intent that no longer holds, or words that do not land) with the evidence for it, and reword or reframe it on the user's answer. Start it when the correction check's answer names a suspect, or when the user says at a correction that a memory misled Claude. It changes nothing without the user's answer for that memory."
user-invocable: false
allowed-tools: mcp__plugin_neocortex_neocortex__get_memory, mcp__plugin_neocortex_neocortex__search_memories, mcp__plugin_neocortex_neocortex__memories_in_play
---

# Treating a memory that made Claude act wrongly

You treat memories whose **words** made Claude act wrongly, with the user, at the moment they corrected you. Three
dysfunctions, and each has one treatment:

| Dysfunction | What shows it | Treatment |
| --- | --- | --- |
| 5. misleads: worded wrongly | its summary or body says other than its why and the user's own words | reword it back to the intent |
| 6. wrong at heart | the intent no longer holds: the code, doc or skill it names says otherwise, what it names is gone, or the user's later words contradict it | reframe it: keep the why, change the conclusion or narrow where it applies |
| 7. right but does not land | it is vague, a list of keywords, or too long to show | reword it for clarity, same meaning |

Anything about **when** a memory arrives (it never arrived, arrived too late, arrived and was ignored, or its arrival
stopped a call) is not this skill's: say so in one line and do not treat it here.

The diagnosis is a hypothesis you show with its evidence, never a verdict. The user may dispute it and is right to.

## 1. One memory at a time

Work on the memory the user pointed you at. Before proposing anything, read it with `get_memory` for its `revision`,
its `why`, its `userWords`, its `summary`, `body`, `title`, `created` and `updated`, and show the user when it was saved
and when it was last changed (`created` and `updated`) beside the evidence below, so they treat it knowing how old its
words are.

If `get_memory` fails, say it failed and quote its error. Never say the memory shows no dysfunction, and change
nothing.

## 2. What the evidence must hold

Show the diagnosis and its evidence **before** you offer anything, with the correction the memory misfired at and the
user's correcting words as the correction check gave them: the user sees what is wrong with the memory before being asked
what to do about it, and nothing is treated on the way. **Name the dysfunction in its own words from the table above** —
"misleads: worded wrongly", "wrong at heart" or "right but does not land" — so the user hears which of the three it is and
not only a description of it. For each dysfunction, show what you actually have — never what you would expect:

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
- **A rule gets no proposed wording at all.** A rule is any memory that loads in full at the start, at a prompt or at an
  edit, or is `enforced`. For a rule you show the diagnosis and its evidence, and you propose **no wording** — not a
  rewording, not one "with the same meaning", not a reframe, and not a suggestion of the words to use. Say that therapy
  proposes no wording for a rule, and that **the user can change the rule itself on their own ask** — rewording it,
  narrowing it, widening it or deleting it — which Claude Code asks them to confirm with what it affects.
  - Do not write out the words they could use, and do not show a before and after: a rule's own wording is theirs to
    set. "Same meaning" is no exception, because every loosened rule reads as the same meaning to the one who loosens it.
  - A rule whose intent no longer holds, or whose wording drifted from its `why`, is changed or deleted by the user on
    their own ask. Neither is a therapy treatment: treat the diagnosis as the whole of what you give.
- **A user memory** reaches every place, not only this one; say so before they answer.
- When the proposed wording says otherwise than **another memory** that arrives at the same moment or in the same
  place, name that memory and quote what it says before they answer; they may treat both, one or neither.

If the memory is a handoff, offer nothing: the next handoff replaces it. If it does not load at all, say it cannot be
treated here, give the reason and what to do (update NeoCortex, or repair the record the index names), and offer
nothing.

## 4. Their answers

- **Yes** → write it. **Their own wording** → show it back and write it as given, and it joins the memory's
  `userWords`.
- **No** → nothing changes, and you do not offer that treatment again in this conversation.
- **A question or an unclear word** ("maybe", "what does that do?") → answer it and ask again, until they give a clear
  answer for that offer; record nothing until then.
- **"All the rest"** → show every remaining offer in one reply, each with what it changes and where its memory then
  arrives; that answer stands for exactly those offers, and each write still goes on its own.
- **They dispute the diagnosis** → nothing changes on it; ask what they see instead, and treat a diagnosis they give
  with its evidence like any other.
- **They ask for a treatment of another kind** → say what it changes and what it leaves as it is (a rewording keeps
  the conclusion a reframe would change), and write it on their word.
- **They ask to change the why** → no: a treatment keeps the why, so the trail of what was said stays. A change to the
  why is their own ask outside therapy, as a delete is.
- **They go on with their work or stop** → nothing changes; go on with their work, and say which offers were not
  answered. Do not offer that treatment again in this conversation, unless that memory misfires again.

## 5. Writing it

Write with `update_memory`, or with `update_rule` when the memory is a rule; both need the `revision` you read. For a
rule you write only the user's own words, never one you proposed, since you propose none.

- On a yes to a wording you proposed, write that wording, and pass the user's correcting words (as the correction
  check gave them) in `userWords`. On their own wording, write theirs and pass theirs.
- **Never change the `why`.** Never save a second memory beside the misfiring one unless they ask for it; if they do,
  say the old one still arrives as before until it is treated too.
- An update keeps the earlier wording, readable with `get_memory`, and says so: only deleting the memory removes it.
- Say what changed **and where that memory now arrives**: at which moments and in which places it will come back, from
  its condition and its marks. A treatment changed what it says, so the moments it already had are unchanged.

A write can be refused. Say why in plain words and offer the way out it names:

- the memory changed since you read it → read it again and offer it on its new text, or say the dysfunction is gone;
- it is gone → say so and pass it;
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
Claude do. Say too that this changed what the memory says, not when it arrives.

Do not check later in this conversation whether it worked; that is not part of this sitting.
