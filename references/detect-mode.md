# Detect Mode Reference

New in v8, from no-ai-slop. Read this when the user asks whether a draft
reads as AI, asks to audit or scan a piece, or asks the skill to flag
patterns without rewriting anything.

---

## Why Detect is a separate job from Edit

An AI-detection classifier gives a probability score with no reasoning the
user can check. This skill instead treats detection as an editorial task:
walk the same 42 patterns used for editing, and report which ones actually
appear, with the exact line quoted. The user gets evidence, not a verdict.
That is a deliberate, narrower claim than "this is AI-written," and the
skill never makes the wider claim. See the "What this isn't" section of the
README for how this fits the project's stance on AI-detection evasion: the
skill is not built to help text get past a detector, and Detect mode is not
built to replace one either. It is a second use of the same pattern library,
pointed at diagnosis instead of revision.

## Output template

Use this structure for every Detect response:

```
DETECT: [N patterns found / no patterns found]

[#7] AI vocabulary
  "...the platform's intricate, multifaceted approach to..."
  Fix: name the specific mechanism instead of "intricate, multifaceted."

[#21] Persuasive authority trope / faux-insight setup
  "Here's what nobody tells you about onboarding funnels."
  Fix: state the claim about onboarding funnels directly.

[#35] Rhythm monotony (fake-profound kicker)
  "The team didn't just ship a feature. They shipped a philosophy."
  Fix: delete; end on the last concrete sentence already in the draft.

Want me to edit this draft? I can fix these without changing anything else.
```

Rules for this output:

- One entry per pattern *type* found, not per occurrence. If "delve" appears
  four times, that is one AI vocabulary entry with one representative quote,
  not four entries.
- Quote the line exactly as written in the draft. Do not paraphrase the
  quote; paraphrasing here would misrepresent what the pattern-check found.
- The fix line is a few words, not a rewrite. "Fix: name the source" is
  correct. Writing out the corrected sentence is not; that is Edit's job.
- If genuinely no patterns are present, say so plainly rather than
  straining to find something. A clean draft is a valid Detect result.
- Close with an offer to run Edit on the same draft, not an automatic
  transition into editing it.

## Using the no-sample voice-signal note during Detect

Edit mode's no-sample step (see SKILL.md, Style Mirroring) asks the model to
note 3-5 voice signals already present in the input before drafting a
rewrite. Detect mode benefits from the same read, for a different reason:
it helps tell a genuine voice quirk apart from a pattern. A writer who
always opens sentences with "and" is using a deliberate fragment style, not
committing pattern #32 (dramatic fragmentation). Read for voice signals
first, then check patterns against what's left. This prevents Detect from
flagging a writer's real habits as AI tells.

## Worked example

**Input (a user's LinkedIn draft, pasted for a Detect request):**

> I've been thinking about onboarding a lot lately. Here's what nobody
> tells you about onboarding funnels: the drop-off isn't at signup. It's
> at day three. We rebuilt our email sequence around that insight,
> reducing churn and enhancing the overall user journey. The results speak
> for themselves — engagement is up, and honestly, that's the whole point.
> At the end of the day, it's not about the tool. It's about the habit.

**Detect output:**

```
DETECT: 4 patterns found

[#21] Faux-insight setup
  "Here's what nobody tells you about onboarding funnels."
  Fix: state the day-three drop-off finding directly.

[#7] AI vocabulary
  "reducing churn and enhancing the overall user journey"
  Fix: say what changed in the email sequence and what it did to churn.

[#14] Em dash
  "engagement is up, and honestly, that's the whole point"
  (dash before "and honestly") — replace with a period or comma.

[#9] Negative parallelism
  "it's not about the tool. It's about the habit."
  Fix: state the habit claim directly.

Want me to edit this draft? I can fix these without changing anything else,
including your opening line and the day-three finding, which both read as
your own voice already.
```

Note what this leaves alone: the opening "I've been thinking about
onboarding a lot lately" and the day-three finding itself are not flagged.
They are specific, sourced, and read as the writer's own observation, not a
pattern.
