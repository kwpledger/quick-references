---
name: first-principles
description: Deconstruct a complex problem or goal down to its fundamentals, separate real constraints from inherited assumptions, rebuild an approach from what is actually true, and turn it into a numbered path of first actions. Use whenever the user says "first principles," "deconstruct," "break this down to basics," "strip this back," or "why do we even do it this way," and also when they are stuck on a tangled problem where the usual approach isn't working, the current solution feels inherited rather than chosen, the effort or cost seems out of proportion, or they ask "where do I even start" on something big and unfamiliar, even if they never name the method. Not for routine tasks with a known answer (just answer, or use how-to when the goal is learning an unfamiliar tool), and not for deciding whether to build something new at all (new-project-triage goes first; this skill can follow once the build is a go).
---

# First Principles

Most problems arrive already shaped like a solution: "I need a better LMS," "I need a faster computer," "I need a new app for this." The shape carries assumptions nobody chose on purpose. This skill finds which parts of the current approach are load-bearing and which are habit, rebuilds from the load-bearing parts, and ends with a path the user can start on today.

The path is not optional. An analysis that ends without a first step is a more sophisticated way of being stuck. Endless "why" chains can also feed overdesign and perfectionism, so treat this as a tool for getting moving, not for achieving perfect understanding.

## Step 0: Check fit

Run the full method when the problem is novel, the conventional approach has failed, the solution feels inherited, the cost or effort seems out of proportion to the outcome, or the user is stuck.

Run a light pass when the problem is well understood and a proven approach exists. Say so in one line, give the conventional approach and a short path, and offer the full deconstruction if they still want it. First principles is expensive; borrowing what already works is the right call most of the time.

If the real question is whether to build something at all, that belongs to new-project-triage. This skill picks up once the answer is yes. The exception is a project that was triaged and shelved earlier and is now being reconsidered. Run this skill on it: decomposition can change the build question itself (a different platform, a smaller first version, a tool the user has since gained), and triage can run again on the result.

## Step 1: Frame and ask

Restate the problem as an outcome, not a solution. "I need a better LMS" becomes "learners need to reach competence X, and the organization needs evidence they did." Solution-shaped statements hide assumptions in their nouns.

Before asking anything, check what you already know: memory about the user, and past conversations on the same problem if you can search them. Earlier attempts and decisions are the best evidence of which constraints are real. Treat what you find as possibly stale, since circumstances change. Note which sources you checked and which you couldn't reach (for example, coding sessions that chat search doesn't cover), so the user can see where your picture may have gaps.

Then ask two to four questions, using AskUserQuestion when available, otherwise a short list. Target what you can't infer:

- What does done look like? How would you know it worked?
- What constraints do you believe you're under? Ask for these explicitly; they are the raw material for Step 3.
- What has been tried, and why didn't it work?
- What tools, platforms, skills, or help do you already have? A constraint built on old information about the user's resources will be wrong, and one question catches it.
- How much time and energy is this worth? This sets how deep to go.

Pick the four that matter most and skip anything already answered. The questions exist to surface assumptions you can't see from outside; past four, the method turns into an interrogation. Anything left unknown gets flagged as an assumption in the analysis instead.

## Step 2: Decompose

Many overwhelming problems are several problems of different kinds tangled together, and each kind needs a different solution. Before breaking down functions, check whether the problem splits into:

- **Removal:** a one-time backlog to clear (clutter, debt, a pile of old tickets).
- **Organization:** giving things a home or a structure, also one-time.
- **Flow:** an ongoing rate, where something arrives faster than it leaves.
- **People:** who does what, how it gets noticed, and how it's enforced.

Sequence matters. Organization often has to come before flow, because a flow can't run without a place for things to go. Clearing a backlog before fixing the flow just rebuilds it. If the split applies, decompose each part separately.

Then break the outcome into the functions it requires: what must happen, not how. Then note the current or default implementation of each function. Functions tend to be fundamental; implementations usually aren't.

Use why-chains to get below surface causes. Stop when you reach bedrock, which is one of:

- **Physics, math, or logic.** Can't be argued with.
- **A hard external rule** the user can't or won't break: law, safety requirements, security rules.
- **A value or priority the user owns**, such as "I won't take this time from my kids." For human problems these are legitimate fundamentals. Treat them as givens unless the user wants to examine them.
- **The point of no decision value**, where the next "why" would not change what you'd do. In practice this is the stopping rule that matters most. Going past it is where analysis paralysis lives.

