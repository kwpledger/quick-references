---
name: new-project-triage
description: "Runs a buy-vs-build-vs-skip check before any architecture talk whenever Kevin proposes building something new — an app, tool, MCP server, script, automation, integration, or workflow. Trigger on phrases like "I'm thinking about building," "would it be feasible to," "how hard would it be to make," "exploratory," "I have an idea for," or any time Kevin describes a problem and then floats a build as the solution — even if he doesn't ask for an evaluation, and even if the question is framed as purely technical. Feasibility is never the real question; opportunity cost is. Use this before answering any "how would I build X" question about a new personal project."
---

# New Project Triage

## What this does

Runs a buy-vs-build-vs-skip check before any architecture talk, because feasibility is never the real question for Kevin — opportunity cost is.

## Steps (in order)

1. Existing-tool check — because the PWA to-do app died when TickTick turned out to already do everything it would have done.
2. Identify the single riskiest assumption and the smallest time-boxed spike that tests it — because the OneNote MCP consumed hours before revealing it could only list_notebooks.
3. Opportunity-cost check against standing goals (YouTube channel before October 2027 is the named one) — because every side build displaces something.
4. Only after 1–3: architecture, stack, and how-to.

## Format & output

Verdict first, one paragraph. Then the reasoning. No tech-stack enthusiasm before the verdict.

## Always do

- Name the riskiest assumption explicitly.
- Propose a time box for the spike.

## Never do

- Never open with "yes, this is feasible" — everything is feasible.
- Never skip step 1 because the idea is technically interesting.

## Examples

**Good:** "Before building the calculator, spend $2 on PCalc Lite and see if exact-fraction mode covers your use case. If it doesn't, the novel piece is the Rational engine — spike that in a playground first."

**Weak:** "Great idea! Here's a SwiftUI architecture..."

*Why it's weak:* it validates the build before checking whether the build should exist.

## When unsure

Ask: "What job does this do that nothing you already own does?"\