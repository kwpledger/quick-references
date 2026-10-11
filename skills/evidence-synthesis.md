---
name: evidence-synthesis
description: "Turn a research question into a claim-by-source evidence matrix that shows where the literature agrees, where it contradicts itself, and where the gaps are. Use when the user says things like 'build an evidence matrix', 'synthesize the literature on X', 'what does the research actually say about X', 'where do studies disagree on X', 'find contradictions in the literature', 'map the evidence for my thesis', 'which claims are supported vs contested', 'find the research gap', 'evidence table with study design and sample size', 'consensus rating for each claim', or 'literature synthesis for my dissertation / lit review chapter'. Produces a cited markdown synthesis report plus a CSV matrix with one row per claim-source pair. Complements, and does not replace, a formal systematic-review protocol."
user-selectable: true
icon: scale
---

# Evidence synthesis

Take a research question and return a **structured map of the evidence**: every distinct claim in
the literature, the sources that support it, the sources that contradict it, how strong each source
is, and — most importantly — where the literature disagrees with itself and where nobody has looked
yet. The gaps are the part a thesis can occupy.

## When to use this skill

Use it when the user wants:

- An evidence matrix, evidence table, or synthesis table for a research question
- To know where studies on a topic **disagree**, not just what they say
- To find the defensible gap for a thesis, dissertation chapter, grant aim, or lit review
- Claim-level consensus ratings across a body of literature
- A comparison of study designs, sample sizes, and effect directions across findings

## Do not use this for

- **A plain literature search or reading list.** If the user just wants "papers about X", hand them
  sources and stop. The deliverable here is the contradiction-and-gap matrix, not a bibliography.
- **A formal systematic review or meta-analysis.** This skill does not implement PRISMA, a
  registered protocol, dual independent screening, risk-of-bias instruments (RoB 2, ROBINS-I,
  Newcastle-Ottawa), or statistical pooling. It is a fast structured synthesis that *complements* a
  systematic-review protocol and can be used to scope one. Say this to the user in the report.
- **Summarizing a single paper**, writing prose sections of a thesis, or reformatting citations.
- **Clinical, legal, or financial decisions.** The output describes what the literature says; it is
  not advice.

## Inputs

Collect these before starting. Infer sensible defaults rather than interrogating the user; state
every assumption you made in the report's Scope section.

| Input | Required | Default if unstated |
|---|---|---|
| Research question or topic | Yes | — |
| Discipline / field | No | Infer from the question |
| Date window | No | Last 15 years, plus any older landmark work |
| Population, setting, or subgroup of interest | No | Unrestricted; note it |
| Languages | No | English-language sources |
| Source types to include | No | Peer-reviewed primary literature; preprints flagged |
| Depth: quick (10-20 sources) / standard (20-40) / deep (40+) | No | Standard |
| User-supplied papers, PDFs, or Drive/Notion folders | No | None |

If the user attached papers or pointed at a folder, read those **first** and treat them as seed
sources — then expand outward from their reference lists and from who cites them.

## Source hierarchy — state this preference explicitly

Prefer **primary, peer-reviewed literature over secondary summaries.** Rank sources in this order
and record the tier for each one:

1. **Primary peer-reviewed studies** — original data: RCTs, cohort and case-control studies,
   experiments, field studies, primary qualitative research.
2. **Systematic reviews and meta-analyses** — use them to *find* primary studies and to check
   whether your reading matches theirs. Cite the primary study for a finding whenever you can
   retrieve it.
3. **Preprints, dissertations, conference papers, working papers** — usable, always flagged as
   non-peer-reviewed.
4. **Narrative reviews, textbooks, institutional reports, standards bodies** — context and framing
   only.
5. **Journalism, blogs, encyclopedias, press releases, marketing material** — never the evidence
   for a claim. At most they point you toward a real source; go get that source.

Say this preference out loud in the report so the reader knows how the matrix was built. When a
claim is only supported by a tier 4-5 source, that is itself a finding: record it and rate consensus
accordingly.

## Integrity rules — non-negotiable

1. **Never fabricate a citation.** No invented titles, authors, years, journals, DOIs, page numbers,
   or URLs. Every row in the matrix must trace to a source you actually retrieved and read, or be
   marked unverified.
2. **Mark, do not drop.** If a claim's source could not be retrieved — paywall, dead link, cited
   only secondhand, no DOI resolvable — keep the row and set `verification = unverified`, with a
   note on what was attempted and what the claim was traced from. Silently deleting an unretrievable
   claim hides a real hole in the literature.
3. **Never invent numbers.** If sample size, effect size, or CI is not stated in the retrieved
   source, write `not reported` — not an estimate.
4. **Distinguish replication from independent finding.** Tag every supporting source as:
   - `independent` — different team, different data, different sample
   - `replication` — deliberate re-test of a prior study's design (mark `direct` or `conceptual`)
   - `reanalysis` — same dataset, new analysis, often the same or an overlapping team
   - `overlapping-sample` — different paper, but the data substantially overlaps another row

   Five papers off one cohort are not five independent confirmations. Say so where it happens.
