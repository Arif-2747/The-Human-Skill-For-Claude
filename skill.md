---
name: human
version: 7.0.0
description: |
  Use this skill whenever the user wants to humanize text, remove AI writing
  patterns, make prose sound natural, fix robotic writing, or write new content
  in their own voice. Triggers on: "this sounds like AI," "make this more
  human," "de-AI this," "rewrite this naturally," "write in my style," "remove
  the AI tells," or any request to edit prose for naturalness. Applies 38
  documented patterns (29 from blader/humanizer + 9 stop-slop structural rules),
  voice injection, Style Mirroring (rewrite or write-from-scratch in the user's
  style from an uploaded sample), and Karpathy editorial discipline. Preserves
  full complexity, depth, and length. Never simplifies or summarizes.
---

# Human

Make AI-generated text sound like a person wrote it. Remove the 38 documented
AI writing patterns. Fix broken sentence structures. Add genuine voice. Keep
every piece of the original meaning, complexity, and length intact.

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
[genre calibration reference](./references/genre-calibration.md).

## Style Mirroring (When a Writing Sample is Provided)

**Mode A:** AI text + writing sample. Remove patterns and apply user's style.
**Mode B:** Writing sample + brief, no input text. Generate new content in their voice.

Both modes require a Style Profile across 12 dimensions before writing
anything. Show it. Get confirmation. Then write.

Below 300 words: flag uncertain dimensions. Score confidence per dimension
(v7), not just once for the whole sample. If the sample contains AI tells,
do not mirror them; flag them.

See [style profile reference](./references/style-profile.md) for the full
profile template, extraction algorithm, genre compatibility guide, and worked
examples of both modes.

## Handling Partially Clean Text

When most sentences have no AI tells, apply the same minimum-intervention rule:

1. Identify only the sentences with documented patterns.
2. Fix those. Leave everything else verbatim.
3. Note which patterns were present and which sentences changed.

Do not rewrite surrounding sentences for flow. If fixing one sentence creates
an awkward join, note it rather than expanding the edit.

## The 38 Patterns

### Content Patterns

**1. Significance inflation**
AI inflates arbitrary details by claiming they "mark" or "represent" something
larger. Remove "marking a pivotal moment," "standing as a testament to,"
"serving as a reminder," "playing a vital role."

Before: "The institute was established in 1989, marking a pivotal moment in
the evolution of regional statistics."
After: "The institute was established in 1989 to collect regional statistics
independently from the national office."

**2. Notability name-dropping**
AI lists outlets to claim importance instead of citing actual coverage.

Before: "Her views have been cited in The New York Times, BBC, and the FT."
After: "In a 2024 New York Times interview, she argued that regulation should
focus on outcomes rather than methods."

**3. Superficial -ing analyses**
Participle phrases tacked on to add fake depth: "symbolizing," "reflecting,"
"underscoring," "highlighting," "showcasing." Remove or replace with sourced fact.

Before: "The color palette resonates with the region's natural beauty,
symbolizing local bluebonnets and reflecting the community's connection."
After: "The architect said the blue and gold referenced local bluebonnets."

**4. Promotional language**
Ad words: "nestled," "breathtaking," "groundbreaking," "vibrant," "renowned,"
"boasts," "must-visit," "stunning," "rich" (figurative).

Before: "Nestled within the breathtaking region of Gonder, the town boasts a
vibrant cultural heritage."
After: "The town is in the Gonder region of Ethiopia, known for its weekly
market and 18th-century church."

**5. Vague attributions**
"Experts believe," "industry observers have noted," "critics argue." Name the
specific source or cut the claim.

Before: "Experts believe the river plays a crucial role in the regional ecosystem."
After: "The river supports endemic fish species, per a 2019 Chinese Academy
of Sciences survey."

**6. Formulaic challenges sections**
"Despite [positive thing], faces challenges. Despite these challenges, continues
to thrive." Replace with the specific facts about what is actually hard.

