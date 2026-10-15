---
name: deep-research-synthesizer
description: Synthesize a pile of mixed source material (internal docs, reports, SME notes, standards, data, web sources) into a decision brief with ranked findings, defined confidence levels, traced provenance, and recommended next steps. Use when the user wants to know what a set of sources adds up to and what to do about it, e.g. "synthesize these reports", "what do these docs tell us", "pull this together into a brief", "what's the bottom line across these sources", "summarize the evidence for this decision". Not for academic literature reviews or thesis gap-finding (use evidence-synthesis for that).
---

# Deep Research Synthesizer

Turn many sources into one decision-ready brief. Every finding is traceable to its sources, rated on a fixed confidence scale, and tied to what it means for the decision.

## Quick mode

Use quick mode when there are fewer than about five sources, or when the user asks for something quick ("just the gist", "quick read"). In quick mode:

1. Skip Step 3 (provenance tracing) and Step 7 (stress test).
2. Skip the source log table. Cite source IDs inline instead.
3. Keep the decision sentence, the claim tags, and the confidence scale.
4. Output only Bottom line, Key findings, and Recommended next steps.
5. Say "Quick mode" at the top of the brief, so the reader knows the provenance check and stress test were skipped.

If a quick-mode run turns up a contradiction or a load-bearing Low-confidence finding, say so and offer the full run.

## Step 1: frame the decision before reading

Pin these down first. Infer them from context when you can, ask one question at a time when you can't, and state every assumption in the brief.

1. **Decision:** what choice this synthesis informs. If there is no decision, ask what the reader will do differently after reading it.
2. **Reader:** who reads the brief, and what they already know.
3. **Deadline or horizon:** sets how much "outdated" matters.
4. **Options on the table:** if they're known, findings get mapped to them.
5. **Handling:** note any markings on the sources (e.g., CUI, OUO, proprietary). The brief carries the most restrictive one.

Write the decision as one sentence at the top of your working notes. Every finding must connect back to it, or it gets cut.

## Step 2: inventory and classify sources

Build a source log before extracting anything. Each source gets an ID (S1, S2, ...) and these fields:

| Field | Values |
|---|---|
| Type | primary data / first-hand account / official policy or standard / analysis or report / secondary summary / opinion or marketing |
| Author or owner | person, team, org, vendor |
| Date | as stated; `undated` if absent |
| Stake | none / interested party (vendor, advocate, team defending its own work) |
| Derived from | source IDs it draws on, if known |

Weighting rules:

1. Primary data and first-hand accounts outweigh summaries of them.
2. Official policy or standards documents are authoritative on what the rule is, not on whether it works.
3. Interested-party sources count, but their claims need corroboration from a source without that stake.
4. Undated sources are treated as stale until shown otherwise.

## Step 3: trace provenance (the independence check)

Before counting how many sources support something, find out whether they're actually independent.

1. For each claim that appears in multiple sources, check the `Derived from` field and the text for shared origins: the same survey, the same SME, the same earlier report, copied language.
2. Collapse dependent sources into one line of evidence. Five reports quoting one survey count as one source.
3. Flag **circular reporting**: claims that look widely supported but trace back to a single origin.

This is the step most syntheses skip. Do not skip it.

## Step 4: extract claims, not summaries

1. Read for discrete, checkable assertions, not document summaries.
2. Give each claim an ID (C1, C2, ...) and list the supporting and contradicting source IDs.
3. Tag each claim with one of:
   - `fact`: directly stated and verifiable in a source.
   - `inference`: your conclusion from combining sources.
   - `opinion`: a source's judgment or recommendation.
   - `speculation`: a forecast or untested idea.
4. For any claim that would change the recommendation, actively look for disconfirming evidence in the set. If nothing contradicts it, say whether that's because the sources agree or because no source tested it.

## Step 5: rate confidence on a fixed scale

Rate each finding on this scale, and nothing else. Show the independent-source count next to every rating.

| Rating | Criteria |
|---|---|
| **High** | 2+ independent sources agree, at least one primary or first-hand; no credible contradiction; current |
| **Moderate** | Supported, but by a single primary source, or by multiple sources with weaker type or some contradiction that's explainable |
| **Low** | Single secondary source, interested-party source only, or stale |
| **Contested** | Credible sources of comparable weight disagree |
| **Unsupported** | Asserted (often widely) but no source in the set actually backs it |

Rate on source weight, not headcount. When you override the count, say why in one line.

## Step 6: rank by decision impact

Order findings by how much they would move the decision, not by how often they appear or how interesting they are.

For each top finding, write a **so-what** line: what this means for the decision or for each option.

## Step 7: stress-test the recommendation

Before writing next steps:

1. Name the 1 to 3 findings the recommendation depends on most.
2. For each, state **what would change the call**: the evidence that, if it turned up, would flip or weaken the recommendation.
3. If a load-bearing finding is Low, Contested, or Unsupported, the first next step is to firm it up. Don't build the recommendation on it as if it were settled.

## Output: the brief

Sections in this order:

1. **Bottom line** (2 to 4 sentences): the answer to the decision question, with overall confidence.
2. **Key findings**, ranked by decision impact. Each one gets:
   - ID, the finding, the confidence rating with independent-source count
   - Source IDs
   - So-what
3. **Where sources disagree:** what the disagreement is about (facts, scope, definitions, or interests), and whether it can be resolved.
4. **Fragile ground:** circular reporting, single-source or interested-party-only claims, stale sources, and claims no source tested.
5. **What would change the call:** from Step 7.
6. **Recommended next steps**, as a numbered list. Each has an owner type and what it resolves.
7. **Assumptions and scope:** from Step 1, plus anything excluded and why.
8. **Source log:** the table from Step 2.

Lead with a diagram when the findings involve a process, system, or causal chain. Keep the brief to what the reader needs to decide. Detail lives in the source log, not the body.

## Constraints

1. Never fabricate sources, statistics, quotes, or dates. Missing data is written as `not stated`.
2. Never present an inference as a fact. The tag from Step 4 travels with the claim.
3. Never average a disagreement into a bland middle. Name both sides.
4. Cut any finding that doesn't connect to the decision, however interesting. If it's worth keeping, put it under a short "Outside scope" note.

## Quality check before delivering

- [ ] Decision stated in one sentence at the top of the brief
- [ ] Every finding has a rating from the fixed scale and an independent-source count
- [ ] Dependent sources collapsed; circular reporting flagged
- [ ] Every claim tagged fact / inference / opinion / speculation
- [ ] Disconfirming evidence sought for load-bearing claims
- [ ] "What would change the call" is filled in
- [ ] Next steps address the weakest load-bearing finding first
- [ ] Handling marking carried to the brief, if any
