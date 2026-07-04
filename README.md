<!-- Copyright (c) 2026 Arif Islam Shaik (github.com/Arif-2747). All rights reserved. -->
# human

An AI skill for removing AI writing patterns from text, or writing new content
in your voice. It makes generated prose sound like a person wrote it, without
simplifying or shortening anything.

---

## 1. Purpose

`human` takes AI-generated (or partially AI-generated) text and removes the
tells that mark it as machine-written: inflated significance, hedge-stacked
sentences, em dashes, rule-of-three lists, passive constructions, and 34
other documented patterns. It fixes both vocabulary and sentence structure,
adds genuine voice where the original has none, and leaves everything else
untouched.

The output matches the input in meaning, length, and complexity. Nothing is
summarized or dumbed down. Only the patterns change.

If a writing sample is provided alongside the request, the skill also
extracts the author's style and applies it to the output: either rewriting
existing text in that voice (Mode A) or generating new content from a brief
in that voice (Mode B). See [Style Mirroring](#5-style-mirroring).

**Getting started:** copy `SKILL.md` and the `references/` folder into your
skills directory. The skill activates automatically on requests to humanize,
de-AI, rewrite for naturalness, remove AI patterns, or write new content in
your style. It does not activate for code review, translation, or original
drafting with no AI text and no style sample involved. To use Style
Mirroring, upload or paste your writing sample in the same message as your
request; the skill detects it and runs the profile extraction on its own.

**New in v7:** two accuracy improvements, both scoped to the skill's own
judgment quality rather than to any external system. First, Style Profile
extraction now scores confidence per dimension instead of one blanket flag
for the whole sample, so a strong read on tone can't mask a weak read on
rhythm or punctuation (see [Style mirroring](#5-style-mirroring)). Second,
a Genre Calibration check stops the skill from misflagging genuinely
human-authored, genre-conventional writing (passive voice in a contract,
hedging in a research paper, jargon in technical documentation) as an AI
tell (see [Design principles](#3-design-principles-the-38-patterns)). Both
changes make pattern detection and style matching more precise; neither
changes what the skill outputs when the input actually is AI-generated, and
neither is designed around, or intended for, evading AI-detection systems.
See [What this isn't](#what-this-isnt) below.

---

## 2. What problem this solves

Two existing projects already cover parts of this job. Neither is complete
on its own.

### blader/humanizer

[blader/humanizer](https://github.com/blader/humanizer) is a pattern
reference built from Wikipedia's *Signs of AI Writing* guide. It documents
29 AI tells with before/after examples: things like significance inflation,
promotional language, vague attributions, and AI vocabulary. It is the best
documented source for what AI prose actually looks like, and it is the
primary reference for this skill's content and language patterns.

**What it doesn't do:** it is a reference, not a process. It tells you what
the patterns are but not how to apply them without collateral damage. Handed
to a model as instructions, it produces the same failure every time: the
model fixes the named patterns and also rewrites everything around them.

### stop-slop

[stop-slop](https://github.com/hvpandya/stop-slop) by Hardik Pandya covers
structural problems that humanizer doesn't: binary contrasts, false agency,
passive voice, rhythm monotony, dramatic fragmentation, and the other
sentence-level habits that make AI prose feel engineered rather than
written. It addresses *how* AI constructs sentences, not just which words it
picks.

**What it doesn't do:** it has no mechanism for scope control. Nothing in
stop-slop tells a model what to do when a paragraph is mostly clean and only
two sentences are broken. The default behavior, rewriting the whole thing
for consistency, is exactly the problem.

### The problem neither source solves: over-correction

Give a capable model either skill and ask it to humanize a piece of text. It
will often rewrite sentences that didn't need rewriting, restructure clean
paragraphs, and "improve" adjacent prose while fixing the broken sentences.
Improving things is what a model defaults to when given latitude and no
stopping condition.

The result removes the AI tells and also changes everything else. The
original voice is gone. The length shifts. The specific details the author
chose get paraphrased into something slightly different. The text sounds
less robotic and also less like the person who wrote it, or less like
whatever the original text was actually trying to do.

### How this skill solves it

`human` merges the 29 humanizer patterns and 9 stop-slop structural rules
into a single numbered set (38 total, detailed in
[Design principles](#3-design-principles-the-38-patterns)), then wraps that
set in an executable editorial process built on four rules adapted from
[Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876)
on how LLMs go wrong when editing code: think before rewriting, minimum
intervention, surgical edits, and defined success criteria. Every change
made during a rewrite has to trace to one of the 38 numbered patterns or a
missing voice beat. If it doesn't trace to something documented, it doesn't
get touched. That constraint is what closes the over-correction gap that
neither upstream project addresses.

### What this isn't

This skill targets writing quality and voice authenticity: making prose
read as though a specific person wrote it, with the specific problems
listed above fixed. It does not target AI-detection classifiers as an
adversary. There is no code path, pattern, or process step in this skill
aimed at defeating a detector, matching a detector's blind spots, or
producing output whose goal is passing as human to a scoring system rather
than actually being well-written. Any change in a detector's score on this
skill's output is an incidental side effect of the same statistical tells
(burstiness, perplexity, stock phrasing) that both a detector and a human
editor would notice, not a design target. v7's Genre Calibration
improvement (below) is scoped the same way: it sharpens the skill's own
judgment about what counts as a tell in genuinely human-authored text; it
does not change how patterns are handled in AI-generated text.

---

## 3. Design principles (the 38 patterns)

The patterns are organized into six categories, matching the structure in
`SKILL.md`. Each one below states what the tell looks like, why AI text
produces it, and how it gets fixed. Full before/after examples and the
complete vocabulary and jargon tables live in
[`references/vocabulary.md`](./references/vocabulary.md).

### Content patterns (1–6)

These are patterns in *what the text claims*, not how it's phrased.

1. **Significance inflation**: attaching outsized meaning to arbitrary
   details ("marking a pivotal moment," "standing as a testament to").
   Fix: state the fact plainly; drop the manufactured importance.
2. **Notability name-dropping**: listing outlets or institutions to imply
   importance instead of citing what was actually said or done there.
   Fix: cite the specific claim, date, and source, or cut the sentence.
3. **Superficial -ing analyses**: participle phrases ("symbolizing,"
   "reflecting," "underscoring") tacked on to simulate interpretive depth
   without doing the analysis. Fix: remove, or replace with a sourced fact.
4. **Promotional language**: ad-copy adjectives ("nestled," "breathtaking,"
   "vibrant," "must-visit") that editorialize instead of describe.
   Fix: replace with a concrete, verifiable detail.
5. **Vague attributions**: "experts believe," "critics argue," "industry
   observers have noted," a claim with no traceable source.
   Fix: name the source or remove the claim entirely.
6. **Formulaic challenges sections**: the templated "Despite X, faces
   challenges. Despite these challenges, continues to thrive" structure.
   Fix: replace with the specific, concrete difficulty and what was done
   about it.

### Language patterns (7–13)

These are patterns in *word and clause choice*.

7. **AI vocabulary**: a closed set of words that appear disproportionately
   in generated text: *delve, crucial, intricate, tapestry, testament,
   underscore, pivotal, landscape (abstract), fostering*, and more. Fix:
   replace with plain, specific language. Full list in the vocabulary
   reference.
8. **Copula avoidance**: replacing plain "is / are" with inflated verbs
   ("serves as," "stands as," "boasts," "represents"). Fix: use the copula.
9. **Negative parallelisms**: "it's not just about X, it's about Y," used
   to manufacture a reveal where a direct statement would do. Fix: state Y.
10. **Rule of three**: forcing ideas into groups of three ("innovation,
    inspiration, and insights") regardless of how many items actually
    exist. Fix: use the natural count, usually one or two.
11. **Synonym cycling**: swapping a repeated noun for near-synonyms
    ("the protagonist... the main character... the central figure") to
    avoid repetition-penalty triggers. Fix: repeat the clearest term.
12. **False ranges**: "from X to Y" spans where X and Y aren't actually
    points on a shared scale. Fix: list the actual items instead of forcing
    a range.
13. **Passive voice and subjectless fragments**: "mistakes were made,"
    sentences with no actor. Fix: find the actor, name them, use active
    voice.

### Style patterns (14–23)

These are formatting and punctuation habits that read as machine-typeset
rather than human-typed.

14. **Em dash overuse**: the single most reliable AI tell. Fix: remove all
    em dashes; replace with a period, comma, hyphen, or colon depending on
    function (prose aside, list separator, or title separator; see the
    vocabulary reference for the three replacement rules).
15. **Boldface overuse**: bold used for prose emphasis instead of UI
    labels. Fix: rewrite the sentence so the emphasis is structural, not
    typographic.
16. **Inline-header lists**: bullets that open with a bolded word and a
    colon ("**Speed:** Code generation is faster"). Fix: convert to prose.
17. **Title case headings**: "Strategic Negotiations And Partnerships"
    instead of sentence case. Fix: use sentence case throughout.
18. **Emojis**: decorative emoji in prose contexts. Fix: remove.
19. **Curly quotes**: typographic quotation marks where straight quotes
    are the norm for the target medium. Fix: use straight quotes.
20. **Hyphenated word pairs**: unnecessary hyphens in common compound
    modifiers ("cross-functional," "data-driven"). Fix: drop the hyphen
    where the pair reads fine without it.
21. **Persuasive authority tropes**: framing devices like "at its core,
    what matters is..." that delay the actual point. Fix: state the point.
22. **Signposting announcements**: "let's dive in," "here's what you need
    to know." Fix: start with the content, skip the announcement.
23. **Fragmented headers**: a heading immediately followed by one sentence
    that just restates it. Fix: merge them, or let the heading stand alone.

### Communication patterns (24–26)

Artifacts of chat-style generation that leak into prose.

24. **Chatbot artifacts**: "I hope this helps," "let me know if you have
    questions," "great question!" Fix: remove; these have no place in
    delivered prose.
25. **Knowledge-cutoff disclaimers**: "as of my last update," "while
    specific details are limited." Fix: find the fact, or cut the sentence.
26. **Sycophantic tone**: "you're absolutely right!", "that's an excellent
    point." Fix: remove; state the substance without the flattery.

### Filler and hedging (27–29)

Patterns that pad sentences without adding information.

27. **Filler phrases**: "in order to" instead of "to," "due to the fact
    that" instead of "because," throat-clearing openers like "here's the
    thing" or "it turns out." Fix: cut. Full list in the vocabulary
    reference.
28. **Excessive hedging**: stacked qualifiers ("could potentially possibly
    be argued that"). Fix: collapse to a single, direct qualifier or none.
29. **Generic positive conclusions**: "the future looks bright, exciting
    times lie ahead," with no specific content. Fix: replace with the
    actual next concrete thing, or cut the sentence.

### Structural rules: stop-slop (30–38)

Sentence- and paragraph-level construction habits, sourced from stop-slop.

30. **Binary contrasts**: telegraphed reversals ("not because X, because
    Y") used to manufacture drama. Fix: state Y directly.
31. **Negative listing**: describing what something *isn't* before saying
    what it *is*. Fix: state the positive claim directly.
32. **Dramatic fragmentation**: "Speed. Quality. Cost." used as performed
    profundity. Fix: write full sentences.
33. **Rhetorical setups**: "what if I told you...?", "think about it."
    Fix: make the point without the setup.
34. **False agency**: inanimate or abstract nouns performing human actions
    ("the decision emerged," "the complaint became a fix"). Fix: name the
    actual person or team who did the thing.
35. **Rhythm monotony**: three consecutive sentences of near-identical
    length, or every paragraph landing on a punchy one-liner. Fix: vary
    sentence length; break the pattern.
36. **Wh- sentence starters**: sentences opening with What, When, Where,
    Which, Who, Why, or How as a structural crutch. Fix: restructure to
    lead with the subject or verb.
37. **Adverbs**: -ly adverbs and intensifiers (*really, just, literally,
    genuinely, honestly, deeply, truly, fundamentally*). Fix: remove; let
    the verb or claim carry the weight.
38. **Business jargon**: "navigate" for handle, "unpack" for explain,
    "lean into" for accept, "deep dive" for analysis. Fix: replace with the
    plain equivalent. Full table in the vocabulary reference.

### Editorial discipline (Karpathy rules)

These four rules govern *how* the 38 patterns get applied. They sit
alongside the patterns as a fifth, equally weighted design principle.
Without them, pattern removal is just substitution at scale.

- **Think before rewriting.** State what the text is trying to do before
  touching it. If scope is ambiguous or two readings exist, surface both;
  never pick silently.
- **Minimum intervention.** Only change what traces to a numbered pattern
  or a missing voice beat. If a sentence is clean, it stays.
- **Surgical edits.** Leave untouched sections untouched. Don't reformat,
  don't "improve" adjacent prose, don't fix unrelated issues; flag them
  instead.
- **Define success before starting.** Turn "humanize this" into something
  verifiable: which patterns apply, target score, length tolerance. Weak
  criteria produce inconsistent output and constant re-clarification.

Voice injection (opinions, first and second person, specific feelings, a
little productive mess) is treated as a required fifth obligation, applied
alongside pattern removal rather than as a polish pass at the end. Text with
zero AI patterns and zero voice still reads as artificial.

### Genre calibration (v7)

Five of the 38 patterns are also normal register choices in specific
professional genres: #7 (AI vocabulary, domain-term subset), #13 (passive
voice), #27 (filler phrases), #28 (hedging), and #38 (jargon). Passive
voice is standard in legal and scientific writing, hedging is standard in
academic writing, and precise terminology is standard in technical
documentation. Flagging these unconditionally produces a false positive on
genuinely human, genre-appropriate writing. It strips a lawyer's or
researcher's actual professional register and replaces it with something
that no longer reads as belonging to that field.

This calibration applies only to text confirmed human-authored: a Style
Mirroring sample, or a draft submitted for line editing. It never applies
to Mode A input text. AI-generated legal or scientific prose overuses
these same patterns well past what a genre convention would justify, so
every pattern still applies there regardless of genre. Em dashes (#14) are
never genre-exempted under any circumstance. The full genre-by-pattern
exception table, worked contrasts, and the boundary against Mode A input
are in
[`references/genre-calibration.md`](./references/genre-calibration.md).

---

## 4. Editing workflow

The skill runs a fixed eight-step process for every rewrite:

1. **Identify the mode.** Mode A (AI text + writing sample), Mode B
   (writing sample + brief, no input text), or no sample (humanize using
   the input's existing register).
2. **Read for scope.** Note complexity, technical depth, and length. All of
   it survives. State assumptions; ask if scope is unclear.
3. **Define success criteria.** Which patterns are present, the target
   score, length constraints, and (if a sample was provided) a Style
   Fidelity target of 8+/10.
4. **Run Style Mirroring, if a sample is provided.** Extract the
   12-dimension Style Profile, show it, and wait for confirmation before
   writing anything. See [Style Mirroring](#5-style-mirroring).
5. **Draft.** Apply all 38 patterns and the confirmed Style Profile (Mode
   A), or generate from the brief with the Style Profile applied from
   sentence one (Mode B). No default to a generic register.
6. **Anti-AI audit.** Ask "what still reads as AI-generated?" and "where
   did the style drift from the profile?" List both.
7. **Final rewrite.** Address every audit item and correct any style
   drift. This produces the delivered version.
8. **Score.** Rate the final version, not the draft, on the six
   dimensions in [Scoring](#6-scoring). Below 35/50 on the first five
   triggers another revision pass.

For handling text that's mostly clean already, the same minimum-intervention
rule applies at sentence granularity: identify only the sentences with
documented patterns, fix those, leave everything else verbatim, and note
which patterns were present rather than rewriting the surrounding prose for
flow.

A worked example of the full process, applied to a real piece of
AI-generated text end to end, is in
[`references/worked-example.md`](./references/worked-example.md).

---

## 5. Style mirroring

If a writing sample is provided alongside the request, the skill runs Style
Mirroring before writing anything.

**Mode A: rewrite in your style.** Give it AI-generated text and a sample
of your writing. It removes the 38 patterns and rewrites the output to match
your voice, not a generic clean register.

**Mode B: write from scratch in your style.** Give it a sample and a topic
or brief, with no input text to rewrite. It generates new content that
sounds like you wrote it.

Both modes start with a **Style Profile**: a 12-dimension analysis of the
sample covering sentence length, sentence structure, paragraph length,
rhythm pattern, first/second person presence, contractions, fragment use,
recurring phrases, tone, opinion presence, typical sentence openers, and
punctuation habits. The skill outputs this profile verbatim and waits for
confirmation before writing a single output sentence. That's the same
think-before-rewriting gate from the Karpathy rules, applied to style.

Two constraints override everything else in the profile:

- **Minimum sample size is 300 words.** Below that, habits can't be
  reliably distinguished from accidents. The skill flags which dimensions
  are uncertain instead of presenting them with false confidence.
- **AI tells in the sample are never mirrored.** If the sample itself
  contains passive voice, adverbs, or significance inflation, those get
  flagged and excluded. The 38 patterns still apply regardless of what the
  sample does. Em dashes specifically are never reproduced even if the
  sample uses them; rule #14 overrides style fidelity.

**Per-dimension confidence (v7):** the profile no longer uses one blanket
flag for the whole sample. Each of the 12 dimensions is rated High, Medium,
or Low independently, based on how many instances the sample actually
gives. A sample can be well above 300 words overall and still return a Low
rating on, say, punctuation habits, if the sample happens to be light on
anything beyond periods and commas. Reporting confidence per dimension
stops a strong, obvious read (tone is usually easy to call) from masking a
weak one (rhythm and punctuation habits usually need more data than tone
does). Low-confidence dimensions are presented as tentative reads, not
firm rules, and default toward the more common register for the genre.

The full profile template, the extraction algorithm for each dimension, the
confidence-scoring method, a genre-compatibility guide (a profile from a
casual newsletter doesn't carry over to a technical spec), and worked
examples of both modes are in
[`references/style-profile.md`](./references/style-profile.md).

---

## 6. Scoring

After the final rewrite, the skill scores the output on six dimensions,
1–10 each:

| Dimension | Low (1–3) | Mid (4–7) | High (8–10) |
|-----------|-----------|-----------|-------------|
| Directness | Announcements, "it's important to note" | States points, hedges conclusions | Every sentence makes a claim or moves forward |
| Rhythm | Metronomic, identical lengths | Some variation, still predictable | Clearly varied cadence |
| Trust | Over-explains, adds disclaimers | Some hand-holding | No pre-chewed conclusions |
| Authenticity | No opinions, no first person | One or two human moments | Opinions, reader addressed, specific |
| Preservation | Shorter or simplified vs. original | Mostly intact | Full complexity and length kept |
| Style Fidelity* | Generic prose, profile ignored | Most dimensions matched | All 12 dimensions match |

*Style Fidelity is scored only when a writing sample was provided.

**Gates:** a combined score below 35/50 on the first five dimensions
triggers another revision. Style Fidelity below 8/10 also triggers a
revision when Style Mirroring is active. These are the only automated
checkpoints in the process. Everything else is editorial judgment, guided
by the [design principles](#3-design-principles-the-38-patterns) above.

The score always applies to the version after the anti-AI audit and final
rewrite, never to the first draft.

---

## 7. References

### Repository layout

```
SKILL.md                          Main skill file
references/
  style-profile.md                Profile template, extraction algorithm,
                                   12-dimension taxonomy, genre compatibility
                                   guide, worked Mode A and Mode B examples
  worked-example.md               Full 8-step process applied to AI-generated
                                   text, with anti-AI audit and scoring
  vocabulary.md                   Full AI vocabulary removal list, jargon
                                   replacement table, throat-clearing openers
  genre-calibration.md            v7: genre-by-pattern exception table for
                                   confirmed human-authored text
README.md                         This file
```

### Sources

- [blader/humanizer](https://github.com/blader/humanizer): 29 AI writing
  patterns sourced from Wikipedia's Signs of AI Writing guide (v2.5.1)
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing):
  the upstream source maintained by WikiProject AI Cleanup
- [stop-slop](https://github.com/hvpandya/stop-slop) by Hardik Pandya:
  structural rules covering binary contrasts, false agency, rhythm, and voice
- [Karpathy on LLM pitfalls](https://x.com/karpathy/status/2015883857489522876):
  the source for the four editorial discipline principles

---

## License

Copyright (c) 2026 Arif Islam Shaik. All rights reserved.

This skill and all files in this repository are the intellectual property of
[Arif Islam Shaik](https://github.com/Arif-2747). Reproduction, distribution,
or use in any form without express written permission is prohibited.

See the [LICENSE](./LICENSE) file for the full terms.