For technique details (enhanced why-chains, moving up the function ladder, component and cost mapping, hard-limit checks), read `references/decomposition-techniques.md`.

## Step 3: Verify constraints

Collect every constraint and classify it. Draw them only from three sources: what the user said, what you know about the user from memory or past conversations, and what is baked into the conventional approach to this problem. Don't attribute a generic assumption to the user just to have something to challenge; a strawman wastes their attention and costs trust. When a constraint comes from memory, say so, so the user can correct it if it's stale.

| Class | What it is | How to treat it |
|---|---|---|
| Hard | Physics, logic, law or safety rules you won't break | Design around it |
| Firm | Policy, budget, deadline, other people's decisions | Changeable at a cost; name the cost |
| Soft | Convention, habit, "how it's done" | Drop it if a better option exists |
| Untested | A belief about how people, tech, or a market will behave | Flag it; plan a cheap test |

For soft and untested constraints, tag where the belief came from: historical, authority, analogy, social, resource, technical, or market. `references/assumption-traps.md` has the full catalog, phrases that signal a hidden assumption, and a stress test.

Don't discard soft constraints by reflex. Most conventions were once good solutions, and many still are. The question is whether the original reason still applies, not whether the rule is old.

## Step 4: Rebuild

Using only the functions and the hard and firm constraints, generate two or three approaches. At least one should be something the conventional approach would not have produced. Pick one and give the reason in two or three sentences. If the user already proposed part of it, credit that in a line and spend the space on what's new. Restating their own idea back to them as a finding wastes their attention.

Then compare it with the conventional approach. If the conventional approach survives, say so plainly. That is a real result: the user now knows why it works, which makes it a choice instead of a habit. Don't manufacture novelty to justify the exercise.

Check that the chosen approach can actually be carried out with the user's real time, money, skills, and tools. A fundamental solution that can't be implemented isn't a solution.

## Step 5: Build the path

Turn the chosen approach into numbered steps.

- Make step 1 concrete and small enough to start today.
- Put a test of the riskiest untested assumption early. Finding out you're wrong at step 2 is cheap; at step 9 it isn't.
- Go five to eight steps deep. Group anything beyond that into milestones. Detailed plans for distant steps get overturned by what the early steps teach, and over-planning is its own way of staying stuck.
- Start each step with a verb and say what done looks like.

## Output format

Lead with a picture; the user understands structure faster visually than in prose.

1. **Decomposition diagram.** Outcome → functions → current implementations, with constraints marked by class. Render it inline with a visual tool when one is available; otherwise use a Mermaid code block. Keep it to about fifteen nodes so it stays readable.
2. **Constraint table.** Columns: Constraint | Class | Source | Note.
3. **Rebuilt approach.** Three to six sentences, including the comparison with the conventional approach.
4. **Path.** The numbered steps from Step 5.
5. **Assumptions challenged.** Two to five bullets: what turned out not to be fundamental, and any assumption that held up under scrutiny. Use the same three sources as Step 3.
6. **The rebuttal question.** End with "Which constraint did I get wrong?", naming the one or two you're least sure of. The user knows things about their situation you can't, and their corrections usually improve the analysis more than anything else in it.

Keep the prose tight. The diagram and table carry the analysis.

When the user rebuts a constraint, revise: reclassify it, check whether the rebuilt approach still holds, and update the path. Show only what changed rather than repeating the whole analysis.

For a light pass, use one line on why the conventional approach fits, then the path.

## Pitfalls

- **Stopping at "it's hard" or "it's expensive."** Difficulty is not impossibility. Go one more level.
- **Going too deep.** Use the stopping rule in Step 2.
- **Treating every convention as suspect.** Analogy and borrowed practice are fine when they fit. First principles is for when they don't.
- **Leaving the problem solution-shaped.** If the outcome statement contains a product or a tool, it isn't an outcome yet.
- **Ignoring implementation.** The rebuild must survive contact with the user's actual resources.
- **Novelty for its own sake.** A rebuilt answer that matches the conventional one is a valid result.
- **Strawman assumptions.** Challenge what the user or the convention actually holds, not what a generic person might believe.
- **Reasoning from stale facts.** Old conversations and memories describe the user as they were. Confirm resources before building a constraint on them.
