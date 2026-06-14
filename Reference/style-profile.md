# Style Profile Reference

Full template, dimension taxonomy, extraction algorithm, genre compatibility
guide, and worked examples for Style Mirroring. Read this when extracting a
Style Profile from a writing sample, diagnosing style drift in output, or
running Mode B (write from scratch in user's style).

---

## The Style Profile Template

Output this verbatim after extracting. The user must be able to read and
correct it before you write a single output sentence.

```
STYLE PROFILE — extracted from: [document name / "pasted sample" / filename]
Sample word count:  [N words — flag if below 300]

Sentence length:    [short <12w / mixed / long >25w — typical range: N–N words]
Sentence structure: [declarative / complex / fragment-heavy / clause-heavy]
Paragraph length:   [1–2 sentences / 3–5 sentences / long blocks]
Rhythm pattern:     [e.g. "short-short-long", "consistently medium", "variable"]
First person:       [always / occasional / never]
Second person:      [always / occasional / never]
Contractions:       [always / sometimes / never]
Fragments:          [yes — example: "[quote from sample]" / no]
Recurring phrases:  [up to 5 quoted from the sample]
Tone:               [dry / warm / blunt / discursive / formal / wry / other]
Opinion presence:   [frequent / occasional / absent]
Typical opener:     [subject-first / verb-first / clause-first / fragment]
Punctuation habits: [e.g. "uses parentheses for asides", "minimal commas", "no semicolons"]
AI patterns found:  [list any AI tells in the sample — these are NOT mirrored]
```

After presenting the profile: "Does this match your writing? Anything to
correct?" Do not write output until the user confirms or corrects.

---

## Dimension Taxonomy

### 1. Sentence length
Count actual words per sentence across the sample. Do not estimate visually.

- Short: median below 12 words
- Mixed: range spans more than 15 words (e.g. 8–27)
- Long: median above 25 words

Include the typical range in the profile (e.g. "mixed — typically 10–22 words").

### 2. Sentence structure
- Declarative: subject + verb + object, minimal subordination
- Complex: frequent subordinate clauses, participial phrases, appositives
- Fragment-heavy: intentional incomplete sentences used for rhythm or emphasis
- Clause-heavy: long sentences built from multiple coordinated or subordinated clauses

### 3. Paragraph length
Count sentences per paragraph across a representative section.
1–2 sentences: punchy, newsletter-style. 3–5: standard essay. Long blocks: academic or discursive.

### 4. Rhythm pattern
Read three consecutive paragraphs aloud. What do you notice?
- "short-short-long": punchy opener, punchy follow, long payoff
- "long-short": heavy setup, quick landing
- "consistently medium": no variation, metronomic (common in AI text)
- "variable": no consistent pattern — this is typical of good human writing

### 5. First and second person
Count "I", "me", "my", "we" (first) and "you", "your" (second).
- Always: present in nearly every paragraph
- Occasional: appears but not dominant
- Never: absent throughout

### 6. Contractions
Scan for: don't, isn't, it's, I've, you're, can't, won't.
- Always: appears freely throughout
- Sometimes: used in informal moments, avoided in formal ones
- Never: consistently expanded ("do not", "it is", "I have")

### 7. Fragments
A fragment is a sentence without a main verb or subject.
"Not ideal." "Three months, no results." "Because of course."
If present, quote one from the sample. Note whether they appear at
paragraph ends (landing-beat use) or mid-paragraph (rhythm use).

### 8. Recurring phrases
Copy five phrases or constructions verbatim from the sample that feel
characteristic. These can be: favorite transitional words, typical clause
openers, recurring hedges, characteristic punctuation patterns. They are
the fingerprints of the writer's voice.

### 9. Tone
- Dry: flat affect, no enthusiasm, understatement preferred
- Warm: inclusive, generous, reader is in the room as a friend
- Blunt: direct, no hedging, sometimes abrupt
- Discursive: exploratory, follows a thought wherever it leads
- Formal: distance maintained, third person preferred, no contractions
- Wry: dry humor, irony, slight detachment from the subject

### 10. Opinion presence
- Frequent: takes positions on most claims, "I think/believe/find"
- Occasional: opinions appear but are not the dominant mode
- Absent: reports without editorializing

### 11. Typical opener
Read the first sentence of each paragraph. What pattern do you see?
- Subject-first: "The problem is..." / "We built this because..."
- Verb-first: "Building this required..." / "Understanding the gap means..."
- Clause-first: "When we looked at the data..." / "Despite the criticism..."
- Fragment: "Three months in." / "Here's what happened."

### 12. Punctuation habits
Note unusual patterns:
- Parentheses for asides (like this)
- Dashes for asides — like this — (note: NOT em dashes if the skill applies)
- Semicolons to join related sentences
- Colons to introduce lists or elaborations
- Minimal commas even in long sentences
- Oxford comma always/never
- Ellipses for trailing thoughts...

---

## Extraction Algorithm

Follow this sequence for any sample, regardless of length.

**Step 1: Read without extracting.**
Read the full sample once. Do not take notes. Form an impression of the
overall register and feel.

**Step 2: Count mechanically.**
Go back through the sample and count:
- Word count total
- Sentences per paragraph (three representative paragraphs)
- Words per sentence (ten representative sentences)
- First-person pronoun frequency
- Contraction count

**Step 3: Extract qualitatively.**
Scan for:
- Five recurring phrases, quoted verbatim
- Three examples of the typical opener structure
- Any fragments — quote one if found
- Tone — pick one from the taxonomy and note why
- Punctuation habits — note anything unusual

**Step 4: Check for AI tells.**
Scan the sample against the 38 patterns. List any found. These are not
mirrored; they are flagged and removed even in style-matched output.

**Step 5: Flag uncertain dimensions.**
If the sample is below 300 words, note which dimensions have too little
data for confident extraction. Common casualties: rhythm (needs multiple
paragraphs), fragment use (needs enough sentences to be statistically
meaningful), recurring phrases (needs enough repetition to confirm habit).

**Step 6: Output the profile.**
Fill the template. Present it. Wait for confirmation.

---

## Genre Compatibility Guide

The sample's genre and the target text's genre must be compatible for the
Style Profile to apply correctly. A profile extracted from one genre applied
to a different genre produces wrong output even if technically accurate.

| Sample genre | Compatible targets | Incompatible targets |
|---|---|---|
| Personal essay / blog | Newsletter, opinion piece, personal narrative | Legal brief, technical spec, academic paper |
| Technical documentation | Technical blog, README, developer newsletter | Personal essay, fiction, marketing copy |
| Academic paper | Research summary, literature review, formal report | Newsletter, personal essay, marketing copy |
| Marketing copy | Product announcement, landing page, pitch deck | Technical documentation, academic paper |
| Newsletter | Blog post, personal essay, opinion piece | Legal brief, technical specification |
| Social media thread | Newsletter opener, blog intro | Long-form essay, technical documentation |

When sample and target conflict: present both genres to the user. Ask which
register to use. Do not guess.

---

## Worked Example: Mode A (Rewrite in User's Style)

### Writing sample provided (185 words — flagged as below 300)

> I've been thinking about this for a while. The argument that AI will take all
> the jobs isn't wrong, exactly. It's just incomplete.
>
> What actually happens is more like: the jobs change. The people who adapt
> quickly do fine. The people who don't, don't. This has always been true.
> The printing press, the loom, the spreadsheet.
>
> The part nobody wants to say out loud is that "adapt quickly" correlates
> pretty strongly with resources. Time, money, access to training. So when
> people say "just learn to code" they're not wrong about what helps. They're
> wrong about how easy it is to just do that.
>
> I don't have a good solution. I'm not sure anyone does yet.

### Style Profile extracted

```
STYLE PROFILE — extracted from: pasted sample
Sample word count:  185 words — below 300, rhythm and recurring phrases uncertain

Sentence length:    short — typical range: 6–18 words
Sentence structure: declarative, some fragment use
Paragraph length:   2–4 sentences
Rhythm pattern:     variable — short punchy paragraph, longer explanatory, short landing [uncertain — small sample]
First person:       occasional (I've, I'm, I don't)
Second person:      never in body, "you" used in reported speech only
Contractions:       always (it's, don't, they're, I'm)
Fragments:          yes — "The printing press, the loom, the spreadsheet."
Recurring phrases:  "just [verb]", "not wrong... just [correction]", list as fragment [uncertain]
Tone:               dry, slightly wry
Opinion presence:   frequent but hedged ("I don't have a good solution")
Typical opener:     subject-first or clause-first
Punctuation habits: minimal, no em dashes, colon used once to introduce elaboration
AI patterns found:  none detected
```

"Does this match your writing? The sample is short so rhythm and recurring
phrases are uncertain — happy to adjust before I rewrite."

### Input to rewrite (AI-generated)

> The discourse surrounding artificial intelligence and employment is
> increasingly complex, requiring nuanced analysis. While some experts argue
> that technological displacement will inevitably render many roles obsolete,
> others maintain that new opportunities will emerge to compensate. The
> relationship between technological change and labor markets has historically
> demonstrated remarkable resilience, suggesting that adaptation remains
> possible. However, it is crucial to acknowledge that the capacity for
> adaptation is not uniformly distributed across socioeconomic groups.

### Rewrite (Mode A — patterns removed, style applied)

> The AI-and-jobs argument isn't wrong. It's just incomplete.
>
> What usually happens is the jobs change. People who adapt do fine. People
> who don't, don't. The printing press, the loom, the spreadsheet. This is
> not a new pattern.
>
> The part that gets left out: "adapting" takes time, money, and access to
> training. When someone says "just reskill," they're not wrong about what
> helps. They're wrong about how easy it is to just do that.

---

## Worked Example: Mode B (Write from Scratch in User's Style)

Using the same Style Profile above.

### Brief provided by user

"Write a short piece about why most productivity advice doesn't work."

### Draft (Mode B — written from scratch in user's style)

> Most productivity advice is designed for people who are already pretty
> productive.
>
> The systems, the apps, the morning routines. They work fine if your problem
> is optimization. If your problem is something else — exhaustion, unclear
> priorities, a job that doesn't make sense — they don't really touch it.
>
> This isn't a failure of the advice. It's a category error. You can't
> time-block your way out of not knowing what you're doing.
>
> I've tried most of them. The ones that stuck were simple. The ones that
> didn't were the ones that assumed I had more bandwidth than I did.

### Style Fidelity check

- Short declarative sentences: yes
- Fragments as landing beats: "A category error." (converted to full sentence for clarity — flagged)
- Contractions: always (isn't, don't, can't, I've)
- Dry, wry tone: yes ("You can't time-block your way out of not knowing what you're doing")
- First person occasional, hedged opinion at end: yes
- No em dashes: yes
- Rhythm variable: yes

Style Fidelity score: 9/10
