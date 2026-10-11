---
name: humanizer
description: Rewrite or draft text so it reads naturally human rather than AI-generated, while preserving the writer's existing voice and the register appropriate to the situation. Remove robotic tone, common AI-giveaway patterns, and formulaic structure without imposing a personality. Use this skill whenever the user asks to humanize text, make writing sound natural or less like AI, remove the "ChatGPT voice," or complains that a draft sounds robotic, stiff, or generated.
---

AI-generated text has a recognizable accent. Readers in 2026 pattern match it in seconds, and the moment they detect it, trust drops: the writing reads as low-effort, and the sender reads as someone who couldn't be bothered. This skill removes the accent. The goal is not to trick anyone; it is to make writing sound like the specific person who sent it, because generic voice fails at the one job writing has: being read.

Every rule below exists because the pattern it bans is statistically overrepresented in AI output. Individually each is harmless; a human might use any one of them. It's the density that gives it away. You are managing density. That said, Humanizer should not override a deliberate voice choice merely because the same construction can also appear in AI writing. Pattern density and function matter more than the presence of any single feature.

**ABOVE ALL ELSE, THE HUMANIZER DOES NOT HAVE A PERSONALITY.**

## When used with Kevin Brand Voice

When both Kevin Brand Voice and Humanizer are used on the same piece, use this sequence:

content/argument → Kevin Brand Voice → Humanizer / Prose Pattern Audit → aloud pass

Kevin Brand Voice establishes how Kevin would express the idea. Humanizer then audits that prose for AI-associated patterns without replacing the voice that produced it. The final aloud pass checks whether the result still sounds natural in the speaker's mouth.

## The kill list: punctuation and structure

**Em dashes (—).** The single loudest tell. Do not use them, even correctly. Restructure instead:
- Two sentences: "The launch failed. Nobody had tested payments." 
- A comma: "It works, mostly."
- Parentheses for true asides: "The budget (all $40 of it) ran out."
- A colon when introducing: "One thing mattered: speed." Do not replace em dashes with spaced hyphens (—) as a mechanical swap; if the sentence needed a dash, it usually needed restructuring.

**Formulaic rhythm.** These constructions are fingerprints:
- The negation pivot: "It's not just X, it's Y." / "This isn't about X. It's about Y."
- The rule of three, everywhere: "faster, smarter, and more reliable." One triple per piece, maximum. Prefer pairs or single adjectives.
- Perfectly parallel sentences and paragraphs of identical length. Humans write lopsided. Vary hard: follow a 30-word sentence with a 4-word one.
- "Whether you're a beginner or a seasoned pro..."
- Ending every section with a tidy summary sentence.

**Structural tells:**
- Bullet points for content that should be prose. Bullets are for genuine lists, not for thinking.
- The bold-term-colon pattern repeated down a page. (Used sparingly in a reference doc, fine. As the skeleton of an essay, a tell.)
- Headers on a 400-word piece. Short writing doesn't need navigation.
- An intro that announces what the piece will say and a conclusion that repeats what it said. Cut both. Start inside the point; stop when done.
- Emoji as section decoration.

## The kill list: words and phrases

Never use these. Each has a plain replacement or can simply be cut:

delve, tapestry, testament, landscape (metaphorical), realm, journey (metaphorical), embark, unlock, unleash, elevate, empower, supercharge, game-changer, seamless, robust, leverage (as a verb), utilize, navigate (metaphorical), dive into, deep dive, at the end of the day, in today's fast-paced world, in an era of, it's important to note, it's worth noting, notably, moreover, furthermore, additionally (sentence-initial), thus, hence, overall (sentence-initial), in conclusion, ultimately, boasts, vibrant, bustling, nestled, rich history, hidden gem, must-visit, comprehensive, crucial, pivotal, foster, harness, streamline, cutting-edge, state-of-the-art, best-in-class, look no further, rest assured, I hope this email finds you well, please don't hesitate, feel free to.

Replacements are boring on purpose: "use" not "utilize", "big" not "significant", "helps" not "empowers", "also" not "additionally". If a banned word carries real meaning in context (e.g., "robust" in a statistics paper), keep it; the ban targets decoration, not meaning.

When Kevin Brand Voice permits a pattern on this kill list, Humanizer may still remove it when its density contributes to an AI-generated feel. Preserve the intended meaning, voice, and cadence rather than mechanically enforcing either guide's punctuation preferences.

## What humans do instead

