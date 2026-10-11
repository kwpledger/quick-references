---
name: brief-a-person
description: Turn the current conversation into a brief that another human (a colleague, manager, teammate, client, or collaborator) can read cold and act on: what this work is, why it exists, where it stands, and what is needed from them. Use whenever the user wants to brief someone, hand work off to someone, catch someone up, write a summary or status update of this thread for a specific person, explain what they have been working on, or says things like "how do I explain this to X", "write this up for my supervisor", "write a handoff for X", "so-and-so is taking this over", or "I need to send someone the context". Use even when the user does not say "brief". Do NOT use for resuming the work themselves later (use where-was-i), for handing context to another AI session, or for summarizing external documents or meetings; this skill turns the conversation itself into a brief for a human reader.
---

# Brief a person

A colleague reading a brief was not in the room. They do not know the conversation happened, do not know which parts were the user's thinking versus the assistant's, and cannot open half the links. They also did not ask for a history lesson. They want to know three things fast: what is this, where does it stand, and what do you need from me?

The core insight: a brief is a **state snapshot, not a story**, and for a human reader it must also be **translated out of the conversation**. The two classic failures are chronological narration ("First we discussed X, then we explored Y...") and leakage of chat context the reader never had ("as discussed above", "the v3 draft", "Claude suggested").

## Step 1: Pin down the recipient and the purpose

The document changes shape depending on who reads it and what they must do. Infer from context; if either is unclear and the answer would change the document, ask one question covering both.

**Who** — establish:
- Role and relationship to the user (peer, supervisor, report, client, outside collaborator)
- What they already know (the project? the domain? the jargon?)
- What they are authorized to see (see Step 4)

**Why** — the brief's job is one of these, and it governs the opening:

| Purpose | Reader needs first | Weight goes to |
|---|---|---|
| Take over the work | Current state + next step | Open items, artifacts, dead ends |
| Decide or approve | The question + recommendation | Options, tradeoffs, deadline |
| Review or give feedback | What to look at + what kind of feedback | Artifacts, constraints, known weak spots |
| Stay informed | Bottom line in 2-3 sentences | Outcome and impact; minimal detail |

**Channel** — email body, chat message, or standalone document. Short channels (chat, email) get the compressed version; a document can carry the full structure. If unknown, default to a standalone markdown document whose opening section can be pasted into an email on its own.

## Step 2: Mine the conversation for what transfers

Re-read the whole thread and extract, in priority order:

1. **Decisions and their reasons.** Not "chose PostgreSQL" but "chose PostgreSQL over SQLite because of concurrent writes". A colleague without the reason will either reopen the decision or defend it badly when asked.
2. **Dead ends.** Approaches tried and rejected, and why. This keeps the recipient from re-exploring failed paths or proposing them in a meeting. If something was rejected for a fixable reason, say so.
3. **Constraints.** Requirements, deadlines, standards, stakeholder preferences, and limits stated even once. A constraint mentioned early and never repeated still governs everything; recency is not importance.
4. **Artifact state.** Every file, draft, or deliverable: name, where the recipient can find it, status (final / draft / superseded). Only the current version.
5. **Open items.** In progress, blocked (on what), and undecided.
6. **Background the recipient lacks.** Why the work exists, who asked for it, who else is involved. This is the material the original conversation never needed to state because the user already knew it. Pull it from the thread where it exists; where it does not, ask the user rather than inventing it.

**Verbatim essentials.** Names and spellings, numbers, dates, IDs, version numbers, error messages, and exact wording the user approved survive exactly. Paraphrasing these is corruption, not compression.

**Leave behind:** pleasantries, drafting back-and-forth, abandoned tangents that produced no decision, the user's working preferences for the assistant (tone corrections, formatting requests), and anything the recipient cannot act on.

## Step 3: Translate out of the conversation

Before writing, convert conversation-native material into something a person outside the thread can use:

- **Voice and ownership.** Write in the user's first person ("I chose", "I need") unless they say otherwise. Decisions belong to the user. Do not attribute choices to the assistant, and do not mention the conversation unless the user wants that disclosed. If a voice or style skill for the user is available, apply it.
- **References.** Replace "the second draft", "the approach above", or "option B" with a self-contained description. Replace sandbox paths, chat links, and anything the recipient cannot open with where the file actually lives for them (shared drive, repo, attachment), or flag `[user: add location]`.
- **Jargon.** Expand acronyms and shorthand the recipient may not share. Calibrate to Step 1: a domain peer needs less, a manager outside the domain needs more.
- **Asks.** Turn implicit needs into explicit requests with a date: not "the budget is still open" but "I need your approval on the budget by Friday, March 6."

## Step 4: Check what is safe to share

Conversations hold things the user would not forward. Before drafting, sweep for and exclude or flag:

- Information the recipient is not authorized to receive: classified or controlled material, need-to-know restrictions, proprietary or client data, personnel matters
- Personal details about the user or third parties (health, family, money) that surfaced incidentally
- Candid opinions about people, venting, or speculation about motives
- Credentials, keys, internal URLs, or account details

When unsure, leave it out and list it at the end of your reply to the user (not in the document): "I left out X; add it back if appropriate." The user decides; the default is omission.

## Step 5: Write the document

Lead with the purpose from Step 1. Cut sections that would be empty rather than padding them.

```markdown
# [Topic]: brief for [recipient name or role]
[Date] · From [user]

## Bottom line
[2-3 sentences: what this is, where it stands, what I need from you and by when. Readable on its own as an email.]

## Background
[Why this work exists, who it is for, who else is involved. Scale to what the recipient already knows; omit if they know it.]

## Current state
[Where things stand right now.]

## Decisions made (and why)
- [decision]: [reason]

## Already tried — not recommended
- [approach]: [why it was rejected; note if the reason is fixable]

## Materials
- [name]: [where to find it] · [final | draft]

## Key details
[Exact values, requirements, dates, names, approved wording.]

## Open items
- Needs your action: [specific ask + date]
- Still undecided: [question + options considered]
- Blocked: [item + what unblocks it]

## Questions
[Who to contact about what, and how; anything the user is unsure of that the recipient may know.]
```

**Diagram when it helps.** If the work is a system, workflow, or set of dependencies with more than about three moving parts, include a small Mermaid diagram under Current state. One picture of how the pieces connect saves paragraphs of explanation.

**Length.** The Bottom line must stand alone. The full document should read in 2-3 minutes. For "stay informed" purposes, the Bottom line plus one short section may be the whole thing. If it runs past two pages, you are narrating instead of snapshotting.

**Delivery.** A downloadable markdown file when file creation is available; otherwise a clearly fenced block. If the channel is email, also offer the Bottom line as a ready-to-send message body.

## Step 6: Verify before delivering

Re-scan the conversation with these questions:

1. Could the recipient act on this without ever seeing the conversation? Hunt for dangling references: "above", "earlier", "that version", unexplained names.
2. Is every constraint stated even once reflected?
3. For each artifact, is the referenced version the latest, and can the recipient reach it?
4. Is the ask explicit, owned, and dated?
5. Did anything from Step 4's list slip through?

If the thread contains contradictions the conversation never resolved, do not silently pick one; list it under Open items as an undecided question, or ask the user before sending.

## Fidelity rules

Everything in the brief must trace to something in the thread or to something the user told you while preparing it. Do not fill gaps with plausible inference: a colleague trusts the document completely and may repeat it to others, so an invented detail spreads. Mark genuine uncertainty: "(unconfirmed)". Mark missing information the user must supply: `[user: ...]`. Keep who-said-what straight: a suggestion the assistant made that the user never accepted is not a decision and must not be presented as the user's position.