Before: "Despite its industrial prosperity, Korattur faces challenges typical
of urban areas. Despite these challenges, with its strategic location, Korattur
continues to thrive."
After: "Traffic congestion increased after 2015 when three IT parks opened.
The municipal corporation started a stormwater drainage project in 2022."

### Language Patterns

**7. AI vocabulary**
Remove: additionally, align with, crucial, delve, emphasizing, enduring,
enhance, fostering, garner, highlight (verb), interplay, intricate, key
(adjective), landscape (abstract), pivotal, showcase, tapestry (abstract),
testament, underscore (verb), valuable, vibrant.
Full list: see [vocabulary reference](./references/vocabulary.md).

**8. Copula avoidance**
AI substitutes elaborate constructions for "is" and "are": "serves as,"
"stands as," "marks," "represents," "boasts," "features."

Before: "Gallery 825 serves as LAAA's exhibition space and boasts 3,000 sq ft."
After: "Gallery 825 is LAAA's exhibition space. It has 3,000 sq ft."

**9. Negative parallelisms**
"It's not just about X, it's Y." State Y directly.

Before: "It's not just about autocomplete; it's about unlocking creativity."
After: "It unlocks creativity at scale."

**10. Rule of three**
AI forces ideas into groups of three: "innovation, inspiration, and insights."
Use the natural number of items, often one or two.

Before: "The event features keynote sessions, panel discussions, and
networking opportunities."
After: "The event has talks and panels, with time between sessions for
informal conversation."

**11. Synonym cycling**
Repetition-penalty logic causes excessive substitution. Repeat the clearest noun.

Before: "The protagonist faces challenges. The main character must overcome
obstacles. The central figure eventually triumphs."
After: "The protagonist faces many challenges but eventually triumphs."

**12. False ranges**
"From X to Y" where X and Y are not on a meaningful scale.

Before: "From the Big Bang to dark matter, from stars to cosmic structure."
After: "The book covers the Big Bang, star formation, and dark matter theories."

**13. Passive voice and subjectless fragments**
Find the actor. Name them.

Before: "Mistakes were made. No configuration file needed."
After: "The team made a mistake. You do not need a config file."

### Style Patterns

**14. Em dash overuse**
Remove all em dashes (—) from prose. Applies in both Mode A and Mode B.
Em dashes are the single most reliable AI tell in generated text.

Three replacement rules:
- Prose aside or connector: period, comma, or hyphen (-); choose whichever preserves rhythm
- Inline list separator: comma or colon
- Title or label separator (project names, headings): colon

"Churn Prediction Pipeline — Deep Learning" → "Churn Prediction Pipeline:
Deep Learning". Do not use a comma for title separators.

See [vocabulary reference](./references/vocabulary.md) for before/after
examples of all three contexts.

**15. Boldface overuse**
Bold is for UI labels, not prose emphasis. Rewrite the sentence instead.

