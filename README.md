<!-- Copyright (c) 2026 Arif Islam Shaik (github.com/Arif-2747). All rights reserved. -->
# human

An AI skill for removing AI writing patterns from text, writing new content
in your voice, or naming which lines in a draft read as AI without touching
them. Covers general and business prose and, since v9, narrative work:
fiction, screenplays, scripts, and personal essays. It makes generated prose
sound like a person wrote it, without simplifying or shortening anything.

---

## 1. Purpose

`human` takes AI-generated (or partially AI-generated) text and removes the
tells that mark it as machine-written: inflated significance, hedge-stacked
sentences, em dashes, rule-of-three lists, passive constructions, and 37
other documented patterns. It fixes both vocabulary and sentence structure,
adds genuine voice where the original has none, and leaves everything else
untouched.

The output matches the input in meaning, length, and complexity. Nothing is
summarized or dumbed down. Only the patterns change.

If a writing sample is provided alongside the request, the skill also
extracts the author's style and applies it to the output: either rewriting
existing text in that voice (Mode A) or generating new content from a brief
in that voice (Mode B). See [Style Mirroring](#5-style-mirroring).

If instead of a rewrite, the request is to check whether a draft reads as
AI, the skill runs Detect mode: it names each pattern present and quotes the
line, without rewriting anything or guessing at authorship. See
[Editing workflow](#4-editing-workflow).

If the input is narrative form, fiction, a screenplay, a script, or a
personal essay published under the writer's name, the skill runs a
structural process first, before its usual pattern work. See
[Section 8](#8-form-narrative-work-v9) below and
[What problem this solves](#2-what-problem-this-solves).

**Getting started:** copy `SKILL.md` and the `references/` folder into your
skills directory. The skill activates automatically on requests to humanize,
de-AI, rewrite for naturalness, remove AI patterns, write new content in
your style, or audit a draft for AI tells, across general prose and
narrative work alike. It does not activate for code review, translation, or
original drafting with no AI text and no style sample involved. To use
Style Mirroring, upload or paste your writing sample in the same message as
your request; the skill detects it and runs the profile extraction on its
own.

**New in v10: Provenance & Watermark Hygiene.** `human` now integrates
provenance and watermark hygiene capabilities from
[guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover).
While `human`'s 42 patterns address vocabulary and syntax (Layers 1-2) and
Form addresses narrative structure (Layer 3), LLMs also inject low-level
machine provenance: invisible Unicode steganography (zero-width spaces, bidi
overrides, tag characters, homoglyphs), statistical token-sampling marks
(Kirchenbauer green-lists, SynthID-Text, Aaronson/Gumbel EXP), and file
container metadata (C2PA Content Credentials, docProps, PDF info). In v10,
`human` establishes a complete 5-layer hierarchy: Layer A deterministic
Unicode hygiene, Layer B statistical entropy dispersion, and Layer C
container metadata stripping, supported by dedicated reference guides:
[watermark classes](./references/watermark-classes.md),
[removal matrix](./references/removal-matrix.md), and
[detectors and stylometry](./references/detectors-and-stylometry.md).

**New in v9: Form.** `human` and a separate narrative-focused skill,
`humanscope`, are now one skill. `humanscope` was built on
[StoryScope](https://arxiv.org/abs/2604.03136) (Russell, Rajendhran, Pham,
Iyyer, and Wieting; University of Maryland and Google DeepMind; COLM 2026),
which found that AI-generated fiction is separable from human fiction at
93.2 macro-F1 using narrative structure alone, and that professional-grade
prose rewriting only brings detection down from 95.5 to 93.9: a 1.6-point
drop. That finding is why the merge is a genuine architectural addition and
not a rename: `human`'s 42 patterns operate on vocabulary and sentence
structure, which is a cosmetic layer sitting on top of a structural signal
those patterns were never built to touch. Form (General or Narrative) is a
dimension that now sits alongside Task Type (Edit or Detect); for Narrative
form, Edit produces a Structure Sheet before drafting and Detect scores
against 30 measured narrative features instead of the 42 patterns alone.
See [Form: General or Narrative](./SKILL.md#form-general-or-narrative) in
`SKILL.md`, or [Section 8](#8-form-narrative-work-v9) below for the fuller
picture, and the [narrative mode reference](./references/narrative-mode.md)
for the full process.

**New in v8:** a Detect mode and four additional patterns, both sourced from
[no-ai-slop](https://github.com/petergyang/no-ai-slop) by Peter Yang (see
[What problem this solves](#2-what-problem-this-solves)). Detect mode
answers "does this read as AI" without rewriting, by naming and quoting the
patterns present instead of returning a probability score. Patterns 39-42
add colon reveals, interpretive metadiscourse, summary-recap endings, and a
portability test (a diagnostic for spotting generic filler, not a phrase to
strip). Every change made under v8's own [What this isn't](#what-this-isnt)
boundary: nothing here is aimed at helping text evade an AI detector, and
Detect mode explicitly refuses to render an authorship verdict of its own.
The same boundary now applies to Form's narrative audit: it reports a
feature count, never a verdict on authorship.

**New in v7:** two accuracy improvements, both scoped to the skill's own
judgment quality rather than to any external system. First, Style Profile
extraction now scores confidence per dimension instead of one blanket flag
for the whole sample, so a strong read on tone can't mask a weak read on
rhythm or punctuation (see [Style mirroring](#5-style-mirroring)). Second,
a Genre Calibration check stops the skill from misflagging genuinely
human-authored, genre-conventional writing (passive voice in a contract,
hedging in a research paper, jargon in technical documentation) as an AI
tell (see [Design principles](#3-design-principles-the-42-patterns)). Both
changes make pattern detection and style matching more precise; neither
changes what the skill outputs when the input actually is AI-generated, and
neither is designed around, or intended for, evading AI-detection systems.
See [What this isn't](#what-this-isnt) below.

---

## 2. What problem this solves

Three existing projects already cover parts of this job. None is complete
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

### no-ai-slop

[no-ai-slop](https://github.com/petergyang/no-ai-slop) by Peter Yang (MIT
licensed) is a newer, self-contained skill covering much of the same ground
as humanizer and stop-slop, plus a few patterns neither names on its own:
colon reveals used as staged punchlines, interpretive metadiscourse that
tells the reader what to notice, summary-recap endings, and faux-insight
setups that flatter the writer as the lone expert. It also documents a
useful diagnostic, the portability test: if a sentence could move unchanged
to a different person, company, or product, it's filler. And it draws a
sharp, correct line between two different jobs: editing a draft versus
auditing one for AI tells without touching it.

**What it doesn't do:** it doesn't integrate with a genre calibration
system, so applying it directly to human-authored legal or scientific
writing risks the same false-positive problem [Genre calibration](#genre-calibration-v7)
was built to prevent. Its em dash rule is also looser than this skill's
(it allows 1-2 per long draft); see the note in
[Design principles](#style-patterns-14-23) for why v8 does not adopt that
looser rule.

### watermarks-remover

[watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover) by
Guillaume Meyer addresses the machine-level provenance layer that semantic
and stylistic tools never touch: invisible Unicode steganographic carriers
(zero-width spaces, bidi overrides, language tag characters, confusable
homoglyphs), statistical token-sampling watermarks (Kirchenbauer green-lists,
SynthID-Text, Aaronson/Gumbel EXP), and document/file metadata (C2PA Content
Credentials, OOXML docProps, PDF trailers/info).

**What it doesn't do:** it is a technical provenance hygiene tool, not a prose
craft tool. It cleans metadata and disrupts token watermarks, but it does not
fix hollow prose, monotone rhythm, inflated significance, or lacking
authorial voice. Integrating it into `human` bridges low-level machine hygiene
with high-level prose authenticity.

### The problem none of the three sources solves: over-correction

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

`human` merges the 29 humanizer patterns, 9 stop-slop structural rules, and
4 non-duplicate no-ai-slop patterns into a single numbered set (42 total,
detailed in [Design principles](#3-design-principles-the-42-patterns)), then
wraps that set in an executable editorial process built on four rules
adapted from
[Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876)
on how LLMs go wrong when editing code: think before rewriting, minimum
intervention, surgical edits, and defined success criteria. Every change
made during a rewrite has to trace to one of the 42 numbered patterns or a
missing voice beat. If it doesn't trace to something documented, it doesn't
get touched. That constraint is what closes the over-correction gap that
none of the three upstream projects fully addresses on its own; no-ai-slop
gets closest, with its "minimum effective edit" principle, but ties that
discipline to its own patterns rather than to a shared numbered library with
genre calibration and Style Mirroring built in.

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
improvement is scoped the same way: it sharpens the skill's own judgment
about what counts as a tell in genuinely human-authored text; it does not
change how patterns are handled in AI-generated text.

v8's Detect mode is scoped the same way too, in the opposite direction.
Detect mode names patterns and quotes lines; it never states or implies a
verdict on whether a text was AI-written, because that would be exactly the
kind of authorship claim this skill declines to make. It is also, by
design, not something someone could use to learn how to slip a specific
draft past a detector: it reports what patterns are present, not how close
the draft is to a detector's decision boundary, and it makes no changes to
the text at all. In v10, the technical provenance and watermark hygiene
capabilities from `watermarks-remover` are integrated to address the
low-level machine provenance layer (invisible Unicode, statistical token
sampling watermarks, and container metadata) for content the user owns or
is authorized to process. This completes the full stack of text and document
hygiene without compromising the project's ethical boundary: cleaning
provenance is about personal privacy, technical hygiene, and removing
unwanted platform telemetry, never about deceptive attribution or academic
fraud. See [Section 9](#9-provenance--watermark-hygiene-v10) below.

v9's narrative audit, folded in from `humanscope`, holds the same line for
Form's Detect path: it reports a count of which of the 30 narrative
features lean AI, never a probability or a verdict, and the Structure Sheet
on the Edit side exists to make narrative writing genuinely better
structured, not to reverse-engineer a specific detector's blind spots.
Nothing in Form is built against any particular tool; it's built against
the pattern the StoryScope researchers actually measured in human writing.

---

## 3. Design principles (the 42 patterns)

The patterns are organized into seven categories, matching the structure in
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
    what matters is..." that delay the actual point, and faux-insight
    setups that flatter the writer as the lone expert ("here's what nobody
    tells you," "what most people get wrong"). Fix: state the claim
    directly.
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
    length, or every paragraph landing on a punchy one-liner, including a
    "fake-profound" kicker sentence at the end of a piece. Fix: vary
    sentence length; break the pattern; delete a kicker rather than
    polishing it into a better metaphor, and end on the clearest concrete
    sentence already in the draft.
36. **Wh- sentence starters**: sentences opening with What, When, Where,
    Which, Who, Why, or How as a structural crutch. Fix: restructure to
    lead with the subject or verb.
37. **Adverbs**: -ly adverbs and intensifiers (*really, just, literally,
    genuinely, honestly, deeply, truly, fundamentally*). Fix: remove; let
    the verb or claim carry the weight.
38. **Business jargon**: "navigate" for handle, "unpack" for explain,
    "lean into" for accept, "deep dive" for analysis. Fix: replace with the
    plain equivalent. Full table in the vocabulary reference.

### From no-ai-slop (39–42, v8)

Four patterns from [no-ai-slop](https://github.com/petergyang/no-ai-slop)
that weren't already covered by the other 38. Detect mode (see
[Editing workflow](#4-editing-workflow)) also draws on this source.

39. **Colon reveals**: a noun phrase, a colon, then a lowercase dramatic
    reveal used as a staged punchline ("the best part: it learns").
    Fix: rewrite as a plain sentence. Colons stay fine for lists, labels,
    and quotes.
40. **Interpretive metadiscourse**: lines that step outside the subject to
    tell the reader what to notice or how to weigh it ("that matters more
    than it sounds," "as you can see"). Fix: delete the aside if the
    surrounding prose already makes the point.
41. **Summary-recap endings**: "in conclusion," "ultimately," "overall," or
    a closing paragraph that restates the piece the reader just read.
    Fix: end on the last concrete point, takeaway, or next action instead.
42. **Portability test**: not a phrase to remove but a diagnostic for any
    sentence that feels generic. If it could move unchanged to a different
    person, company, or product, it's filler. Fix: replace with a fact,
    mechanism, or example specific to this subject, or cut it.

### Editorial discipline (Karpathy rules)

These four rules govern *how* the 42 patterns get applied. They sit
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

Five of the 42 patterns are also normal register choices in specific
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
to Mode A input text or to Detect mode findings; patterns 39-42 added in
v8 are not genre-exempted either, since none of the four describe a
legitimate professional register. AI-generated legal or scientific prose
overuses these same patterns well past what a genre convention would
justify, so every pattern still applies there regardless of genre. Em
dashes (#14) are never genre-exempted under any circumstance. The full
genre-by-pattern exception table, worked contrasts, and the boundary
against Mode A input are in
[`references/genre-calibration.md`](./references/genre-calibration.md).

---

## 4. Editing workflow

The skill first checks Form (General or Narrative, see
[Section 8](#8-form-narrative-work-v9)), then decides which of two jobs is
being asked for, then runs a fixed process for whichever one applies. For
Narrative form, both Detect and Edit below are replaced by the narrative
audit and Structure Sheet process in
[`references/narrative-mode.md`](./references/narrative-mode.md); this
section describes the General-form process.

### Detect (v8, from no-ai-slop)

If the request is to check whether a draft reads as AI, audit it, or flag
patterns without rewriting: walk the 42 patterns against the draft and, for
each one present, quote the line and name the pattern and its number. No
rewrite, no score, and no claim about whether AI actually wrote it; the
output is quoted evidence the user can judge, not a verdict. Offer to run
Edit on the same draft afterward. Full output template and a worked example
are in
[`references/detect-mode.md`](./references/detect-mode.md).

### Edit (default)

For a rewrite, the skill runs a fixed seven-step process:

1. **Identify form and mode.** Form (General or Narrative) first; for
   General form, Mode A (AI text + writing sample), Mode B (writing sample
   + brief, no input text), or no sample (humanize using the input's
   existing register and voice signals already present in it).
2. **Read for scope and define success criteria.** Note complexity,
   technical depth, and length; all of it survives. State assumptions; ask
   if scope, audience, or outcome is unclear. Note which patterns are
   present, target score, length constraints, and (if a sample was
   provided) a Style Fidelity target of 8+/10.
3. **Run Style Mirroring, if a sample is provided.** Extract the
   12-dimension Style Profile, show it, and wait for confirmation before
   writing anything. See [Style Mirroring](#5-style-mirroring).
4. **Draft.** Apply all 42 patterns and the confirmed Style Profile (Mode
   A), or generate from the brief with the Style Profile applied from
   sentence one (Mode B). No default to a generic register.
5. **Anti-AI audit and final rewrite.** Ask "what still reads as
   AI-generated?" and "where did the style drift from the profile?"
   Address every item found; this produces the delivered version.
6. **Score.** Rate the final version, not the draft, on the six
   dimensions in [Scoring](#6-scoring). Below 35/50 on the first five
   triggers another revision pass.
7. **Deliver with a What Changed section (v8).** A short receipt listing
   which numbered patterns were fixed and where, distinct from the
   internal audit list in step 5, so the user can check the edit against
   their own read of the draft instead of taking the rewrite on faith.

For handling text that's mostly clean already, the same minimum-intervention
rule applies at sentence granularity: identify only the sentences with
documented patterns, fix those, leave everything else verbatim, and note
which patterns were present rather than rewriting the surrounding prose for
flow.

A worked example of the process, applied to a real piece of AI-generated
text end to end (using an earlier, more granular step numbering than the
seven-step list above), is in
[`references/worked-example.md`](./references/worked-example.md).

---

## 5. Style mirroring

If a writing sample is provided alongside the request, the skill runs Style
Mirroring before writing anything.

**Mode A: rewrite in your style.** Give it AI-generated text and a sample
of your writing. It removes the 42 patterns and rewrites the output to match
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
  flagged and excluded. The 42 patterns still apply regardless of what the
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

**No sample provided (v8, from no-ai-slop):** most requests don't come with
a separate writing sample to upload. For those, the skill runs a lighter
version of the same think-before-rewriting gate: before drafting, it notes
3-5 voice signals already present in the input text itself (vocabulary,
cadence, bluntness, humor, uncertainty, digressions) and keeps them while
removing patterns. This is what stops the no-sample path from defaulting to
generic, voiceless "clean" prose, which was a real gap in the v7 process:
Style Mirroring protected voice when a sample was uploaded, but the far more
common case, no sample at all, had no equivalent protection. Detect mode
uses the same read, for a different reason: telling a genuine voice quirk
(a writer who always opens with "and") apart from an actual pattern. See
[`references/detect-mode.md`](./references/detect-mode.md).

---

## 6. Scoring

Scoring applies to Edit mode only; Detect mode returns quoted findings, not
a score (see [Editing workflow](#4-editing-workflow)). After the final
rewrite, the skill scores the output on six dimensions, 1–10 each:

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
by the [design principles](#3-design-principles-the-42-patterns) above.

The score always applies to the version after the anti-AI audit and final
rewrite, never to the first draft. Step 7, the What Changed section (v8),
is delivered alongside the score, not in place of it. This scoring table
applies to General form only; Narrative form's Detect path reports a
feature count instead, per [Section 8](#8-form-narrative-work-v9).

---

## 7. References

### Repository layout

```
SKILL.md                          Main skill file
references/
  style-profile.md                Profile template, extraction algorithm,
                                   12-dimension taxonomy, genre compatibility
                                   guide, worked Mode A and Mode B examples
  worked-example.md               Full process applied to AI-generated
                                   text, with anti-AI audit and scoring
  vocabulary.md                   Full AI vocabulary removal list, jargon
                                   replacement table, throat-clearing openers
  genre-calibration.md            v7: genre-by-pattern exception table for
                                   confirmed human-authored text
  detect-mode.md                  v8: Detect mode output template, no-sample
                                   voice-signal notes, worked example
  narrative-mode.md               v9: Structure Sheet, narrative audit
                                   process, scope limits by form
  narrative-features.md           v9: all 30 core narrative features with
                                   human and AI baseline numbers
  narrative-fingerprints.md       v9: per-model tells for Claude, GPT,
                                   Gemini, DeepSeek, Kimi
  watermark-classes.md            v10: multi-vendor AI provenance mark
                                   taxonomy (Unicode, sampling, C2PA)
  removal-matrix.md               v10: operational matrix for mark
                                   detection, mitigation, and verification
  detectors-and-stylometry.md     v10: detector families, zero-LLM
                                   stylometry metrics (burstiness, MATTR)
README.md                         This file
```

### Sources

- [blader/humanizer](https://github.com/blader/humanizer): 29 AI writing
  patterns sourced from Wikipedia's Signs of AI Writing guide (v2.5.1)
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing):
  the upstream source maintained by WikiProject AI Cleanup
- [stop-slop](https://github.com/hvpandya/stop-slop) by Hardik Pandya:
  structural rules covering binary contrasts, false agency, rhythm, and voice
- [no-ai-slop](https://github.com/petergyang/no-ai-slop) by Peter Yang
  (MIT licensed): Detect mode, patterns 39–42, and the portability test (v8)
- [StoryScope](https://arxiv.org/abs/2604.03136) (Russell, Rajendhran, Pham,
  Iyyer, Wieting; UMD and Google DeepMind; COLM 2026), via the standalone
  `humanscope` skill: Form, the Structure Sheet, and the narrative audit (v9)
- [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover):
  provenance mark taxonomy, invisible Unicode detection & stripping (Layer A),
  statistical token-sampling watermarks (Layer B), container/C2PA metadata
  cleaning (Layer C), and zero-LLM stylometric estimators (v10)
- [Karpathy on LLM pitfalls](https://x.com/karpathy/status/2015883857489522876):
  the source for the four editorial discipline principles

### Evolution & Integration

`watermarks-remover` was initially evaluated for v8 alongside no-ai-slop and
held in reserve to protect the project's non-adversarial boundary. In v10,
its technical taxonomy and hygiene principles were integrated under a clear
mandate: authorial privacy, technical cleanup, and telemetry removal on
content the user owns. It completes the 5-layer hierarchy without compromising
the core discipline.

---

## 8. Form: narrative work (v9)

v8.1 shipped `human` and a companion skill, `humanscope`, side by side,
with the two triggering separately and a documented boundary between them.
v9 reverses that decision: `humanscope`'s content is folded directly into
`human` as **Form**, a dimension that sits alongside Task Type (Edit or
Detect). There is only one skill now.

**Why the reversal.** The side-by-side architecture worked, but it put the
routing burden on two separate frontmatter descriptions trying to stay in
sync, and it required the user (or Claude) to correctly guess which of two
similarly-named skills a request belonged to before either one could run.
Folding narrative work in as a mode of the same skill removes that guess:
`human` now checks Form itself, as the first thing it does, the same way
it already checks Task Type.

**What Form actually changes.** Check the input's form before choosing a
task type: **Narrative** is a story, screenplay, scene, short film or reel
script, or a personal essay the user will publish under their own name;
**General** is everything else. For Narrative form:

- **Edit** produces a Structure Sheet of ten story-shape decisions
  (timeline, subplots, what causes the ending, how it ends, whether the
  narrator states the theme, and five more) before drafting or
  restructuring, gets sign-off, then drafts. Only after structure is set
  does the standard 42-pattern pass apply, as a cosmetic layer, never a
  substitute for it. This ordering is load-bearing: StoryScope found that
  AI-generated fiction is separable from human fiction at 93.2 macro-F1
  using structure alone, and that professional-grade prose rewriting only
  brings that down to 93.9. A 1.6-point drop. Running the pattern pass
  first, or alone, on narrative text produces prose that reads more
  smoothly but still carries the structural fingerprint.
- **Detect** scores the draft against 30 measured narrative features
  instead of scanning for the 42 patterns alone, reports which lean AI as
  a plain count (never a probability or verdict, consistent with
  [What this isn't](#what-this-isnt)), and splits structural fixes from
  cosmetic ones so the user knows which fixes will actually move the
  needle.

Full process, the ten-decision table with human and AI baseline rates,
scope limits by form (feature film vs. reel script vs. documentary), and
per-model fingerprints are in the
[narrative mode reference](./references/narrative-mode.md),
[narrative features reference](./references/narrative-features.md), and
[narrative fingerprints reference](./references/narrative-fingerprints.md).

**Provenance.** This material originates from `humanscope`, a standalone
skill built on StoryScope (Russell, Rajendhran, Pham, Iyyer, and Wieting;
University of Maryland and Google DeepMind; COLM 2026). Its Structure
Sheet, narrative audit process, and reference tables are carried into this
repository largely unchanged; only the routing layer, some terminology
(its "Mode A" and "Mode B" are renamed to avoid colliding with this
skill's own Style Mirroring Mode A/B), and the Integration/References
sections were adapted to fit a single-skill structure.

---

## 9. Provenance & watermark hygiene (v10)

v10 integrates the technical provenance and watermark hygiene framework from
[guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover).
This expands `human` from an editorial and narrative craft skill into a complete
5-layer text and document hygiene system.

### Why low-level provenance matters

Prior to v10, `human` addressed vocabulary and phrasing (Layer 1), syntax and
rhythm (Layer 2), narrative structure (Layer 3), and voice authenticity (Layer 4).
However, modern language models inject machine-level provenance signals into
generated output that no vocabulary checklist can touch:

1. **Layer A (Edit-based & Invisible Unicode)**: Zero-width spaces (`U+200B`),
   word joiners (`U+2060`), byte order marks (`U+FEFF`), bidi directional
   overrides, language tag characters, and space homoglyphs injected as
   steganographic carrier channels.
2. **Layer B (Generative Token-Sampling Watermarks)**: Mathematical biases
   applied during token generation (Kirchenbauer green-lists, Google
   SynthID-Text tournament sampling, Aaronson/Gumbel EXP). These survive
   isolated synonym swaps because the underlying n-gram and token transition
   probabilities remain intact.
3. **Layer C (Container & Document Provenance Metadata)**: C2PA Content
   Credentials manifests, OOXML `docProps/core.xml` creator tags, PDF object
   stream info dictionaries, HTML generator tags, and Markdown YAML keys.

### The 5-layer hierarchy

```
┌────────────────────────────────────────────────────────┐
│ Layer 4: Voice & Style (Style Profile, 12 dimensions)  │
├────────────────────────────────────────────────────────┤
│ Layer 3: Narrative Structure (StoryScope, 30 features) │
├────────────────────────────────────────────────────────┤
│ Layer 2: Syntax & Rhythm (Stop-slop, cadence, em dash) │
├────────────────────────────────────────────────────────┤
│ Layer 1: Vocabulary & Phrasing (42 humanizer patterns) │
├────────────────────────────────────────────────────────┤
│ Layer 0: Machine Provenance (watermarks & metadata)    │
│   ├─ Layer A: Invisible Unicode & steganography        │
│   ├─ Layer B: Statistical token-sampling watermarks    │
│   └─ Layer C: Container & C2PA provenance metadata     │
└────────────────────────────────────────────────────────┘
```

### Operational process

When processing text or documents under v10:

1. **Layer A Pre-Flight Scrub**: Scan for non-load-bearing invisible Unicode
   and exotic spaces, normalizing them before rewriting. Crucially, load-bearing
   codepoints are preserved: emoji ZWJ sequences, complex script orthography
   (Persian ZWNJ, Devanagari conjuncts), and language subdivision flag tags are
   never stripped. See [watermark classes reference](./references/watermark-classes.md).
2. **Layer B Structural Disruption**: Instead of superficial synonym cycling,
   the model reorganizes sentence dependencies, inverts clauses, varies
   sentence lengths (targeting burstiness $CV \ge 0.45$), and diversifies
   local vocabulary (targeting MATTR $\ge 0.72$). This dilutes token-sampling
   correlations below statistical detection thresholds. See
   [detectors and stylometry reference](./references/detectors-and-stylometry.md).
3. **Layer C Container Sanitization**: For Markdown and HTML files, AI generator
   tags and identifying YAML keys are purged. For binary files (DOCX, PDF, images),
   full cleaning can be performed via the optional `watermarks-remover` HTTP
   service (`POST /clean`). See [removal matrix reference](./references/removal-matrix.md).
4. **Post-Flight Hygiene Check**: Confirm that no em dashes or formatting
   anomalies were introduced during the rewrite.

### Responsible use boundary

Consistent with `human`'s founding principles, provenance hygiene is built for
**content you own or are authorized to process**: safeguarding personal privacy,
removing unwanted corporate tracking or telemetry, and restoring clean authorial
hygiene. It is not intended for academic fraud or deceptive claims of human
authorship.

---

## License

Copyright (c) 2026 Arif Islam Shaik. All rights reserved.

This skill and all files in this repository are the intellectual property of
[Arif Islam Shaik](https://github.com/Arif-2747). Reproduction, distribution,
or use in any form without express written permission is prohibited.

See the [LICENSE](./LICENSE) file for the full terms.
