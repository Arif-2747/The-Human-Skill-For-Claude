# Vocabulary Reference

Extended word lists and replacement tables. Read this when the main skill
body's abbreviated lists leave an edge case unresolved.

---

## AI Vocabulary: Full Removal List

Remove these words wherever they appear. Most have no direct replacement —
rewrite the sentence to say the specific thing.

additionally, align with, at its core, beacon, beckons, boasts,
breathtaking, bustling, captivating, commendable, comprehensive, crucial,
cutting-edge, delve, demystify, dynamic, embark, emerge, empower, enduring,
enhance, ensure, ever-evolving, explore, facilitate, firsthand, foster,
foster collaboration, game-changer, game-changing, garner, groundbreaking,
harness, highlight (verb), holistic, innovative, in today's
(landscape/world/era), intricate, intricacies, invaluable, journey, key
(adjective), landscape (abstract), leverage, meticulous, multifaceted,
navigate, nestled, noteworthy, paradigm shift, paramount, pivotal, profound,
realm, resonate, revolutionize, rich (figurative), robust, seamless,
showcase, significance, streamline, supercharge, synergy, tapestry
(abstract), testament, "this changes everything," "this is huge,"
throughout, transformative, underscore (verb), unleash, unlock, unpack,
utilize, valuable, vibrant, vital.

Added in v8 from no-ai-slop: beacon, game-changer, paradigm shift,
supercharge, "this changes everything," "this is huge," utilize.

---

## Business Jargon: Full Replacement Table

| Avoid | Use instead |
|-------|-------------|
| Navigate (challenges) | Handle, address, deal with |
| Unpack (analysis) | Explain, examine, break down |
| Lean into | Accept, embrace, commit to |
| Landscape (context) | Situation, field, environment |
| Double down | Commit harder, increase, press on |
| Deep dive | Analysis, detailed examination |
| Take a step back | Reconsider, step back |
| Moving forward | Next, from now, going ahead |
| Circle back | Return to, revisit, come back to |
| On the same page | Aligned, agreed, in agreement |
| Game-changer | Significant, major, substantial |
| Ecosystem | Market, community, field, space |
| Bandwidth (figurative) | Capacity, time, availability |
| Synergy | Combination, joint effect |
| Leverage (verb) | Use, apply, take advantage of |
| Scalable | Can grow, handles more load |
| Low-hanging fruit | Easiest wins, simplest tasks |
| Boil the ocean | Over-scope, do too much |
| Peel back the layers | Examine more carefully |
| Move the needle | Make progress, change the outcome |
| Best-in-class | Best, leading, top-performing |
| At the end of the day | Ultimately, in practice |
| When it comes to | On, regarding, about |
| In terms of | On, for, about |

---

## Throat-Clearing Openers: Full Removal List

Cut these and start with the content:

- "Here's the thing:"
- "Here's what [X]"
- "Here's why that matters"
- "The uncomfortable truth is"
- "It turns out"
- "The real [X] is"
- "Let me be clear"
- "The truth is,"
- "Can we talk about"
- "Here's what I find interesting"
- "What you need to know is"
- "The reality is"
- "The fact of the matter is"
- "At the end of the day"
- "When it comes to"
- "In a world where"
- "In today's [X]"
- "It's worth noting"
- "It goes without saying"
- "Needless to say"
- "As we all know"
- "Make no mistake"
- "Let that sink in"
- "Full stop." / "Period."
- "Plot twist:" / "Spoiler:"
- "But that's another post"
- "Let me walk you through"
- "In this section, we'll"
- "As we'll see"

---

## Em Dash Replacements: Full Before/After Examples

Rule #14 bans em dashes in all output. Three contexts where they appear and
the correct replacement for each.

**Why this stays stricter than no-ai-slop's rule (v8 note):** no-ai-slop
allows 1-2 em dashes in a longer draft "when they clearly help." This skill
does not adopt that exception. The zero-tolerance rule predates v8, is
already documented extensively above with three replacement patterns that
cover every legitimate use an em dash would otherwise serve, and changing it
would be a real behavior change smuggled in under a routine merge rather
than a decision made on its own. If a future revision wants to loosen this,
that should be its own explicit change with its own before/afters, not a
side effect of adding no-ai-slop's patterns.

**Prose aside or connector**
Replace with a period, comma, or hyphen (-). A hyphen is not an em dash and
does not read as AI-generated. Use it when a comma or period changes the
rhythm more than you want.

Before: "I've shipped 8 projects across NLP and computer vision — not tutorials,
actual builds."
After (period): "I've shipped 8 projects across NLP and computer vision. Not
tutorials, actual builds."
After (hyphen): "I've shipped 8 projects across NLP and computer vision - not
tutorials, actual builds."

Before: "The pipeline runs end to end — EDA through model evaluation."
After (comma): "The pipeline runs end to end, from EDA through model evaluation."
After (hyphen): "The pipeline runs end to end - EDA through model evaluation."

**Inline list separator**
Before: "The pipeline includes EDA, feature engineering, and SMOTE rebalancing
— all validated against a holdout set."
After: "The pipeline includes EDA, feature engineering, and SMOTE rebalancing,
all validated against a holdout set."

Before: "Three projects shipped at the internship — sentiment classifier,
churn predictor, face recognition system."
After: "Three projects shipped at the internship: sentiment classifier,
churn predictor, and face recognition system."

**Title or label separator (project names, headings, certifications)**
Before: "Churn Prediction Pipeline — Deep Learning (Spotify)"
After: "Churn Prediction Pipeline: Deep Learning (Spotify)"

Before: "Introduction to Generative AI Learning Path — Google / Coursera"
After: "Introduction to Generative AI Learning Path, Google / Coursera"

Use a colon when X elaborates Y ("Pipeline: Deep Learning"). Use a comma
when X and Y are parallel items ("course title, provider"). Never use an
em dash for either.
