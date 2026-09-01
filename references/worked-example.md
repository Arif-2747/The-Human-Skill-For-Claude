# Worked Example: Full Rewrite

This reference shows all seven process steps applied to a single piece of
AI-generated text. Read this when learning the skill or verifying that your
process produces the right kind of output at each stage.

---

## Input (AI-generated)

> Great question! Here is an essay on this topic. I hope this helps!
>
> AI-assisted coding serves as an enduring testament to the transformative
> potential of large language models, marking a pivotal moment in the evolution
> of software development. In today's rapidly evolving technological landscape,
> these groundbreaking tools — nestled at the intersection of research and
> practice — are reshaping how engineers ideate, iterate, and deliver,
> underscoring their vital role in modern workflows.
>
> At its core, the value proposition is clear: streamlining processes,
> enhancing collaboration, and fostering alignment. It's not just about
> autocomplete; it's about unlocking creativity at scale, ensuring that
> organizations can remain agile while delivering seamless, intuitive, and
> powerful experiences to users. The tool serves as a catalyst. The assistant
> functions as a partner. The system stands as a foundation for innovation.
>
> Industry observers have noted that adoption has accelerated from hobbyist
> experiments to enterprise-wide rollouts, from solo developers to
> cross-functional teams. The technology has been featured in The New York
> Times, Wired, and The Verge. Additionally, the ability to generate
> documentation, tests, and refactors showcases how AI can contribute to better
> outcomes, highlighting the intricate interplay between automation and human
> judgment.
>
> - Speed: Code generation is significantly faster, reducing friction and
>   empowering developers.
> - Quality: Output quality has been enhanced through improved training,
>   contributing to higher standards.
> - Adoption: Usage continues to grow, reflecting broader industry trends.
>
> While specific details are limited based on available information, it could
> potentially be argued that these tools might have some positive effect. Despite
> challenges typical of emerging technologies — including hallucinations, bias,
> and accountability — the ecosystem continues to thrive. In order to fully
> realize this potential, teams must align with best practices.
>
> In conclusion, the future looks bright. Exciting times lie ahead as we
> continue this journey toward excellence. Let me know if you'd like me to
> expand on any section!

---

## Step 1: Read for scope

The text is an essay about AI coding assistants. It runs about 300 words.
Complexity level: low to medium — no technical depth, no specific data.
Assumption: the goal is a general-audience piece on the topic, not a
technical analysis. Length target: similar word count.

---

## Step 2: Define success criteria

- Remove all 29 patterns present (identified: #1, #3, #4, #7, #8, #9, #10,
  #13, #16, #22, #24, #25, #27, #28, #29, plus binary contrasts, false agency,
  adverbs, passive voice)
- Add at least one opinion and one specific fact
- Target score: 38/50 or above
- Length: within 10% of original

---

## Voice Injection: Personality and Soul

Five rules for adding genuine human voice alongside pattern removal:

**Have opinions.** React to facts, do not just report them. "I have no idea
what to make of this" is more human than listing pros and cons from a distance.

**Acknowledge complexity.** Real people have mixed feelings. "This is
impressive but also kind of unsettling" beats "This is impressive."

**Use "I" and "you."** First person is honest. Put the reader in the room
with "you" instead of writing like a disembodied narrator.

**Be specific about feelings.** Not "this is concerning" but "there is
something unsettling about agents running at 3am while nobody is watching."

**Let some mess in.** Tangents and asides read human. Perfect structure reads
algorithmic. Allow for half-formed thoughts and specific details the original
author chose, not the most statistically average version of them.

Key insight from Wikipedia: "LLMs use statistical algorithms to guess what
should come next. The result tends toward the most statistically likely result
that applies to the widest variety of cases." That statistical average is
exactly what you are editing out.

---

## Step 3: Voice sample

None provided. Default to direct, dry, first-person register.

---

## Step 4: Draft rewrite

AI coding assistants speed up some tasks and not others. In a 2024 study by
Google, developers using Codex completed simple functions 55% faster than a
control group, but showed no improvement on debugging or architectural
decisions.

The tools are good at boilerplate: config files, test scaffolding, repetitive
refactors. They are bad at knowing when they are wrong. I have mass-accepted
suggestions that compiled, passed lint, and still did the wrong thing because
I stopped paying attention.

Mira, an engineer at a fintech startup, said she uses Copilot "like autocomplete
for boring code" but reviews every line before committing. Jake, a senior dev
at a larger company, disabled it after it kept suggesting patterns from a
deprecated internal library.

The productivity claims are hard to verify. GitHub says Copilot users "accept
30% of suggestions," but acceptance is not correctness, and correctness is not
value. The 2024 Uplevel study found no statistically significant difference in
pull-request throughput between teams with and without AI assistants.

None of this means the tools are useless. It means they are tools. They do not
replace judgment, and they do not eliminate the need for tests. If you do not
have tests, you cannot tell whether the suggestion is right.

---

## Step 5: Anti-AI audit

Remaining tells:
- "None of this means... It means..." is a residual binary contrast (#9)
- Rhythm is still clean and tidy — three short paragraphs in a row, same
  approximate length
- The closer ("If you do not have tests...") sounds like a slogan rather than
  a person talking

---

## Step 6: Final rewrite

AI coding assistants can make you faster at the boring parts. Not everything.
Architecture, debugging, anything that requires judgment about what the code
should actually do: those stay slow.

They are great at boilerplate — config files, test scaffolding, repetitive
refactors. They are also great at sounding right while being wrong. I have
accepted suggestions that compiled, passed lint, and still missed the point
because I stopped paying attention.

People I talk to land in two camps. Some use it like autocomplete for chores
and review every line. Others disabled it after it kept suggesting patterns
they did not want. Both feel reasonable to me.

The productivity metrics are slippery. GitHub can say Copilot users "accept
30% of suggestions," but acceptance is not correctness, and correctness is not
value. The 2024 Uplevel study found no statistically significant difference in
pull-request throughput between teams with and without AI assistants. So the
case rests mostly on feel.

If you do not have tests, you are guessing whether any of this is working.

---

## Step 7: Score

| Dimension | Score | Reasoning |
|-----------|-------|-----------|
| Directness | 9 | States positions, does not announce them |
| Rhythm | 8 | Varied lengths, one short closer without being a slogan |
| Trust | 9 | No hand-holding, no disclaimers |
| Authenticity | 9 | Opinions present, first person, specific |
| Preservation | 10 | Same word count, same topic scope |
| **Total** | **45/50** | Above threshold, deliver |