**Specifics over abstractions.** "Increased efficiency across multiple workflows" says nothing. "Cut invoice processing from two days to twenty minutes" says everything. When the source text is vague, ask the user for the concrete detail rather than inventing one.

**Contractions.** It's, don't, we're, can't. Their absence is stiffness. (Exception: keep the user's register; a formal legal letter stays formal.)

**Claims with an owner.** AI hedges into mush: "This could potentially be seen as somewhat problematic." A person writes: "I think this is a mistake." One hedge is honest; three is evasion.

**Mild imperfection.** Fragments are fine. Starting with And or But is fine. A slightly informal aside is fine. Do NOT fake typos or inject slang; overcorrection into forced casualness is its own tell, and worse than the original stiffness.

**Write like you'd say it.** The test for any sentence: would the user plausibly say this out loud to a colleague? "I wanted to reach out to touch base regarding..." fails. "Quick question about..." passes.

## What natural prose does (higher-order pattern checks)

**Do not confuse polish with artificiality.** Flag a pattern because of its function and frequency, not merely because it is rhetorically effective.

**Prefer earned irregularity over manufactured irregularity.** Do not add sentence fragments, slang, parentheticals, typos, random asides, or awkwardness merely to make prose appear human. Humanization comes from preserving authentic reasoning and cadence, not simulating imperfection.

**Watch for excessive argumentative efficiency.** AI prose often gives every paragraph exactly one job and makes every transition advance the thesis cleanly. Real prose can linger, qualify, double back briefly, or discover a secondary implication. Do not force inefficiency, but allow it when it reflects genuine thought.

**Audit symmetrical scaffolding.** Constructions such as "All true. But…," "That’s not X. That’s Y," and setup/reveal paragraph pairs are not forbidden. Flag them when several occur in the same piece or when the contrast could have been stated naturally without the rhetorical staging.

**Preserve intentional terminology.** A triad or repeated phrase should survive when the terms carry defined meaning. "Supported, verifiable, and traceable" should not be broken apart merely because three-part constructions are common in AI prose.

**Look for unnecessary compression as well as unnecessary verbosity.** Humanization is not always shortening. AI-assisted editing can over-compress prose into maximally efficient sentences that no longer resemble natural speech.

**Pronoun drift is a prose clue.** Check whether "we," "you," "they," and "the organization" change without an argumentative reason. Consistent perspective often makes prose sound more naturally spoken—not because it is an AI marker, but because fixing it prevents edited prose from feeling assembled.

**The final paragraph does not have to tie a bow.** AI prose has a strong tendency to restate the thesis in a polished final formulation. Prefer an ending that feels like the thought naturally arrived somewhere: a concrete consequence, understated observation, question, or useful final line.

**Do a final read-aloud check.** Do not ask only "Does this sound natural?" Track who is speaking, who is being addressed, and where the speaker would naturally emphasize, pause, qualify, or add a thought.

## Workflow

1. **Establish voice.** If rewriting the user's text, their existing word choices, formality level, and quirks are the target voice; preserve them. If drafting fresh, ask for or infer register (who's reading it, what's the relationship) before writing.
2. **Rewrite at sentence level, not word level.** Swapping banned words for synonyms leaves the robotic skeleton intact. Restructure the sentence around its actual point.
3. **Preserve every fact.** Humanizing must not change meaning, numbers, names, or commitments. If shortening loses a fact, keep the fact.
4. **Do a density pass.** Reread the output hunting for: any em dash, any kill-list word, more than one triple, uniform paragraph lengths, a throat-clearing opener, a summarizing closer. Fix what you find before showing the user.
5. **Show, don't lecture.** Return the rewritten text. Only explain the changes if asked, and never annotate the text with meta-commentary.

## Example

**Before (AI accent):**
"In today's fast-paced business landscape, effective communication is crucial. Our comprehensive platform doesn't just streamline workflows—it empowers teams to unlock their full potential. Whether you're a startup founder or a seasoned executive, our robust suite of tools will elevate your productivity journey."

**After (human):**
"Most teams waste hours a week on status updates. Ours cuts that to one 15-minute sync. We built it for our own team first, and we still use it every day."

Note what changed: the abstraction became a number, the em dash became a period, the audience-pandering line disappeared, and the credibility now comes from a concrete detail instead of adjectives.

## Register warning

Humanizing is not the same as casualizing. A condolence note, a board memo, and a group-chat message are all human, and none of them sound alike. Match the situation. The skill removes the AI accent from whatever register the writing needs, it does not drag everything toward breezy startup-speak.