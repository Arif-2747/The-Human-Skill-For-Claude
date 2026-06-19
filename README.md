<!-- Copyright (c) 2026 Arif Islam Shaik (github.com/Arif-2747). All rights reserved. -->
# human

An AI skill for removing AI writing patterns from text, or writing new content
in your voice. It makes generated prose sound like a person wrote it, without
simplifying or shortening anything.

---

## What it does

You give it AI-generated text. It removes the patterns that make it obvious a
machine wrote it, fixes sentence structures that no person would use, and adds
genuine voice where the original has none. The meaning, length, and complexity
of the input survive intact. Nothing gets summarized. Nothing gets dumbed down.

The only things that change are the tells.

If you also provide a writing sample, it extracts your style and applies it to
the output. This works in two modes: rewrite existing AI text to sound like you
wrote it (Mode A), or generate entirely new content from a brief in your voice
(Mode B).

---

## Why this exists

Two good skills already do parts of this job.

[blader/humanizer](https://github.com/blader/humanizer) is a pattern reference
built from Wikipedia's Signs of AI Writing guide. It names 29 specific AI tells
with before/after examples. It is the best documented source for what AI prose
actually looks like, and it is the primary reference for this skill.

[stop-slop](https://github.com/hvpandya/stop-slop) by Hardik Pandya covers
structural problems: binary contrasts, false agency, passive voice, rhythm
monotony, dramatic fragmentation, and the rest of the sentence-level habits
that make AI prose feel engineered. It addresses how AI constructs sentences,
not just which words it picks.

Both are useful. Neither is complete on its own, and neither addresses the
failure mode that matters most in practice: an AI editor that over-corrects.

---

## The problem neither source solves

Give a capable model either skill and ask it to humanize a piece of text. It
will often rewrite sentences that didn't need rewriting. It will restructure
clean paragraphs. It will "improve" adjacent prose while fixing the broken
sentences, because improving things is what it defaults to when given latitude.

The result is a rewrite that removed the AI tells and also changed everything
else. The original voice is gone. The length changed. The specific details the
author chose got paraphrased into something slightly different. The text sounds
less robotic and also less like the person who wrote it.

This is the over-correction problem. It happens because pattern-removal without
editorial discipline is just substitution at scale.

---

## What this skill adds

This skill combines the 38 humanizer and stop-slop patterns with four
behavioral principles adapted from
[Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876)
on how LLMs go wrong when editing code. The principles translate directly to
prose editing.

**Think before rewriting.** State what you assume the text is trying to do
before touching anything. If the scope is ambiguous, ask. If two
interpretations exist, surface both. Hidden assumptions produce rewrites that
fix the wrong thing.

**Minimum intervention.** Only change what has a documented AI tell. Every
changed sentence must trace to one of the 38 patterns or a missing voice beat.
Nothing else.

**Surgical edits.** Leave clean sentences alone, even if you would phrase them
differently. Don't reformat sections that weren't touched. Don't improve
adjacent prose. If you notice something unrelated that could be better, name it
in a note and leave it for the writer to decide.

**Define success before starting.** "Humanize this" is not a goal. "Remove all
patterns, score 38/50 or above, length within 10% of original" is. Strong
criteria let the model self-correct. Weak criteria produce inconsistent results
and require constant clarification.

These four rules govern how to edit. The 38 patterns govern what to edit for.
Neither set works well without the other.

---

## What makes it different from humanizer

Humanizer is a reference. It tells you what the patterns are. This skill is an
executable workflow: it identifies the mode, states assumptions, defines success
criteria, extracts a Style Profile when a sample is provided, drafts, audits,
and scores the output before delivering. It also adds voice injection as a
first-class step, so the output doesn't just lack AI tells but actually sounds
like a person with opinions and a specific way of seeing things.

Humanizer does not address over-correction. This skill does.

---

## What makes it different from stop-slop

Stop-slop focuses on structural patterns at the sentence level. It is excellent
at identifying the constructions that feel engineered. It does not cover the 29
content and language patterns from Wikipedia's AI writing guide, and it does
not have a process for ensuring the rewrite stays within scope.

Stop-slop also does not address what to do when the text is mostly clean, only
three sentences are broken, and the naive approach is to rewrite everything.
This skill tells you to fix the three sentences and leave the rest.

---

## Style Mirroring

If you provide a writing sample alongside your request, the skill runs Style
Mirroring before writing anything.

**Mode A — Rewrite in your style.** Give it AI-generated text and a sample of
your writing. It removes the 38 patterns and rewrites the output to match your
voice, not a generic clean register.

**Mode B — Write from scratch in your style.** Give it a sample and a topic or
brief, with no input text to rewrite. It generates new content that sounds like
you wrote it.

Both modes start with a Style Profile: a 12-dimension analysis of your sample
covering sentence length, rhythm, fragment use, contractions, tone, opinion
presence, punctuation habits, and more. The skill outputs the profile and waits
for your confirmation before writing anything. This is the Karpathy gate applied
to style: state assumptions explicitly, surface them, never pick silently.

Minimum reliable sample size is 300 words. Below that, the skill flags which
dimensions are uncertain rather than presenting everything with false confidence.

If your sample contains AI tells (passive voice, adverbs, significance
inflation), those are not mirrored. The skill flags them and applies the 38
patterns regardless.

The full profile template, extraction algorithm, dimension taxonomy, genre
compatibility guide, and worked examples for both modes are in
`references/style-profile.md`.

---

## The scoring system

After every rewrite or draft, the skill scores the output on six dimensions,
each rated 1-10:

| Dimension | Question |
|-----------|----------|
| Directness | Statements or announcements? |
| Rhythm | Varied or metronomic? |
| Trust | Respects reader intelligence? |
| Authenticity | Sounds human? |
| Preservation | Full complexity kept? |
| Style Fidelity | All 12 profile dimensions matched? |

A score below 35/50 on the first five triggers another revision. Style Fidelity
applies only when a writing sample was provided; below 8/10 also triggers a
revision. These are the only automated gates in the process. Everything else
is judgment.

---

## Installation

Copy `SKILL.md` into your skills directory. Copy the `references/` folder
alongside it. The skill activates when you ask Claude to humanize, de-AI,
rewrite for naturalness, remove AI patterns, or write new content in your style.

It does not activate for code review, translation, factual research, or original
drafting where no style sample exists and no AI text needs humanizing.

**To use Style Mirroring:** upload or paste your writing sample in the same
message as your request. The skill detects it and runs the Style Profile
extraction automatically.

---

## File structure

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
README.md                         This file
```

---

## References

- [blader/humanizer](https://github.com/blader/humanizer) — 29 AI writing
  patterns sourced from Wikipedia's Signs of AI Writing guide (v2.5.1)
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) —
  the upstream source maintained by WikiProject AI Cleanup
- [stop-slop](https://github.com/hvpandya/stop-slop) by Hardik Pandya —
  structural rules covering binary contrasts, false agency, rhythm, and voice
- [Karpathy on LLM pitfalls](https://x.com/karpathy/status/2015883857489522876) —
  the source for the four editorial discipline principles

---

## License

Copyright (c) 2026 Arif Islam Shaik. All rights reserved.

This skill and all files in this repository are the intellectual property of
[Arif Islam Shaik](https://github.com/Arif-2747). Reproduction, distribution,
or use in any form without express written permission is prohibited.

See the [LICENSE](./LICENSE) file for the full terms.
