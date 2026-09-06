---
name: human
version: 9.0.0
description: |
  Use this skill to humanize text, remove AI writing patterns, strip multi-vendor
  AI watermarks (invisible Unicode Layer A, statistical sampling Layer B, and
  C2PA/metadata), make prose sound natural, fix robotic writing, write in the
  user's voice, or check if a draft reads as AI. Covers general/business prose,
  narrative work (fiction, screenplays, scripts, essays), and file provenance.
  Triggers on "this sounds like AI," "make this more human," "de-AI this,"
  "rewrite this naturally," "write in my style," "remove the AI tells," "does
  this read as AI," "strip AI watermarks," "remove C2PA," "clean AI metadata,"
  "remove invisible Unicode," /remove-ai-marks, or narrative drafting. Applies 42
  patterns (humanizer, stop-slop, no-ai-slop), Unicode hygiene, token entropy
  dispersion, voice injection, Style Mirroring, Detect mode, Karpathy editorial
  discipline, and StoryScope 30-feature narrative audit. Preserves full
  complexity, depth, and length; never simplifies or summarizes.
---

# Human

Make AI-generated text sound like a person wrote it, or tell someone which
lines in a draft read as AI without touching them. Remove the 42 documented
patterns, fix broken sentence structures, and add genuine voice, keeping
every piece of the original meaning, complexity, and length intact. For
narrative form, also run a Structure Sheet or a 30-feature audit before the
patterns apply; see [Form](#form-general-or-narrative).

## Core Concepts

Do not simplify, summarize, or shrink scope. Output matches input length and
depth. Only the patterns change.

**Why patterns persist after editing:** Most tools fix vocabulary but leave
sentence structure intact. AI text fails at both levels simultaneously. Fixing
one without the other produces cleaner but still obviously generated text.

**Over-correction is the primary failure mode:** A model with latitude to
"make this sound better" rewrites clean sentences alongside broken ones. Every
change must trace to a documented pattern. If it does not, leave it alone.

**Voiceless prose reads as artificial even without tells:** No AI vocabulary,
no em dashes, no adverbs, but still no opinions, no specific feelings, no
reader addressed directly. Pattern removal and voice injection are both required.

**Genre governs whether a pattern is a tell (v7):** passive voice, hedging,
and jargon are legitimate in legal, scientific, and technical writing.
Confirm genre before flagging these in human-authored text; see the
[genre calibration reference](./references/genre-calibration.md). **Editing
and detecting are different jobs (v8):** ask which one the user wants before
doing either; see [Task Type](#task-type-edit-or-detect). **Structure
carries narrative AI-detection (v9):** StoryScope (Russell et al., COLM
2026) separates human from AI fiction at 93.2 macro-F1 using structure
alone, and stays at 93.9 after prose rewriting: a 1.6-point drop. For
fiction, screenplays, scripts, and essays, set structure first; the 42
patterns are a cosmetic pass after. See [Form](#form-general-or-narrative).

## Form: General or Narrative

Check the input's form before choosing a task type below. **Narrative:** a
story, screenplay, scene, short film or reel script, or a personal essay
the user will publish under their own name. **General:** everything else.

For Narrative form, Edit and Detect both change shape: **Edit** produces a
Structure Sheet of ten story-shape decisions (timeline, subplots, what
causes the ending, and seven more) before drafting or restructuring, gets
sign-off, then drafts; only after structure is set does the pattern pass
below apply, as a cosmetic layer, never a substitute. **Detect** scores the
draft against 30 measured narrative features instead of the 42 patterns
alone, reports which lean AI, and splits structural fixes from cosmetic
ones.

Full process, the ten-decision table, scope limits by form, and model
fingerprints are in the
[narrative mode reference](./references/narrative-mode.md). For General
form, proceed directly to Task Type below.

## Task Type: Edit or Detect

**Edit (default).** The user wants a rewrite. Run the full process below
and return the edited draft.

**Detect.** The user asks whether a draft reads as AI, or asks to audit,
scan, or flag it without rewriting. Walk the 42 patterns against the draft,
quoting the exact line and naming the pattern and number for each one
present. Don't rewrite, score, or claim to know whether AI wrote it: this
gives quoted evidence to judge, not a verdict. Offer to run Edit after. See
[detect mode reference](./references/detect-mode.md) for the output
template and a worked example.

## Style Mirroring (When a Writing Sample is Provided)

**Mode A:** AI text + writing sample. Remove patterns and apply user's style.
**Mode B:** Writing sample + brief, no input text. Generate new content in their voice.

Both modes require a Style Profile across 12 dimensions before writing
anything: show it, get confirmation, then write. Below 300 words, flag
uncertain dimensions and score confidence per dimension (v7), not once for
the whole sample. If the sample contains AI tells, don't mirror them; flag
them. See [style profile reference](./references/style-profile.md) for the
full template, extraction algorithm, genre guide, and worked examples.

**No sample provided (v8):** before drafting, note 3-5 voice signals already
in the input itself (vocabulary, cadence, bluntness, humor, uncertainty,
digressions) and keep them while removing patterns, so output doesn't drift
into generic, voiceless "clean" prose. See
[detect mode reference](./references/detect-mode.md) for how this note is
also used during a Detect pass.

## Handling Partially Clean Text

When most sentences have no AI tells, apply the same minimum-intervention
rule: fix only sentences with documented patterns, leave the rest verbatim,
and note which patterns were present. Don't rewrite surrounding sentences
for flow; note an awkward join instead of expanding the edit.

## The 42 Patterns

### Content Patterns

**1. Significance inflation**
AI inflates arbitrary details by claiming they "mark" or "represent"
something larger: "marking a pivotal moment," "standing as a testament to."
Before: "The institute was established in 1989, marking a pivotal moment."  After: "The institute was established in 1989 to collect regional statistics."

**2. Notability name-dropping**
AI lists outlets to claim importance instead of citing actual coverage.
Before: "Her views have been cited in The New York Times, BBC, and the FT."  After: "In a 2024 New York Times interview, she argued for outcome-based regulation."

**3. Superficial -ing analyses**
Participle phrases tacked on for fake depth: "symbolizing," "underscoring,"
"highlighting," "showcasing." Remove or replace with a sourced fact.
Before: "The palette resonates with the region, symbolizing local bluebonnets."  After: "The architect said the blue and gold referenced local bluebonnets."

**4. Promotional language**
Ad words: "nestled," "breathtaking," "groundbreaking," "vibrant," "renowned,"
"boasts," "must-visit," "stunning," "rich" (figurative).
Before: "Nestled in breathtaking Gonder, the town boasts vibrant heritage."  After: "The town is in the Gonder region, known for its weekly market."

**5. Vague attributions**
"Experts believe," "industry observers have noted," "critics argue," "studies
show." Name the specific source or cut the claim.
Before: "Experts believe the river plays a crucial role in the ecosystem."  After: "The river supports endemic fish species, per a 2019 CAS survey."

**6. Formulaic challenges sections**
"Despite [X], faces challenges. Despite these, continues to thrive."
Replace with the specific facts about what is hard.
Before: "Despite prosperity, Korattur faces challenges; despite these,
it continues to thrive."  After: "Traffic congestion rose after 2015."

### Language Patterns

**7. AI vocabulary**
Remove: additionally, align with, crucial, delve, emphasizing, enduring,
enhance, facilitate, fostering, garner, harness, highlight (verb),
intricate, key (adjective), landscape (abstract), paradigm shift, pivotal,
showcase, supercharge, tapestry (abstract), testament, underscore (verb),
utilize, valuable, vibrant. Full list: see
[vocabulary reference](./references/vocabulary.md).

**8. Copula avoidance**
AI substitutes elaborate constructions for "is" and "are": "serves as," "stands as," "marks," "represents," "boasts," "features."
Before: "Gallery 825 serves as LAAA's exhibition space and boasts 3,000 sq ft."  After: "Gallery 825 is LAAA's exhibition space. It has 3,000 sq ft."

**9. Negative parallelisms**
"It's not just about X, it's Y." State Y directly.
Before: "It's not just about autocomplete; it's about unlocking creativity."  After: "It unlocks creativity at scale."

**10. Rule of three**
AI forces ideas into groups of three. Use the natural number, often one or two.
Before: "The event features keynotes, panels, and networking opportunities."  After: "The event has talks and panels, with time between for conversation."

**11. Synonym cycling**
Repetition-penalty logic causes excessive substitution. Repeat the clearest noun.
Before: "The protagonist faces challenges. The main character overcomes obstacles."  After: "The protagonist faces and overcomes many challenges."

**12. False ranges**
"From X to Y" where X and Y aren't on a meaningful scale.
Before: "From the Big Bang to dark matter, from stars to cosmic structure."  After: "The book covers the Big Bang, star formation, and dark matter theories."

**13. Passive voice and subjectless fragments**
Find the actor and name them.
Before: "Mistakes were made. No configuration file needed."  After: "The team made a mistake. You do not need a config file."

### Style Patterns

**14. Em dash overuse**
Remove all em dashes (—) from prose, no exceptions for length or count; the
single most reliable AI tell, and stricter than looser "1-2 per draft"
conventions elsewhere. Replace by function: aside/connector → period,
comma, or hyphen; list separator → comma or colon; title separator → colon,
never a comma ("Pipeline — Deep Learning" → "Pipeline: Deep Learning").
Full examples: see [vocabulary reference](./references/vocabulary.md).

**15. Boldface overuse**
Bold is for UI labels, not prose emphasis. Rewrite the sentence instead.

**16. Inline-header lists**
Bullet points starting with a bold word and colon ("**Speed:** Code generation is faster") read as AI structure. Convert to prose.

**17. Title case headings**
Use sentence case: "Strategic negotiations and partnerships," not "Strategic Negotiations And Partnerships."

**18. Emojis**
Remove all emojis in prose contexts, including emoji section headings.
Before: "We launched three new features! 🚀 Check them out below."  After: "We launched three new features. Details below."

**19. Curly quotes**
Use straight quotes ("") not curly quotes (" ").

**20. Hyphenated word pairs**
Drop hyphens from common pairs that don't need them: "cross-functional
teams" → "cross functional teams," "data-driven decisions" → "data driven decisions."

**21. Persuasive authority tropes**
Framing that delays the point instead of stating it ("at its core, what
matters is..."), and faux-insight setups flattering the writer as the lone
expert ("here's what nobody tells you," "what most people get wrong").
Before: "The part everyone misses: distribution is the real moat."  After: "Distribution is the moat."

**22. Signposting announcements**
"Let's dive in," "Here's what you need to know," "Let me walk you through." Start with the content.

**23. Fragmented headers**
A heading followed by one sentence that just restates it. Merge them, or
let the heading carry the weight alone.

### Communication Patterns

**24. Chatbot artifacts**
Remove: "I hope this helps," "Let me know if you have questions," "Great question!", "Of course!", "Would you like me to expand?"

**25. Knowledge-cutoff disclaimers**
Remove: "As of my last update," "While specific details are limited." Find the fact or cut the sentence.

**26. Sycophantic tone**
Remove: "You're absolutely right!", "That's an excellent point."

### Filler and Hedging

**27. Filler phrases**
"In order to" → "to." "Due to the fact that" → "because." Cut
throat-clearing openers: "Here's the thing," "It turns out," "The reality
is." Full list: see [vocabulary reference](./references/vocabulary.md).

**28. Excessive hedging**
"Could potentially possibly" → "may." Cut stacked hedges.
Before: "It could potentially be argued that these tools might help."  After: "These tools help with repetitive tasks."

**29. Generic positive conclusions**
"The future looks bright. Exciting times lie ahead." Replace with the specific next thing, or cut.
Before: "In conclusion, the future looks bright. Exciting times lie ahead."  After: "The company plans to open two more locations next year."

### Structural Rules (Stop-Slop)

**30. Binary contrasts**
Telegraphed reversals ("Not because X. Because Y.") manufacture drama. State Y.

**31. Negative listing**
Listing what something is *not* before saying what it *is*. State it directly.

**32. Dramatic fragmentation**
"Speed. Quality. Cost." Fragments read as performed profundity. Use sentences.

**33. Rhetorical setups**
"What if I told you...?" "Think about it." Make the point without the setup.

**34. False agency**
Inanimate things do not perform human actions.
Before: "The decision emerged." / "The complaint became a fix."  After: "The team decided." / "Sarah fixed it that week."

**35. Rhythm monotony**
Three consecutive same-length sentences: break one. Three-item lists:
reduce to two or one. A fake-profound kicker ending on a punchy "deep"
aphorism: delete it rather than polish it, and end on the clearest
concrete sentence already in the draft, or a plain takeaway.

**36. Wh- sentence starters**
Sentences starting with What, When, Where, Which, Who, Why, How. Restructure
to lead with the subject or verb.
Before: "What makes this hard is the coordination overhead."  After: "The coordination overhead is the hard part."

**37. Adverbs**
Kill all -ly adverbs, plus really, just, literally, genuinely, honestly, simply, actually, deeply, truly, fundamentally, inherently.
Before: "This is genuinely important and will significantly impact teams."  After: "This changes how teams collaborate."

**38. Business jargon**
Navigate → handle. Unpack → explain. Lean into → accept. Landscape →
situation. Double down → commit. Deep dive → analysis. Full table: see
[vocabulary reference](./references/vocabulary.md).

### From no-ai-slop (39-42, v8)

**39. Colon reveals**
A noun phrase, a colon, then a lowercase dramatic reveal used as a staged
punchline. Colons stay fine for lists, labels, and quotes.
Before: "The detail that makes it work: a separate agent grades it."  After: "A separate agent does the grading, which is what makes it work."

**40. Interpretive metadiscourse**
Lines that step outside the subject to tell the reader what to notice
("that matters more than it sounds," "the key point is," "as you can see").
Delete the aside if the surrounding prose already makes the point.

**41. Summary-recap endings**
"In conclusion," "Ultimately," "Overall," or a closing paragraph restating
the piece the reader just read. End on the last concrete point, takeaway,
or next action instead.

**42. Portability test (diagnostic, not a removable phrase)**
A check to run on any sentence that feels generic: if it could move
unchanged to a different person, company, or product without anyone
noticing, it is filler. Replace with a fact, mechanism, or example specific
to this subject, or cut it.
Before: "The integration improved efficiency significantly."  After: "The integration cut deploy time from 40 minutes to 4."

## Editorial Discipline (Karpathy Rules)

These govern *how* to edit. Voice injection (opinions, "I" and "you,"
specific feelings, some mess) is the fifth obligation; apply alongside
pattern removal, not after.

**Think before rewriting.** State what you assume the text is doing; if
ambiguous, surface the interpretations instead of picking silently, and ask
if audience or outcome is unclear and would change the edit.

**Minimum intervention.** Only change what has a documented pattern; every
changed sentence traces to one of the 42 patterns or a missing voice beat,
proportional to the slop actually present.

**Surgical edits only.** Leave clean sentences alone, don't reformat
untouched sections, and flag unrelated issues instead of fixing them.

**Define success before starting.** Turn the task into a verifiable goal:
"Humanize this" → patterns removed, score 38/50+, length ±10%. "Write in my
style" → Style Fidelity 8+/10. "Is this AI slop?" → Detect mode: every
pattern named and quoted, nothing rewritten.

## Process

**Step 1: Identify form and task type.**
Narrative or General (see [Form](#form-general-or-narrative)); Narrative
routes to the narrative mode reference before this process continues. Then:
Detect, no rewrite, jump to [Task Type](#task-type-edit-or-detect). Edit,
Mode A: AI text + sample, rewrite in user's style. Edit, Mode B: sample +
brief with no input text, write from scratch. Edit, no sample: humanize
using the input's existing register and voice signals.

**Step 2: Read for scope and define success criteria.**
Note complexity, technical depth, and length; all survive. State
assumptions and ask if scope, audience, or outcome is unclear. Note which
patterns are present, target score, length constraints, and add "Style
Fidelity 8+/10" if a sample was provided.

**Step 3: Run Style Mirroring if a sample is provided.**
Extract the 12-dimension Style Profile, show it, wait for confirmation, and
confirm genre compatibility. Then proceed.

**Step 4: Draft.**
Mode A: apply all 42 patterns, touching only documented tells, and apply the
confirmed Style Profile to every structural decision. Mode B: generate from
the brief with the Style Profile applied from sentence one; no default to a
generic register.

**Step 5: Anti-AI audit and final rewrite.**
Ask "what makes this still obviously AI-generated?" and "where did the
style drift from the profile?" Address every item found; this produces the
final version.

**Step 6: Score (1-10 each; below 35/50 on first five: revise).**

| Dimension | Low (1-3) | Mid (4-7) | High (8-10) |
|-----------|-----------|-----------|-------------|
| Directness | Announcements, "it's important to note" | States points, hedges conclusions | Every sentence makes a claim or moves forward |
| Rhythm | Metronomic, identical lengths | Some variation, still predictable | Clearly varied cadence |
| Trust | Over-explains, adds disclaimers | Some hand-holding | No pre-chewed conclusions |
| Authenticity | No opinions, no first person | One or two human moments | Opinions, reader addressed, specific |
| Preservation | Shorter or simplified vs. original | Mostly intact | Full complexity and length kept |
| Style Fidelity* | Generic prose, profile ignored | Most dimensions matched | All 12 dimensions match |

*Style Fidelity scored only when a sample was provided. Below 8: revise.

**Step 7: Deliver with a What Changed section (v8).**
List which numbered patterns were fixed and where, in a few lines: a short
receipt so the user can check the edit rather than take it on faith.

## Gotchas

1. **Fixing vocabulary while leaving structure intact.** Swapping "delve" for
   "examine" doesn't help if the sentence is still passive and still ends
   on a punchy slogan. Vocabulary and structure both need work.
2. **Voice injection that contradicts the original tone.** "I genuinely
   think..." belongs in a blog post, not a technical spec. Match the register.
3. **Treating the checklist as a sequential filter.** It's a pre-delivery
   gate. Read the full text, form a picture of what's broken, then rewrite
   with all patterns in mind at once, not item by item.
4. **Partial clean text becomes full rewrite.** The model tends to "improve"
   surrounding sentences for flow. Fix only the broken ones and stop there.
5. **Scoring the draft, not the final version.** The score applies after
   the anti-AI audit and final rewrite, never before it.
6. **Missing the register mismatch after voice calibration.** An ignored
   sample produces generic prose, not a rewrite in the sample's own voice.
7. **Mirroring across incompatible genres.** A casual-newsletter profile
   doesn't transfer to a technical spec; flag the mismatch and ask.
8. **Sample too short to extract reliably.** Below 300 words, flag
   uncertain dimensions rather than guessing.
9. **User's sample contains AI patterns.** Don't mirror them; flag the
   tells so the user knows they weren't reproduced.
10. **Rewriting during a Detect request (v8).** Fixing it too defeats the
    point of asking for evidence to check themselves. Name and quote only.
11. **Guessing at AI authorship in Detect mode (v8).** Naming patterns
    isn't a verdict; the patterns are evidence, not a classifier score.
12. **Running the pattern pass on narrative text before structure is set
    (v9).** The fingerprint is structural; run the Structure Sheet or the
    30-feature audit before, not instead of, the 42 patterns.
13. **Misreading form from subject matter alone (v9).** A personal essay
    about a real event is still Narrative if it's published under the
    writer's name; a business case study about a real event is General.
    Form is about publication and genre, not whether the content is true.

## Quick Checklist

Before delivering, confirm:

**Task type (v8)**
- [ ] Edit or Detect identified before starting
- [ ] Detect requests: no rewrite produced, no AI-authorship verdict given
- [ ] Input form checked before starting (General/Narrative, v9); Narrative: Structure Sheet or 30-feature audit run before the pattern pass

**Style Mirroring (if sample provided)**
- [ ] Mode identified (A: rewrite in style / B: write from scratch); Style Profile extracted across all 12 dimensions
- [ ] Sample word count noted, flag if below 300 words; AI patterns in sample noted and excluded from mirroring
- [ ] Profile shown to user and confirmed before writing; genre compatibility confirmed
- [ ] Sentence length, fragments, contractions, person, rhythm, and tone all match the sample, not a generic register
- [ ] Punctuation reproduced from sample except em dashes, which are never mirrored; rule #14 overrides style fidelity
- [ ] Style Fidelity scored 8+/10 before delivering; no sample: 3-5 voice signals from the input noted and preserved

**Karpathy discipline**
- [ ] Assumptions about the text's purpose stated before editing; success criteria defined
- [ ] Every changed sentence traces to a numbered pattern or voice gap; no clean sentences rewritten speculatively; no unrequested restructuring
- [ ] No em dashes introduced during drafting [#14]: check before writing, not only after

**Pattern removal**
- [ ] No adverbs [#37]; no passive voice [#13]; no inanimate actors [#34]; no Wh- starters [#36]; no throat-clearing openers [#27]
- [ ] No "not X, it's Y" [#30]; no colon reveals [#39]; no fake-profound kicker [#35]; no interpretive metadiscourse [#40]; no summary-recap ending [#41]
- [ ] No em dashes [#14]; no vague declaratives [#1]; no rule-of-three [#10]; no curly quotes, boldface, emojis, inline-header bullets [#15-19]
- [ ] No AI vocabulary [#7]; no chatbot artifacts [#24]; no generic conclusion [#29]; no signposting or faux-insight setups [#21, #22]; no unnecessary hyphens [#20]
- [ ] Generic sentences pass the portability test or were cut [#42]

**Voice and delivery**
- [ ] Personality present: opinions, specific feelings, reader addressed [Soul]
- [ ] What Changed section included with the delivered edit (Edit mode only)

## Integration

- `karpathy-guidelines`: source for the four editorial discipline rules
- `humanizer`: upstream pattern reference; consult for edge cases beyond the 42
- `stop-slop`: upstream structural rules; consult when a structural pattern is ambiguous
- `no-ai-slop`: source of Detect mode, patterns 39-42, and the portability test (v8)
- StoryScope (Russell et al., COLM 2026): source of Form (v9), the
  Structure Sheet, the 30-feature narrative audit, and model fingerprints;
  see [narrative mode reference](./references/narrative-mode.md)

## References

Internal:
- [Style profile reference](./references/style-profile.md) — profile template,
  12-dimension taxonomy, extraction algorithm, genre guide, worked examples
- [Worked example](./references/worked-example.md) — the process applied
  step by step to AI-generated text (older numbering; What Changed, step 7
  above, is v8-only and not shown there)
- [Vocabulary reference](./references/vocabulary.md) — full AI vocabulary list,
  jargon table, and throat-clearing openers
- [Genre calibration reference](./references/genre-calibration.md) — v7:
  genre-by-pattern exceptions for confirmed human-authored text
- [Detect mode reference](./references/detect-mode.md) — v8: output template,
  no-sample voice-signal notes, and a worked Detect-mode example
- [Narrative mode reference](./references/narrative-mode.md) — v9:
  Structure Sheet, narrative audit process, scope limits by form
- [Narrative features reference](./references/narrative-features.md) — v9:
  all 30 core features with human and AI baseline numbers
- [Narrative fingerprints reference](./references/narrative-fingerprints.md) — v9:
  per-model tells for Claude, GPT, Gemini, DeepSeek, Kimi

External:
- [blader/humanizer](https://github.com/blader/humanizer) — 29 patterns from Wikipedia's Signs of AI Writing (v2.5.1)
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
- [stop-slop](https://github.com/hvpandya/stop-slop) by Hardik Pandya
- [no-ai-slop](https://github.com/petergyang/no-ai-slop) by Peter Yang (v8, MIT licensed)
- [StoryScope](https://arxiv.org/abs/2604.03136) (Russell, Rajendhran, Pham, Iyyer, Wieting; UMD/Google DeepMind; COLM 2026) — source of Form (v9)
- [Karpathy on LLM pitfalls](https://x.com/karpathy/status/2015883857489522876)

---

## Skill Metadata

**Created**: 2025-06-07  **Updated**: 2026-08-31  **Version**: 9.0.0
**Based on**: blader/humanizer v2.5.1, stop-slop, no-ai-slop, StoryScope (via humanscope), human v8.1.0, Karpathy guidelines
