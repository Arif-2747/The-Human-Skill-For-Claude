# Genre Calibration Reference

New in v7. Read this before flagging patterns #7, #13, #27, #28, or #38 in
text that is confirmed human-authored rather than AI-generated or mixed.

## Why this exists

A handful of the 42 patterns are also normal, expected register choices in
specific professional genres. Passive voice is standard in legal and
scientific writing. Hedging is standard in academic writing. Domain jargon
is standard in technical writing. Without a genre check, the skill treats a
lawyer's or scientist's ordinary professional register as an AI tell and
"corrects" it into something that no longer reads as belonging to that
field. That is a false positive: the text was never AI-generated, and the
pattern was never a tell in the first place.

This reference exists to stop that specific failure mode. It does not
change how patterns are treated in AI-generated or mixed-origin text — see
[Scope](#scope) below.

## Scope

This calibration applies only when the text's origin is confirmed
human-authored: a Style Mirroring sample, a user's own draft submitted for
line editing, or any input the user has stated is not AI-generated.

It does **not** apply to:
- Mode A input text (the AI-generated text being rewritten) — every one of
  the 42 patterns still applies there, regardless of genre. AI-generated
  legal or scientific text overuses these same patterns far beyond what a
  human writer in that genre would produce; genre alone does not excuse it.
- Pattern #14 (em dashes). No genre uses em dashes as a structural
  requirement; this pattern is never genre-exempted.
- Any pattern not listed in the table below. The exception list is narrow
  by design — most of the 42 patterns are tells in every genre.

## Genre-by-pattern exception table

| Genre | Pattern normally a tell | Why it's a convention here | Still flag if |
|---|---|---|---|
| Legal (contracts, briefs, statutes) | #13 Passive voice | Passive voice is the standard construction for obligations and prohibitions ("the payment shall be made") | The actor is knowable and the passive is used to obscure it, not for legal convention |
| Legal | #27 Filler phrases ("in order to," "pursuant to") | Formal connective tissue expected in the register | Used outside of formal clauses, in plain narrative sections |
| Scientific / academic | #13 Passive voice | Methods sections conventionally foreground the procedure, not the researcher | Used in the discussion or conclusion where the author's claims should be attributed |
| Academic | #28 Hedging | Calibrated uncertainty is a genre requirement ("these results suggest," "may indicate") | Hedging is stacked three or more qualifiers deep, past what the actual uncertainty warrants |
| Technical documentation | #7 AI vocabulary (subset: domain terms only) | Precise technical terminology is required for accuracy, not padding | The term is a vague abstraction rather than a precise technical one (e.g. "leverage the framework" vs. "call the framework's API") |
| Technical documentation | #38 Business/technical jargon | Some jargon is the accurate term of art, not a euphemism | The term has a plainer exact equivalent and is used to sound impressive rather than to be precise |

## How to apply this

1. Confirm the text's origin (see Scope). If uncertain whether text is
   human-authored or AI-generated, do not apply calibration — treat all
   patterns as tells and ask the user to confirm origin if it matters.
2. Identify the genre. If the genre is ambiguous, do not guess; ask, or
   default to treating the pattern as a tell.
3. Check the table. If the specific instance matches the "why it's a
   convention" column and does not match the "still flag if" column, leave
   it. Note in your summary that it was left as a genre convention, not
   missed.
4. Everything not covered by the table follows the normal rules in
   `SKILL.md` with no exception.

## Worked contrast

**Legal clause (human-authored, leave as is):**
"Notice shall be deemed given when delivered to the address specified in
Section 4." — Passive voice, but this is standard contract drafting; the
obligation-holder is the subject of the surrounding clause, not hidden.

**Marketing copy using the same construction (flag as #13):**
"Real results are delivered, every time." — Passive voice here hides who
delivers the results and reads as evasive rather than professional; this is
a tell, not a convention, because marketing copy has no genre requirement
for passive voice.

## Relationship to the Genre Compatibility Guide

This file governs whether a *pattern* is a tell given the text's genre. The
[Genre Compatibility Guide in style-profile.md](./style-profile.md#genre-compatibility-guide)
governs a different question: whether a Style Profile extracted from one
genre can be applied to a target text in a different genre. Use both when
running Style Mirroring on a human-authored sample: this file to avoid
misflagging the sample's genre-conventional patterns as AI tells, and the
compatibility guide to confirm the sample and target genres match.