**16. Inline-header lists**
Bullet points starting with a bold word and colon ("**Speed:** Code generation
is faster") read as AI structure. Convert to prose.

**17. Title case headings**
Use sentence case: "Strategic negotiations and partnerships," not "Strategic
Negotiations And Partnerships."

**18. Emojis**
Remove all emojis in prose contexts.

Before: "We launched three new features! 🚀 Check them out below."
After: "We launched three new features. Details below."

**19. Curly quotes**
Use straight quotes ("") not curly quotes (" ").

**20. Hyphenated word pairs**
Drop hyphens from common pairs that do not need them.

Before: "cross-functional teams" / "data-driven decisions" / "client-facing roles"
After: "cross functional teams" / "data driven decisions" / "client facing roles"

**21. Persuasive authority tropes**
"At its core, what matters is..." State the point without the framing device.

Before: "At its core, this is a question of trust."
After: "This is a question of trust."

**22. Signposting announcements**
"Let's dive in," "Here's what you need to know," "Let me walk you through."
Start with the content.

**23. Fragmented headers**
A heading followed by one sentence that just restates it. Merge them or
let the heading carry the weight alone.

### Communication Patterns

**24. Chatbot artifacts**
Remove: "I hope this helps," "Let me know if you have questions," "Great
question!", "Of course!", "Certainly!", "Would you like me to expand?"

**25. Knowledge-cutoff disclaimers**
Remove: "As of my last update," "While specific details are limited based on
available information." Find the fact or cut the sentence.

**26. Sycophantic tone**
Remove: "You're absolutely right!", "That's an excellent point."

### Filler and Hedging

**27. Filler phrases**
"In order to" → "to." "Due to the fact that" → "because." "At this point in
time" → "now." Cut throat-clearing openers: "Here's the thing," "It turns
out," "The reality is." Full list: see [vocabulary reference](./references/vocabulary.md).

**28. Excessive hedging**
"Could potentially possibly" → "may." Cut stacked hedges.

Before: "It could potentially be argued that these tools might have some
positive effect."
After: "These tools help with repetitive tasks."

**29. Generic positive conclusions**
"The future looks bright. Exciting times lie ahead." Replace with the specific
next thing, or cut.

Before: "In conclusion, the future looks bright. Exciting times lie ahead."
After: "The company plans to open two more locations next year."

### Structural Rules (Stop-Slop)

**30. Binary contrasts**
Telegraphed reversals ("Not because X. Because Y.") manufacture drama. State Y.

**31. Negative listing**
Listing what something is *not* before saying what it *is*. State Z directly.

**32. Dramatic fragmentation**
"Speed. Quality. Cost." Fragments read as performed profundity. Use sentences.

**33. Rhetorical setups**
"What if I told you...?" "Think about it." Make the point without the setup.

**34. False agency**
Inanimate things do not perform human actions.

Before: "The decision emerged." / "The complaint became a fix."
After: "The team decided." / "Sarah fixed it that week."

**35. Rhythm monotony**
Three consecutive same-length sentences: break one. Every paragraph ending
with a punchy one-liner: vary it. Three-item lists: reduce to two or one.

**36. Wh- sentence starters**
Sentences starting with What, When, Where, Which, Who, Why, How. Restructure
to lead with the subject or verb.

Before: "What makes this hard is the coordination overhead."
After: "The coordination overhead is the hard part."

**37. Adverbs**
Kill all -ly adverbs. Also remove: really, just, literally, genuinely,
honestly, simply, actually, deeply, truly, fundamentally, inherently.

Before: "This is genuinely important and will significantly impact how teams
effectively collaborate going forward."
After: "This changes how teams collaborate."

**38. Business jargon**
Navigate → handle. Unpack → explain. Lean into → accept. Landscape → situation.
Double down → commit. Deep dive → analysis. Moving forward → next.
Full table: see [vocabulary reference](./references/vocabulary.md).

## Editorial Discipline (Karpathy Rules)

These govern *how* to edit. Voice injection (opinions, "I" and "you,"
specific feelings, some mess) is the fifth obligation; apply alongside
pattern removal, not after.

**Think before rewriting.** State what you assume the text is doing. If
scope is ambiguous or two interpretations exist, surface them. Never pick
silently.

**Minimum intervention.** Only change what has a documented pattern. Every
changed sentence traces to one of the 38 patterns or a missing voice beat.

**Surgical edits only.** Leave clean sentences alone. Do not reformat
untouched sections. Flag unrelated issues; do not fix them.

**Define success before starting.**
Transform the task into a verifiable goal before drafting:
- "Humanize this post" → "All patterns removed. Score 38/50+. Length ±10%."
- "Make this less robotic" → "No AI vocabulary [#7]. No passive voice [#13]. One opinion added."
- "Write in my style" → "Style Fidelity 8+/10. All 12 profile dimensions applied."

## Process

**Step 1: Identify the mode.**
- Mode A: AI text + sample → rewrite in user's style
- Mode B: Sample + brief, no input text → write from scratch in user's style
- No sample: humanize using the input text's existing register

**Step 2: Read for scope.**
Note complexity, technical depth, and length. All survive. State assumptions.
Ask if scope is unclear.

**Step 3: Define success criteria.**
Which patterns are present, target score, length constraints. Add "Style
Fidelity 8+/10" if a sample was provided.

**Step 4: Run Style Mirroring if a sample is provided.**
Extract the 12-dimension Style Profile. Show it. Wait for confirmation.
Confirm genre compatibility. Then proceed.

**Step 5: Draft.**
Mode A: Apply all 38 patterns. Touch only documented tells. Apply the
confirmed Style Profile to every structural decision.
Mode B: Generate from the brief. Apply the Style Profile from sentence one.
Do not default to a generic register.

**Step 6: Anti-AI audit.**
"What makes this still obviously AI-generated?" List remaining tells.
"Where did the style drift from the profile?" List those too.

**Step 7: Final rewrite.**
Address every audit item. Correct style drift. Produce the final version.

**Step 8: Score (1-10 each; below 35/50 on first five: revise).**

| Dimension | Low (1-3) | Mid (4-7) | High (8-10) |
|-----------|-----------|-----------|-------------|
| Directness | Announcements, "it's important to note" | States points, hedges conclusions | Every sentence makes a claim or moves forward |
| Rhythm | Metronomic, identical lengths | Some variation, still predictable | Clearly varied cadence |
| Trust | Over-explains, adds disclaimers | Some hand-holding | No pre-chewed conclusions |
| Authenticity | No opinions, no first person | One or two human moments | Opinions, reader addressed, specific |
| Preservation | Shorter or simplified vs. original | Mostly intact | Full complexity and length kept |
| Style Fidelity* | Generic prose, profile ignored | Most dimensions matched | All 12 dimensions match |

*Style Fidelity scored only when a sample was provided. Below 8: revise.

## Gotchas

1. **Fixing vocabulary while leaving structure intact.** Replacing "delve" with
   "examine" and "pivotal" with "significant" does not humanize the text if the
   sentence structure is still passive, the paragraph still ends with a punchy
   slogan, and the argument still uses a three-part list. Vocabulary and
   structure must both be addressed.

2. **Voice injection that contradicts the original tone.** Adding "I genuinely
   think..." to a formal institutional document or a technical spec inserts the
   wrong register. Voice injection should match what the text is for. A
   quarterly report and a blog post need different kinds of humanity.

3. **Treating the checklist as a sequential filter.** The checklist is a
   pre-delivery gate, not a rewriting method. Do not go line by line through
   the text checking for item 1, then item 2. Read the full text first, form a
   picture of what is broken, then rewrite with all patterns in mind at once.

4. **Partial clean text becomes full rewrite.** When only a few sentences are
   broken, the model tends to rewrite surrounding sentences "for flow." This
   violates the minimum-intervention rule. Fix the broken sentences. Note the
   joins that feel awkward. Stop there.

5. **Scoring the draft, not the final version.** The score applies to the
   output after the anti-AI audit and final rewrite, not to the draft. Scoring
   the draft and calling it done skips the most important iteration.

6. **Missing the register mismatch after voice calibration.** If a voice sample
   is provided and ignored, the output defaults to generic clean prose. The
   purpose of Style Mirroring is to produce a rewrite that sounds like the
   specific person, not a humanized version of nobody in particular.

7. **Mirroring across incompatible genres.** A Style Profile from a casual
   newsletter does not apply to a technical spec; flag genre mismatches and ask.

8. **Sample too short to extract reliably.** Below 300 words, habits can't
   be distinguished from accidents; flag uncertain dimensions and ask for more.

9. **User's sample contains AI patterns.** Do not mirror them. Mirror
   structural and tonal characteristics only. Flag the AI tells found so
   the user knows they are not being reproduced.

## Quick Checklist

Before delivering, confirm:

**Style Mirroring (if sample provided)**
- [ ] Mode identified: A (rewrite in style) or B (write from scratch in style)
- [ ] Style Profile extracted across all 12 dimensions
- [ ] Sample word count noted: flag if below 300 words
- [ ] AI patterns found in sample noted and excluded from mirroring
- [ ] Profile shown to user and confirmed before writing
- [ ] Genre compatibility confirmed
- [ ] Sentence length matches profile's typical range
- [ ] Fragments used only if sample uses them
- [ ] Contractions match sample exactly
- [ ] First/second person presence matches sample
- [ ] Rhythm pattern matches sample, not generic varied cadence
- [ ] Tone matches sample register
- [ ] Punctuation habits reproduced from sample (exception: em dashes in the sample are NOT reproduced; rule #14 overrides style fidelity)
- [ ] Style Fidelity scored 8+/10 before delivering

**Karpathy discipline**
- [ ] Assumptions about the text's purpose stated before editing
- [ ] Success criteria defined (target score, length, patterns to hit)
- [ ] Every changed sentence traces to a numbered pattern or voice gap
- [ ] No clean sentences rewritten speculatively
- [ ] No unrequested restructuring or reformatting
- [ ] No em dashes introduced during drafting [#14]: check before writing, not only after

**Pattern removal**
- [ ] No adverbs [#37]
- [ ] No passive voice: actor named [#13]
- [ ] No inanimate actors ("the decision emerged") [#34]
- [ ] No Wh- sentence starters [#36]
- [ ] No throat-clearing openers [#27]
- [ ] No "not X, it's Y" contrasts [#30]
- [ ] No three consecutive same-length sentences [#35]
- [ ] No paragraph ending with a punchy one-liner (unless varied) [#35]
- [ ] No em dashes anywhere [#14]
- [ ] No vague declaratives ("the implications are significant") [#1]
- [ ] No rule-of-three lists: reduce to two or one [#10]
- [ ] No curly quotes, boldface emphasis, emojis, inline-header bullets [#14-19]
- [ ] No AI vocabulary [#7]: see vocabulary reference for full list
- [ ] No chatbot artifacts [#24]
- [ ] No generic positive conclusion [#29]
- [ ] No signposting announcements [#22]
- [ ] No unnecessary hyphens in word pairs [#20]

**Voice**
- [ ] Personality present: opinions, specific feelings, reader addressed [Soul]

## Integration

- `karpathy-guidelines`: source for the four editorial discipline rules
- `humanizer`: upstream pattern reference; consult for edge cases beyond the 38
- `stop-slop`: upstream structural rules; consult when a structural pattern is ambiguous

## References

Internal:
- [Style profile reference](./references/style-profile.md) — profile template,
  12-dimension taxonomy, extraction algorithm, genre guide, worked examples
- [Worked example](./references/worked-example.md) — full eight-step process
  applied to AI-generated text
- [Vocabulary reference](./references/vocabulary.md) — full AI vocabulary list,
  jargon table, and throat-clearing openers
- [Genre calibration reference](./references/genre-calibration.md) — v7:
  genre-by-pattern exceptions for confirmed human-authored text

External:
- [blader/humanizer](https://github.com/blader/humanizer) — 29 patterns from Wikipedia's Signs of AI Writing (v2.5.1)
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
- [stop-slop](https://github.com/hvpandya/stop-slop) by Hardik Pandya
- [Karpathy on LLM pitfalls](https://x.com/karpathy/status/2015883857489522876)

---

## Skill Metadata

**Created**: 2025-06-07  **Updated**: 2026-07-04  **Version**: 7.0.0
**Based on**: blader/humanizer v2.5.1, stop-slop, human v6.0.0, Karpathy guidelines, skill-creator eval
