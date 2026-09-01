# The 30 core features

From StoryScope (Russell, Rajendhran, Pham, Iyyer, Wieting. University of Maryland and Google DeepMind. COLM 2026). Table 16 of the paper.

These are the thirty narrative features that stayed important and stable across all five AI models tested. Together they hold about 91% of the full narrative model's separating power using 30 features instead of 257.

**How to read the columns.** For features marked `scale` the number is a mean on a 1 to 5 rating. For features marked `ordinal` it is a mean over integer codes. For features written with an arrow the number is a percentage of stories where that specific option showed up. The AI column is averaged across Claude Sonnet 4.6, GPT-5.4, Gemini 3 Flash, DeepSeek V3.2, and Kimi K2.5.

**How to use them.** For each feature, place the draft on the axis, then compare to both numbers. Ask which of the two rates the draft is closer to. Do not treat the human number as a target to hit exactly. Human writing is spread wide; that spread is the actual signal.

---

## Group 1. Thematic over-determination (AI does more of this)

This is the largest and most reliable cluster. AI spells out meaning instead of trusting the reader to find it.

| Feature | Human | AI |
|---|---|---|
| Thematic explicitness and moralizing (scale 1-5) | 3.28 | 3.94 |
| Moral or philosophical weighting (scale 1-5) | 3.26 | 3.68 |
| Thematic unity, how much everything serves one idea (scale 1-5) | 4.41 | 4.74 |
| Narrator explicitly states the theme → yes | 52% | 77% |
| Dialogue used for philosophical debate | 34% | 59% |
| References are implicit echoes rather than named | 50% | 72% |

**What to look for in a draft.** A final paragraph that names the lesson. A character who says out loud what the film is about. Two characters debating an abstract idea in a scene that does not advance anything. Subplots that all rhyme too neatly with the main theme.

---

## Group 2. Sensory and embodied performance (AI does more of this)

AI shows feeling through bodies and weather instead of naming it.

| Feature | Human | AI |
|---|---|---|
| Emotion conveyed through embodied metaphor | 38% | 81% |
| Setting mirrors the character's inner state (scale 1-5) | 3.58 | 4.07 |
| Environmental and ecological emphasis (scale 1-5) | 2.83 | 3.21 |
| Smell used as a sensory channel | 57% | 82% |
| Overall sensory density (scale 1-5) | 3.66 | 3.93 |
| Depth of access to interior thought (scale 1-5) | 3.67 | 3.93 |

**The single biggest gap in the study is the first row.** 81% versus 38%. Tightening chest, cold sweat, dimming lamplight. Rain during the sad scene. Sunshine when it resolves.

**What to look for.** Count how many emotions in the draft are delivered as physical sensation versus stated plainly. If it is above roughly two thirds physical, that alone is a strong AI signal.

---

## Group 3. Structural streamlining (AI does more of this)

AI tells one clean track from cause to effect with no loose ends.

| Feature | Human | AI |
|---|---|---|
| Continuity of the main causal chain (scale 1-5) | 3.92 | 4.20 |
| Spatial granularity, how finely space is described (ordinal) | 2.27 | 2.53 |
| Resolution driven by protagonist's own choice | 46% | 69% |
| Lead character introduced by outside description | 30% | 52% |
| No subplots at all | 57% | 79% |
| Ending resolved through internal understanding or acceptance | 27% | 47% |
| Opening grounds the reader in a specific place (ordinal) | 2.12 | 2.33 |
| Investment built in the character before the danger arrives (scale 1-5) | 2.76 | 2.99 |

**What to look for.** Every scene causes the next scene with nothing hanging. The protagonist earns the ending by deciding something. The ending is a realisation rather than an event. The opening paragraph is a described room.

---

## Group 4. Intertextual richness (humans do more of this)

| Feature | Human | AI |
|---|---|---|
| Uses an explicit named reference to a real text, work, or author | 47% | 24% |
| Mixes explicit and implicit references rather than only one kind | 37% | 16% |

**What to look for.** AI writes "an old book of poems." A human writes "the Neruda." AI avoids naming real brands, real places, real films. Humans name them constantly.

This one is cheap to fix and genuinely effective. Naming real things is a structural choice, not a word swap, because it commits the story to a real world.

---

## Group 5. Reader engagement (humans do more of this)

| Feature | Human | AI |
|---|---|---|
| Fourth wall permeability (ordinal) | 0.67 | 0.39 |
| Frequency of direct reader address (ordinal) | 0.28 | 0.07 |

Humans break the fourth wall 67% versus 39%, and address the reader directly 28% versus 7%. As the paper puts it, AI writes as if no one is watching.

**Caution.** This one is genre-dependent and overusing it is obvious. A single aside in a piece that otherwise never does it can read as a mistake. Use it when the voice supports it.

---

## Group 6. Temporal complexity (humans do more of this)

| Feature | Human | AI |
|---|---|---|
| How much a revelation forces you to reread earlier scenes (scale 1-5) | 3.28 | 2.95 |
| Degree of chronological discontinuity (scale 1-5) | 2.40 | 2.12 |
| Nonlinear framing used to delay a disclosure (scale 1-5) | 1.96 | 1.68 |
| Anachrony intensity, reliance on flashback and flash-forward (scale 1-5) | 2.58 | 2.31 |

**Note the absolute values.** Human writing sits at 2.40 out of 5 on time jumps. That is mostly linear. The gap is real but the human baseline is not "always fragmented." Do not turn a piece into a puzzle box to score points here.

The paper's example: a human mystery might open at the funeral and spiral backward through decades. AI tells the same story from first clue to grand reveal.

---

## Group 7. Narrative diversity (humans do more of this)

| Feature | Human | AI |
|---|---|---|
| Variety of distinct locations (ordinal) | 1.34 | 1.08 |
| Proportion of dialogue to narration (scale 1-5) | 2.95 | 2.70 |
| Subplot that runs thematically parallel to the main line | 42% | 21% |
| Protagonist framed as morally ambivalent or mixed | 59% | 38% |
| Emotion conveyed through an explicit named feeling | 29% | 8% |

**The last row is the flip side of the biggest gap.** Humans just write "she was angry" nearly four times as often as AI does.

**Moral ambivalence is the one most worth taking seriously for scripts.** AI writes protagonists whose choices the story approves of. Humans write protagonists the story is not sure about.

---

## Scoring a draft

Go feature by feature. For each, write:

- where the draft actually sits, with the line or scene that proves it
- which side it leans, human or AI
- whether fixing it requires restructuring or only rewording

Then count how many of the thirty lean AI, and report that number plainly.

**The count is a diagnostic, not a verdict.** The published classifier used XGBoost over encoded feature vectors trained on 52,000 stories. A hand count of thirty features by eye is not that. Say so. Report it as "twenty of the thirty features lean toward the AI side" and never as "this is 93% likely to be AI."

**Sort the fixes before offering them.** Anything in groups 1, 3, 4, 6, and 7 is structural. Most of group 2 is structural too, because how emotion is delivered is a scene-level choice, not a word choice. Only sensory density and prose rhythm are cosmetic, and the paper showed that fixing cosmetics moves detection by 1.6 points.
