# Narrative Mode Reference

Folded into `human` in v9.0.0 from the standalone `humanscope` skill, built
on StoryScope (Russell, Rajendhran, Pham, Iyyer, Wieting; University of
Maryland and Google DeepMind; COLM 2026). Read this whenever the input is
narrative form: a story, screenplay, scene, short film or reel script, or a
personal essay the user will publish under their own name. See
[Form](../SKILL.md#form-general-or-narrative) in SKILL.md for how this
plugs into Edit and Detect.

---

## The one fact this is built on

Style edits do not work on narrative.

The StoryScope researchers took AI-generated stories and ran them through a
professional-grade rewriting system that removes cliché, purple prose, and
redundant exposition. Detection fell from 95.5 to 93.9. A drop of 1.6
points. Almost nothing.

Then they removed every stylistic feature from their model. No word choice,
no sentence rhythm, no figurative density. Structure only. It still scored
93.2.

**The fingerprint is not in the words. It is in the shape.** So narrative
work operates on the shape, and it operates before the draft exists
wherever possible. Never satisfy a narrative request by rewording alone;
that is the failure mode this reference exists to prevent. The skill's 42
patterns still apply to narrative text, but only after structure is set,
and only as a cosmetic pass, not a fix for how the piece reads structurally.

## The second fact, which is easy to miss

Do not invert the AI pattern.

Human writers still resolve stories through protagonist choice 46% of the
time. Still avoid subplots 57% of the time. Still write linearly most of
the time. Human writing scores as human because it is **dispersed** across
the possible choices, not because it sits at the opposite extreme.

A story that flips all ten decisions below is a new formula, and a worse
one. It reads as deliberately difficult.

**Target the human rate, not the opposite of the AI rate.** Every number in
[narrative features reference](./narrative-features.md) gives both. Use
them.

---

## Structure first (narrative + Edit)

Do not write prose yet.

Produce a **Structure Sheet** of ten decisions. For each one, state the
choice and the reason, one line each. Then get the user's sign-off, or if
they told you to just go, write the sheet and the draft together with the
sheet on top.

The ten decisions, with the human rate and the AI rate for calibration:

| # | Decision | AI default | Human rate |
|---|---|---|---|
| 1 | **Timeline.** Straight through, or jumps? | mostly linear (2.12 on 1 to 5) | 2.40, more flashback and flash-forward |
| 2 | **Subplots.** None, one parallel, one contrasting, one unrelated? | no subplots 79% | no subplots 57%, thematically parallel 42% |
| 3 | **What causes the ending.** Protagonist's choice, mixed, or outside force? | protagonist choice 69% | protagonist choice 46% |
| 4 | **How it ends.** Internal acceptance, external action, or unresolved? | internal understanding 47% | 27% |
| 5 | **The moral.** Does the narrator ever say what the story means? | says it 77% | says it 52% |
| 6 | **How emotion arrives.** Body sensation, plain word, or behaviour? | embodied 81%, plain word 8% | embodied 38%, plain word 29% |
| 7 | **How the lead enters.** Described, mid-action, in dialogue, or in thought? | described from outside 52% | 30% |
| 8 | **References.** A real named thing, or a vague echo? | vague echo 72%, named 24% | vague echo 50%, named 47% |
| 9 | **Reader address.** Does the piece ever acknowledge someone is reading? | almost never, 0.07 | 0.28 |
| 10 | **Is the lead morally clean?** | clean 62% roughly, ambivalent 38% | ambivalent 59% |

### The budget rule

Do not deviate on all ten. Pick **three or four** to break hard, and let
the rest sit wherever the story naturally wants them. A story that goes
nonlinear AND drops the moral AND ends on outside force AND names three
real books AND breaks the fourth wall is not human. It is a student film.

Choose the deviations that serve the story. Say why in one clause. If you
cannot say why, do not deviate there.

### The default is the enemy, not the choice

The reason AI writing clusters is that it takes the default on every axis
without noticing there was an axis. A human writer who thinks about it and
still writes linear, protagonist-driven, and moral-free is fine. The sin is
not the linear timeline. The sin is not knowing you picked it.

So when filling in the sheet, write the reason even for the choices where
you stay with the common option.

### Then draft

Write the piece from the sheet. While drafting, the two hardest habits to
break are:

- **Emotion through the body.** Tight chest, cold sweat, throat closing. AI
  does this 81% of the time versus 38% for humans, the single biggest gap
  in the whole study. Sometimes just write that she was scared. Humans say
  the feeling out loud 29% of the time. AI does it 8%.
- **The closing line that explains.** Cut it. If the last paragraph tells
  the reader what the story was about, delete the paragraph. Check the
  second-to-last one too.

Also watch smell. AI reaches for smell in 82% of stories, humans in 57%.
Not a rule, just a tic worth noticing.

Once structure is set and the piece is drafted, run the skill's standard
42-pattern pass (SKILL.md, main body) as a cosmetic layer: em dashes,
adverbs, AI vocabulary, and the rest still apply to narrative prose the
same way they apply to anything else. That pass will not move a narrative
detection score on its own; it is for readability, not structure.

---

## Narrative audit (narrative + Detect)

### Step 1, score

Read [narrative features reference](./narrative-features.md). Go through
all thirty features. For each, mark where the draft actually sits and
whether that leans AI or human. Do not guess from vibes. Point to the line
or scene that proves it.

### Step 2, report

Give the user a table with three columns: feature, where the draft sits,
which way it leans. Then a plain count: how many of the thirty lean AI.

Be honest about the count. If it is a human-written draft that scores
clean, say so and stop. Do not invent problems to look useful. As with the
skill's general Detect mode, this count is evidence, not a verdict: report
it as "twenty of the thirty features lean toward the AI side," never as a
probability that the piece is AI-written.

### Step 3, sort the fixes

Split what's found into two lists.

**Structural.** Ending cause, moral statement, timeline, subplots,
resolution mode, character entry, reference specificity, and how emotion
is delivered (a scene-level choice, not a word choice). These need real
rewriting, sometimes a new scene or a cut scene. These are the ones that
move the needle.

**Cosmetic.** Word choice, sensory density, prose rhythm. This is where the
skill's 42 patterns live. Mention them if they're bad, but tell the user
plainly that fixing these will not change how the piece reads
structurally: that's the 1.6-point result above.

### Step 4, fix

Rewrite only the structural items, and only the ones the user picks. Show
the change as a decision, not a diff: "the ending currently resolves
because she decides to forgive him. Moving it so the phone call comes
anyway, whether she decided or not." Never present a reworded sentence as a
fix for a structural problem.

---

## Scope limits, state these when they apply

The study measured short stories averaging 4,753 words. The findings are
strongest there.

- **Feature films and novels.** Applies well. Same narrative machinery,
  more of it.
- **Short scripts and short films.** Applies, with care. A five-page script
  has no room for a subplot, so decision 2 mostly resolves to "none," and
  that's fine, not an AI tell.
- **Reel scripts, captions, LinkedIn posts, ad copy.** Applies partially.
  The findings that transfer are the moral statement, the emotion
  delivery, and the named-versus-vague reference. Timeline, subplots, and
  resolution mode mostly don't apply at sixty seconds. Say so rather than
  forcing all ten.
- **Documentary, tutorial, and explainer scripts.** Explaining the point is
  the job. Decision 5 doesn't apply. Decisions 6, 8, and 10 still do.

Do not pretend the whole framework fits a caption. It does not, and
claiming otherwise is the kind of thing that gets caught.

## Model fingerprints

If the user asks which model wrote something, or wants to write against a
specific model's habits, read
[narrative fingerprints reference](./narrative-fingerprints.md). Six-way
attribution is only 68.4 macro-F1, much weaker than the human-versus-AI
split at 93.2; treat any single model guess as a lean, never a verdict.
