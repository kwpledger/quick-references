---
name: where-was-i
description: Turn the current conversation into a re-entry note the user can open days or weeks later and resume from in minutes: where things stand, why they chose what they chose, what they were about to try, and the exact first action to take on return. Use whenever the user says they will "pick this up later", is stopping for the day or the week, is switching to another project, worries they will lose the thread, wants to save where things stand, or asks for a "handoff", "save point", or summary for themselves. Also offer it proactively when a long working thread is clearly winding down unfinished. Do NOT use for briefing someone else (use brief-a-person), for handing context to another AI session, for setting reminders or tasks, or for summarizing external documents or meetings.
---

# Where was I

Future-you is a strange reader. They half-remember everything, which is worse than remembering nothing: the gist feels familiar, so they skip re-reading, then relitigate a settled decision because the reason evaporated. They also return cold, often tired, often after a context switch, and the hardest part of resuming is not the work but figuring out where to start.

So this note has two jobs:
1. **Restore intent**: why things are the way they are.
2. **Cut re-entry cost**: make the first action so obvious and small that starting takes no decision.

The core insight is the same as any good handoff: a **state snapshot, not a story**. No chronological narration. Future-you needs where things stand, what is ruled out, and what to do first.

## Step 1: Mine the conversation for what transfers

Re-read the whole thread and extract, in priority order:

1. **Decisions and their reasons.** Phrase as reminders of intent: "You chose PostgreSQL over SQLite because of concurrent writes." The reason is the part that evaporates; without it, future-you reopens the decision.
2. **Dead ends.** Approaches tried and rejected, and why. Future-you is especially prone to re-proposing a failed idea because it feels freshly clever. If something was rejected for a fixable reason, say so.
3. **Corrections and preferences.** Every time the user pushed back ("shorter", "not that tone", "the deadline is Friday, not Monday"), that correction was paid for once. Sweep for these in throwaway lines.
4. **Constraints stated once.** A requirement mentioned early and never repeated still governs everything. Recency is not importance.
5. **Artifact state.** Every file, draft, or deliverable: name, exact location, status (final / draft / superseded). Only the current version, and what distinguishes it from earlier ones. Note any file that lives only in a temporary place (chat sandbox, downloads folder) and needs to be moved somewhere durable.
6. **Momentum.** What the user was about to try, hunches, half-formed ideas, "I think the problem is X" statements. These are the most perishable thing in the thread and the most useful on return. Record them as hunches, not facts.
7. **Open items.** In progress, blocked (on what), and the next step.

**Verbatim essentials.** Names and spellings, numbers, dates, URLs, IDs, version numbers, error messages, key code snippets, and exact phrasings the user approved survive exactly. Paraphrasing these is corruption, not compression.

**Leave behind:** pleasantries, drafting back-and-forth (keep final state and rationale), and tangents that produced no decision. Tangents the user explicitly wanted to keep go in Parked, not in Open items.

## Step 2: Build the re-entry path

This is the part generic summaries miss. Before writing, work out:

- **The first action.** One concrete step, ideally 5-15 minutes, that needs no decision to start: "Open `scene_main.unity` and run the timer test" beats "continue testing". If the true next step is large, name its first slice.
- **The next sequence.** At most five numbered steps after the first action. More than five means the plan is not settled; put the rest in Later.
- **The stopping rule.** What counts as done for the next session, and what is explicitly not required yet. Use must-have versus later. This guards against a returning session turning into a redesign.
- **Staleness checks.** Anything that may have changed while away: a pending reply from someone, a deadline, an external tool version, a price, a decision someone else owed. Flag each as "check before trusting".
- **Parked.** Ideas and improvements deliberately deferred, so they are recorded and stop competing with the next step. Parking is not a promise to do them.

If the thread does not contain enough to define the first action or the stopping rule, ask the user one question before writing rather than inventing one.

## Step 3: Write the document

Put the re-entry path first. Future-you reads top-down and may stop early. Cut sections that would be empty.

```markdown
# Where was I: [topic]
[Date] · [one line: what this thread was for]

## Resume here
- First action: [one concrete, small step with exact file/location]
- Done for next session means: [stopping rule]
- Check first: [anything that may have gone stale]

## Objective
[The goal in 1-2 sentences, with success criteria if established.]

## Where things stand
[The "you are here" marker. Include a small Mermaid diagram if the work is a system or workflow with more than about three parts.]

## Why it is this way
- You chose [X] because [reason].

## Dead ends — do not retry
- [approach]: [why it failed; note if fixable]

## Where your head was
- [hunch, half-formed idea, or "about to try"] (hunch, unverified)

## Artifacts
- [name] · [exact location] · [final | draft | superseded by X]
- ⚠ [anything in a temporary location that must be moved]

## Verbatim essentials
[Exact values, requirements, approved phrasings, error messages.]

## Preferences learned
[Corrections and style preferences from this thread.]

## Next steps
1. [after the first action]
2. ...
(5 maximum)

## Later
- [must-haves deferred past next session]

## Parked
- [ideas deliberately set aside; no commitment]
```

**Length.** Resume here must work alone in 30 seconds. The whole document should read in 2-3 minutes; even a very long thread compresses to one or two pages. If it runs longer, you are narrating instead of snapshotting.

**Delivery.** A downloadable markdown file when file creation is available, named `YYYY-MM-DD_topic_where-was-i.md` so it sorts by date and is findable by topic. Otherwise a clearly fenced block. Remind the user in one line to store it where they will look when resuming (next to the project files, not in Downloads).

## Step 4: Verify against the thread

Before delivering, re-scan with these questions:

1. Can the first action be started without making a decision?
2. Is any user correction missing from Preferences learned?
3. Is any constraint stated exactly once missing?
4. For each artifact, is the version referenced actually the latest, and is its location durable?
5. Is anything in Next steps really a Parked idea in disguise?

If the thread contains contradictions it never resolved, do not silently pick a winner; list the question in Resume here under "Check first" so it is settled before work resumes.

## Fidelity rules

Everything in the note must trace to something in the thread. Future-you trusts this note more than their own memory, so an invented detail becomes an invisible landmine. Mark uncertainty: "(unconfirmed: implied but never stated)". Mark hunches as hunches. Keep who-said-what straight: a suggestion the assistant made that the user never accepted is not a decision and must not be recorded as "You chose".