5. **Quote precisely.** When a claim hinges on exact wording, quote the source verbatim and cite the
   location.
6. **Retraction and correction check.** For any load-bearing source, check for retractions,
   expressions of concern, or major corrections; flag them in the notes column.

## Workflow

Follow these steps in order.

### 1. Fix the question

Restate the research question in one sentence and decompose it into 3-8 sub-questions. Where it
fits the field, frame it as PICO/PECO (population, intervention/exposure, comparator, outcome) or
the qualitative equivalent. Write down what is in scope and what is explicitly out. This framing is
the first section of the report.

### 2. Build the search

Write a search plan before searching: concept blocks, synonyms, and known landmark authors or
papers per concept. Run multiple query phrasings per concept rather than one query — different
phrasings surface different literatures. Search across general web, scholarly indexes, and any
user-connected document sources. Chase citations both directions: reference lists of the best hits
(backward) and papers citing them (forward). Record the searches you ran; they go in an appendix so
the work is reproducible.

### 3. Screen and retrieve

Screen titles and abstracts against the scope from step 1. For everything that passes, retrieve the
actual source — full text where reachable, abstract plus metadata at minimum. Log the count at each
stage: found → screened → retrieved → included, with the reason for each exclusion category. Keep a
running list of sources you could not retrieve; they feed the unverified rows.

### 4. Extract claims, not papers

This is the step that makes the output different from a reading list. Read for **claims**, not
summaries. A claim is a single falsifiable assertion about the world:

> "Spaced retrieval practice improves long-term retention relative to massed restudy in adult
> learners."

Not "Smith et al. studied memory." Split compound claims. Merge near-duplicate phrasings into one
canonical claim and note the variants. Assign each claim a stable ID (`C1`, `C2`, …). Expect 8-25
distinct claims for a standard-depth synthesis.

### 5. Fill the matrix — one row per claim-source pair

For every claim, find both sides: sources that support it and sources that contradict it. Actively
hunt for disconfirming evidence — search the negation of the claim, look for null results, failed
replications, letters and commentaries, and registered reports. A matrix with no contradictions
usually means the search was one-sided, not that the field agrees.

Each row records:

| Column | Content |
|---|---|
| `claim_id` | Stable ID (C1, C2, …) |
| `claim` | The canonical claim statement |
| `stance` | `supports` / `contradicts` / `mixed` / `qualifies` |
| `citation` | Full citation: authors, year, title, journal/venue, volume/pages, DOI |
| `url` | Direct, working URL or DOI link |
| `source_tier` | 1-5 from the source hierarchy above |
| `peer_reviewed` | yes / no / unclear |
| `study_design` | RCT, prospective cohort, case-control, cross-sectional, lab experiment, quasi-experiment, modeling study, qualitative, systematic review, meta-analysis, other |
| `sample_size` | N and unit (participants, firms, trials, sites); `not reported` if absent |
| `population_setting` | Who and where |
| `effect_direction` | positive / negative / null / mixed / n/a |
| `effect_size` | Reported statistic with CI or p where available; `not reported` otherwise |
| `evidence_type` | `independent` / `replication-direct` / `replication-conceptual` / `reanalysis` / `overlapping-sample` |
| `verification` | `verified-fulltext` / `verified-abstract` / `unverified` |
| `limitations_notes` | Confounds, conflicts of interest, funding source, retraction/correction flags, why unverified |

### 6. Rate consensus per claim

After the rows are in, rate each **claim** on this fixed scale. Use it consistently and show the
supporting/contradicting counts next to every rating.

| Rating | Criteria |
|---|---|
| **Strong consensus** | ≥3 independent sources support; no credible contradicting source; designs are strong (RCT/large cohort) or converge across methods |
| **Moderate consensus** | Multiple independent supporting sources; contradictions exist but are weaker in design, smaller, or clearly context-bounded |
| **Contested** | Credible, comparable-quality evidence on both sides; no clean resolution |
| **Emerging** | Supported, but by few sources, small samples, or short follow-up; direction plausible, magnitude unstable |
| **Single-source** | Exactly one source supports it and nothing independent has tested it |
| **Unverified** | Circulates in the literature but the primary source could not be retrieved |
| **Contradicted** | Weight of evidence runs against the claim as commonly stated |

Rate on evidence quality, not vote count: one large preregistered multi-site trial can outweigh six
small underpowered studies. When you override a count-based reading, say why in one line.

### 7. Surface the disagreements

For each contested claim, write a short analysis that answers: what exactly is the disagreement
about — direction, magnitude, or scope conditions? Do the studies differ in population, measurement
instrument, dose/intensity, time horizon, or analytic choices? Is the split explainable (moderator,
methodological artifact, publication bias, funding source) or genuinely unresolved? Never average
away a contradiction into a bland middle claim; name it.

Also flag, in their own lists:

- **Single-source claims** — repeated in the field but resting on one study
- **Unverified claims** — with what was attempted
- **Replication-inflated claims** — where apparent support collapses once overlapping samples and
  reanalyses are collapsed into their underlying data
- **Methodological monocultures** — every source uses the same design, so shared limitations are
  invisible

### 8. Name the gaps

The payoff section. Derive gaps from the matrix itself, never from vibes. Give each gap an ID, and
for each one state the evidence pattern that produced it, why it matters, and a concrete study or
thesis question that would close it. Gap types to check for systematically:

- **Empty cells** — claims with no contradicting evidence because nobody tested them
- **Population gaps** — findings established in one population and assumed for others
- **Design gaps** — only correlational evidence exists where a causal test is feasible
- **Measurement gaps** — no validated or shared instrument across studies
- **Temporal gaps** — no long-term follow-up, or the evidence base predates a relevant change
- **Replication gaps** — influential single-source claims nobody has retested
- **Contradiction gaps** — a contested claim where the moderator that would resolve it has never
  been directly tested

Rank gaps by tractability for a thesis: what could realistically be executed by one researcher.

### 9. Produce the artifacts

Write both files, then summarize in chat. Never paste the whole matrix into the chat — point at the
files and give the top findings.

## Output artifacts

Write both to the workspace. Use a slug of the research question and a date.

### 1. `evidence-synthesis-<topic-slug>-<YYYY-MM-DD>.md`

The synthesis report, with inline markdown links to every source at the point the claim is made.
Sections in this order:

1. **Research question and scope** — restated question, sub-questions, inclusion/exclusion, date
   window, assumptions made on the user's behalf
2. **Method and source preference** — searches run, screening counts (found → screened → retrieved →
   included), and the explicit statement that primary peer-reviewed literature was preferred over
   secondary summaries
3. **Claim map** — every claim with its consensus rating and support/contradict counts, as a table
4. **Where the literature disagrees** — per-contested-claim analysis from step 7
5. **Fragile evidence** — single-source, unverified, replication-inflated, and monoculture claims
6. **Gaps and thesis openings** — from step 8, ranked by tractability
7. **Limitations of this synthesis** — search bias, language and access restrictions, unretrieved
   sources, and the statement that this complements rather than replaces a systematic-review
   protocol
8. **Full source list** — every source with full citation and URL, grouped by tier
9. **Appendix: search log** — queries, databases/sources, dates run

### 2. `evidence-matrix-<topic-slug>-<YYYY-MM-DD>.csv`

One row per **claim-source pair**, with exactly the columns from step 5, in that order, plus a
trailing `consensus_rating` column carrying the claim-level rating from step 6 (repeated on each row
of that claim). Quote fields containing commas; keep one line per row so the file opens cleanly in
Excel, Sheets, and pandas. Every row must have a non-empty `citation` and `url`, or
`verification = unverified` with an explanation in `limitations_notes`.

If the user asks for a spreadsheet instead of CSV, deliver an `.xlsx` with the matrix on one sheet
and the claim-level consensus summary on a second.

## Worked micro-example

Research question: *Does spaced practice improve long-term retention compared with massed practice
in adult learners?*

Claim `C3`: "Spacing benefits persist at follow-up intervals beyond six months."

| claim_id | stance | citation (abbrev.) | design | N | effect | evidence_type | verification |
|---|---|---|---|---|---|---|---|
| C3 | supports | Author A et al., 2019, *J. Applied Cognition* | RCT | 240 | d = 0.42 [0.16, 0.68] | independent | verified-fulltext |
| C3 | supports | Author B, 2021, same cohort as Author A | reanalysis | 240 | d = 0.39 | reanalysis | verified-fulltext |
| C3 | contradicts | Author C et al., 2022, *Memory Studies* | RCT | 512 | null at 8 months | independent | verified-fulltext |
| C3 | qualifies | Author D, 2020, preprint | quasi-experiment | 88 | effect only for conceptual material | independent | verified-abstract |

Consensus: **Contested** (1 independent support, 1 reanalysis of the same cohort, 1 larger
independent null). Disagreement is about **durability**, not existence: the null study used a longer
interval and free recall, the supporting study used cued recall at six months.
Gap `G2`: no study has crossed retention interval with test format in the same design — a tractable
thesis experiment.

## Quality checklist before delivering

- [ ] Every row has a full citation and a working URL, or is marked `unverified` with a reason
- [ ] No claim was dropped for being unretrievable
- [ ] Contradicting evidence was actively searched for, not just whatever turned up
- [ ] Replications, reanalyses, and overlapping samples are tagged and not counted as independent
- [ ] Every claim has a consensus rating from the fixed scale
- [ ] Single-source and contested claims are listed explicitly
- [ ] Gaps trace to specific patterns in the matrix, each with a concrete study question
- [ ] The primary-over-secondary preference is stated in the report
- [ ] Both artifacts written; the report is readable without opening the CSV
- [ ] The systematic-review caveat appears in the limitations section
